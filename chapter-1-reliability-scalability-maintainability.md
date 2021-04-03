# 1장 소프트웨어의 신뢰성, 확장성, 유지보수성에 대하여

## 계산 중심 데이터
Many applications today are *data-intensive*, as opposed to *compute-intensive*.
Raw CPU power is rarely a limiting factor for these applications-bigger problems are usually
the amount of data, the complexity of data, and the speed at which it is changing.

해석:  
어플리케이션은 대부분 데이터 중심적이다. 컴퓨팅 중심적이지 않다.
오늘날 대부분 문제점들은 cpu 성능이 아닌, 데이터의 양, 데이터 복잡성 및 데이터의 변화 속도에 의해서 생긴다.

-> 공감한다.

## 데이터 중심 어플리케이션이 하는 일
For example, many application need to:
- Store data so that they, or another application, can find it again later (database)
- Remember the result of an expensive operation, to speed up reads (caches)
- Allow users to search data by keyword or filter it in various ways (search indexes)
- Send a message to another process, to be handled asynchronously (stream processing)
If that sounds painfully obvious, that's just because these data systems are such a successful abstraction.

해석:  
데이터 어플리케이션이 하는 일
- 나중에 찾을 수 있게 데이터를 *저장한다*. (데이터베이스의 역할)
- 값비싼 계산의 결과들을 *기록해둔다*. 읽기 성능을 올리는게 목적이다. (캐시의 역할)
- 유저들이 데이터를 검색할 수 있게 한다. (검색 인덱스의 역할)
- 메시지를 다른 곳에 보낸다. 비동기적으로 작동한다. (stream processing)  
이 모든 것들이 당연하게 들린다면, 그건 당연하기 때문이 아니라 데이터 시스템들이 성공적으로 추상화가 됐기 대문이다.

-> 공감한다. 백엔드 개발자들이 하는 일을 잘 설명한 것 같다!

## fault와 failure의 차이
Note that a fault is not the same as a failure. A fault is usually defined as one component of the system deviating
from its spec, whereas a failure is when the system as a whole stops providing the required service to the user.

-> failure: 시스템 실패.
fault: 시스템의 부분적 실패?
failure는 0%로 줄이는 게 가능하지만, fault는 0%로 줄이는 게 불가능하다?
시스템이 망가지는 걸 복구할 수 있으면 fault다.

## 트위터 예제
12p
- 트위터 같은 경우 사용자가 워낙 많다보니, 빠른 처리를 위해 캐시를 사용한다.
- 사용자마다 각각 캐시를 두어, 중앙 디비에 부하가 적게 가도록 한다.
- 그런데 이렇게 사용자마다 캐시를 두면, 몇십만명의 팔로워가 있는 유명인사들이 글을 남기면, 수십만개의 write가 발생할 것이다.
- write 부하가 클 수 있다.
- 팔로워가 많은 유명인사들에 대해서는 다른 데이터 처리 방식을 사용한다.


## p95, p99, p999 용어 설명
p95 응답 시간이 3분이다.
-> 95%의 사용자는 3분 이하의 응답 시간을 경험했다.


## SQL의 추상화
SQL is an abstraction that hides complex on-disk and in-memory data structures,  
concurrent requests from other clients, and inconsistencies after crashes.  
-> 우리가 SQL을 사용할 때 어떤 부분을 몰라도 되는지 설명하고 있어서 좋았따.
-> concurrent request 같은 경우, sql을 작성할 때 고려를 해야 하지 않을까?(ex: transaction lock)



