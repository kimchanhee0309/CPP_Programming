# 복사 생성자

<details>
<summary>1. 얕은 복사(shallow copy)와 깊은 복사(deep copy)</summary>
<div markdown="1">       

#### 차이점
* 얕은 복사는 **충돌**이 생길 수 있는 문제점이 있는 반면, 깊은 복사는 원본이 소유한 모든 것을 복사하기 때문에 충돌이 발생하지 않음

#### 객체의 얕은 복사와 깊은 복사
* 예시를 통해 보는 차이점
```C++
class Person {
  int id;
  char *name;
};
```
* 원본 Person 객체의 id는 1이고 name 포인터는 “Kitae” 문자열을 가진 **동적 할당 배열**이라고 가정
  * 얕은 복사가 이루어지면 원본 객체의 id와 name 멤버는 현재 상태 그대로 사본 객체에 복사되므로, 사본의 name은 **원본의 name 메모리를 공유**하게 됨
  * 깊은 복사는 원본의 name 포인터가 가리키는 **메모리까지 복사**하여 원본과 사본의 name은 **별개의 메모리**를 가리키므로, **완전한 복사**가 이루어짐

#### 객체의 얕은 복사 문제점
* 원본과 사본이 각각 name 포인터로 문자열 배열을 **공유하고 있기 때문에** 사본 객체에서 name 문자열을 변경하면 **원본 객체의 name 문자열이 변경되는 문제**가 발생함
</div>
</details>

___

<details>
<summary>2. 복사 생성 및 복사 생성자</summary>
<div markdown="1">       

#### 복사 생성자(copy constructor) 선언
* `복사 생성`은 객체가 생성될 때 원본 객체를 복사하여 생성되는 경우임
* C++에는 복사 생성 시에만 실생되는 특별한 `복사 생성자(copy constructor)` 가 있음
* 선언 방법
```C++
class ClassName {
  ClassName(const ClassName& c);  //복사 생성자
};
```
* 복사 생성자의 **매개 변수는 오직 하나**이며, **자기 클래스에 대한 참조**로 선언됨
* 복사 생성자는 클래스에 오직 **한 개만 선언 가능함**
* 복사 생성자 선언 예시
```C++
class Circle {
  .....
  Circle(const Circle& c); //복사 생성자 선언
  .....
};

Circle::Circle(const Circle& c) { //복사 생성자 구현
  .....
}
```

#### 복사 생성자 실행
* 복사 생성의 사례(src 객체를 복사하여 dest를 생성하는 코드)
```c++
Circle src(30);  //보통 생성자 호출
Circle dest(src); //src 객체를 복사하여 dest 객체 생성. 복사 생성자 Circle(Circle& c) 호출
```
* dest 객체가 생성될 때 보통 생성자 대신, 복사 생성자 Circle(Circle& c)을 호출하도록 컴파일 함
  * Circle(Circle& c)이 호출될 때, src 객체가 **참조 매개 변수 c**로 전달됨
```C++
Circle::Circle(const Circle& c) {
  this->radius = c.radius;
}
```
* 예시 코드(Circle 클래스의 복사 생성자와 객체 복사)
```C++
#include <iostream>
using namespace std;

class Circle {
private:
  int radius;
public:
  Circle(const Circle& c);  //복사 생성자 선언
  Circle() { radius = 1; } 
  Circle(int radius) { this->radius = radius; }
  double getArea() { return 3.14*radius*radius; }
};

Circle::Circle(const Circle& c) { //복사 생성자 구현
  this->radius = c.radius;
  cout << "복사 생성자 실행 radius = " << radius << endl;
}

int main() {
  Circle src(30); //src 객체의 보통 생성자 호출
  Circle dest(src); //dest 객체의 복사 생성자 호출
  
  cout << "원본의 면적 = " << src.getArea() << endl;
  cout << "사본의 면적 = " << dest.getArea() << endl;
}

<실행 결과>
복사 생성자 실행 radius = 30
원본의 면적 = 2826
사본의 면적 2826

```

#### 디폴트 복사 생성자
* 클래스가 복사 생성자를 가지고 있지 않은 경우, 컴파일러는 `디폴트 복사 생성자(default copy constructor)` 를 묵시적으로 삽입하고 이 생성자를 호출하도록 컴파일 함
* 컴파일러가 삽입하는 **디폴트 복사 생성자 코드**는 **얕은 복사를 실행**하도록 만들어진 코드임
* 컴파일러가 삽입한 복사 생성자는 원본 객체의 모든 멤버를 일대일로 사본(this)에 복사하도록 구성됨
</div>
</details>

___

<details>
<summary>3. 얕은 복사 생성자의 문제점</summary>
<div markdown="1">       

