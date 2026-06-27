# Pick & Go 영화 데이터 & API 연동 설명 문서

## 0. 전체 데이터 흐름 개요

Pick & Go의 영화 데이터는 **TMDB API**를 핵심으로,
**YouTube API**, **GMS(OpenAI) API**를 보조적으로 활용하여 구성된다.

```
TMDB API
  ├── 영화 기본 정보 (제목·줄거리·포스터·평점·인기도·투표수)
  ├── 장르 정보
  ├── 감독·배우 크레딧 정보
  ├── 예고편 video_key → YouTube 임베드
  └── 현재 상영작 (now_playing)

YouTube Data API
  └── TMDB에서 video_key 못 찾을 때 fallback 검색

GMS API (GPT-4.1-mini)
  ├── 오늘의 추천 AI 큐레이션
  └── 픽키 AI 챗봇 추천

DB (SQLite)
  ├── Movie 테이블 — 1,400+ 편 저장
  ├── Genre 테이블 — 19개 장르
  ├── MovieCalendarEntry 테이블 — 유저별 달력 기록
  └── MovieAiMatch 테이블 — MVTI별 AI 매칭 점수 캐시
```

### 실제 URL 및 페이지 구성

| URL | Vue 파일 | 설명 |
|-----|---------|------|
| `/` | HomeView.vue | 홈 (MVTI 기반 배너) |
| `/mood` | MoodRecommendView.vue | 오늘의 추천 (감정 기반 AI) |
| `/mvti-intro` | MvtiIntroView.vue | MVTI 소개 슬라이드 |
| `/mvti-test` | MvtiTestView.vue | MVTI 테스트 |
| `/mvti-result` | MvtiTestView.vue | MVTI 결과 포토카드 |
| `/recommended` | RecommendedView.vue | MVTI 맞춤 추천 영화 목록 |
| `/platform` | Top10View.vue | TOP 10 PickScore 랭킹 |
| `/cinema` | CinemaView.vue | 현재 상영작 |
| `/friend-match` | FriendMatchView.vue | 친구 궁합 |
| `/movies` | MoviesExploreView.vue | 영화 탐색·검색 |
| `/movies/:id` | MovieDetailView.vue | 영화 상세·예고편 |
| `/community` | CommunityView.vue | 리뷰 커뮤니티 |
| `/community/create` | ReviewCreateView.vue | 리뷰 작성 |
| `/community/:id` | ReviewDetailView.vue | 리뷰 상세·댓글 |
| `/calendar` | CalendarView.vue | 영화 달력 |
| `/profile` | ProfileView.vue | 내 프로필 |
| `/profile/:username` | ProfileView.vue | 친구 프로필 |

---

## 1. TMDB API 기본 연동

### 1-1. 사용 엔드포인트 목록

| 엔드포인트 | 용도 |
|-----------|------|
| `/discover/movie` | 영화 대량 수집 (인기순) |
| `/genre/movie/list` | 장르 목록 수집 |
| `/movie/{id}/credits` | 감독·배우 크레딧 정보 |
| `/movie/{id}/videos` | 예고편 YouTube video_key |
| `/movie/now_playing` | 현재 상영작 |
| `/search/movie` | 제목 기반 영화 검색 (실시간) |

### 1-2. 인증 방식

```python
# settings.py
TMDB_ACCESS_TOKEN = os.environ.get("TMDB_ACCESS_TOKEN", "")

# 요청 헤더
headers = {"Authorization": f"Bearer {TMDB_ACCESS_TOKEN}"}
```

TMDB는 API Key 방식과 Bearer Token 방식을 모두 지원한다.
Pick & Go는 **Bearer Token (Access Token)** 방식을 사용한다.

### 1-3. 공통 요청 파라미터

```python
params = {
    "language": "ko-KR",   # 한국어 응답
    "region": "KR",        # 한국 지역 기준
    "page": 1,
}
```

---

## 2. 영화 데이터 수집 — update_movies 커맨드

### 2-1. 수집 전략

TMDB `/discover/movie` 엔드포인트를 페이지 단위로 반복 호출하여
DB에 영화를 저장한다.

