### COURSE: APACHE KAFKA® 101

- [Reference](https://developer.confluent.io/learn-kafka/apache-kafka/events/?_ga=2.90235475.850282464.1681944803-1299099104.1681944803&_gac=1.123057273.1681993505.Cj0KCQjwxYOiBhC9ARIsANiEIfYpADoV3EZJ0jMrp9lVx7LgVqqQRoU-w1UiXdBeM0Neu_SLMC7Av4AaAgowEALw_wcB)

--- 

<details>
   <summary> 영상 내용 정리 </summary>

### Apache Kafka 란?

- 대규모로 데이터를 수집, 처리, 저장 및 통합하는 이벤트 스트리밍 플랫폼
- distributed logging, stream processing, data integration, pub/sub messaging 등 다양하게 사용

- **이벤트 스트리밍 플랫폼**이란?
    - Event란?
        - 소프트웨어나 애플리케이션에서 확인되거나 기록된 모든 유형의 행동, 사건 또는 변경 사항
        - 예를 들어, 결제, 웹사이트 클릭 또는 온도 측정 같은 일이 발생한 것
        - 다른 활동을 트리거하는 데 사용될 수 있는 알림 요소와 상태의 조합
        - 상태는 일반적으로 상당히 작으며, 메가바이트 미만이고, JSON이나 Apache Avro 또는 프로토콜 버퍼로 직렬화된 객체와 같이 구조화된 형식으로 표시됌
    - Kafka와 Event - key/value pair
        - 카프카는 분산 커밋 로그의 추상화에 기반한다. 로그를 파티션으로 나눔으로써 카프카는 시스템을 확장할 수 있다. ← 따라서 카프카는 이벤트를 key/value 쌍으로 모델링한다.
        - 내부적으로 key와 value는 바이트의 연속이지만 외부적으로는 선택한 프로그래밍 언어에서 구조화된 객체로 표현된다.
        - 카프카에서 language types과 내부 바이트 간의 변환을 직렬화, 역직렬화라고 부름
            - 형식은 보통 JSON, JSON schema, Avro, Protobuf가 있다.
        - value는 일반적으로 애플리케이션 도메인 객체의 직렬화된 표현이거나 센서 출력 같이 raw message input의 형식이다.
        
        - key 역시 복잡한 도메인 객체일 수 있지만, 대게 문자열이나 정수와 같이 primitive type이다. 카프카 이벤트의 키 부분은 RDB의 행의 기본키처럼 이벤트의 고유 식별자일 필요는 없다. 대신 시스템 내의 어떤 엔티티, 사용자, 주문 또는 특정 연결된 장치와 같은 식별자일 가능성이 높다.
        - 나중에 카프카가 parallelization and data locality를 다룰 때 key가 중요한 역할을 하는 걸 확인할 수 있을 것!

</details>

--- 

<details>
   <summary>실습하며 생긴 일</summary>

1. 컨플루언트 Kafka 가입 -> promo로 KAFKA101 하니까 크레딧 더 줌
2. topic 생성하고 pub
   
      <img width="1523" alt="image" src="https://user-images.githubusercontent.com/84627144/233657122-aa81b497-8d03-4f00-9bba-9d7a1db7bc56.png">

3. Confluent CLI 설치
  
    ```bash
    $ curl -sL --http1.1 https://cnfl.io/cli | sh -s -- latest
    ```
    
4. CLI 접속해 로그인 
  
  - CLI guide에선 `confluent login --save` 하면 바로 실행되던데 제대로 안 됌
  - 일단 아래처럼 환경 변수 설정함
     ```bash
     $ export PATH=$PATH:/Users/seo/bin
     ```
  -  그리고 `confluent login --save` 다시 했는데, 아래 에러 발생
     ```bash
     Error: unable to open web browser for authorization: exec: "open": executable file not found in $PATH
     ```
    
  - confluent CLI가 설치된 거 찾아서 클릭했더니 뭔가 혼자 돌아감
     
      <img width="627" alt="image" src="https://user-images.githubusercontent.com/84627144/233817913-3cceaf76-6979-4398-9b87-987e4e439412.png">

  - 그리고 난 뒤에 `confluent login --save` 하니까 정상 로그인 화면 나옴
      
      <img width="794" alt="image" src="https://user-images.githubusercontent.com/84627144/233817923-3925938b-5882-41f7-b376-fa3523b36627.png">
   
  - 환경 설정
    - User의 Confluent 환경 확인
    - 카프카의 클러스터, 스키마 레지스트리, 커넥터와 같은 구성 요소가 포함

      ```bash
      $ confluent environment list
      ```
   
  - 환경 설정을 갖다 쓰자
    - 위에 나온 환경 리스트 중 사용할 환경 ID 선택해 지정
   
       ```bash
       $ confluent environment use {ID}
       ```
   
   - 카프카 클러스터 목록 확인
   
      ```bash
      $ confluent kafka cluster list
      ```

   - 카프카 클러스터 사용 설정

       ```bash   
       $ confluent kafka cluster use {ID}
       ```
   
   - API 키 생성

       ```bash   
       $ confluent api-key create --resource {ID}
       ```
   
   - API 키 사용 설정
   
       ```bash   
       $ confluent api-key use {API Key} --resource {ID}
       ```

</details>

