# CHAPTER 01 C++ 시작
## C++ 언어의 특징
**C++ 언어의 주요한 설계 목표**
* C언어로 작성된 프로그램과의 **호환성(compatability)** 을 유지*
*  데이터 캡슐화, 상속, 당형성 등 **객체 지향 개념**을 도입
   * 소프트웨어의 재사용을 통해 소프트웨어 생산성을 높이고, 복잡하고 큰 규모의 소프트웨어 작성, 관리, 유지 보수를 쉽게 하기 위해 도입
* **타입 체크를 엄격히 하여** 실행 시간 오류의 가능성을 줄이고 디버깅을 도움
* 실행 시간의 **효율성 저하를 최소화** 함
   * 객체 지향 개념의 도입으로 멤버 함수 호출이 잦아지고 이로 인해 실행 시간이 저하되는 비효율성을 막기 위해 멤버 함수에 인라인 함수를 도입하는 등 함수 호출로 인한 시간 저하를 막음

   ___
  
**C언어에 추가한 기능**
* **함수 중복(function overloading)**
   * 매개 번수의 개수나 타입이 서로 다른 동일한 이름의 함수들을 선언할 수 있게 함(6장에서 자세히 다룸)
* **디폴트 매개 변수(default parameter)**
   * 매개 변수에 값이 전달되지 않는 경우 디폴트 값이 전달되도록 함수를 선언할 수 있게 함(6장에서 자세히 다룸) 
* **참조(reference)와 참조 변수**
   * 변수에 별명을 붙여 변수 공간을 같이 사용할 수 있는 참조의 개념을 도입함(5장에서 자세히 다룸)  
* **참조에 의한 호출(call-by-reference)**
   * 함수 호출시 참조를 전달할 수 있게 함(5장에서 자세히 다룸) 
* **new와 delete 연산잔**
   * 동적 메모리 할당 및 해제를 위한 new, delete 연산자를 도입함(4장에서 자세히 다룸) 
* **연산자 재정의(operator overloading)**
   * 기존의 연산자에 새로운 연산자를 정의할 수 있게 함(7장에서 자세히 다룸) 
* **제네릭 함수와 클래스(generics)**
   * 함수나 클래스를 데이터 타입에 의존하지 않고 일반화시킬 수 있게 함(10장에서 자세히 다룸) 

   ___

**C++의 객체 지향 특성(캡슐화, 상속성, 다형성)**
* **객체와 캡슐화(Encapsulation)**
   * 데이터를 캡슐로 싸서 외부의 접근으로부터 데이터를 보호하는 객체 지향 특성
   * 클래스(class) : C++에서 캡슐의 역할을 함, 객체를 정의하는 틀
   * 객체 : 클래스라는 틀에서 생겨난 실체(instance)
   * C++ 클래스는 멤버 변수들과 멤버 함수들로 이루어지며, 멤버들은 캡슐 외부에 공개하거나(public), 보이지 않게(private) 선언할 수 있음
   * 예시 코드
```C++
class Circle
{
private:
  int radius; //반지름 값
public:
  Circle(int r) { radius = r; }
  double getArea() { return 3.14*radius*radius; }
};
```
* **상속성(Interitance)**
   * 상속이란, 자식이 부모의 유전자를 물려받는 것과 유사함
   * C++에서 상속은 객체를 정의하는 클래스 사이에 상속 관계를 두어, 자식 클래스의 객체가 생성될 대 자식 클래스에 선언된 멤버뿐 아니라 부모 클래스에 선언된 멤버들도 함께 가지고 탄생하게 함
   * 구현된 코드의 재사용성을 높여서 소프트웨어 생산성을 높임
   * 예시 코드
```C++
class Phone
{
  void call();
  void receive();
};

class MobilePhone : public Phone //phone을 상속 받음
{
  void connectWireless();
  void recharge();
};

class MusicPhone : public MobilePhone //MobilePhone을 상속 받음
{
  void downloadMusic();
  void play();
};
```
* **다형성(Poltmorphism) - 연산자 중복, 함수 중복, 함수 오버라이딩**
   * 하나의 기능이 경우에 따라 서로 다르게 보이거나 다르게 작동하는 현상
   * **연산자 중복(operator overloading)**
     * 피연산자에 따라 서로 다른 연산이 이루어지는 것
     * 예시 사례
       - 2+3 -> 5
       - "남자"+"여자"->"남자여자"
       - redColor 객체+blueColor 객체->purpleColor 객체
   * **함수 중복(function overloading)**
     * 같은 이름의 함수가 매개 변수의 개수나 타입이 다르면 서로 다른 함수로 인식되는 것
     * 예시 코드
