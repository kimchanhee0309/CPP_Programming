# 객체와 객체 배열의 동적 생성 및 반환

<details>
<summary>1. 객체의 동적 생성 및 반환</summary>
<div markdown="1">       

#### new를 이용한 객체의 동적 생성과 생성자
* new 연산자를 이용하여 객체를 **동적 생성**하는 구문
  >클래스 이름 *포인터변수 = new 클래스이름;  //기본 생성자 호출
  >클래스 이름 *포인터변수 = new 클래스이름(매개변수);  //매개변수 있는 생성자 호출
* new는 `클래스 크기`의 메모리를 할당받아 객체를 생성하며, 이때 생성자를 호출함
* Circle 타입의 객체를 생성하는 사례
  >Circle *p = new Circle;  //기본 생성자 Circle() 호출. p = new Circle();와 같음
  >Circle *q = new Circle(30);  //생성자 Circle(30) 호출

#### delete를 이용한 객체 반환과 소멸자
* 동적으로 생성된 객체는 객체에 대한 `포인터변수`를 이용하여 다음과 같이 반환함
  >delete 포인터변수;
* 객체를 반환하는 예
  >Circle *p = new Circle;  //생성자 Circle() 호출. p = new Circle();와 같음
  >
  >Circle *q = new Circle(30);  //생성자 Circle(30) 호출
  >
  > delete p;  //Circle 객체 반환
  >
  >delete q;  //Circle 객체 반환
* delete 사용 시 `포인터변수`는 반드시 `new를 이용하여 동적 할당받은 메모리의 주소`이어야 함
  >Circle donut;
  >
  >Circle *p = &donut;
  >
  >delete p;  //실행 오류. p가 가리키는 객체는 동적 할당받은 것이 아님
* delete가 실행되면, **객체를 반환하기 직전**에 객체의 소멸자가 실행됨

#### 객체 동적 생성 및 반환 예시 코드
* Circle 객체의 동적 생성 및 반환
```C++
#include <iostream>
using namespace std;

class Circle {
	int radius;
public:
	Circle();
	Circle(int r);
	~Circle();
	void setRadius(int r) { radius = r; }
	double getArea() { return 3.14 * radius * radius; }
};

Circle::Circle() {
	radius = 1;
	cout << "생성자 실행 redius = " << radius << endl;
}

Circle::Circle(int r) {
	radius = r;
	cout << "생성자 실행 radius = " << radius << endl;
}

Circle::~Circle() {
	cout << "소멸자 실행 radius = " << radius << endl;
}

int main() {
	Circle* p, * q;
	p = new Circle;
	q = new Circle(30);
	cout << p->getArea() << endl << q->getArea() << endl;
	delete p; //생성한 순서에 관계 없이 원하는 순서대로 delete 할 수 있음
	delete q;
}

<실행 결과>
생성자 실행 redius = 1
생성자 실행 radius = 30
3.14
2826
소멸자 실행 radius = 1
소멸자 실행 radius = 30
```
* Circle 객체의 동적 생성 및 반환 응용
```C++
#include <iostream>
using namespace std;

class Circle {
	int radius;
public:
	Circle();
	Circle(int r);
	~Circle();
	void setRadius(int r) { radius = r; }
	double getArea() { return 3.14 * radius * radius; }
};

Circle::Circle() {
	radius = 1;
	cout << "생성자 실행 redius = " << radius << endl;
}

Circle::Circle(int r) {
	radius = r;
	cout << "생성자 실행 radius = " << radius << endl;
}

Circle::~Circle() {
	cout << "소멸자 실행 radius = " << radius << endl;
}

int main() {
	int radius;
	while (true) {
		cout << "정수 반지름 입력(음수이면 종료)>> ";
		cin >> radius;
		if (radius < 0) break; //음수가 입력되어 종료한다.
		Circle* p = new Circle(radius); //동적 객체 생성
		cout << "원의 면적은 " << p->getArea() << endl;
		delete p; //객체 반환
	}
}

<실행 결과>
정수 반지름 입력(음수이면 종료)>> 5
생성자 실행 radius = 5
원의 면적은 78.5
소멸자 실행 radius = 5
정수 반지름 입력(음수이면 종료)>> 9
생성자 실행 radius = 9
원의 면적은 254.34
소멸자 실행 radius = 9
정수 반지름 입력(음수이면 종료)>> -1
```
</div>
</details>

___

<details>
<summary>2. 객체 배열의 동적 생성 및 반환</summary>
<div markdown="1">       

#### 객체 배열의 동적 생성과 생성자
* new를 이용하여 객체 배열을 동적으로 생성하는 구문
  >클래스이름 *포인터변수 = new 클래스 이름 [배열 크기];
  >
  >Circle *pArray = new Circle[3];  //3개의 Circle 객체 배열의 동적 생성
* 연속된 3개의 Circle 객체 배열을 동적 할당받고, 배열의 주소를 pArray에 저장함
* 각 객체에 대해 기본 생성자 Circle()이 호출됨
* new를 이용하여 동적으로 배열을 생성할 때, 매개변수 있는 생성자를 직접 호출할 수는 없음
  >Circle *pArray = new Circle[3](30);  //구문 오류, 컴파일 오류
* 대신, 다음과 같이 배열을 각 원소 객체로 초기화 할 수 있음
  >Circle *pArray = new Circle[3] { Circle(1), Circle(2), Circle(3) };
  >
  >//3개의 객체를 반지름 1, 2, 3으로 각각 초기화
  