```
수집 기준:
- 인기도(popularity) 높은 순
- 한국어 정보 제공 여부
- vote_count ≥ 10 (품질 최소 기준)
- 최대 1,400+ 편 수집
```

```python
# movies/management/commands/update_movies.py 핵심 로직

import requests
from movies.models import Movie, Genre
from django.conf import settings

TMDB_TOKEN = settings.TMDB_ACCESS_TOKEN
BASE_URL = "https://api.themoviedb.org/3"
HEADERS = {"Authorization": f"Bearer {TMDB_TOKEN}"}

def fetch_movies(page=1):
    res = requests.get(
        f"{BASE_URL}/discover/movie",
        headers=HEADERS,
        params={
            "language": "ko-KR",
            "sort_by": "popularity.desc",
            "page": page,
            "vote_count.gte": 10,
        },
        timeout=10,
    )
    return res.json().get("results", [])

def save_movie(m, genre_map):
    movie, _ = Movie.objects.update_or_create(
        title=m["title"],
        release_date=m.get("release_date") or "1900-01-01",
        defaults={
            "overview": m.get("overview", ""),
            "poster_path": m.get("poster_path", ""),
            "popularity": m.get("popularity", 0),
            "vote_count": m.get("vote_count", 0),
            "vote_average": m.get("vote_average", 0),
        }
    )
    genre_ids = m.get("genre_ids", [])
    genres = [genre_map[gid] for gid in genre_ids if gid in genre_map]
    movie.genres.set(genres)
    return movie
```

### 2-2. 장르 데이터 수집

```python
def fetch_genres():
    res = requests.get(
        f"{BASE_URL}/genre/movie/list",
        headers=HEADERS,
        params={"language": "ko-KR"},
    )
    genres = res.json().get("genres", [])
    genre_map = {}
    for g in genres:
        obj, _ = Genre.objects.get_or_create(
            id=g["id"], defaults={"name": g["name"]}
        )
        genre_map[g["id"]] = obj
    return genre_map
```

TMDB 장르 ID는 전 세계 공통 고정값이다.

| ID | 장르 | ID | 장르 |
|----|------|----|------|
| 28 | 액션 | 18 | 드라마 |
| 35 | 코미디 | 10749 | 로맨스 |
| 878 | SF | 27 | 공포 |
| 14 | 판타지 | 53 | 스릴러 |
| 80 | 범죄 | 9648 | 미스터리 |
| 10751 | 가족 | 16 | 애니메이션 |

### 2-3. 실행

```bash
python manage.py update_movies
# → 총 1,400+ 편 DB 저장
```

---

## 3. 감독·배우 데이터 수집 — update_credits 커맨드

### 3-1. 기획 배경

초기 Movie 모델에는 `director`, `actors` 필드가 없었다.
"봉준호 영화 찾아줘", "톰 크루즈 영화" 같은 검색을 지원하려면
감독·배우 정보를 DB에 직접 저장해야 했다.

```
문제: title 검색만 가능 → 감독·배우명으로 검색 불가
해결: Movie 모델에 director, actors 필드 추가
     + TMDB /movie/{id}/credits API로 2,604편 처리
```

### 3-2. 모델 변경

```python
# movies/models.py
class Movie(models.Model):
    title        = models.CharField(max_length=100)
    # ... 기존 필드 ...
    director = models.CharField(max_length=200, blank=True, default='')  # 신규 추가
    actors   = models.CharField(max_length=500, blank=True, default='')  # 신규 추가
    genres   = models.ManyToManyField(Genre)
```

### 3-3. 핵심 제약 — tmdb_id 없는 문제

Movie 모델에 `tmdb_id` 필드가 없어서 TMDB ID를 직접 알 수 없었다.

```
해결 방법:
1. title로 TMDB /search/movie 검색 → 첫 번째 결과의 id를 임시 tmdb_id로 사용
2. 해당 tmdb_id로 /movie/{id}/credits 호출
3. crew에서 job == "Director" 추출
4. cast 상위 3명 추출
```

