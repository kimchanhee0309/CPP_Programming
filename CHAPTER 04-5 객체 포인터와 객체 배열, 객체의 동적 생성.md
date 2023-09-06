# this 포인터

<details>
<summary>1. this의 기본 개념</summary>
<div markdown="1">       

* 객체 자신에 대한 `포인터`로서 **클래스의 멤버 함수 내에서만** 사용됨
* this는 전역 변수도 아니고 함수 내에 선언된 지역 변수도 아님
* 보이지는 않지만 모든 멤버에 쓰이는 **객체의 위치저장 변수**
* 객체의 멤버 함수가 호출될 때, 컴파일러에 의해 보이지 않게 전달되는 **객체에 대한 저장소**임
```C++
#include <iostream>
using namesapce std;

class Circle{
  int radius;
public:
  Circle() { this->radius = 1; }
  Circle(int radius) { this->radius = radius; }
  void setRadius(int radius) { this->radius = radius; }
  . . . .
};
```
</div>
</details>

___

<details>
<summary>2. this와 객체</summary>
<div markdown="1">       

* this-객체의 관계를 보여주는 코드
```C++
class Circle{
  int radius;
public:
  Circle() { this->radius=1; }
  Circle(int radius) { this->radius = radius; }
  void setRadius(int radius) { this->radius = radius; }
};

int main()
{
  Circle c1;
  Circle c2(2);
  Circle c3(3); 

  c1.setRadius(4);
  c2.setRadius(5);
  c3.setRadius(6);
}
```
* c1,c2,c3 속에 있는 각 this는 **객체 자신에 대한 포인터**임
* 각 객체 속의 this는 다른 객체 속의 this와 서로 다른 포인터임을 기억하기
</div>
</details>

___

<details>
<summary>3. this가 필요한 경우</summary>
<div markdown="1">       

* **멤버 변수의 이름과 동일한 이름으로 매개 변수 이름을 짓고자** 할 때
  * 만일 this를 생략한다면, 모두 매개 변수를 지칭하게 되어 멤버 변수에 값을 쓰는 목적이 왜곡됨
* 객체의 멤버 함수에서 **객체 자신의 주소를 리턴**할 때
```C++
class Sample{
public:
  Sample* f() {
    . . . .
    return this; //현재 객체의 주소 리턴
  }
};
```
* **연산자 중복**을 구현할 때
</div>
</details>

___

<details>
<summary>4. this의 제약 조건</summary>
<div markdown="1">       

* 클래스의 `멤버 함수`에서만 사용할 수 있음
  * 멤버가 아닌 함수들은 어떤 객체에도 속하지 않기 때문임
* 멤버 함수라도 `정적 멤버 함수(static member function)` 는 this를 사용할 수 없음
  * 정적 멤버 함수는 객체가 생성되기 전에 호출될 수 있으며,
  * 정적 멤버 함수가 실행되는 시점에서 ‘현재 객체’는 존재하지 않을 수 있기 때문임
</div>
</details>
