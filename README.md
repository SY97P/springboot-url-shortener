# springboot-urlMapping-shortener

SprintBoot URL Shortener 구현 미션 Repository 입니다.

# 요구사항

- [ ]  URL 입력폼 제공 및 결과 출력
- [ ]  URL Shortening Key는 8 Character 이내로 생성
- [ ]  단축된 URL 요청 시 원래 URL로 리다이렉트
- [ ]  단축된 URL에 대한 요청 수 정보 저장 `optional`
- [ ]  Shortening Key를 생성하는 알고리즘 2개 이상 제공하며 애플리케이션 실행중 동적으로 변경 가능 `optional`

# API 명세

### URL 입력 폼

| Method | URI |
| --- | --- |
| GET | / |

### 원본 URL을 단축 URL로 매핑

| Method | URI |
| --- | --- |
| POST | /shortener |

| Request Parameter | Type | Description |
| --- | --- | --- |
| originUrl | String | 원본 URL |

| Response Parameter | Type | Description |
| --- | --- | --- |
| originUrl | String | 원본 URL |
| shortUrl | String | 원본 URL에 대한 단축 URL |
| requestCount | long | 단축 URL 호출횟수 |

### 단축 URL에 대한 원본 URL로 리다이렉션

| Method | URI |
| --- | --- |
| GET | /{shortUrl} |

| Path Variable | Type | Description |
| --- | --- | --- |
| shortUrl | String | 단축 URL |

### 원본 URL로 URL 매핑 정보 조회

| Method | URI |
| --- | --- |
| GET | /shortener |

| Request Parameter | Type | Description |
| --- | --- | --- |
| originUrl | String | 원본 URL |

| Response Parameter | Type | Description |
| --- | --- | --- |
| originUrl | String | 원본 URL |
| shortUrl | String | 원본 URL에 대한 단축 URL |
| requestCount | long | 단축 URL 호출횟수 |

# DataBase 테이블

# 동작화면

## Short URL Service

### 읽으면 좋은 레퍼런스

- [Naver 단축 URL API](https://developers.naver.com/docs/utils/shortenurl/)
- [짧게 줄인 URL의 실제 URL 확인 원리 및 방법](https://metalkin.tistory.com/50)
- [짧게 줄인 URL 알고리즘 고찰](https://metalkin.tistory.com/53)
- [단축 URL 원리 및 개발](https://blog.siyeol.com/26)

### Short URL의 동작 과정

1. 원본 URL을 입력하고 Shorten 버튼을 클릭합니다.
2. Unique Key를 7문자 생성합니다.
3. Unique Key와 원본 URL을 DB에 저장합니다.
4. bitly.com/{Unique Key} 로 접근하면, DB를 조회하여 원본 URL로 redirect합니다.

### Short URL의 특징

단축 URL서비스는 간편하지만, 단점(위험성)이 있습니다.
링크를 클릭하는 사용자는 단축된 URL만 보고 클릭하기 때문에 어떤 곳으로 이동할지 알 수 없습니다.

- Short URL 서비스는 주로 요청을 Redirect 시킵니다. (Redirect와 Forward의 차이점에 대해 검색해보세요.)
- 긴 URL을 짧은 URL로 압축할 수 있다.
- short url만으로는 어디에 연결되어있는 지 알 수 없다. 때문에 피싱 사이트 등의 보안에 취약하다.
- 광고를 본 뒤에 원본url로 넘겨주기도 한다. 이 과정에서 악성 광고가 나올 수 있다.
- 당연하지만 이미 존재하는 키를 입력하여 들어오는 사람이 존재할 수 있다.
- 기존의 원본 URL 변경되었더라도 단축 URL을 유지하여, 혼란을 방지할 수 있다.

### 예시 사이트

[https://urlMapping.kr/](https://urlMapping.kr/)
