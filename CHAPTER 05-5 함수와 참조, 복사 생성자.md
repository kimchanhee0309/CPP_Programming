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
  
* 깊은 복사 생성자를 가진 정상적인 Person 클래스
```C++
#include <iostream>
#include <cstring>
using namespace std;

class Person {  //Person 클래스 선언
  char* name;
  int id;
public:
  Person(int id, const char* name); //생성자
  Person(const Person& person); //복사 생성자
  ~Person(); //소멸자
  void changeName(const char *name);
  void show() { cout << id << ',' << name << endl; }
};

Person::Person(int id, const char* name) { //생성자
  this->id = id;
  int len = strlen(name);  //name의 문자 개수
  this->name = new char [len+1];  //name 문자열 공간 할당
  strcpy(this->name, name);  //name에 문자열 복사
}

Person::Person(const Person& Person) {  //복사 생성자
  this->id = Person.id;  //id 값 복사
  int len = strlen(person.name);  //name의 문자 개수
  this->name = new char [len+1];  //name을 위한 공간 할당
  strcpy(this->name, person.name);  //name의 문자열 복사
  cout << "복사 생성자 실행. 원본 객체의 이름" << this->name << endl;
}

Person::~Person() { //소멸자
  if(name)  //만일 name에 동적 할당된 배열이 있으면
    delete [] name;  //동적 할당 메모리 소멸
}

void Person::changeName(const char* name) { //이름 변경 
  if(strlen(name) > strlen(this->name))
    return;  //현재 name에 할당된 메모리보다 긴 이름으로 바꿀 수 없다.
  strcpy(this->name, name);
}

int main() {
  Person father(1, "Kitae");            //(1) father 객체 생성
  Person daughter(father);              //(2) daughter 객체 복사 생성. 복사 생성자 호출

  cout << "daughter 객체 생성 직후 ----" << endl;
  father.show();                        //(3) father 객체 출력
  daughter.show();                      //(3) daughter 객체 출력

  daughter.changeName("Grace");         //(4) daughter의 이름을 "Grace"로 변경
  cout << "daughter 이름을 Grace로 변경한 후 ----" << endl;
  father.show();                        //(5) father 객체 출력
  daughter.show();                      //(5) daughter 객체 출력

  return 0;                             //(6), (7) daughter, father 객체 소멸
}
```
* main() 함수 실행
  * (1) father 객체 생성
    >Person father(1, “Kitae”);
      * father 객체가 생성되고, id에 1이 설정되며, name 포인터에 char[] 배열이 동적 할당되고 “Kitae”로 초기화됨
  * (2) father를 복사한 daughter 객체 생성
    > Person daughter(father);  //복사 생성자  Person(Person&) 호출
      * 이 코드로 daughter 객체가 생성될 때 다음 복사 생성자가 호출됨
      * daughter의 name에 **메모리가 따로 동적 할당**되고, father의 name 문자열이 복사되어 같은 문자열 “Kitae”로 초기화됨
        >Person::Person(const Person& person) { //복사 생성자
        >
        >this->id = person.id; //id 값 복사
        >
        >int len = strlen(person.name);  //name의 문자 개수
        >
        >this->name = new char [len+1];  //name을 위한 공간 할당
        >
        >strcpy(this->name, person.name); //name의 문자열 복사
        >
        >cout << "복사 생성자 실행. 원본 객체의 이름 " << this->name << endl;
        
  * (3) father와 daughter 객체 출력
    >father.show();
    >
    >daughter.show();
    >
    ><실행 결과>
    >
    >1,Kitae
    >
    >1,Kitae
      * father와 daughter는 각각 초기화된 name 문자열을 출력함
        
  * (4) daughter 객체의 이름 변경
    >daughter.changeName(”Grace”);
      * 이 코드에 의해 daughter의 name이 “Grace”로 변경되지만, father의 name은 “Kitae”로 그대로 남아 있음
        
  * (5) father와 daughter 객체 출력
    >father.show();
    >
    >daughter.show();
    >
    ><실행 결과>
    >
    >1,Kitae
    >
    >1.Grace
      * daughter의 이름 변경이 잘 이루어졌는지 확인하기 위한 코드
      * 실행 결과, daughter의 이름이 “Grace”로 변경됨
        
  * (6), (7) main() 함수 종료
    >Person::~Person() { //소멸자
    >
    >if(name)  //만일 name에 동적 할당된 배열이 있으면
    >
    >delete [] name;  //할당받은 메모리 반환
    >
    >}
      * return 0; 문이 실행되면 daughter 객체가 먼저 소멸됨
      * 이때 다음 소멸자가 실행되고 daughter의 name에 할당된 메모리를 함께 반환함
      * daughter의 소멸 뒤 father 객체가 소멸됨
      * father 객체의 소멸자 역시 **자신의 name에 할당된 메모리**를 힙에 반환함
</div>
</details>

___

<details>
<summary>5. 묵시적 복사 생성</summary>
<div markdown="1">        

* `묵시적 복사 생성`은 컴파일러가 복사 생성자를 자동으로 호출하는 경우를 뜻함
* 묵시적 복사 생성의 3가지 경우
  * **객체로 초기화하여 객체가 생성될 때**
  * **'값에 의한 호출'로 객체가 전달될 때**
  * **함수가 객체를 리턴할 때**
    * return 문은 **리턴 객체의 복사본을 생성**하여 호출한 곳으로 전달함
    * g()가 mother 객체를 리턴할 때, mother 객체의 복사본을 만들어 넘겨줌
    * 복사본이 만들어질 때 복사 생성자가 호출됨
```C++
Person g() {
  Person mother(2, "Jang");
  return mother;  //mother의 복사본을 생성하여 복사본 리턴. 사본이 만들어질 때 복사 생성자 호출
}
g();
```
* 묵시적 복사 생성에 의해 복사 생성자가 자동 호출되는 예시 코드
```C++
void f(Person person) {  //'값에 의한 호출'로 객체가 전달될 때, person 객체의 복사 생성자 호출
  person.changeName("dummy");
}

Person g() {
  Person mother(2, "Jane");
  return mother; //함수에서 객체를 리턴할 때, mother 객체의 복사본 생성. 복사본의 복사 생성자 호출
}

int main() {
  Person father(1, "Kitae");
  Person son = father;  //복사 생성자 호출, 객체로 초기화하여 객체가 생성될 때, son 객체의 복사 생성자 호출
  f(father);  //복사 생성자 호출
  g();  //복사 생성자 호출
}

<실행 결과>
복사 생성자 실행 Kitae 
복사 생성자 실행 Kitae
복사 생성자 실행 Jane
```
</div>
</details>
