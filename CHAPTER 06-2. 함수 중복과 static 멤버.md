# 디폴트 매개 변수

<details>
<summary>1. 디폴트 매개 변수 선언</summary>
<div markdown="1">       

### 디폴트 매개 변수(default parameter)란?
* 함수가 호출될 때 매개 변수에 값이 넘어오지 않을 때, 미리 정해진 디폴트 값을 받도록 선언된 매개 변수를 말함. 기본 매개 변수라고도 불림
* **'매개 변수 = 디폴트 값'** 형태로 선언됨
  >void star(int a=5); //a의 디폴트 값 5
  * 디폴트 매개 변수를 가진 함수를 호출할 때는 디폴트 매개 변수에 값을 넘겨주어도 되고 생략해도 됨
  * 생략하는 경우 자동으로 디폴트 값이 매개 변수에 전달됨
  >start(); //매개 변수 a에 디폴트 값 5 자동 전달. star(t);와 동일
  >
  >star(10); //매개 변수 a에 10 전달

### 디폴트 매개 변수 사례
* 보통 매개 변수 id와 디폴트 매개 변수 text를 가진 함수 msg()
  >void msg(int id, string text=”Hello”);  //text의 디폴트 값은 “Hello”
* msg()는 다음과 같이 호출 가능함
  >msg(10);  //id에 10, text에 “Hello” 전달. msg(10, “Hello”);로 처리됨
  >
  >msg(20, “Good Morning”);  //id에 20, text에 “Good Morning” 전달
  * 두 번째 매개 변수가 생략되면 자동으로 “Hello”가 전달됨
  * 디폴트 매개 변수에 값을 전달하는 것은 선택 사항이지만, 보통 매개 변수에는 반드시 값을 전달하여야 함
    >msg();  //컴파일 오류. 매개 변수 id에 값이 전달되지 않았음
    >
    >msg(”Hello”);  //컴파일 오류. 매개 변수 id에 값이 전달되지 않았음
</div>
</details>

___

<details>
<summary>2. 디폴트 매개 변수에 관한 제약 조건</summary>
<div markdown="1">       

* 디폴트 매개 변수는 모두 **끝 쪽에 몰려 선언**되어야 함
* 디폴트 매개 변수는 보통 **매개 변수 앞에 선언될 수 없음**
  >void calc(int a, int b=5, int c, int d=0);  //컴파일 오류
  >
  >void sum(int a=0, int b, int c);  //컴파일 오류
  * 다음과 같이 수정하면 성공적으로 컴파일이 됨
    >void calc(int a, int b=5, int c=0, int d=0);  //컴파일 성공
</div>
</details>

___

<details>
<summary>3. 매개 변수에 값을 정하는 규칙</summary>
<div markdown="1">       

* 함수 호출문에 나열된 실인자 값들을 **앞에서부터 순서대로** 함수의 매개 변수에 전달하고,
* **나머지는 디폴트 값으로 전달**함

### 디폴트 매개 변수만 가진 함수
* 2개의 디폴트 매개 변수로 구성된 함수 예시
  >void square(int width=1, int height=1);

* square() 함수는 다음과 같이 다양하게 호출할 수 있음
  >square();  //square(1, 1);
  >
  >square(5);  //square(5, 1);
  >
  >square(3, 8);  //square(3, 8);
  
### 디폴트 매개 변수와 보통 매개 변수를 가진 경우
* 디폴트 매개 변수를 여러 개 가진 함수 g() 예시
  > void g(int a, int b=0, int c=0, int d=0);
  
* 함수 g()는 다음과 같이 다양하게 호출할 수 있음
  >g(10);  //g(10, 0, 0, 0);
  >
  >g(10, 5);  //g(10, 5, 0, 0);
  >
  >g(10, 5, 20);  //g(10, 5, 20, 0);
  >
  >g(10, 5, 20, 30);  //g(10, 5, 20, 30);

