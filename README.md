# class101-clone-demo
 class101-clone-demo
 
### 프로젝트 환경
- JDK 1.8
- Spring Boot 2.4.0.RELEASE
- JPA2
- Gradle 4.8+
- DB : h2

### 핵심 아이디어
- ApplicationRunner 를 이용해 CommandLine 형태의 프로그램 작성
- 상품 데이터 불러오기는 프로그램 구동 시 초기화
- 프로덕트 클래스 정보가 변경되어 장바구니에 담은 정보와 다른 경우 예외처리
- Kit 상품 구매 시 수량 update 처리는 Isolation.SERIALIZABLE 처리
- 물건 주문 로직 비동기 처리 (@Async)
- SoldOutException 테스트 구현 // exception.expect(SoldOutException.class);


### 기능 구현 내용
- 한 번에 여러개의 상품을 같이 주문할 수 있다.
- 상품번호, 수량은 반복적으로 입력 받을 수 있다.
    - 클래스는 한번의 결제에 동일한 클래스 1개 수량만 주문이 가능
    - 동일 클래스 추가 주문 시 중복 주문 방지 처리
    - 결제가 완료되고 나면, 다음 주문에선 이전에 결제와 무관하게 클래스 주문 가능
- 주문은 상품번호, 수량을 입력받습니다
    - empty 입력 (space + ENTER)이 되었을 경우 해당 건에 대한 주문이 완료되고 결제 진행
    - 결제 시 재고확인 후 재고가 부족할 경우 결제를 시도하면 **SoldOutException**이 발생
- 키트 상품의 경우, 주문 금액이 5만원 미만이면 배송료 5,000원이 추가
    - 클래스와 키트를 함께 주문할 시 배송료는 포함되지 않아야 합니다
    - [상품번호 9236] 를 1개 구입할 경우에는 9,900 * 1개 + 5,000원이 되어야 합니다
    - [상품번호 45947] 와 [상품번호 91008] 1개를 구입할 경우에는 249,500 + 28,000이 되어 5만원이 넘어가므로 배송료는 추가하지 않음
- 주문이 완료 되었을 경우(space + ENTER) 주문 내역과 주문 금액, 결제 금액 (배송비 포함) 을 화면에 출력
- 'q'를 입력하면 프로그램이 종료 됨

### 제약 사항 추가 내용
- 키트 상품인 경우 재고 차감 후, 클래스는 재고 차감 않음(무제한)

### Git Commit Template
- reference : https://jeong-pro.tistory.com/207
```
feat: Implement user model class 

<description>
로그인 기능 구현을 위해, User 모델 작성. 

<issue-number>
#512
```
```
<type>: <title> 
# <type>
# - feat   : 새로운 기능 추가
# - fix    : 버그 수정 
# - docs   : 문서 수정
# - style  : 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
# - refact : 코드 리펙토링
# - test   : 테스트 코드, 리펙토링 테스트 코드 추가
# - chore  : 빌드 업무 수정, 패키지 매니저 수정

# <title>
# 제목의 길이는 최대 40글자까지 한글로 간단 명료하게 작성 
# <type>: 한칸 띄고 <title 작성>
# 제목을 작성하고 반드시 빈 줄 한 줄을 만들어야 함 
# 제목에 .(마침표) 금지 

<description> 
# 내용의 길이는 한 줄당 60글자 내외에서 줄 바꿈. 한글로 간단 명료하게 작성 
# 어떻게 보다는 무엇을, 왜 변경했는지를 작성할 것 (필수) 

<issue-number> 
# 연관된 이슈 첨부, 여러 개 추가 가능 
```