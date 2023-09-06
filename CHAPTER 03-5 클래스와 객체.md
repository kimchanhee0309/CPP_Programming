# 소멸자

<details>
<summary>1. 소멸자란?</summary>
<div markdown="1">       

* 객체가 소멸되면 객체 메모리는 시스템으로 반환되며, 객체 소멸 시 소멸자 함수가 실행됨
* 소멸자는 객체가 소멸되는 시점에서 자동으로 호출되는 클래스의 멤버 함수임
* 소멸자의 특징
  * 소멸자의 목적은 객체가 사라질 때 필요한 **마무리 작업을 위함**이다.
    * 객체가 소멸할 때, 동적으로 할당받은 메모리를 운영체제에게 돌려주거나,
    * 열어 놓은 파일을 저장하고 닫거나,
    * 연결된 네트워크를 해제하는 등 객체가 사라지기 전 필요한 조치를 하도록 하기 위함
  * 소멸자의 이름은 클래스 이름 앞에 **~를 붙임**
  * 소멸자는 리턴 타입이 없으며 **어떤 값도 리턴해서도 안 됨**
    * 생성자와 같이 리턴 타입 없이 선언되며, 어떤 값도 리턴해서는 안 됨
  * 소멸자는 **오직 한 개만 존재**하며 **매개 변수를 가지지 않음**
    * 생성자와 달리 한 클래스에 오직 한 개만 존재하며, 매개 변수를 가지지 않음
  * 소멸자가 선언되어 있지 않으면 기본 소멸자(default destructor)가 자동으로 생성됨
</div>
</details>

___

<details>
<summary>2. 소멸자 실행</summary>
<div markdown="1">       

```C++
//Circle 클래스에 소멸자 작성 및 실행 예시 코드

#include <iostream>
using namespace std;

class Circle {
public:
  int radius;
  Cricle();
  Circle(int r);
  ~Cricle(); //소멸자 선언
  double getArea();
};

Circle::Circle() {
  radius = 1;
  cout << "반지름 " << radius << " 원 생성" << endl;
}

Circle::Circle(int r) {
  radius = r;
  cout << "반지름 " << radius << " 원 생성" << endl;
}

Circle::~Circle() { //소멸자 구현
  cout << "반지름 " << radius << " 원 소멸" << endl;
}

double Circle::getArea() {
  return 3.14*radius*radius;
}

int main() {
  Circle donut;
  Circle pizza(30);
  return 0; // main() 함수가 종료하면 main() 함수의 스택에 생성된 객체들이 생성된 역순으로 소멸됨
}

<실행 결과>
반지름 1 원 생성
반지름 30 원 생성
반지름 30 원 소멸
반지름 1 원 소멸
```
* main()의 스택에 donut, pizza의 순서로 객체가 생성되며, return 0; 문이 실행되면 생성된 반대순으로 객체가 소멸됨

#### 생성자/소멸자 실행 순서

```C++
class Circle {
  ....
};
Circle globalCircle; //전역 객체
void f() {
  Circle localCircle; //지역 객체
}
```
* **지역 객체(local object)** : 함수 내에서 선언된 객체
  * 함수가 실행될 때 생성되고 함수가 종료할 때 소멸됨
* **전역 객체(global object)** : 함수 바깥에 선언된 객체
  * 프로그램이 로딩될 때 생성되고 main()이 종료한 뒤 프로그램 메모리가 사라질 때 소멸됨
* 두 객체 모두 생성된 순서의 반대순으로 소멸됨
```C++
//지역 객치와 전역 객체의 생성 및 소멸 순서 예시 코드

#include <iostream>
using namespace std;

class Circle {
public:
  int radius;
  Cricle();
  Circle(int r);
  ~Cricle(); //소멸자 선언
  double getArea();
};

Circle::Circle() {
  radius = 1;
  cout << "반지름 " << radius << " 원 생성" << endl;
}

Circle::Circle(int r) {
  radius = r;
  cout << "반지름 " << radius << " 원 생성" << endl;
}

Circle::~Circle() { //소멸자 구현
  cout << "반지름 " << radius << " 원 소멸" << endl;
}

double Circle::getArea() {
  return 3.14*radius*radius;
}

Circle globalDonut(1000); //전역 객체 생성
Circle globalPizza(2000); //전역 객체 생성

void f() {
  Circle fDonut(100); //지역 객체 생성
  Circle fPizza(200); //지역 객체 생성
}

int main() {
  Circle mainDonut; //지역 객체 생성
  Circle mainpizza(30); //지역 객체 생성
  f();
}

<실행 결과>
반지름 1000 원 생성
반지름 2000 원 생성
반지름 1 원 생성
반지름 30 원 생성
반지름 100 원 생성
반지름 200 원 생성
반지름 200 원 소멸
반지름 100 원 소멸
반지름 30 원 소멸
반지름 1 원 소멸
반지름 2000 원 소멸
반지름 1000 원 소멸
```
</div>
</details>