```python
def find_tmdb_id(title):
    res = requests.get(
        f"{BASE_URL}/search/movie",
        headers=HEADERS,
        params={"query": title, "language": "ko-KR", "page": 1},
        timeout=8,
    )
    results = res.json().get("results", [])
    return results[0]["id"] if results else None

def get_credits(tmdb_id):
    res = requests.get(
        f"{BASE_URL}/movie/{tmdb_id}/credits",
        headers=HEADERS,
        params={"language": "ko-KR"},
        timeout=8,
    )
    data = res.json()

    # 감독 추출
    crew = data.get("crew", [])
    directors = [p["name"] for p in crew if p.get("job") == "Director"]
    director = directors[0] if directors else ""

    # 주연 배우 최대 3명 (cast 순서 = 비중 순)
    cast = data.get("cast", [])
    actors = ", ".join([p["name"] for p in cast[:3]])

    return director, actors
```

### 3-4. 실행

```bash
python manage.py update_credits
# → 총 2,604편 감독·배우 정보 업데이트 완료
# → 50편마다 진행상황 출력
```

### 3-5. 통합 검색 적용

```python
# movies/views.py — movie_search

movies = movies.filter(
    Q(title__icontains=query) |
    Q(director__icontains=query) |
    Q(actors__icontains=query)
)
```

이로써 **"봉준호"**, **"톰 크루즈"**, **"노란문"** 등
제목·감독·배우 통합 검색이 가능해졌다.

---

## 4. 영화 탐색 — DB 검색 + TMDB 실시간 보완

### 4-1. 탐색 흐름

```
사용자가 검색어 입력
    ↓
1. DB에서 title + director + actors 검색 (icontains)
    ↓
DB 결과 3편 미만?
    ├── YES → TMDB /search/movie 실시간 검색으로 보완
    └── NO  → DB 결과 반환
```

### 4-2. 백엔드 — DB 검색

```python
# movies/views.py — movie_search

@require_safe
def movie_search(request):
    query = request.GET.get("q", "").strip()
    genre_id = request.GET.get("genre", "")
    movies = Movie.objects.all()

    if query:
        movies = movies.filter(
            Q(title__icontains=query) |
            Q(director__icontains=query) |
            Q(actors__icontains=query)
        )
    if genre_id:
        movies = movies.filter(genres__id=genre_id)

    movies = movies.order_by("-popularity").distinct()[:100]
    return JsonResponse({"movies": [serialize_movie(m, request.user) for m in movies]})
```

### 4-3. 프론트 — TMDB 실시간 보완

```javascript
// frontend/src/views/MoviesExploreView.vue

async function searchMovies(query) {
    // 1. DB 검색
    const dbResults = await moviesStore.searchMovies(query)

    if (dbResults.length >= 3) {
        movieSearchResults.value = dbResults.slice(0, 8)
        return
    }

    // 2. DB 결과 부족 → TMDB 실시간 보완
    try {
        const res = await axios.get('/api/movies/tmdb-search/', {
            params: { q: query }
        })
        const tmdbResults = res.data.movies || []
        const dbTitles = new Set(dbResults.map(m => m.title))

        // DB 중복 제거 후 합치기
        const combined = [
            ...dbResults,
            ...tmdbResults.filter(m => !dbTitles.has(m.title))
        ]
        movieSearchResults.value = combined.slice(0, 8)
    } catch {
        movieSearchResults.value = dbResults.slice(0, 8)
    }
}
```

### 4-4. TMDB 실시간 검색 API

```python
# movies/views.py — tmdb_search

@require_safe
def tmdb_search(request):
    query = request.GET.get("q", "").strip()
    if not query:
        return JsonResponse({"movies": []})

    import requests as req
    res = req.get(
        "https://api.themoviedb.org/3/search/movie",
        headers={"Authorization": f"Bearer {settings.TMDB_ACCESS_TOKEN}"},
        params={"query": query, "language": "ko-KR", "page": 1},
        timeout=5,
    )
    results = res.json().get("results", [])
    movies = []
    for m in results[:8]:
        if not m.get("poster_path"):   # 포스터 없는 영화 제외
            continue
        movies.append({
            "id": m["id"],
            "title": m.get("title", ""),
            "poster_path": m.get("poster_path", ""),
            "vote_average": round(m.get("vote_average", 0), 1),
            "release_date": m.get("release_date", ""),
            "overview": m.get("overview", ""),
        })
    return JsonResponse({"movies": movies})
```

