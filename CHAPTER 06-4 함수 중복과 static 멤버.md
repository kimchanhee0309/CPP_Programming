# static 멤버

<details>
<summary>1. static의 특성</summary>
<div markdown="1">       

* static은 변수와 함수의 `생명 주기(life ctcle)`와 `사용 범위(scope)`를 지정하는 방식(storage class)중 하나임
  * **생명 주기** : 프로그램이 시작할 때 생성되고 프로그램이 종료할 때 소멸
  * **사용 범위** : 변수나 함수가 선언된 범위 내에서 사용. 전역(global) 혹은 지역(local)으로 구분
* non-static 멤버와의 차이
  * non-static 멤버는 객체가 생성될 때 생성되고, 각 객체마다 별도로 생성됨. 즉, 객체와 생명 주기를 같이 함
  * static 멤버는 객체의 멤버이지만, 객체가 생기기 전에 이미 생성되어 있고 객체가 사라져도 소멸되지 않음
  * 또한 static 멤버는 모든 객체들의 공통된 멤버로서 객체 사이에 공유됨
  * non-static 멤버는 각 객체마다 별도로 생성되므로 **인스턴스(instance) 멤버**라고 부르며
  * static 멤버는 클래스 당 하나만 생기고 모든 객체들이 공유하므로 **클래스(class) 멤버**라고도 부름
</div>
</details>

___

<details>
<summary>2. static 멤버 선언</summary>
<div markdown="1">       

* 멤버를 static으로 선언하려면 멤버 함수나 멤버 변수의 선언문 앞에 static 지정자를 붙이면 됨
* static 멤버들은 어떠한 접근 지정도 가능함
```C++
class Person {
public:
  int money; //개인 소유의 돈
  void addMoney(int money) {
    this->money += money;
  }
  
  static int sharedMoney; //공금
  static void addShared(int n) {
    sharedMoney += n;
  }
};

int Person::sharedMoney = 10;  //sharedMoney를 10으로 초기화, static 변수 공간 할당
                               //반드시 프로그램의 전역 공간에 선언

//money는 인스턴스 멤버로서 각 Person 객체의 개인 소유의 돈을 표현함
//sharedMoney는 static 멤버로서 모든 Person 객체들이 공유하는 공금을 표현
//money는 Person 객체마다 생성되는 반면,
//sharedMoney는 Person 객체의 개수와 상관없이 단 한 개만 생성되며, 모든 객체가 sharedMoney와 addShared()를 자신의 멤버로 인식하면서 공유함
```
* **static 멤버 변수는 외부에 전역(global) 변수로 선언되어야 한다.**
  * static 멤버 변수는 변수의 공간을 할당받는 선언문이 추가적으로 필요함
  * 이 선언문은 클래스 바깥의 전역 공간에 선언되어야 함
</div>
</details>

___

<details>
<summary>3. static 멤버 사용 : 객체의 멤버로 접근하는 방법</summary>
<div markdown="1">       

* static 멤버는 객체 이름이나 객체 포인터를 이용하여 보통 멤버와 동일하게 다루면 됨
  >객체.static 멤버
  >
  >객체포인터->static 멤버
* 간단한 예시 사례
  >Person lee;
  >
  >lee.sharedMoney = 500;  //객체 이름으로 접근
  >
  >Person *p;
  >
  >p = &lee;
  >
  >p→addShared(200);  //객체 포인터로 접근

* 예시 코드
```C++
#include <iostream>
using namespace std;

class Person {
public:
  int money; //개인 소유의 돈
  void addMoney(int money) {
    this->money += money;
  }

  static int sharedMoney;  //공금
  static void addShared(int n) {
    sharedMoney += n;
  }
};

//static 변수 생성. 전역 공간에 생성
int Person::sharedMoney=10;  //10으로 초기화

//main() 함수
int main() {
  Person han;
  han.money = 100; //han의 개인 돈 = 100
  han.sharedMoney = 200; //static 멤버 접근. 공금 = 200

  Person lee;
  lee.Money = 150; //lee의 개인 돈 = 150
  lee.addMoney(200); //lee의 개인 돈 = 150
  lee.addShared(200); //static 멤버 접근 공금 = 400

  cout << han.money << ' '
    << lee.money << endl;
  cout << han.sharedMoney << ' ' 
    << lee.sharedMoney << endl;
}

<실행 결과>
100 350  //han과 lee의 money는 각각 100, 350
400 400  //han과 lee의 sharedMoney는 공통 400
```
</div>
</details>

