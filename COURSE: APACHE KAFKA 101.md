### COURSE: APACHE KAFKA® 101

- [Reference](https://developer.confluent.io/learn-kafka/apache-kafka/events/?_ga=2.90235475.850282464.1681944803-1299099104.1681944803&_gac=1.123057273.1681993505.Cj0KCQjwxYOiBhC9ARIsANiEIfYpADoV3EZJ0jMrp9lVx7LgVqqQRoU-w1UiXdBeM0Neu_SLMC7Av4AaAgowEALw_wcB)

--- 

<details>
   <summary> 영상 내용 정리 </summary>
<br/>
   
   <details>
      <summary> Apache Kafka 란? </summary>

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

   <details>
      <summary> Kafka Topic 이란? </summary>

   - 이벤트는 쉽게 확산되기 때문에, 그걸 조직화 하는 시스템이 필요함
   - Topic
       - Kafka의 가장 기본적인 조직 단위로 RDB의 테이블과 비슷한 개념이다.
       - Kafka를 다룬다면 토픽은 추상화 할 때 가장 많이 생각해봐야 한다.
       - 서로 다른 종류의 이벤트를 저장하거나 동일한 종류의 이벤트를 필터링하고 변환한 버전을 저장하기 위해 다양한 토픽을 만든다.

       - 토픽은 이벤트의 log다.
           - log의 특징
               - append-only / 임의의 offset에서 검색한 다음 순차적인 log 항목을 스캔해 읽기 가능 / immutability
           - 위 특징을 바탕으로 카프카가 토픽에서 지속적으로 높은 처리량을 제공하는게 가능해지며 토픽의 복제에 대해 추론하기 쉬워진다.
           - 로그는 기본적으로 지속 가능해야 한다. 카프카의 토픽은 로그이기 때문에, 그 안의 데이터는 본질적으로 일시적인 것이 아니다. 모든 토픽은 데이터가 일정한 age에 도달한 후 만료되도록 설정할 수 있으며, 단 몇 초에서 몇 년에 이르기까지, 심지어 메시지를 무기한으로 보존할 수도 있다.
           - Kafka 토픽을 구성하는 로그는 디스크에 저장된 파일이다.
       - log의 단순함과 불변성은 카프카가 현대 데이터 인프라의 중요한 구성 요소로 성공한 핵심 역할을 함

   - 카프카를 사용하면, 분산 시스템의 일부로 작동하는 여러 노드에 토픽이 복제될 수 있고 이걸로 가용성과 내결함성이 향상된다. 즉, 어떤 노드가 실패하더라도 데이터 손실이나 처리 지연을 방지할 수 있다.
       - 또한, 카프카는 데이터를 소비하는 다양한 시스템 간에 이벤트를 전달할 수 있으므로, 각각의 시스템이 독립적으로 데이터를 처리할 수 있게 된다.
   - 실시간 처리를 위해 데이터를 빠르게 처리하고 변환할 수 있으며, 이를 통해 사용자와 시스템 간의 지연 시간이 줄어들게 된다.

   - 요약
       - Kafka의 topic은 log라는 간단한 데이터 구조를 기반으로 하며, 그 안의 데이터는 불변성(immutability)을 가짐.
       - 이 특성 덕분에 Kafka는 높은 처리량을 제공하며, 다양한 데이터를 안정적으로 전달하고 저장할 수 있다.
       - 또한 Kafka는 데이터를 실시간으로 처리하고, 복제를 통해 높은 가용성과 내결함성을 제공하며, 다양한 시스템 간에 데이터를 전달하는 데 중요한 역할을 한다.
       - 위와 같은 이유로 Kafka는 현대 데이터 인프라의 핵심 구성 요소로 간주되고 있다.

   </details>
   

   <details>
   <summary> Kafka Partitioning 이란? </summary>

   - 분산 시스템인 카프카는 많은 머신에서 많은 토픽을 관리할 수 있지만, 단일 토픽이 너무 커지거나 많은 읽기와 쓰기를 수용할 수는 없는 한계가 생긴다. 
   → 카프카는 이 문제를 해결하기 위해 토픽을 파티션으로 나누는 기능을 제공한다.

   - Partitioning
       - 단일 토픽 로그를 여러 개의 로그로 분할하며, 각각의 카프카를 클러스터 내의 다른 노드에서 실행할 수 있다. 이렇게 메시지를 저장하고 새로운 메시지를 작성하고 기존 메시지를 처리하는 작업을 클러스터 내의 여러 노드로 분산할 수 있다.

        - 토픽을 파티션으로 분할한 후, 어떤 메시지를 어떤 파티션에 작성할 지 결정하는 방법이 필요
            - 일반적으로 메시지에 키가 없는 경우 → 라운드 로빈 방식으로 분배
               - 모든 파티션은 데이터를 균등하게 공유하지만, 입력 메시지의 순서를 보존하지 않음
            - 메시지에 키가 있는 경우 → 키의 해시값을 계산해 목적지 파티션을 정함
               - 동일한 키를 가진 메시지가 항상 동일한 파티션에 위치하게 해 순서를 보장할 수 있음
               - 예를 들어, 동일한 고객과 관련된 모든 이벤트를 생성하는 경우, 고객 ID를 키로 사용하면 특정 고객의 모든 이벤트가 항상 순서대로 도착함을 보장할 수 있지만, 이 위험은 실제로는 작고 발생할 때 관리 가능하다.
   
      </details>

   <details>
   <summary> Brokers </summary>


- 브로커는 카프카 클러스터 내에서 메시지를 생선하거나 소비하는 시스템
- 브로커는 각각 Kafka 브로커 프로세스를 실행하는 독립적인 머신으로, 각각 일부 파티션을 호스팅하며 이런 파티션에 대한 새로운 읽기나 쓰기 요청을 처리한다. 또한 파티션 간의 복제도 처리한다.
   </details>
   
   <details>
   <summary>Replication </summary>

   - 브로커와 기본 storage는 장애에 취약하기 때문에 데이터를 안전하게 보관하기 위해 다른 여러 브로커에 복사해야 한다.

   - 이런 복사본을 follower replication, 메인 파티션을 leader replication이라고 한다. leader와 follower는 함께 작업해 새로운 쓰기를 follower에 복제한다.

   - 위 작업은 자동으로 이뤄지며, 프로듀서에서 일부 설정을 조정해 다양한 수준의 내구성을 보장할 수 있으나, 일반적으로 카프카에서 개발자가 고려해야 하는 프로세스는 아니다. 다만, 데이터가 안전하다는 것과 클러스터의 한 노드가 죽으면 다른 노드가 그 역할을 대신한다는 것을 알면 된다.

   </details>
   
   <details>
   <summary>Producers </summary>

   - 자바에서는 KafkaProducer라는 클래스를 사용해 클러스터에 연결함
       - 클러스터의 몇 개의 브로커 주소, 적절한 보안 구성 및 프로듀서의 네트워크 동작을 결정하는 config를 포함한 configuration mapt이 제공된다.
   - 클러스터로 전송할 key-value pair를 보유하기 위해 ProducerRecord라는 다른 클래스도 있다.
   - 라이브러리는 connection pooling, network buffering, 브로커가 메시지를 인식할 때까지 대기하며 필요한 경우 메시지를 재전송하고 다른 세부 정보를 관리한다.

   </details>
   
   <details>
   <summary>Consumers </summary>

   - 클러스터에 연결하기 위해 KafkaConsumer라는 클래스 사용
   - connection을 사용해 하나 이상의 토픽을 구독한다. 토픽에서 메시지를 사용할 수 있을 때, ConsumerRecords라는 컬렉션에서 메시지가 반환된다. 이 컬렉션에는 ConsumerRecord 객체 형태의 개별 메시지 인스턴스가 포함된다.
   - KafkaConsumer는 KafkaProducer와 마찬가지로 connection pooloing 및 네트워크 프로토콜을 관리하지만, 읽기 측면에서의 역할은 network plumbing 이상의 역할을 한다.
       - network plumbing : 네트워크 통신을 위한 구성요소 및 기술
   - 먼저 Kafka는 메시지를 읽는 것이 메시지를 파괴하지 않는 것이기 때문에 기존의 메시지 큐와 달리 이미 읽은 메시지를 다른 컨슈머가 읽을 수 있다.
   - 사실, 많은 컨슈머가 하나의 토픽에서 읽는 것이 Kafka에서는 정상적인데 이런 사실은 Kafka 주변에 나타나는 소프트웨어 아키텍처 종류에 긍정적인 영향을 미친다.
   - 또한, 컨슈머는 한 애플리케이션 인스턴스가 따라가기에는 메시지 소비 속도와 단일 메시지 처리의 계산 비용이 함께 너무 높은 시나리오를 처리할 수 있어야 한다. 즉, 컨슈머가 확장 가능해야 한다. Kafka에서 컨슈머 그룹을 auto-scaling이 가능함

   </details>
   
   <details>
   <summary>Kafka Ecosystem </summary>

   - 만약 브로커가 파티션화되고 복제된 토픽을 관리하며 점점 더 많은 프로듀서와 컨슈머가 이벤트를 작성하고 읽는 시스템만 있다면, 이미 매우 유용한 시스템이 된다. 하지만 경험에 따르면, 일부 패턴이 나타나면서 개발자들이 핵심 Kafka에 계속해서 동일한 기능을 개발하게 된다.
   - 특정하지 않은 일부 작업을 반복하는 공통된 기능 계층을 개발하게 되는데 이 코드는 중요한 작업을 수행하지만 실제 비즈니스와 직접적으로 연결되어 있지 않다. 이건 인프라에 해당하므로 인프라는 커뮤니티나 인프라 공급업체에서 제공해야 한다.
   - Kafka Connect, Confluent Schema Registry, Kafka Streams 및 ksqlDB는 이러한 종류의 인프라 코드 예다.

   </details>
   
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

   
5. pub test
   
   - 터미널 두 개 띄우고, 하나는 consume
   
      ```bash
      $ confluent kafka topic consume --from-beginning poem
      ```
   
   - 다른 하나는 produce
   
      ```bash
      $ confluent kafka topic produce poem --parse-key
      ```
      
      - 당연하지만 key-value 형태로 produce 해야 함
      - ex)
   
         ```bash
         8: "모든 노력을 집중시켜 끝이 보일 때까지 유지해야 한다. 행동하는 사람은 불안에 빠지지 않는다.  잘못된 신념만이 우리를불안으로 이끌 뿐이다."
         9: "아무리 노력해도 의도한 것과 그 결과가 너무나도 다를 때 우리는 무엇을  해야 하는지 어떻게 알 수 있을까? 거절해야 할 것과 받아들여야 할 것은 어떻게 알 수 있을까? 언제 자격을 갖추게 되는지, 언제 목표에 도달할 수  있는지, 언제 길에서 벗어났는지 우리는 어떻게 알 수 있을까? 명확히 알 수 없는 이 런 질문들 때문에 혼란에 휩싸이고 싶지 않다면 방향을 정확히 잡고 노력해야 한다."
         10: "우리는 언제나 제어할 수 있다고 생각하지만 정말 그럴까? 한 번 쾌락에 맛을 들이게 되면 쾌락 으로부터 기권할 수 있는 자유를 잃어버린다."
         11: " 이성적인 존재, 그것은 무엇을 말하는가?"
         12: "우리의 미래 계획은 여전히 과거로부터 물려받았다는 사실을 잊지 말자."
         13: "외적인 요소로 내적인 문제를 해결하지 못한다. 돈이나 물질로는 내면의 문제를 해결할 수 없다."
         14: "좋은 사람이라는 평판은 그가 한 말 때문이 아니라 그가 행한 바람직한 행동 때문에 만들어진다."
         ```