---

## 5. 예고편 연동 — TMDB + YouTube API

### 5-1. 핵심 제약

Movie 모델에 `tmdb_id` 필드가 없다.
예고편을 찾으려면 **제목으로 TMDB를 검색해서**
tmdb_id를 동적으로 얻어야 한다.

### 5-2. 예고편 조회 3단계 흐름

```
영화 상세 페이지 접근
    ↓
/api/movies/{movie_pk}/trailer/ 호출
    ↓
Step 1. movie.title → TMDB /search/movie → tmdb_id 획득
    ↓
Step 2. tmdb_id → /movie/{tmdb_id}/videos 호출
         한국어(ko-KR) 우선 → 없으면 영어(en-US) 재시도
    ↓
Step 3. TMDB에서도 못 찾음?
         YouTube Data API fallback 검색
    ↓
{"video_id": "YouTube키"} 반환
    ↓
프론트: <iframe src="https://www.youtube.com/embed/{video_id}" />
```

### 5-3. 백엔드 구현

```python
# movies/views.py — movie_trailer

@require_safe
def movie_trailer(request, movie_pk):
    import requests as req

    TMDB_TOKEN = getattr(settings, "TMDB_ACCESS_TOKEN", "")
    YOUTUBE_KEY = getattr(settings, "YOUTUBE_API_KEY", "")

    try:
        movie = Movie.objects.get(pk=movie_pk)
    except Movie.DoesNotExist:
        return JsonResponse({"error": "영화를 찾을 수 없어요"}, status=404)

    try:
        # Step 1. 제목으로 TMDB ID 동적 조회
        search_res = req.get(
            "https://api.themoviedb.org/3/search/movie",
            headers={"Authorization": f"Bearer {TMDB_TOKEN}"},
            params={"query": movie.title, "language": "ko-KR", "page": 1},
            timeout=5,
        )
        results = search_res.json().get("results", [])
        tmdb_id = results[0]["id"] if results else None

        # Step 2. TMDB videos (한국어 → 영어 순으로 시도)
        if tmdb_id:
            for lang in ["ko-KR", "en-US"]:
                vres = req.get(
                    f"https://api.themoviedb.org/3/movie/{tmdb_id}/videos",
                    headers={"Authorization": f"Bearer {TMDB_TOKEN}"},
                    params={"language": lang},
                    timeout=5,
                )
                videos = vres.json().get("results", [])
                trailers = [
                    v for v in videos
                    if v.get("site") == "YouTube"
                    and v.get("type") in ["Trailer", "Teaser"]
                ]
                if trailers:
                    return JsonResponse({"video_id": trailers[0]["key"]})
    except Exception:
        pass

    # Step 3. YouTube Data API fallback
    if YOUTUBE_KEY:
        try:
            yres = req.get(
                "https://www.googleapis.com/youtube/v3/search",
                params={
                    "part": "snippet",
                    "q": f"{movie.title} 공식 예고편 trailer",
                    "type": "video",
                    "maxResults": 1,
                    "key": YOUTUBE_KEY,
                },
                timeout=5,
            )
            items = yres.json().get("items", [])
            if items:
                return JsonResponse({"video_id": items[0]["id"]["videoId"]})
        except Exception:
            pass

    return JsonResponse({"video_id": None})
```

---

## 6. 현재 상영작 — TMDB now_playing

### 6-1. 기능 개요

극장에서 현재 상영 중인 영화 목록을 실시간으로 표시한다.
DB에 저장하지 않고 **매 요청마다 TMDB에서 직접 조회**한다.

`/cinema` 페이지(`CinemaView.vue`)에서 사용한다.

### 6-2. 구현

