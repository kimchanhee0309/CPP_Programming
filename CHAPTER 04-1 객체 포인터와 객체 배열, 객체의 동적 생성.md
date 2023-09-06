# 객체 포인터

* C++에서 객체를 다루기 위해
  * 객체에 대한 **포인터 변수**를 선언하고,
  * 이 포인터 변수로 객체의 멤버 변수를 읽고 값을 쓰거나 멤버 함수를 호출할 수 있음
* 객체에 대한 포인터 변수 선언
  * Circle 타입의 객체에 대한 포인터 변수 p 선언 방법
    >Circle *p;  //선언된 포인터 변수 p는 아무 객체도 가리키지 않고 있음
* 포인터 변수에 객체 주소 지정
  * 객체의 주소는 객체 이름 앞에 **& 연산자**를 사용하여 표현함
  * 예시(포인터 변수 p에 donut 객체의 주소를 저장하는 코드)
   >>p = &donut; //p에 donut 객체의 주소 저장
   >
   >>Circle* p = &donut; //포인터 변수 선언 시 같이 객체 주소로 초기화 할 수도 있음
* 포인터를 이용한 객체 멤버 접근
  * 객체 이름으로 멤버를 접근할 때는 **점(.) 연산자**를 이용하지만,
   >d = donut.getArea(); //객체 이름으로 멤버 함수 호출
  * 객체 포인터로 멤버를 접근할 때는 **→ 연산자**를 사용함
   >>d = p→getArea(); //포인터로 객체 멤버 함수 호출(아래와 같은 표현 방법도 사용 가능)
   >
   >>d = (p*).getArea(); 
* 초기화되지 않은 객체 포인터를 이용할 경우, 오류가 발생하는 점 잊지 말기!
* 예시 코드(객체 포인터 선언 및 활용)
```C++
#include <iostream>
using namespace std;

class Circle {
	int radius;
public:
	Circle() { radius = 1; }
	Circle(int r) { radius = r; }
	double getArea();
};

double Circle::getArea() {
	return 3.14 * radius * radius;
}

int main() {
	Circle donut;
	Circle pizza(30);

	//객체 이름으로 멤버 접근
	cout << donut.getArea() << endl;

	//객체 포인터로 접근
	Circle* p;
	p = &donut;
	cout << p->getArea() << endl; //donut의 getArea() 호출
	cout << (*p).getArea() << endl; //donut의 getArea() 호출

	p = &pizza;
	cout << p->getArea() << endl; //pizza의 getArea() 호출
	cout << (*p).getArea() << endl; //pizza의 getArea() 호출
}
```