* 포인터 타입의 멤버 변수가 없는 클래스의 경우, 얕은 복사는 전혀 문제가 없음
  * 모든 멤버 변수를 일대일로 복사해도 **공유의 문제**가 발생하지 않기 때문에
* 포인터 멤버 변수를 가지고 있는 경우,
  * 원본 객체의 포인터 멤버 변수가 사본 객체의 포인터 멤버 변수에 복사되면, 같은 메모리를 가리키게 되어 심각한 문제를 야기함

#### 예시 코드(얕은 복사 생성자를 사용하여 프로그램이 비정상 종료되는 경우)
```C++
#include <iostream>
#include <cstring>
using namespace std;

class Person { //Person 클래스 선언
  char* name;
  int id;
public:
  Person(int id, const char* name); //생성자
  ~Person(); //소멸자
  void changeName(const char *name);
  void show() { cout << id << ',' << name << endl; }
};

Person::Person(int id, const char* name { //생성자
  this->id = id;
  int len = strlen(name); //name의 문자 개수
  this->name = new char [len+1]; //name 문자열 공간 할당
  strcpy(this->name, name); //name에 문자열 복사
}

Person::~Person() { //소멸자
  if(name) //만일 name에 동적 할당된 배열이 있으면
    delete [] name; //동적 할당 메모리 소멸
}

void Person::changeName(const char* name) { //이름 변경
  if(strlen(name) > strlen(this->name))
    return; //현재 name에 할당된 메모리보다 긴 이름으로 바꿀 수 없다.
  strcpy(this->name, name);
}

int main() {
  Person father(1, "Kitae");  //(1) father 객체 생성
  Person daughter(father);    //(2) daughter 객체 복사 생성. 복사 생성자 호출

  cout << "daughter 객체 생성 직후 ----" << endl;

  fathre.show();              //(3) father 객체 출력
  daughter.show();            //(3) daughter 객체 출력

  daughter.changeName("Grace");  //(4) daughter의 이름을 "Grace"로 변경
  cout << "daughter 이름을 Grace로 변경한 후 ----" << endl;
  father.show();                 //(5) father 객체 출력
  daughter.show();               //(5) daughter 객체 룰력

  return 0;                      //(6), (7) daughter, father 객체 소멸
}
```
* **디폴트 복사 생성자 자동 삽입**
  * 디폴트 복사 생성자를 자동으로 삽입함
  * 참조 매개 변수 p로 원본 객체의 id와 name 포인터를 사본 객체(this)에 복사함
 > Person::Person(const Person& p) { //컴파일러에 의해 삽입된 디폴트 복사 생성자
 >
 >  this→id = p.id;
 >
 >  this→name = p.name;
  
* **main() 함수 실행**
  * **(1) father 객체 생성**
  * **(2) father를 복사한 daughter 객체 생성**
  * **(3) father와 daughter 객체 출력**
  * **(4) daughter 객체의 이름 변경**
  * **(5) father와 daughter 객체 출력**
  * **(6), (7) main() 함수 종료**
</div>
</details>

___

<details>
<summary>4. 사용자 복사 생성자 작성</summary>
<div markdown="1">       

### 참조 개념 - 메뚜기는 유재석의 별명
* `참조 변수(reference variable)` 는 **이미 선언된 변수에 대한 별명(alias)** 을 뜻함
* 참조 변수를 선언하기 위해서는 `& 기호(참조자)`를 사용함
  
### 참조 변수
#### 참조 변수 선언
* `참조 변수` : 이미 선언된 변수(원본 변수)에 대한 별명
  * `참조자(&)`를 이용하여 선언하며, 선언 시 반드시 **원본 변수로 초기화**하여야 함
  * 참조 변수를 선언하는 예시 코드
```C++
int n=2;
int &refn = n;  //참조 변수 refn 선언. refn은 n에 대한 별명, refn과 n은 동일한 변수

Circle circle;
Circle &refc = circle;  //참조 변수 refc 선언. refc는 circle에 대한 별명, 
                        //refc와 circle은 동일한 변수
```
  * refn은 이미 선언된 변수 n에 대한 별명, refc는 circle 객체에 대한 별명
  * refn, refc는 따로 변수 공간을 가지지 않고, 각각 n과 circle을 공유함
  * 참조 변수가 선언되면 **참조 변수 이름만 생성**되며, 별도의 공간이 할당되지 않음
  * 대신, 참조 변수는 초기화로 지정된 **원본 변수의 공간을 공유함**
    
#### 참조 변수 사용
* 사용 방법은 보통 변수와 동일, 참조 변수에 대한 사용은 **원본 변수의 사용**임!
* 예시
  >refn = 3;
  >
  >n = 5;  //n=5, refn=5가 됨
  >
  >refn++; //n=6, refn=6이 됨
* 참조 변수는 포인터가 아니므로, 다음처럼 사용해서는 안됨
  >refc→setRadius(30);  //컴파일 오류,
  >
  >refc.setRadius(30)으로 해야 함
* 참조 변수에 대한 포인터 만드는 법
  >int *p = &refn;  //p는 refn의 주소를 가짐. p는 n의 주소
  >
  >*p = 20;  //n=20, refn=20
  
#### 참조 변수 선언 시 주의사항
* 초기화가 없다면 컴파일 오류가 발생한다.
  >int n=2;
  >
  >int &refn2;  //컴파일 오류. refn2가 어떤 변수에 대한 참조인지 초기화되지 않음
* 참조자 &의 위치에 무관하다.
  >int &refn = n;
  >
  >int & refn = n;
  >
  >int& refn = n;
* 참조자 &의 사용에 유의해야 한다.
  >& int refn = n;  //컴파일 오류
  >
  >int refn & = n;  //컴파일 오류
* 참조 변수의 배열을 만들 수 없다.
  >char &n[10];  //컴파일 오류. 참조의 배열을 만들 수 없다.
* 참조 변수에 대한 참조 선언이 가능하다.
  >int &r = refn;  //참조 변수 refn에 대한 참조 변수 r 선언 가능
* r과 refn 모두 n의 공간을 공유하며 구분 없이 사용 가능함
  
#### 예시 코드
* 기본 타입 변수에 대한 참조
```C++
#include <iostream>
using namespace std;

int main() {
  cout << "i" << '\t' << "n" << '\t' << "refn" << endl;
  int i = 1;
  int n = 2;
  int &refn = n;  //참조 변수 refn 선언. refn은 n에 대한 별명
  n = 4;
  refn++;  //refn=5, n=5
  cout << i << '\t' << n << '\t' << refn << endl;

  refn = i;  //refn=1, n=1
  refn++;  //refn=2, n=2
  cout << i << '\t' << n << '\t' << refn << endl;

  int *p = &refn;  //p는 refn의 주소를 가짐. p는 n의 주소
  *p = 20; //refn=20, n=20
  cout << i << '\t' << n << '\t' << refn << endl;
}

<실행 결과>
i   n    refn
i   5    5
i   2    2
i   20   20
```
* 객체에 대한 참조
```C++
#include <iostream>
using namespace std;

class Circle{
  int radius;
public:
  Circle() { radius = 1; }
  Circle(int radius) { this->radius = radius; }
  void setRadius(int radius) { this->radius = radius; }
  double getArea() { return 3.14*radius*radius; }
};

int main() {
  Circle circle;
  Circle &refc = circle;
  refc.setRadius(10);
  cout << refc.getArea() << " " << circle.getArea(); //두 호출은 동일 객체에 대한 호출
}

<실행 결과>
314 314
```

### 참조에 의한 호출. call by reference
* 함수의 매개 변수를 참조 타입으로 선언하여 매개 변수가 함수를 호출하는 쪽의 **실인자를 참조**하여 **실인자와 공간을 공유**하도록 하는 인자 전달 방식
* 참조 타입으로 선언된 함수의 매개 변수를 `참조 매개 변수(reference parameter)`라고 부름
* 예시 코드
```C++
#include <iostream>
using namespace std;

void swap(int &a, int &b) { //참조 매개 변수 a, b
  int tmp;

  tmp = a;
  a = b;     //참조 매개변수를 보통 변수처럼 사용
  b = tmp;
}

int main() {
  int m=2, n=9;
  swap(m, n);
  cout << m << ' ' << n;
}

<실행 결과>
9 2
```
* swap() 함수가 호출되면, 참조 매개 변수 a,b는 실인자 m,n을 참조하도록 초기화 되며, 함수 내에서는 보통 변수처럼 사용함
* 변수 m,n은 main() 스택에 생성되지만, 참조 매개 변수 a,b는 이름만 존재하며 swap()의 스택에 공간을 할당받지 않음
* 참조 매개 변수가 필요한 사례(참조 매개 변수로 평균 리턴하기)
```C++
#include <iostream>
using namespace std;

bool average(int a[], int size, int& avg) {
  if(size <= 0)
    return false;
  int sum = 0;
  for(int i=0; i<size; i++)
    sum += a[i];
  avg = sum/size;  //참조 매개 변수 avg에 평균값 전달
  return true; 
}

int main() {
  int x[] = {0,1,2,3,4,5};
  int avg;
  if(average(x, 6, avg)) cout << "평균은 " << avg << endl;
  else cout << "매개 변수 오류" << endl;

  if(average(x, -2, avg)) cout << "평균은 " << avg << endl;
  else cout << "매개 변수 오류 " << endl;
}

<실행 결과>
평균은 2
매개 변수 오류
```
* 참조에 의한 호출의 장점
  * 간단히 변수를 넘겨주기만 하면 되고,
  * 함수 내에서도 참조 매개 변수를 보통 변수처럼 사용하기 때문에 작성하기 쉽고 보기 좋은 코드가 됨
    
### 참조에 의한 호출로 객체 전달
* `값에 의한 호출`과 `참조에 의한 호출` 비교
  * 값에 의한 호출로 객체를 매개 변수에 전달할 시 유의 사항
    * 함수 내에서 매개 변수 객체를 변경하여도, **원본 객체를 변경시키지 않음**
    * 매개 변수 객체의 **생성자가 실행되지 않고 소멸자만 실행**되는 비대칭 구조로 작동함
  * 참조에 의한 호출로 매개 변수에 전달할 시 유의 사항
    * 참조 매개 변수로 이루어진 모든 연산은 **원본 객체에 대한 연산이 됨**
    * 참조 매개 변수는 이름만 생성되므로, **생성자와 소멸자는 아예 실행되지 않음**
* 예시 코드
  * 참조에 의한 호출로 Circle 객체의 참조 전달 예시 코드
```C++
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
  void SetRadius(int radius) { this->radius = radius; }
};

Circle::Circle() {
  radius = 1;
  cout << "생성자 실행 radius = " << radius << endl;
}

Circle::Circle(int radius) {
  this->radius = radius;
  cout << "생성자 실행 raidus = " << radius << endl;
}

Circle::~Circle() {
  cout << "소멸자 실행 radius = " << radius << endl;
}

void increase(Circle &c) { //c는 참조 매개 변수
  int r = c.gerRadius();
  c.setRadius(r+1);  //c가 참조하는 원본 객체의 반지름 1 증가
}

int main() {
  Circle waffle(30);
  increase(waffle);
  cout << waffle.getRadius() << endl;
}

<실행 결과>
생성자 실행 radius = 30 //waffle 객체 생성
31
소멸자 실행 radius = 31 //waffle 객체 소멸
```
  * 참조 매개 변수를 가진 함수 만들기 연습 코드
```C++
#include <iostream>
using namespace std;

class Circle {
  int radius;
public:
  Circle() { radius = 1; }
  Circle(int radius) { this->radius = radius; }
  void setRadius(int radius) { this->radius = radius; }
  double getArea() { return 3.14*radius*radius; }
};

void readRadius(Circle &c) {
  int r;
  cout << "정수 값으로 반지름을 입력하세요>>";
  cin >> r; //반지름 값 입력
  c.setRadius(r); //반지름 설정
}

int main() {
  Circle donut;
  readRadius(donut);
  cout << "donut의 면적 = " << donut.getArea() << endl;
}

<실행 결과>
정수 값으로 반지름을 입력하세요>>3
donut의 면적 = 28.26
```

### 참조 리턴
* C++에서는 함수가 참조를 리턴할 수 있음
  * `참조 리턴`이란 변수 등과 같이 현존하는 공간에 대한 참조의 리턴을 말함
* 값을 리턴하는 함수 get()과 참조를 리턴하는 함수 find() 비교
```C++
<문자 값을 리턴하는 get()>
char c = 'a';

char get() {  //char 값 리턴
  return c;   //변수 c의 값 리턴
}

char a = get(); //a = 'a'가 됨

get() = 'b'; //컴파일 오류


<char 타입의 참조를 리턴하는 find()>
char c = 'a';

char& find() { //char 타입의 공간에 대한 참조 리턴
  return c;    //변수 c에 대한 참조 리턴
}

char a = find();  //a = 'a'가 됨

char &ref = find();  //ref는 c에 대한 참조
ref = 'M'; //c='M'

find() = 'b'; //c = 'b'가 됨
```
* 참조 리턴 예시 코드
```C++
#include <iostream>
using namespace std;

//배열 5의 index 원소 공간에 대한 참조를 리턴하는 함수
char& find(char s[], int index) {
  return s[index]; //s[index] 공간의 참조 리턴
}

int main() {
  char name[] = "Mike";
  cout << name << endl;

  find(name, 0) = 'S'; //name[0] = 'S'로 변경, find()가 리턴한 위치에 문자 'S' 저장
  cout << name << endl;

  char& ref = find(name, 2); //ref는 name[2]에 대한 참조
  ref = 't'; //name = "Site"
  cout << name << endl;
}

<실행 결과>
Mike
Sike
Site
```
</div>
</details>

___

<details>
<summary>5. 묵시적 복사 생성</summary>
<div markdown="1">       
  
### 얕은 복사(shallow copy)와 깊은 복사(deep copy)
### 복사 생성 및 복사 생성자
### 얕은 복사 생성자의 문제점
### 사용자 복사 생성자 작성
### 묵시적 복사 생성
  
</div>
</details>