```python
# movies/views.py — now_playing

@require_safe
def now_playing(request):
    import requests as req

    TMDB_TOKEN = settings.TMDB_ACCESS_TOKEN
    movies = []

    try:
        res = req.get(
            "https://api.themoviedb.org/3/movie/now_playing",
            headers={"Authorization": f"Bearer {TMDB_TOKEN}"},
            params={"language": "ko-KR", "region": "KR", "page": 1},
            timeout=5,
        )
        for m in res.json().get("results", [])[:20]:
            # MVTI 있는 유저는 장르 일치도 기반 match_percent 계산
            match_percent = None
            if (request.user.is_authenticated
                    and hasattr(request.user, "mvti_type")
                    and request.user.mvti_type):
                genre_ids = MVTI_GENRE_IDS.get(request.user.mvti_type, [])
                matched = len(set(genre_ids) & set(m.get("genre_ids", [])))
                match_percent = 95 if matched >= 2 else (75 if matched == 1 else 45)

            movies.append({
                "id": m.get("id"),
                "title": m.get("title", ""),
                "poster_path": m.get("poster_path", ""),
                "vote_average": round(m.get("vote_average", 0), 1),
                "release_date": m.get("release_date", ""),
                "overview": m.get("overview", ""),
                "genre_ids": m.get("genre_ids", []),
                "match_percent": match_percent,
            })

    except Exception:
        # TMDB 실패 시 DB 인기 영화로 fallback
        movies = [
            serialize_movie(m, request.user)
            for m in Movie.objects.order_by("-popularity")[:20]
        ]

    return JsonResponse({
        "movies": movies,
        "cinemas": [
            {"id": "cgv",    "name": "CGV",       "color": "#E50914"},
            {"id": "lotte",  "name": "롯데시네마", "color": "#E8211C"},
            {"id": "megabox","name": "메가박스",   "color": "#7C3AED"},
        ],
    })
```

---

## 7. 영화 뽑기 — movie_pool

### 7-1. 기능 개요

전체 영화 DB에서 조건에 맞는 영화 풀을 제공하여
프론트엔드에서 룰렛·랜덤 탐색에 활용한다.

### 7-2. 구현

```python
# movies/views.py — movie_pool

@require_safe
def movie_pool(request):
    """룰렛/탐색용 전체 영화 풀 — vote_count 기준 필터링"""
    movies = (
        Movie.objects.prefetch_related("genres")
        .filter(vote_count__gte=5)      # 최소 품질 기준
        .order_by("-vote_average")[:1000]
    )
    data = []
    for m in movies:
        data.append({
            "id": m.pk,
            "title": m.title,
            "poster_path": m.poster_path or "",
            "vote_average": round(float(m.vote_average), 1),
            "release_date": str(m.release_date) if m.release_date else "",
            "overview": m.overview or "",
            "genres": [{"id": g.id, "name": g.name} for g in m.genres.all()],
            "popularity": float(m.popularity),
            "match_percent": None,
        })
    return JsonResponse({"movies": data})
```

---

## 8. 영화 달력 DB — MovieCalendarEntry

### 8-1. 개선 배경

초기 설계에서 영화 달력은 `localStorage`에 저장했다.

```
문제: 다른 기기에서 로그인하면 달력 기록이 사라짐
     → 배포 환경에서 사용 불가

해결: MovieCalendarEntry 모델 신규 생성 + REST API 구현
     → DB에 저장되어 어느 기기에서도 동기화
```

### 8-2. 모델

```python
# movies/models.py

class MovieCalendarEntry(models.Model):
    user        = models.ForeignKey('accounts.User', on_delete=models.CASCADE,
                                    related_name='calendar_entries')
    movie_title = models.CharField(max_length=200)
    poster_path = models.CharField(max_length=200, blank=True, default='')
    watch_date  = models.DateField()
    memo        = models.TextField(blank=True, default='')
    created_at  = models.DateTimeField(auto_now_add=True)

    class Meta:
        ordering = ['-watch_date']
```

### 8-3. API

