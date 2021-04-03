# 2장 데이터 모델과 쿼리

## Relational Database 의 특징
The roots of relational database lie in business data processing,
typically transaction processing(entering sales or banking transactions, airline reservations,
stock-keeping in warehouses)
and batch processing(customer invoicing, payroll, reporting)

해석:
전통적으로 rds는 다음 곳들에서 쓰여졌다.
- 비즈니스 데이터 처리(특히 트랜잭션 처리)
    - 입출금 트랜잭션션, 항공권 예약
    - stock-keeping in warehouses(잘 모르겠음)
- batch 처리
    - customer invoicing(고객 송장?)
    - payroll
    - reporting


-> 전통적인 rds의 역할을 설명하고 있어 좋았다.


## SQL(RDS) vs NoSQL
책 내용뿐 아니라, 개인의 생각도 들어가 있어요.
### NoSQL 입소문 타게 된 배경
- 대규모 데이터셋이나 매우 높은 쓰기 처리를 관계형 데이터베이스보다 쉽게 하 수 있는 구조 필요
- 상용 데이터베이스 제품보다 무료 오픈소스 소프트웨어에 대한 선호도 확산
- 관계형 모델에서 지원하지 않는 특수 질의 동작 필요
- 관계형 스키마의 제한에 대한 불만과 더욱 동적이고 표현력이 풍부한 데이터 모델에 대한 기대

### SQL과 NoSQL의 비교
- NoSQL 장점: 데이터 모델이 유연하고, 기존에 나뉘어졌던 rds 테이블들이 합쳐진 구조이기에
지역성이 좋고(join할 필요가 없음), 어플리케이션 데이터 구조와 비슷해서 개발하기 편하다.
- 단 relational database가 필요할 때가 있다. relational database에서는 다대일 및 다대다 관계의 데이터들을 처리하기 쉽다.

## analytics application and nosql
For example, many-to-many relationships may never be needed ini an analytics application that uses
a document database to record which events occurred at which time.  
-> 즉 이벤트 타임라인 데이터를 저장할 때는 relational database보다는 nosql을 쓰는게 낫다?

## nosql에서는 부분적 업데이트가 불가하다
On updates to a document, the entire document usually needs to be rewritten.
For these reasons, it is generally recommended that you keep documents fairly small and avoid writes that
increase the size of a document.  
-> 듣고 보니 elasticsearch가 생각남.  
-> elasticsearch에서도 부분적 업데이트가 불가능함.  
-> 물론 사용자단에서는 부분적 업데이트가 되는 것처럼 보이지만, 실제로는 삭제 후 문서 재생성함.  

## Imperative code vs Declarative code
Imperative code is very hard to parallelize across multiple cores and multiple machines,
because it specifies instructions that must be performed in a particular order.
Declarative languages have a better chance of getting faster in parallel execution because
they specify only the pattern of the results, not the algorithm that is used to determine the results.  
-> 결국 Imperative code와 Declarative code의 핵심 차이는 순서가 있는지 여부다  
-> Imperative code: 순서가 있다.  
-> Declarative code: 순서가 없다.  
-> css도 declarative code다  
