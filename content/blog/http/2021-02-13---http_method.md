---
title: 'HTTP 응답 코드와 메소드'
date: 2021-02-13 15:21:13
category: 'HTTP method'
draft: false
---

## HTTP

HTTP 란 Hyper Text Transper Protocol 입니다.
하이퍼텍스트 문서를 교환하기 위해 만들어진 통신 규약입니다.  
웹상에서 네트워크로 서버끼리 통신을 할때 어떤 형식으로 통신을 할지 규정해놓은 통신 형식, 통신 구조 입니다.

### HTTP 응답코드 종류

|       응답대역       | 응답코드 | 설명                                                                                |
| :------------------: | :------: | :---------------------------------------------------------------------------------- |
|       정보전송       |   100    | Continue 클라이언트로 부터 일부 요청을 받았으며 나머지 정보를 계속 요청             |
|       임시응답       |   101    | Switching protocols                                                                 |
|         성공         |   200    | OK 요청이 성공적으로 수행 되었음                                                    |
|                      |   201    | CreatedPUT 메소드에 의해 원격지 서버에 파일 생성됨                                  |
|                      |   202    | Accepted웹 서버가 명령 수신함                                                       |
|                      |   203    | Non-authoritative information서버가 클라이언트 요구 중 일부만 전송                  |
|                      |   204    | No content PUT, POST, DELETE 요청의 경우 성공은 했지만 전송할 데이터가 없는 경우    |
|      리다이렉션      |   301    | Moved permanently 요구한 데이터를 변경된 타 URL에 요청함 Redirect된 경우            |
|                      |   302    | Not temporarily                                                                     |
|                      |   304    | Not modified 컴퓨터 로컬의 캐시 정보를 이용함, 대개 gif등은 웹 서버에 요청하지 않음 |
| 클라이언트 요청 에러 |   400    | Bad Request 사용자의 잘못된 요청을 처리할 수 없음                                   |
|                      |   401    | unauthorized 인증이 필요한 페이지를 요청한 경우                                     |
|                      |   402    | Payment required 예약됨                                                             |
|                      |   403    | Forbidden 접근 금지, 디렉터리 리스팅 요청 및 관리자 페이지 접근 등을 차단           |
|                      |   404    | Not found 요청한 페이지 없음                                                        |
|                      |   405    | Method not allowed 허용되지 않는 http 메소드를 사용함                               |
|                      |   407    | Proxy authentication required 프록시 인증이 요구됨                                  |
|                      |   408    | Request timeout 요청 시간 초과                                                      |
|                      |   410    | Gone 영구적으로 사용금지                                                            |
|                      |   412    | Precondition failed 전체 조건 실패                                                  |
|                      |   414    | Request-URI too long 요청 URI 길이가 긴 경우                                        |
|       서버에러       |   500    | Internal server error 내부 서버 오류                                                |
|                      |   501    | Not implemented 웹 서버가 처리할 수 없음                                            |
|                      |   503    | Service unnailable 서비스 제공 불가                                                 |
|                      |   504    | Gateway timeout 게이트웨이 시간 초과                                                |
|                      |   505    | HTTP version not supported 해당 http 버전이 지원되지 않음                           |

### HTTP 메소드들

- GET
  요청받은 URI의 정보를 검색하여 응답합니다.

- HEAD
  GET방식과 동일하지만, 응답에 BODY가 없고 응답 코드와 HEAD만 응답합니다.  
  주로 웹서버 정보확인, 헬스체크, 버전확인, 최종 수정일자 확인등의 용도로 사용됩니다.

- POST
  요청된 자원을 생성합니다.(create) 새로 작성된 리소스인 경우 HTTP헤더 항목 Location : URI주소를 포함하여 응답합니다.

- PUT
  요청된 자원을 수정합니다.(update) 내용 갱신을 위해 주로 사용되며, Location: URI를 보내지 않아도 됩니다.
  클라이언트 측은 요청된 URI를 그대로 사용하는 것으로 간주합니다.

- PATCH
  PUT과 유사하게 요청된 자원을 수정(update)할때 사용합니다. PUT의 경우 자원 전체를 갱신하는 의미지만,
  PATCH는 해당 자원의 일부를 교체하는 의미로 사용합니다.

- DELETE
  요청된 자원을 삭제할 것을 요청합니다.

- CONNECT
  동적으로 터널 모드를 교환합니다. 프록시 기능을 요청시 사용합니다.

- TRACE
  원격지 서버에 루프백 메시지를 호출하기 위해 테스트용으로 사용합니다.

- OPTIONS
  웹 서버에서 지원되는 메소드의 종류를 확인할 경우 사용합니다.

### 헷갈리기 쉬운 메소드들 간의 차이점

#### POST와 PUT

POST는 보통 INSERT의 개념으로 사용되고, PUT은 UPDATE의 개념으로 생각하면 됩니다.  
또한 동일한 자원을 여러번 POST할 경우에는 서버자원에는 변화가 생기지만, 여러번 PUT하는 경우에는 변화가 생기지 않습니다.  
예를들면 POST의 경우 클라이언트가 리소스의 위치를 지정하지 않는 경우 사용됩니다.

```
POST /people HTTP/1.1
{"name" : "재영", "age": 27}
```

위의 요청을 여러번 수행하게 되면, people/1, people/2 매번 새로운 자원이 생성됩니다.  
반면 PUT의 경우는 클라이언트가 명확하게 리소스의 위치를 지정합니다 people/1  
그렇기에 많이 수행되더라도 리소스의 위치가 지정되어 있기 때문에 새로운 자원이 생성되지 않으며,
동일한 리소스를 수정하기 때문에 여러번 요청하더라도 새로운 자원이 생기지 않습니다.

```
PUT /people/1 HTTP/1.1
{"name" : "재영", "age": 27}
```

#### PUT과 PATCH

의미를 보면 PUT은 해당 자원의 전체를 교체하는 의미를 지니고, PATCH는 일부를 변경한다는 의미를 지닙니다.  
PUT의 경우 전체가 아닌 일부 필드만 전달할 경우 null이나 초기값으로 처리가 되니 주의해야 합니다.  
또한 동일한 자원에 대해 반복적인 요청 시, PUT의 경우에는 같은 결과값을 만들어 주지만, PATCH의 경우에는 같은 결과를 보장할 수 없습니다(자원의 일부가 변경되기 때문에).
조금 더 어려운 말로 표현한다면 PATCH의 경우 멱등성을 보장할 수 없다고 합니다.