```python
# movies/views.py

@csrf_exempt
@login_required
@require_http_methods(['GET', 'POST'])
def calendar_list(request):
    if request.method == 'GET':
        year  = request.GET.get('year')
        month = request.GET.get('month')
        entries = MovieCalendarEntry.objects.filter(user=request.user)
        if year:  entries = entries.filter(watch_date__year=year)
        if month: entries = entries.filter(watch_date__month=month)
        return JsonResponse({'entries': [{
            'id': e.id,
            'movie_title': e.movie_title,
            'poster_path': e.poster_path,
            'watch_date': str(e.watch_date),
            'memo': e.memo,
        } for e in entries]})

    body = json.loads(request.body)
    entry = MovieCalendarEntry.objects.create(
        user=request.user,
        movie_title=body.get('movie_title', ''),
        poster_path=body.get('poster_path', ''),
        watch_date=body.get('watch_date'),
        memo=body.get('memo', ''),
    )
    return JsonResponse({
        'id': entry.id,
        'movie_title': entry.movie_title,
        'watch_date': str(entry.watch_date),
    }, status=201)


@csrf_exempt
@login_required
@require_http_methods(['DELETE'])
def calendar_detail(request, entry_pk):
    try:
        entry = MovieCalendarEntry.objects.get(pk=entry_pk, user=request.user)
    except MovieCalendarEntry.DoesNotExist:
        return JsonResponse({'error': '기록을 찾을 수 없어요'}, status=404)
    entry.delete()
    return JsonResponse({'message': '삭제됐어요'})
```

### 8-4. 프론트 전환 핵심

```javascript
// CalendarView.vue — localStorage → DB API 전환

// 기존 (localStorage)
function getCalData() {
    return JSON.parse(localStorage.getItem(CAL_KEY.value) || '{}')
}

// 변경 (DB API)
async function loadCalData() {
    const res = await axios.get('/api/movies/calendar/')
    const entries = res.data.entries || []
    const data = {}
    entries.forEach(e => {
        if (!data[e.watch_date]) data[e.watch_date] = { movies: [] }
        data[e.watch_date].movies.push({
            entryId: e.id,            // DB PK (삭제 시 사용)
            title: e.movie_title,
            poster_path: e.poster_path,
        })
    })
    calendarData.value = data
}
```

---

## 9. API 사용 현황 및 보안 관리

### 9-1. API 목록

| API | 용도 | 인증 방식 | 환경변수 |
|-----|------|----------|---------|
| TMDB API | 영화 수집·검색·예고편·상영작 | Bearer Token | `TMDB_ACCESS_TOKEN` |
| YouTube Data API | 예고편 fallback 검색 | API Key | `YOUTUBE_API_KEY` |
| GMS API (OpenAI) | AI 추천·픽키 챗봇 | Bearer Token | `GMS_API_KEY` |

### 9-2. 키 보안 관리

```python
# .env 파일 — GitLab 커밋 절대 금지 (.gitignore에 포함)
TMDB_ACCESS_TOKEN=eyJhbGciOiJIUzI1NiJ9...
GMS_API_KEY=sk-...
YOUTUBE_API_KEY=AIza...
SECRET_KEY=django-secret-...

# settings.py — python-dotenv로 환경변수 로드
from dotenv import load_dotenv
import os
load_dotenv()

TMDB_ACCESS_TOKEN = os.environ.get("TMDB_ACCESS_TOKEN", "")
GMS_API_KEY       = os.environ.get("GMS_API_KEY", "")
YOUTUBE_API_KEY   = os.environ.get("YOUTUBE_API_KEY", "")
```

---

## 10. 기술적 도전과 해결 요약

| 문제 | 원인 | 해결 |
|------|------|------|
| 감독·배우 검색 불가 | Movie 모델에 director·actors 필드 없음 | 필드 추가 + update_credits 커맨드 (2,604편 처리) |
| 예고편 연동 불가 | Movie 모델에 tmdb_id 없음 | 제목 → TMDB search → tmdb_id 동적 조회 |
| 달력 다기기 미동기화 | localStorage는 브라우저 로컬 저장 | MovieCalendarEntry DB 모델 신규 구현 |
| DB 없는 영화 검색 불가 | 1,400편 외 영화 검색 안 됨 | TMDB 실시간 검색 fallback 추가 |
| TMDB 예고편 한국어 없음 | 영화에 따라 한국어 예고편 미제공 | ko-KR → en-US → YouTube API 3단계 fallback |
| TMDB 호출 실패 시 빈 화면 | 네트워크 불안정 | try-except + DB 인기 영화로 fallback |