___

<details>
<summary>4. static 멤버 사용 : 클래스명과 범위지정 연산자(::)로 접근</summary>
<div markdown="1">       

### 사용 방법
* static 멤버는 클래스 당 하나만 존재하므로 **클래스의 이름으로도 접근**할 수 있음
* 클래스 이름과 static 멤버 사이에 **범위 지정 연산자(::)** 를 사용하여 static멤버를 접근함
* 클래스명으로 static 멤버를 접근하는 예시
  >Person::sharedMoney = 200;  //클래스명으로 접근
  >
  >Person::addShared(200);  //클래스명으로 접근
* 객체 이름으로 접근하는 예시
  >han.sharedMoney = 200;     < - >     Person::sharedMoney = 200;  //동일한 표현
  >
  >lee.addShared(200);             < - >     Person::addShared(200);  //동일한 표현
* non-static 멤버는 객체의 이름이나 포인터로만 접근 가능함
  >lee.money = 100;
  >
  >Person *p = &lee;
  >
  >p→addMoney(200);

### 코드 사례
* static 멤버들은 객체가 생기기 전부터 사용 가능함
* 클래스명과 범위지정 연산자(::)를 이용하면, 객체를 언급하지 않고도 static 멤버를 활용할 수 있음
* 클래스명으로 static 멤버를 접근하는 예시 사례
```C++
#include <iostream>
using namespace std;

class Person {
public:
  int money;  //개인 소유의 돈
  void addMoney(int money) {
    this->money += money;
  }

  static int sharedMoney;  //공금
  static void addShared(int n) {
    sharedMoney += n;
  }
};

//static 변수 생성, 전역 공간에 생성
int Person::sharedMoney=10;  //10으로 초기화

//main() 함수
int main() {
  Person::addShared(50);  //static 멤버 접근, 공금=60
  cout << Person::sharedMoney << endl;

  Person han;
  han.money = 100;
  han.sharedMoney = 200;  //static 멤버 접근, 공금=200
  Person::sharedMoney = 300; //static 멤버 접근, 공금=300
  Person::addShared(100); //static 멤버 접근, 공금=400

  cout << han.money << ' '
    << Person::sharedMoney << endl;

<실행 결과>
60
100 400 
```

</div>
</details>

___

<details>
<summary>5. static의 활용</summary>
<div markdown="1">       

### 전역 변수나 전역 함수를 클래스에 캡슐화
* 전역 함수나 전역 변수를 없애고 모든 함수나 변수를 클래스 안에 선언하도록 함
* C++은 전역 변수와 전역 함수를 사용하는 C언어와의 호환성 때문에 100% 캡슐화하지 못함
* **전역 변수**와 **전역 함수**를 선언하지 말고, 클래스에 **static 멤버**로 선언하여 캡슐화하기!
* 캡슐화 되지 못한 좋지 않은 코드 vs 캡슐화 된 좋은 코드 비교
```C++
<좋지 못한 코드>
int abs(int a) { return a > 0 ? a : -a; }
int max(int a, int b) { return (a>b) ? a : b; }
int min(int a, int b) { return (a>b) ? b : a; }

int main() {
  cout << abs(-5) << endl;
  cout << max(10, 8) << endl;
  cout << min(-3, -8) << endl;
}

---------------------------------------------------------------------------------

<좋은 코드>
#include <iostream>
using namespace std;

class Math {
public:
  static int abs(int a) { return a > 0 ? a : -a; }
  static int max(int a, int b) { return (a>b) ? a : b; }
  static int min(int a, int b) { return (a>b) ? b : a; }
};

int main() {
  cout << Math::abs(-5) << endl;
  cout << Math::max(10, 8) << endl;
  cout << Math::min(-3, -8) << endl;

<실행 결과>
5
10
-8
```

