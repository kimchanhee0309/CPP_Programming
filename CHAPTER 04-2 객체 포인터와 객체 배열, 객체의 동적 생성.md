# 객체 배열

<details>
<summary>1. 객체 배열 선언 및 활용</summary>
<div markdown="1">       

* 객체 배열은 원소가 객체라는 점을 빼고는 기본 타입의 배열을 선언하고 활용하는 방법과 동일함
  * 예시 코드(Circle 클래스의 배열 선언 및 활용)
```C++
class Circle {
	int radius;
public:
	Circle() { radius = 1; }
	Circle(int r) { radius = r; }
	void setRadius(int r) { radius = r; }
	double getArea();
};

double Circle::getArea() {
	return 3.14 * radius * radius;
}

int main() {
	Circle circleArray[3]; //(1) Circle 객체 배열 생성

	//배열의 각 원소 객체의 멤버 접근
	circleArray[0].setRadius(10); //(2)
	circleArray[1].setRadius(20);
	circleArray[2].setRadius(30);

	for (int i = 0; i < 3; i++) // 배열의 각 원소 객체의 멤버 접근
		cout << "Circle " << i << "의 면적은 " << circleArray[i].getArea() << endl;

	Circle* p; //(3)
	p = circleArray; //(4)
	for (int i = 0; i < 3; i++) { //객체 포인터로 배열 접근
		cout << "Circle " << i << "의 면적은 " << p->getArea() << endl;
		p++; //(5)
	}
}
```
* 객체 배열 선언
  >Circle circleArray[3]; //Circle 객체의 배열 circleArray를 선언하는 코드
* 객체 배열 선언문은 기본 생성자를 호출한다.
  * 객체 배열 선언문은 오직 **매개 변수 없는 기본 생성자**를 호출함
  * 매개 변수를 가진 생성자가 선언된 경우와 기본 생성자가 선언된 경우의 코드 비교
```C++
<생성자가 선언되어 있지 않은 Circle 클래스>
#include <iostream>
using namespace std;

class Circle {
  int radius;
public:
  double getArea() {
    return 3.14*radius*radius;
  }
};

int main() {
  Circle circleArray[3]; //기본 생성자 Circle() 호출
}

----------------------------------------------------------------------------------------

#include <iostream>
using namespace std;

class Circle {
  int radius;
public:
  Circle(int r) { radius = r; }
  double getArea() {
    return 3.14*radius*radius;
  }
};

int main() {
  Circle waffle(15);
  Circle circleArray[3]; //기본 생성자가 없으므로 컴파일 오류
}
```
* 객체 배열 사용
  * circleArray의 각 Circle 객체의 setRadius() 멤버 함수를 호출하는 코드
  * 원소 객체와 멤버 사이에 점(.) 연산자를 사용하는 점에 주목
  * Circle 클래스의 포인터를 이용하여 배열을 다룰 수 있음
    >circleArray[0].setRadius(10);
    >
    >circleArray[1].setRadius(20);
    >
    >circleArray[2].setRadius(30);
    
* 배열 소멸과 소멸자
  * 배열이 소멸되면, 모든 원소 객체가 소멸되며 각 원소 객체마다 소멸자가 호출됨
  * 높은 인덱스에서부터 원소 객체가 소멸되고, 각 객체마다 ~Circle() 소멸자가 실행됨
 
* 객체 포인터를 사용하여 객체 배열을 다루는 다양한 사례
```C++
(1) 포인터 p를 이용하여 객체처럼 접근

Circle *p = circleArray;
for(int i=0; i<3; i++)
  cout << (*p++).getArea() << endl;

--------------------------------------------------------------------------------------------

(2) 배열의 이름 circleArray를 포인터로 사용

for(int i=0; i<3; i++)
  cout << (circleArray+i)->getArea() << endl;

--------------------------------------------------------------------------------------------

(3) 포인터 p의 정수 연산 이용

Circle *p = circleArray;
for(int i=0; i<3; i++)
  cout << (p+i)->getArea();
```
</div>
</details>

___

<details>
<summary>2. 객체 배열 초기화</summary>
<div markdown="1">       

* 객체 배열을 생성할 때 **생성자를 사용**하여 **원소 객체를 초기화**할 수 있음
  * 예시 인용문
    >Circle circleArray[3] = { Circle(10), Circle(20), Circle() };
  * 예시 코드(객체 배열 초기화)
```C++
#include <iostream>
using namespace std;

class Circle {
  int radius;
public:
  Circle() { radius = 1; }
  Circle(int r) { radius = r; }
  void setRadius(int r) { radius = r; }
  double getArea();
};

double Circle::getArea() {
  return 3.14*radius*radius;
}

int main() {
  Circle circleArray[3] = { Circle(10), Circle(20), Circle() };

  for(int i=0; i<3; i++)
    cout << "Circle " << i << "의 면적은 " << circleArray[i].getArea() << endl;
}

<실행 결과>
Circle 0의 면적은 314
Circle 1의 면적은 1256
Circle 2의 면적은 3.14
```
</div>
</details>

___

<details>
<summary>3. 다차원 객체 배열</summary>
<div markdown="1">       

* 다차원 객체 배열 선언 구조
 >Circle circles[2][3]; //2행 3열의 2차원 객체 배열 생성
* 일차원 배열과 동일하게, 각 원소 객체가 생성될 때 기본 생성자 Circle()이 실행되어 모든 객체의 radius 값이 1이 됨
* 예시(각 객체의 radius 값을 1~6까지로 초기화 하고자 할 때)
```C++
circles[0][0].setRadius(1);
circles[0][1].setRadius(2);
circles[0][2].setRadius(3);
circles[1][0].setRadius(4);
circles[1][1].setRadius(5);
circles[1][2].setRadius(6);
```
* { } 안에 생성자를 지정하여 배열을 초기화할 수 있음
```C++
Circle circles[2][3] = { { Circle(1), Circle(2), Circle(3) },
                         { Circle(4), Circle(5), Circle()  } }
```
* 예시 코드(Circle 클래스의 2차원 배열 선언 및 활용)
```C++
#include <iostream>
using namespace std;

class Circle {
	int radius;
public:
	Circle() { radius = 1; }
	Circle(int r) { radius = r; }
	void setRadius(int r) { radius = r; }
	double getArea();
};

double Circle::getArea() {
	return 3.14 * radius * radius;
}

int main() {
	Circle circleArray[2][3]; 

	//배열의 각 원소 객체의 멤버 접근
	circleArray[0][0].setRadius(1);
	circleArray[0][1].setRadius(2);
	circleArray[0][2].setRadius(3);
	circleArray[1][0].setRadius(4);
	circleArray[1][1].setRadius(5);
	circleArray[1][2].setRadius(6);

	for (int i = 0; i < 2; i++) // 배열의 각 원소 객체의 멤버 접근
		for (int j = 0; j < 3; j++) {
			cout << "Circle {" << i << "," << j << "]의 면적은 ";
			cout << circleArray[i][j].getArea() << endl;
		}
}

<실행 결과>
Circle {0,0]의 면적은 3.14
Circle {0,1]의 면적은 12.56
Circle {0,2]의 면적은 28.26
Circle {1,0]의 면적은 50.24
Circle {1,1]의 면적은 78.5
Circle {1,2]의 면적은 113.04
```
</div>
</details>
