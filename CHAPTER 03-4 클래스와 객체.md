# CHAPTER 03-4. 생성자
<details>
<summary>1. 생성자란?</summary>
<div markdown="1">       

 * 객체가 생성될 때 자동으로 실행되는 `생성자(constructor)` 라는 특별한 멤버 함수를 통해 **객체를 초기화**함
   * 한 클래스에 여러 개의 생성자를 둘 수 있으나, 이 중 하나만 실행됨
```C++
class Circle
{
  ....
  Circle(); //클래스 이름과 동일
  Circle(int r); //리턴 타입 명기하지 않음
  .......... //2개의 생성자 함수 선언
};

Circle::Circle(){ //매개 변수 없는 생성자 함수 구현
  ........
}

Circle::Circle(int r){ //매개 변수를 가진 생성자 함수 구현
  ........
}
```
 * 생성자의 목적은 객체가 생성될 때 필요한 **초기 작업을 위함**임
 * 생성자 함수는 **오직 한 번만 실행**됨
 * 생성자 함수의 이름은 **클래스 이름과 동일하게 작성**되어야 함
 * 생성자 함수의 원형에 **리턴 타입을 선언하지 않음**
   * void 리턴 타입을 설정해서도 안 됨
   * return문을 사용할 수는 있지만, 어떤 값도 리턴하면 안 됨
 * 예시 코드
```C++
class Circle
{
  ....
  Circle(); //정상적인 생성자 선언. 리턴 타입 선언하지 않음
  void Circle(int r); //컴파일 오류. 생성자는 리턴 타입 없음
  int Circle(double r); //컴파일 오류. 생성자는 리턴 타입 없음
};

Circle::Circle()
{
  ...
  return; //생성자 함수를 종료하는 정상적인 리턴문
}
Circle::Circle()
{
  ...
  return 0; //컴파일 오류. 생성자 함수는 값을 리턴해서는 안 됨
}
```
 * 생성자는 **중복 가능**함
   * 생성자는 한 클래스에 여러 개 만들 수 있지만, **매개 변수 개수나 타입이 서로 다르게 선언**되어야 함
     > Circle(); //매개 변수 없는 생성자
     >  >Circle(int r); //매개 변수 있는 생성자

</div>
</details>

___
<details>
<summary>2. 객체 생성과 생성자 실행</summary>
<div markdown="1">       

* 2개의 생성자를 가진 Circle 클래스 예시 코드
```C++
#include <iostream>
using namespace std;

class Circle {
public:
  int radius;
  Circle(); //기본 생성자 
  Circle(int r); //매개 변수 있는 생성자
  double getArea();
};

Circle::Circle() {
  radius = 1; //반지름 값 초기화
  cout << "반지름 " << radius << " 원 생성" << endl;
}

Circle::Circle(int r) {
  radius = r; //반지름 값 초기화
  cout << "반지름 " << radius << " 원 생성" << endl;
}

double Circle::getArea() {
  return 3.14*radius*radius;
}

int main() {
  Circle donut; //매개 변수 없는 생성자 호출, Circle(); 호출
  double area = donut.getArea();
  cout << "donut 면적은 " << area << endl;

  Circle pizza(30); //매개 변수 있는 생성자 호출. 30이 r에 전달됨, Circle(30); 호출
  area = pizza.getArea();
  cout << "pizza 면적은 " << area << endl;
}
```
* donut이 생성될 때 매개 변수 없는 Circle()이 호출되며, pizza가 생성될 때 Circle(int r)을 호출하고 매개 변수 r에 30이 전달됨
* 객체의 생성은 객체 크기의 공간을 할당한 후, 객체 내의 생성자 함수가 실행되는 과정으로 나누어짐
* 즉, donut 객체와 pizza 객체는 서로 별도의 공간을 할당받고, radius 멤버 변수 역시 각 객체의 공간에 별도로 생성됨
</div>
</details>

___
<details>
<summary>3. 위임 생성자(delegating constructor), 생성자가 다른 생성자 호출</summary>
<div markdown="1">       

* 한 클래스의 생성자들에게는 대개 객체를 초기화하는 비슷한 코드가 중복됨. 이러한 부분을 간소화하기 위해 중복된 초기화 코드를 하나의 생성자로 몰고, 다른 생성자에서 이 생성자를 호출할 수 있게 함
```C++
Circle::Circle() : Circle(1) { } //위임 생성자, Circle(int r)의 생성자 호출

Circle::Circle(int r) //타겟 생성자
{
  radius = r;
  cout<<"반지름 "<<radius<<" 원 생성"<<endl;
}
```
* 타겟 생성자 : 객체의 초기화 작업이 코딩된 Circle(int r)
* 위임 생성자 : Circle() 생성자는 객체의 초기화를 다른 생성자에 위임한다고 해서 이렇게 불림
* 타겟 생성자에 객체 초기화를 전담시킴으로써 객체의 생성 과정이 명료해지고 단순해짐
* 위임 생성자에서는 타겟 생성자를 호출한 뒤, 자신만의 코드를 추가하면 됨
* 예시 코드(위임 생성자 만들기 연습)
```C++
#include <iostream>
using namespace std;

class Circle {
public:
  int radius;
  Circle(); //위임 생성자
  Circle(int r); //타겟 생성자
  double getArea();
};

Circle::Circle() : Circle(1) { } //위임 생성자, 호출. r에 1 전달

Circle::Circle(int r) {
  radius = r;
  cout << "반지름 " << radius << " 원 생성" << endl;
}

double Circle::getArea() {
  return 3.14*radius*radius
}

int main() {
  Circle donut; //매개 변수 없는 생성자 호출
  double area = donut.getArea();
  cout << "donut 면적은 " << area << endl;

  Circle pizza(30); //매개 변수 있는 생성자 호출
  area = pizza.getArea();
  cout << "pizza 면적은 " << area << endl;
}
```
</div>
</details>