```
class Phone
{
  void call();
  void receive();
};

class MobilePhone : public Phone //phone을 상속 받음
{
  void connectWireless();
  void recharge();
};

class MusicPhone : public MobilePhone //MobilePhone을 상속 받음
{
  void downloadMusic();
  void play();
};
```
   * **함수 재정의 or 함수 오버라이딩(function overrding)**
     * 부모 클래스에 구현된 함수를 동일한 이름으로 자식 클래스에서 다르게 구현하는 것
       
   ___

**C++ 언어에서 객체 지향 개념을 도입한 목적**
* 소프트웨어의 **생산성 향상**
   * 소프트웨어의 재사용을 위한 객체 지향적 장치를 내장하고 있기 때문에(다형성, 객체, 캡슐화 등)
     -> 이미 만들어진 C++ 클래스를 상속받거나 C++ 객체를 가져다 재사용하거나, 부분 수정을 통해 소프트웨어를 작성하는 부담을 대폭 줄일 수 있음  
* **실세계에 대한 쉬운 모델링**
   * 실세계의 현상을 보다 쉽게 프로그래밍 하기 위해 객체를 중심으로 하는 객체 지향 언어가 등장함
   * 객체 지향 언어는 게임에 등장하는 각 요소를 객체로 정의하고, 객체의 속성과 행위를 묘사하고 객체 사이의 상호 작용을 표현하는 방법으로 효과적인 프로그래밍을 할 수 있게 함

   ___

**절차 지향 프로그래밍과 객체 지향 프로그래밍**
* **절차 지향 프로그래밍(procedural programming)**
   * 실행하고자 하는 절차대로 일련의 명령어를 나열하여 프로그래밍하는 방법
   * 작업을 절차로 표현하며, 명령들의 순서나 흐름에 중점을 둠
   * But, 실제 응용의 세계는 일련의 행위뿐만 아니라 각 물체 간의 관계, 상호 작용 등 훨씬 복잡하게 구성되어 있음
    -> 이런 단점을 극복하기 위해 등장한 것이 객체 지향 개념임
* **객체 지향 프로그래밍(Object-Oriented Programming, OOP)**
   * 프로그램을 보다 실제 세상에 가깝게 모델링(modeling)하여 실제 세상의 물체를 객체로 표현함
   * 객체들의 관계, 상호 작용을 객체 지향 기법으로 구현
   * 객체를 추출하고 객체들의 관계를 결정하고 이들의 상호 작용을 멤버 함수와 멤버 변수로 구현함

   ___

**C++ 언어와 제네릭 프로그래밍(generic programming)**
* 제네릭 프로그래밍이란?
   * 동일한 프로그램 코드에 다양한 데이터 타입을 적용할 수 있도록 함수와 클래스를 일반화시킨 '제네릭 함수(generic function)'와 '제네릭 클래스(generic clss)'를 만들고,
   * 개발자가 원하는 데이터 타입을 적용시켜 프로그램 코드를 틀에서 찍어내는 듯이 생산하는 기법
   * 입출력 라이브러리를 과감하게 템플릿(template)으로 선언하여 제네릭화하고, 응용 프로그램 개발에 필요한 대부분의 자료 구조를 제네릭 함수와 제네릭 클래스로 구현한 STL(Standard Template Library)을 표준화함  

   ___

**C++ 언어의 아킬레스**
* C 코드와의 호환성으로 인해, 캡슐화의 원칙이 다소 무너짐
   * 캡슐화의 기본 원칙은 변수와 함수를 클래스(캡슐) 안에 선언하도록 하는 것임
   * But, C언어로 작성된 프로그램을 수용하기 위해 함수 바깥에 전역 변수를 선언할 수 있는 C언어의 특성을 받아들일 수밖에 없음
     -> So, 전역 변수의 사용에 따른 부작용이 여전히 존재하게 되었음 