#### 객체 배열의 사용
* 동적으로 생성된 객체 배열은 보통 객체 배열처럼 사용함
  >Circle *pArray = new Circle[3];  //객체 배열의 동적 생성
  >
  >pArray[0].setRadius(10);  //배열의 첫 번째 객체의 setRadius() 멤버 함수 호출
  >
  >pArray[1].setRadius(20);
  >
  >pArray[2].setRadius(30);
  >
  >for(int i=0; i<3; i++)
  >
  >cout << pArray[i].getArea();  //배열의 i 번째 객체의 getArea() 멤버 함수 호출
* pArray가 포인터이므로 앞의 코드를 다음과 같이 작성할 수도 있음
  >pArray→setRadius(10);
  >
  >(pArray+1)→setRadius(20);
  >
  >(pArray+2)→setRadius(30);
  >
  >for(int i=0; i<3; i++)
  >
  >cout << (pArray+i)→getArea();

#### 배열의 반환과 소멸자
* delete 연산자를 이용하여 동적으로 할당받은 배열을 반환하는 형식
  >delete [] 포인터변수;  //포인터변수가 가리키는 배열을 반환한다.
* pArray가 가리키고 있는 배열을 반환하는 delete 문의 예
  >delete [] pArray;
* delete는 pArray가 가리키는 배열을 반환하기 직전, 배열의 각 원소 객체의 소멸자를 실행함
* 소멸자의 실행 순서는 생성의 반대 순임
  >pArray[2] 객체의 소멸자 → pArray[1] 객체의 소멸자 → pArray[0] 객체의 소멸자

#### 배열의 동적 및 생성 반환 예시 코드
* Circle 배열의 동적 생성 및 반환
```C++
#include <iostream>
using namespace std;

class Circle {
	int radius;
public:
	Circle();
	Circle(int r);
	~Circle();
	void setRadius(int r) { radius = r; }
	double getArea() { return 3.14 * radius * radius; }
};

Circle::Circle() {
	radius = 1;
	cout << "생성자 실행 redius = " << radius << endl;
}

Circle::Circle(int r) {
	radius = r;
	cout << "생성자 실행 radius = " << radius << endl;
}

Circle::~Circle() {
	cout << "소멸자 실행 radius = " << radius << endl;
}

int main() {
	Circle* pArray = new Circle[3]; //객체 배열의 동적 생성, 각 객체의 기본 생성자 실행

	pArray[0].setRadius(10);
	pArray[1].setRadius(20);
	pArray[2].setRadius(30);

	for(int i=0;i<3;i++){
		cout << pArray[i].getArea() << endl;
	}

	Circle* p = pArray; //포인터 p에 배열의 주소값 설정
	for (int i = 0; i < 3; i++)
	{
		cout << p->getArea() << endl;
		p++; //다음 원소의 주소로 증가
	}

	delete[] pArray; //객체 배열 반환
}

<실행 결과>
생성자 실행 redius = 1
생성자 실행 redius = 1
생성자 실행 redius = 1
314
1256
2826
314
1256
2826
소멸자 실행 radius = 30
소멸자 실행 radius = 20
소멸자 실행 radius = 10
```
* Circle 배열의 동적 생성 및 반환 응용
```C++
#include <iostream>
using namespace std;

class Circle {
	int radius;
public:
	Circle();
	~Circle() { }
	void setRadius(int r) { radius = r; }
	double getArea() { return 3.14 * radius * radius; }
};

Circle::Circle() {
	radius = 1;
}

int main() {
	cout << "생성하고자 하는 원의 개수?";
	int n, radius;
	cin >> n; //원의 개수 입력
	if (n <= 0) return 0; //1보다 작은 정수가 입력되면 프로그램 종료
	Circle* pArray = new Circle[n]; //n 개의 Circle 배열 생성
	for (int i = 0; i < n; i++) { //입력받은 원의 개수만큼 배열 생성
		cout << "원" << i + 1 << ": "; //프롬프트 출력
		cin >> radius; //반지름 입력
		pArray[i].setRadius(radius); //각 Circle 객체를 반지름으로 초기화
	}

	int count = 0; //카운트 변수
	Circle* p = pArray;
	for (int i = 0; i < n; i++) {
		cout << p->getArea() << ' '; //원의 면적 출력
		if (p->getArea() >= 100 && p->getArea() <= 200)
			count++;
		p++;
	}
	cout << endl << "면적이 100에서 200 사이인 원의 개수는 " << count << endl;
	delete[] pArray; //객체 배열 소멸
}

<실행 결과>
생성하고자 하는 원의 개수?4
원1: 5
원2: 6
원3: 7
원4: 8
78.5 113.04 153.86 200.96
면적이 100에서 200 사이인 원의 개수는 2
```

#### 동적으로 할당받은 메모리는 반드시 반환해야 하는가?
* `힙(heap)` 은 프로그램이 실행 중에 new를 이용하여 동적으로 할당받아 사용할 수 있는 메모리
* 할당받은 후 필요 없게 된 메모리를 힙에 반환하지 않거나 코딩 잘못으로 메모리 누수가 생기면, 힙에 메모리가 부족하여 할당받을 수 없게 되니 주의가 필요함
* 다행히 프로그램 종료 시, 힙 전체가 운영체제에 의해 반환되므로 누수 메모리에 대한 걱정은 하지 않아도 됨

#### 동적 메모리 할당과 메모리 누수(memory leak)
* 동적으로 할당받은 메모리의 주소를 잃어버려 힙에 반환할 수 없게 되면 `메모리 누수`가 발생함
* 메모리 누수가 계속 발생하여 힙의 크기가 줄어들게 되면, 실행 중에 메모리를 할당받을 수 없는 심각한 상황이 발생할 수 있음
</div>
</details>
