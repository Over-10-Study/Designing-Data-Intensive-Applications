# Stream Processing
## 요약
11장은 stream processing에 대한 내용이다.
stream의 주요 특징은 unbounded(종료점이 없다)는 것이다.
stream의 unbounded한 특성은 bounded한 배치와 분명한 차이가 있다.

배치는 시작지점과 종료지점이 있기에, 24시간 지속적으로 처리되어야 하는 일에는 적합하지 않다.
stream을 사용하면, 준실시간으로 24시간 이벤트를 처리할 수 있다.

### stream processing 주요 개념
event: 특정 시간에 생긴 사건 정보를 담은 immutable한 객체
producer: event 생성자  
consumer: event 처리자  
topic(stream): event의 그룹  
message broker: 메시지 스트림을 처리하는 데이터베이스  

### stream processing에서 고려해야할 문제
stream processing 구조에서 고려해야할 상황들이 있다.
1. consumer가 처리할 수 있는 속도 이상으로 producer가 이벤트를 전송하는 경우:  
backpressure가 필요하다. backpressure는 producer가 이벤트를 일정 속도 이상으로 보내는 것을 막는 제어기를 의미한다.
2. 이벤트가 유실될 경우:  
네트워크 유실, consumer 노드의 다운 등의 상황에서 이벤트가 유실될 수 있다.
message broker에서 이벤트들을 로그 형태로 쌓는다면, consumer에서 특정 위치의 이벤트 로그부터 수신받는 형식으로
이벤트 유실 상황에 대비할 수 있다.(consumer가 유실된 이벤트 로그 offset부터 이벤트를 발송하도록 message broker에게 요청할 수 있다) 


## 느낀점
회사에서 이벤트 시스템(stream processing)을 적극적으로 사용한다.(책에서 말하는 stream processing과, 실무에서 쓰이는 용어 '이벤트 시스템'은 같은 의미라고 생각한다.)
이벤트 시스템은 팀별로 독립적인 시스템을 구축하는 데 큰 도움이 됐다.
회원 팀과 예약 팀이 이전에는 하나의 디비로 묶여있는 구조였다면,
회원 팀과 예약 팀이 서로 다른 디비를 사용하고 API/이벤트로 서로 시스템이 소통하도록 바뀌었다.

만약 이벤트가 없었다면, API로 일정시간마다 polling해서 다른 팀의 정보를 받아와서 적용해야했을 것이다.
(ex 예약시스템이 일정시간마다 회원시스템을 polling하여 삭제된 회원 반영)
아니면 회원시스템에서 회원이 삭제될 때 직접 예약시스템 update api를 호출하는 식으로 예약을 삭제하는 방식도 가능할 것이다.

이벤트 시스템을 적용함으로써, 회원시스템/예약시스템이 서로 직접적으로 API를 호출해야 할 상황이 적어졌다.
각 시스템에 장애가 생겼을 때 장애가 전파되는 상황도 줄어들었다고 볼 수 있다.
시스템 간의 격리 수준을 높였을 때의 장점이다.

회사에서는 AWS SQS/SNS로 이벤트 시스템을 구현하고 있다.
SNS/SQS 모두 message broker라고 볼 수 있다.
각 서비스 모두 이벤트를 수신/발신하지만 주요 차이점이 있다.
차이점은 다음과 같다.
### SNS의 특징
- SNS는 자신을 구독하는 모든 consumer에게 같은 이벤트를 보낸다.
- SNS에서 acknowledgement같은 개념은 없다. 구독자의 수신 여부와 상관없이 그 이벤트는 폐기된다.
- SNS는 메시지를 저장하고 있지 않다.

### SQS의 특징
- SQS는 자신을 구독하는 모든 consumer에게 load balancing형태로 이벤트를 전달한다.
    - consumer들이 각각 같은 이벤트를 받을 일은 없다.
- SQS는 acknowledgement와 같은 개념이 있다.
    - acknowledgement를 수신하지 못한 이벤트는 다른 consumer에서 처리되거나, DLQ에 쌓인다.
- SQS에서 처리되지 못한 이벤트는 저장된다.
- SQS에서 처리된 이벤트는 삭제된다.

SQS/SNS를 사용함으로써, 각 시스템은 update api를 노출시킬 필요 없이 독립적으로 시스템을 유지할 수 있다.
가끔 SNS/SQS에서 장애 상황이 발생할 때가 있다. 이 경우 AWS에서 처리한다.