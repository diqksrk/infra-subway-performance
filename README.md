## 안정적인 서비스 만들기
### 1단계 - 화면 응답 개선하기
#### 요구사항
```
[X] Reverse Proxy 개선하기
    [X] gzip 압축
    [X] cache
    [X] TLS, HTTP/2 설정
[X] WAS 성능 개선하기
    [X] Spring Data Cache
[X] 부하 테스트(smoke, load, stress)
```

<br />

#### 캐시 적용 기준
* 사용자가 가장 많이 요청할 것으로 추측되는 요청들은 `메인 페이지 - 경로 검색 페이지 - 경로 검색 요청`로 이어지는 흐름이라고 판단했습니다.
  * 위의 판단을 기준으로 해당 흐름과 관련있는 처리 메서드들에 대해 집중적으로 `cache`를 적용하였습니다.
  * 지하철 호선 목록
  * 다익스트라 알고리즘을 이용해 최단거리 탐색
  * 지하철 역 조회

<br />

#### pageSpeed 개선 결과
| desktop | FCP  | SI   | LCP  | TTI  |
|--------|------|------|------|------|
| 기존     | 2.7s | 2.7s | 2.8s | 2.8s |
| 개선     | 1.2s | 1.6s | 1.3s | 1.3s |

<br />

<b> https://jisu1211.kro.kr에서 수행된 결과입니다. </b>

1. 성능 개선 결과를 공유해주세요 (Smoke, Load, Stress 테스트 결과)
* `src/main/resources/static/loadtest`에 포함해두었습니다.
  * before: 개선 전의 부하 테스트 결과를 포함하였습니다.
  * after: 개선 후의 부하 테스트 결과를 포함하였습니다.
    * nginx/gzip: nginx gzip 압축만 개선했을 때의 부하 테스트 결과입니다.
    * nginx/cache: nginx gzip 압축, cache를 적용했을 때의 부하 테스트 결과입니다.
    * nginx/http2: nginx gzip 압축, cache 적용, http2를 적용했을 때의 부하 테스트 결과입니다.
    * all: nginx 개선과 was 개선을 모두 적용했을 때의 부하 테스트 결과입니다.
  * smoke 테스트는 1분, load 테스트는 30분, stress 테스트는 1시간 10분 동안 수행하였습니다.

<br />

2. 어떤 부분을 개선해보셨나요? 과정을 설명해주세요
* Reverse Proxy 개선
  * gzip 압축
  * cache 적용
  * TLS, HTTP/2 설정
* WAS 성능 개선
  * Spring Data Cache
---

### 2단계 - 스케일 아웃

1. Launch Template 링크를 공유해주세요.

2. cpu 부하 실행 후 EC2 추가생성 결과를 공유해주세요. (Cloudwatch 캡쳐)

```sh
$ stress -c 2
```

3. 성능 개선 결과를 공유해주세요 (Smoke, Load, Stress 테스트 결과)

---

### 3단계 - 쿼리 최적화

1. 인덱스 설정을 추가하지 않고 아래 요구사항에 대해 1s 이하(M1의 경우 2s)로 반환하도록 쿼리를 작성하세요.

- 활동중인(Active) 부서의 현재 부서관리자 중 연봉 상위 5위안에 드는 사람들이 최근에 각 지역별로 언제 퇴실했는지 조회해보세요. (사원번호, 이름, 연봉, 직급명, 지역, 입출입구분, 입출입시간)

---

### 4단계 - 인덱스 설계

1. 인덱스 적용해보기 실습을 진행해본 과정을 공유해주세요

---

### 추가 미션

1. 페이징 쿼리를 적용한 API endpoint를 알려주세요