### 예시 코드
```C++
<디폴트 매개 변수를 가진 함수 선언 및 호출>
#include <iostream>
#include <string>
using namespace std;

//원형 선언(디폴트 매개 변수 선언)
void star(int a=5);
void msg(int id, string text="");

//함수 구현
void star(int a) {
  for(int i=0; i<a; i++) cout << '*';
  cout << endl;
}

void msg(int id, string text) {
  cout << id << ' ' << text << endl;
}

int main() {
  star();  //star(5);
  star(10);

  msg(10);  //msg(10, "");
  msg(10, "Hello");

<실행 결과>
*****
**********
10
10 Hello
```
<디폴트 매개 변수를 가진 함수 만들기 연습>
#include <iostream>
using namespace std;

//원형 선언
void f(char c= ' ', int line=1);

//함수 구현
void f(char c, int line) {
  for(int i=0; i<line; i++) {
    for(int j=0; j<10; j++)
      cout << c;
    cout << endl;
  }
}

int main() {
  f();  //한 줄에 빈칸을 10개 출력한다.
  f('%'); //한 줄에 '%'를 10개 출력한다.
  f('@', 5);  //다섯 줄에 '@'를 10개 출력한다.
}

<실행 결과>

%%%%%%%%%%
@@@@@@@@@@
@@@@@@@@@@
@@@@@@@@@@
@@@@@@@@@@
@@@@@@@@@@
```C++

```
</div>
</details>

___

<details>
<summary>4. 함수 중복 간소화</summary>
<div markdown="1">       

* 디폴트 매개 변수의 최대 장점 : **함수 중복을 간소화**할 수 있음
* 예시 사례
```C++
class Circle {
  .....
public:
  Circle() { radius = 1; }
  Circle(int r) { radius = r; }
  .....
};

이 코드를 간소화 한다면?
class Circle {
  .....
public:
  Circle(int r = 1) { radius = r; }
  .....
};
```

* 디폴트 매개 변수를 가진 함수는 같은 이름의 중복 함수들과 함께 선언될 수는 없음
```C++
class Circle {
  .....
public:
  Circle() { radius = 1; }
  Circle(int r) { radius = r; }
  Circle(int r=1) { radius = r; }  //중복된 함수와 함께 사용 불가
  .....
};
```

* 예시 코드
```C++
<디폴트 매개 변수를 이용하여 중복 함수 간소화 연습>
Q. 다음 두 개의 중복 함수를 디폴트 매개 변수를 가진 하나의 함수로 작성하라.

void fillLine() { //25개의 '*' 문자를 한 라인에 출력
  for(int i=0; i<25; i++)
    cout << '*';
  cout << endl;
}

void fillLine(int n, char c) { //n개의 c 문자를 한 라인에 출력
  for(int i=0; i<n; i++)
    cout << c;
  cout << endl;
}

--------------------------------------수정 후-------------------------------------------

#include <iostream>
using namespace std;

void fillLine(int n=25, char c='*') { //n개의 c 문자를 한 라인에 출력
  for(int i=0; i<n; i++)
    cout << c;
  cout << endl;
}

int main() {
  fillLine();  //25개의 '*'를 한 라인에 출력
  fillLine(10, '%');  //10개의 '%'를 한 라인에 출력

<실행 결과>
*************************
%%%%%%%%%%
```

```C++
<중복된 생성자들을 디폴트 매개 변수를 이용한 간소화 연습>
Q. 다음 클래스에 중복된 생성자를 디폴트 매개 변수를 가진 하나의 생성자로 작성하라.

class MyVector{
  int *p;
  int size;
public:
  MyVector() { 
    p = new int [100];
    size = 100;
  }
  MyVector(int n) {
    p = new int [n];
    size = n;
  }
  ~MyVector() { delete [] p; }
};

--------------------------------------수정 후-------------------------------------------

#include <iostream>
using namespace std;

class MyVector{
  int *p;
  int size;
public: //간소화된 생성자
  MyVector(int n=100) {
    p = new int [n];
    sizee = n;
  }
  ~MyVector() { delete[] p; }
};

int main() {
  MyVector *v1, *v2;
  v1 = new MyVector();  //디폴트로 정수 배열 100 동적 할당
  v2 = new MyVector(1024);  //정수 배열 1024 동적 할당

  delete v1;
  delete v2;
}
```
</div>
</details>
