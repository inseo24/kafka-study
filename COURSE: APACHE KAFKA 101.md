### COURSE: APACHE KAFKA® 101

- [Reference](https://developer.confluent.io/learn-kafka/apache-kafka/events/?_ga=2.90235475.850282464.1681944803-1299099104.1681944803&_gac=1.123057273.1681993505.Cj0KCQjwxYOiBhC9ARIsANiEIfYpADoV3EZJ0jMrp9lVx7LgVqqQRoU-w1UiXdBeM0Neu_SLMC7Av4AaAgowEALw_wcB)


1. 컨플루언트 Kafka 가입 -> promo로 KAFKA101 하니까 크레딧 더 줌
2. topic 생성하고 pub
   - <img width="1523" alt="image" src="https://user-images.githubusercontent.com/84627144/233657122-aa81b497-8d03-4f00-9bba-9d7a1db7bc56.png">

3. Confluent CLI 설치
  
    ```bash
    $ curl -sL --http1.1 https://cnfl.io/cli | sh -s -- latest
    ```
    
4. CLI 접속해 로그인 
  
  - CLI guide에선 `confluent login --save` 하면 바로 실행되던데 제대로 안 됌
  - 일단 아래처럼 환경 변수 설정함
     ```bash
     $ export PATH="$PATH:/Users/seoin/bin"
     ```
  -  그리고 `confluent login --save` 다시 했는데, 아래 에러 발생
     ```bash
     Error: unable to open web browser for authorization: exec: "open": executable file not found in $PATH
     ```
  - 그래서 그냥 key 생성해서 함 근데도 안 됌 망할 내일 한다
    ```bash
    $ confluent login --token <API_KEY>:<API_SECRET>
    ```
   