### 객체 사이에 공유 변수를 만들고자 할 때
* static 멤버는 클래스의 모든 인스턴스(객체)들이 공유하는 변수나 함수를 만들고자 할 때 사용됨
* static 멤버를 공유의 목적으로 활용하는 사례
```C++
#include <iostream>
using namespace std;

class Circle {
private:
  static int numOfCircles;  //생성된 원의 개수 기억
  int radius;
public:
  Circle(int r=1);
  ~Circle() { numOfCircles--; } //생성된 원의 개수 감소
  double getArea() { return 3.14*radius*radius; }
  static int getNumOfCircles() { return numOfCircles; }
};

Circle::Circle(int r) { 
  radius = r;
  numOfCircles++;  //생성된 원의 개수 증가
}

int Circle::numOfCircles = 0;  //0으로 초기화

int main() { 
  Circle *p = new Circle[10];  //10개의 생성자 실행
  cout << "생존하고 있는 원의 개수 = " << Circle::getNumOfCircles() << endl;

  delete [] p;  //10개의 소멸자 실행
  cout << "생존하고 있는 원의 개수 = " << Circle::getNumOfCircles() << endl;

  Circle a;  //생성자 실행
  cout << "생존하고 있는 원의 개수 = " << Circle::getNumOfCircles() << endl;

  Circle b;  //생성자 실행
  cout << "생존하고 있는 원의 개수 = " << Circle::getNumOfCircles() << endl;

<실행 결과>
생존하고 있는 원의 개수 = 10
생존하고 있는 원의 개수 = 0
생존하고 있는 원의 개수 = 1
생존하고 있는 원의 개수 = 2
```
</div>
</details>

___

<details>
<summary>6. static 멤버 함수의 특징</summary>
<div markdown="1">       

### static 멤버 함수는 오직 static 멤버들만 접근
* static 멤버 함수는 오직 static 멤버 변수에 접근하거나 static 멤버 함수만 호출할 수 있음
* static 멤버 함수는 객체가 생성되지 않은 어떤 시점에서도 호출될 수 있고, 클래스 이름으로 직접 호출될 수 있기 때문에, static 멤버 함수에서 non-static 멤버에 접근하는 것은 허용되지 않음
* 반대로 non-static 멤버 함수는 static 멤버를 접근하는데 전혀 제약이 없음
* 비교 코드
```C++
<static 멤버 함수가 non-static 멤버에 접근하도록 작성된 코드>
class PersonError {
  int money;
public:
  static int getMoney() { return money; } //컴파일 오류. non-static 멤버에 접근 불가

  void setMoney(int money) {
    this->money = money;  //정상 코드
  }
};

int main() {
  int n = PersonError::getMoney();

  PersonError errorKim;
  errorKim.setMoney(100);
}

--------------------------------------------------------------------------

<non-static 멤버 함수가 static 멤버에 접근하도록 작성된 코드>
class Person { 
public:
  double money;  //개인 소유의 돈
  static int sharedMoney;  //공금
  ....
  int total() { //non-static 함수는 non-static이나 static 멤버에 모두 접근 가능
    return money + sharedMoney;
  }
};
```

### static 멤버 함수는 this를 사용할 수 없다
* static 멤버 함수는 객체가 생기기 전부터 호출 가능하므로, static 멤버 함수에서 this를 사요할 수 없도록 제약함
* Person 클래스의 addShared() 함수를 다음과 같이 수정하면 컴파일 오류가 발생함
```C++
class Person {
public:
  double money;  //개인 소유의 돈
  static int sharedMoney;  //공금
  ....
  static void addShared(int n) { //static 함수에서 this 사용 불가
    this->sharedMoney += n;  //this를 사용하므로 컴파일 오류
    //sharedMoney += n; 으로 하면 정삼 컴파일
  }
};
```
</div>
</details>
