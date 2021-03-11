# 오류처리

- 오류 코드보다 예외를 사용하라.

- Try-Catch-Finally 문부터 작성하라.
  - 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성하라.

- 미확인 예외를 사용하라.
  - 확인된 예외는 OCP(개방폐쇠원칙)를 위반한다.
  - 최하위 함수에서 예외가 발생하면 최상위 단계까지 연쇄적인 수정이 일어난다.
    - 해당 경로의 모든 함수가 예외를 알아야 하므로 캡슐화가 깨진다.

- 예외에 의미를 제공하라.
  - 오류 메시지에 정보를 담아 예외와 함께 던져라.

- 호출자를 고려해 예외 클래스를 정의하라.
  - 오류를 정의할 때 프로그래머에게 가장 중요한 관심사는 **오류를 잡아내는 방법.**
  - 실제 외부 API를 사용할 때는 감싸기 기법이 최선이다.
    - 감싸기 기법을 사용하면 특정 업체가 API를 설계한 방식에 발목 잡히지 않는다.

- 정상 흐름을 정의하라.
  - **특수 사례 패턴 :** 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식.
    - 클래스나 객체가 예외적인 상황을 캡슐화 해서 처리함.

- null을 반환하지 마라.
  - 메서드에서 null을 반환하고픈 유혹이 든다면 예외를 던지거나 특수 사례 객체를 반환해라.
  - 사용하려는 외부 API가 null을 반환한다면
    - 감싸기 메서드를 구현
    - 특수 사례 객체를 반환
 
- null을 전달하지 마라.
  - null반환만큼 null전달도 나쁜 방식.
  - 정상적인 인수로 null을 기대하는 APIr가 아니라면 메서드로 null을 전달하는 코드는 최대한 피한다.

- 깨끗한 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다.
</br>

# 경계

- 외부코드와 우리 코드를 깔끔하게 통합해야 한다.

- 외부코드 사용하기.
  - Map과 같은 경계 인터페이스를 이용할 때는 이를 이용하는 클래스나 클래스 계열 밖으로 노출되지 않도록 주의한다.
  - Map 인스턴스를 공개 API의 인수로 넘기거나 반환값으로 사용하지 않는다.

- 경계 살피고 익히기.
  - **학습 테스트 :** 통제된 환경에서 API를 제대로 이해하는 확인하는 방법.

- 학습 테스트는 공짜 이상이다.
  - 패키지 새 버전이 나온다면 학습 테스트를 돌려 차이가 있는지 확인한다.
  - 이런 경계 테스트가 있어야 새 버전으로 패키지를 이전하기 쉬워진다.

- 아직 존재하지 않는 코드를 사용하기.
  - 필요한 API가 있다면 자체적으로 설계 및 구현을 해볼것?

---
### **Adapter Pattern**
- 기존 클래스의 소스코드를 수정해서 인터페이스에 맞추는 작업보다는 기존 소스를 수정하지 않고 타켓 인터페이스에 맞춰서 동작을 가능하게 하는 것.
- 어댑터의 두 종류
  - 객체 어댑터
  - 클래스 어댑터
    - 다중상속이 필요하지만, 자바에서는 다중 상속이 불가능하다.

#### __example__
- ASIS Enumerator 인터페이스를 사용해야 하는 경우가 종종 있지만, ToBe에서는 Iterator만 사용하려 할 때.
<img width="400" src="https://t1.daumcdn.net/cfile/tistory/267FFE4E575EB98A2E" alt="Adapter Pattern Example" title="Adapter Pattern">

```
public class EnumerationIterator implements Iterator {
          Enumeration enumeration;

          public EnumerationIterator(Enumeration enumeration) {
                   this.enumeration= enumeration;
          }

          @Override
          public boolean hasNext(){ 
                   return enumeration.hasMoreElements();
          }

          @Override
          public Object next() {
                   return enumeration.nextElement();
          }

          @Override
          public void remove() {
                   throw new UnsupportedOperationException(); //예외 던짐 UnsupportedOperationException 지원
          }
 }
```
---


- 깨끗한 경계.
  - 소프트웨어의 설계가 우수하다면 변경하는데 많은 투자와 재작업이 필요하지 않다.
  - 기대치에 정의하는 테스트 케이스도 작성한다.
  - 외부 패키지를 호출하는 코드를 가능한 줄여 경계를 관리하자.
