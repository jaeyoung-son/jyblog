---
title: 'HTTP 응답 코드와 메소드'
date: 2021-02-20 15:21:13
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