6. 파티션 실습
   - 파티션을 이용해 토픽을 chunk로 나눠 여러 노드에 걸쳐 저장할 수 있음
   - CLI를 통해 각각 다른 파티션 수를 갖는 토픽을 만드는 방법 & 토픽의 파티션 수 변화가 데이터 분포에 미치는 영향을 보자
   
   - 토픽 list 출력
   
      ```bash
      $ confluent kafka topic list
      ```
   
   - 자세한 정보를 확인하기 위해선 describe를 쓰면 된다.
   
      ```bash
      $ confluent kafka topic describe ${topic_name}
      ```
   
      - 앞서 생성한 poem의 경우 num.partitions 값이 6임
   
   - 각각 1개, 4개의 파티션을 가진 2개의 토픽을 생성해보자
   
      ```bash
      $ confluent kafka topic create --partitions 1 poem_1
      $ confluent kafka topic create --partitions 4 poem_4
      ```
   
   - --parse-key 와 함께 produce 명령을 사용해 토픽에 데이터 생성
   
      - poem_1, poem_4 모두 진행
   
      ```bash
         $ confluent kafka topic produce poem_1 --parse-key
      ```
   
      ```bash
      1:”All that is gold does not glitter”
      2:"Not all who wander are lost"
      3:"The old that is strong does not wither"
      4:"Deep roots are not harmed by the frost"
      5:"From the ashes a fire shall awaken"
      6:"A light from the shadows shall spring"
      7:"Renewed shall be blad that was broken"
      8:"The crownless again shall be king"
      ```
   - Confluent Clould 콘솔에 가서 생성된 두 토픽을 확인하자
      - poem_1 토픽은 1개의 파티션에서 전체 8개의 메시지를 포함하고 있음을 알 수 있다.
      - poem_4 토픽의 경우 4개의 파티션으로 데이터가 균등하게 분산되어 있음을 알 수 있다.
   
   
</details>

