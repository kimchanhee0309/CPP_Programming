# 함수 호출 시 객체 전달

<details>
<summary>1. '값에 의한 호출'로 객체 전달</summary>
<div markdown="1">       

#### '값에 의한 호출' 과정
```C++
void increase(Circle c) { //매개 변수 객체 c 생성
  int r = c.getRadius();
  c.setRadius(r+1); //c의 반지름 1 증가
}

int main() {
  Circle waffle(30); //waffle 객체 생성
  increase(waffle); //함수 호출 시 객체 복사
  cout << waffle.getRadius() << endl; //화면에 30 출력 
}

<실행 결과>
30
```
* 함수에 객체를 전달할 때 객체 이름만 적는다.
* increase()가 호출되면, waffle 객체가 현재 상태 그대로 매개 변수 c에 복사됨
* 이후 increase()는 객체 c의 반지름 1을 증가시켜 31이 되지만, waffle 객체의 반지름은 변하지 X
* increase()가 종료하면 매개 변수 객체 c가 함께 소멸되고,
* main() waffle 객체는 increase()를 호출하기 전과 동일하게 반지름이 30
* 값에 의해 호출 시 객체 복사 시간
  * 함수 안에서 매개 변수 객체에 어떤 변화가 일어나도 실인자(원본 객체)를 훼손시키지 않는 장점이 있음
  * 실인자 객체의 크기가 크면 객체를 복사하는 시간이 커진다는 단점도 있음

#### '값에 의한 호출'로 객체를 전달할 때 문제점
* 객체를 매개 변수로 가지는 함수의 경우, C++ 컴파일러는 매개 변수 객체의 **생성자는 실행되지 않고 소멸자만 실행**되도록 컴파일함
* 예제 5-1
```C++
<'값에 의한 호출'시 매개 변수의 생성자 실행되지 않는 예시 코드>
#include <iostream>
using namespace std;

class Circle {
private:
  int radius;
public:
  Circle();
  Circle(int r);
  ~Circle();
  double getArea() { return 3.14*radius*radius; }
  int getRadius() { return radius; }
  void setRadius(int radius) { this->radius = radius; }
};

Circle::Circle() {
  radius = 1;
  cout << "생성자 실행 radius = " << radius << endl;
}

Circle::Circle(int radius) {
  this->radius = radius;
  cout << "생성자 실행 radius = " << radius << endl;
}

Circle::~Circle() {
  cout << "소멸자 실행 radius = " << radius << endl;
}

void increasae(Circle c) { //객체 c의 생성자 실행되지 않음
  int r = c.getRadius();
  c.setRadius(r+1);
} //객체 c의 소멸자 실행됨

int main() {
  Circle waffle(30);
  increase(waffle);  //waffle의 내용이 그대로 c에 복사
  cout << wallfe.getRadius() << endl;
}

<실행 결과>
생성자 실행 radius = 30 //waffle 생성
소멸자 실행 radius = 31 //c 소멸, c의 생성자 실행되지 않았음
30
소멸자 실행 radius = 30 //waffle 소멸
```

#### 왜 매개 변수 객체의 생성자가 실행되지 않도록 컴파일 되는가?
* 예제 5-1 기준으로 설명
  * increase() 함수의 매개 변수 객체 c가 생성되고 소멸되는 과정을 보여줌
    >Circle waffle(30);
    >
    >increase(waffle);
  * 반지름 30인 waffle 객체를 생성하고, increase() 함수를 호출하여 waffle 객체를 전달
  * increase() 함수의 매개 변수 c에 waffle 객체가 전달된 후,
  * 만일 객체 c의 생성자 Circle()이 실행된다면, 객체 c의 반지름(radius 멤버 변수)이 1로 초기화되어, **전달받은 원본의 상태**를 잃어버리게 됨
  * 이런 문제가 발생하지 않도록 컴파일러는 매개 변수 객체의 생성자가 실행되지 않도록 함
  * 소멸자는 increase()가 리턴하면 c의 소멸자는 실행되고 c는 사라짐
</div>
</details>

___

<details>
<summary>2. '주소에 의한 호출'로 객체 전달</summary>
<div markdown="1">       

#### '주소에 의한 호출' 과정
```C++
void increase(Circle *p) {
  int r = p->getRadius();
  p->setRadius(r+1);
}

int main() {
  Circle waffle(30);
  increase(&waffle);
  cout << waffle.getRadius();
}

<실행 결과>
31
```
* increase() 함수는 ‘주소에 의한 호출’이 일어나도록 다음과 같이 선언
  >void increase(Circle *p)
* main() 함수에서 다음과 같이 increase()를 호출하면,
  > Circle waffle(30);
  > 
  > increase(&waffle);  //waffle 객체의 주소를 전달한다.
* waffle 객체의 주소가 포인터 p에 전달됨
* p는 객체가 아니므로 생성자나 소멸자와 상관 없음
* 다음 코드가 실행되면, waffle 객체의 반지름이 1 증가됨
  >p→setRadius(r+1);
  
#### '주소에 의한 호출'의 특징
* 원본 객체를 복사하는 시간 소모가 없으며, 매개 변수가 단순 포인터이므로, ‘값에 의한 호출’ 시에 발생하는 생성자 소멸자의 비대칭 문제도 없음
* But, 매개 변수 포인터로 의도하지 않게 원복 객체를 훼손할 가능성이 있기 때문에 코딩에 조심해야 함
</div>
</details>
