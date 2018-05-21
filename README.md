# iOS 4조 간이스터디 (라고 쓰고 지식뜯김이라 읽는다)

<br>

### 통신에 대한 이해

> Network 프로세스는 택배 회사, 또는 인터넷 쇼핑의 프로세스와 같다. 

#### GET 의 경우 (택배를 받는 경우)

> 1,5,6,7은 클라이언트(iOS)에서 진행되고, 2,3,4는 서버사이드에서 진행된다.

1. 주문을 한다. (Alamofire로 GET 매소드 request 요청을 한다)
2. 창고에서 물건을 가져온다 (서버 측에서 데이터 베이스에 접근)
3. 준비한 물건을 택배 규격에 맞게 넣는다. (택배회사는 택배 규격에 맡는 물건만 가져간다.) (JSON 또는 XML로 직렬화(Serialize) 한다)
4. 물건을 발송한다 (HTTP 또는 HTTPS로 Response 응답)
5. 물건을 받는다 (Alamofire의 클로저 인수로 Response가 넘어온다. 이중 JSON은 Response.result.value에 들어있다.)
6. 물건을 뜯어 내가 사용할 수 있는 상태로 만든다 (Response를 JSONdecoder에 넣어 객체로 변환)
7. 물건을 쓴다(변환된 객체를 사용함)


#### POST, PUT , DELETE 의 경우 (택배를 발송하는 경우)

> 1,2,3,8 은 클라이언트, 4,5,6,7은 서버사이드에서 진행된다.

1. 물건을 택배 규격에 맞게 상자에 넣는다 (JSONencoder로 json으로 변환한다)
2. 택배 송장을 만든다 (Alamofire를 이용하며, request body에 json을 넣는다)
3. 택배를 발송한다 (Alamofire를 이용하여 요청한다)
4. 업체에서 받는다 ( Response의 Body를 파싱한다) 
5. 하자가 있는 지 검사한다 ( * 이 과정을 Validation 이라고 한다 *)
6. 하자가 없다면 창고에 넣는다. ( Body를 데이터베이스에 저장한다 )
7. 발송자에게 완료 문자 또는 카톡을 보낸다 (Response)
8. 발송자는 Response 의 상태(200은 성공, 503은 서버에러 등등)를 받는다 (Alamofire의 res로 넘어온다)
 


### 용어

- 직렬화 (Serialize)
- 역직렬화 (Deserialize)
- JSON (Javscript Object Notation)
- HTTP method : HTTP 통신에서 사용되는 도구이며, 10가지 이상의 메소드가 있지만 가장 많이 쓰이는 것은 아래 네개, 각각 DB와 관련
    - GET   : 데이터 죄회를 요청 / Read 
    - POST  : 데이터 생성을 요청 / Create
    - PUT   : 데이터 갱신을 요청 / Update
    - DELETE: 데이터 삭제를 요청 / Delete
- *추가* Object Mapper : Object Mapper는 json과 같은 문자열에서 사용가능한 객체로 바꿔주는 객체를 말함(iOS : JSONdecoder, Android : Gson)