___
<details>
<summary>4. 생성자와 멤버 변수의 초기화</summary>
<div markdown="1">       

* 클래스의 멤버 변수들은 자동으로 초기화되지 않기 때문에 생성자에서 초기화함
* 생성자 코드에서 멤버 변수 초기화
* 예시 코드(2개의 생성자가 각각 멤버 변수를 초기화하는 Point 클래스 사례)
```C++
class Point{
  int x, y;
public:
  Point();
  Point(int a, int b);
};
Point::Point() { x = 0; y = 0; }
Point::Point(int a, int b) { x = a; y = b; }
```
* 생성자 서두에 초깃값으로 초기화
```C++
Point::Point() : x(0), y(0) { //멤버 변수 x, y를 0으로 초기화
}
Point::Point(int a, int b) //멤버 변수 x=a로, y=a로 초기화
  : x(a), y(a) { //콜론(:) 이하 부분을 다음 줄에 써도 됨
}

//위에서 생성한 2개의 생성자를 생성자 코드의 구현부에 멤버 변수와 초깃값을 쌍으로 지정하고,
//이들을 콤마(,)로 나열하여 작성할 수 있음
```
```C++
Point::Point(int a)
  : x(a), y(a) { //멤버 변수 x=a, y=a로 초기화
}
Point::Point(int a)
  : x(100+a), y(100) { //멤버 변수 x=100+a, y=100으로 초기화
}
```
* 클래스 선언부에서 직접 초기화
```C++
class Point {
  int x=0, y=0; //클래스 선언부에서 x, y를 0으로 직접 초기화
  . . .
};
```
* 예시 코드(멤버 변수 초기화와 위임 생성자 활용)
```C++
Q. 다음 Point 클래스의 멤버 x, y를 생성자 서두에 초깃값으로 초기화하고 위임 생성자를 이용하여 재작성하라.

class Point {
  int x, y;
public:
  Point();
  Point(int a, int b);
};
Point::Point() { x = 0, y = 0; }
Point::Point(int a, int b) { x = a; y = b; }
----------------------------------------------------------------------------------------------------

#include <iostream>
using namespace std;

class Point {
  int x, y;
public:
  Point();
  Point(int a, int b);
  void show() { cout << "(" << x << ", " << y << ")" << endl; }
};

Point::Point() : Point(0, 0) { } //Point(int a, int b) 생성자 호출, 위임 생성자

Point::Point(int a, int b)  //타겟 생성자
  : x(a), y(b) { } 

int main() {
  Point origin;
  Point target(10, 20);
  origin.show();
  target.show();
}

<실행 결과>
(0, 0)
(10, 20)
```
</div>
</details>

___
<details>
<summary>5. 생성자는 꼭 있어야 하는가?</summary>
<div markdown="1">       

* 꼭 있어야 하며, 클래스에 여러 개의 생성자가 있다 해도, C++ 컴파일러는 생성자 중 반드시 하나를 호출하도록 컴파일함
* 생성자가 없는 클래스에 대해서는 컴파일러가 기본 생성자(default constructor)를 만들어 삽입하고, 자신이 삽입한 기본 생성자를 호출하도록 컴파일함
</div>
</details>

___
<details>
<summary>6. 기본 생성자(매개 변수 없는 생성자)</summary>
<div markdown="1">       

* 기본 생성자가 자동으로 생성되는 경우
  * 생성자가 하나도 없는 클래스의 경우 컴파일러가 보이지 않게 기본 생성자를 삽입함
* 기본 생성자가 자동으로 생성되지 않는 경우
  * 생성자가 하나라도 선언된 클래스의 경우, 기본 생성자를 자동으로 삽입하지 않음
```C++
class Circle {
public:
  int radius;
  double getArea();
  Circle(int r); //Circle 클래스에 생성자가 선언되어 있기 때문에, 컴파일러는 기본 생성자를 자동 생성하지 x
};

Circle::Circle(int r) {
  radius = r;
}

int main() {
  Circle pizza(30);
  Circle donut; //컴파일 오류, 기본 생성자가 없기 때문에
}
```
* 예시 코드
```C++
#include <iostream>
using namespace std;

class Rectangle {
public:
  int width, height; //너비와 높이
  Rectangle(); //생성자  
  Rectangle(int w, int h); //생성자
  Rectangle(int length); //생성자
  bool isSquare();
};

Rectangle::Rectangle() { //Rectangle::Rectangle() : Rectangle(1) {}로 해도 됨
  width = height = 1; 
}

Rectangle::Rectangle(int w, int h) {
  width = w; height = h;
}

Rectangle::Rectangle(int length) { //Rectangle::Rectangle(int length) : Rectangle(length){}로 해도 됨
  width = height = length;
}

bool Rectangle::isSquare() { //정사각형이면 true를 리턴하는 멤버 함수
  if(width == height) return true;
  else return false;
}

int main() {
  Rectangle rect1; //Rectangle() 호출
  Rectangle rect2(3, 5); //Rectangle(int w, int h) 호출
  Rectangle rect3(3); //Rectangle(int length) 호출

  if(rect1.isSquare()) cout << "rect1은 정사각형이다." << endl;
  if(rect2.isSquare()) cout << "rect2은 정사각형이다." << endl;
  if(rect3.isSquare()) cout << "rect3은 정사각형이다." << endl;
}

<실행 결과>
rect1은 정사각형이다.
rect3는 정사각형이다.
```
</div>
</details>
