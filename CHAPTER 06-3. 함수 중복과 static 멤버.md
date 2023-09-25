# 함수 중복의 모호성

<details>
<summary>1. 형 변환으로 인한 모호성</summary>
<div markdown="1">       

* 일반적으로 함수의 매개 변수 타입과 호출 문의 실인자 타입이 일치하지 않는 경우, 컴파일러는 보이지 않게 **형 변환(type conversion)** 을 시도함
  >double square(double a);  //double 타입의 매개 변수를 가진 함수
  >
  >. . . .
  >
  >square(3);  //int 타입의 매개 변수 전달. 컴파일 오류 발생하지 않음
* 컴파일러는 다음과 같이 작은 타읍을 큰 타입으로 **자동 형 변환**함
* 화살표 왼쪽에 있는 타입이 오른쪽에 있는 어떤 타입으로도 자동 형 변환 가능함
  >char -> int -> long -> float -> double
* But, 다음과 같이 2개의 중복된 square() 함수가 작성되어 있는 경우, 컴파일 오류 발생
  >float square(float a);
  >
  >double square(double a);
  >square(3);  //함수 3을 float 타입으로 형변환할지, double 타입으로 할지 모호한 경우, 컴파일 오류 발생
  * 이 호출에 적합한 square(int a); 의 함수가 없기 때문에 컴파일러는 정수 3을 float 타입으로 변환할지 double 타입으로 변화낳ㄹ지 모호함
 
* 예시 코드
```C++
<형 변환으로 인해 함수 중복이 모호한 경우>
#include <iostream>
using namespace std;

float square(float a) {
  return a*a;
}

double square(double a) {
  return a*a;
}

int main() {
  cout << square(3, 0);  //square(double a); 호출
  cout << square(3);  //중복된 함수에 대한 모호한 호출로서, 컴파일 오류
```
</div>
</details>

___

<details>
<summary>2. 참조 매개 변수로 인한 모호성</summary>
<div markdown="1">       

* 중복된 함수 중에서 **참조 매개 변수**를 가진 함수가 있는 겨우, 이들 사이에 모호성이 존재할 수 있음
  >int add(int a, int b);
  >
  >int add(int a, int &b);
  >
  >int s=10, t=20;
  >
  >add(s, t);  //컴파일 오류. 함수 호출의 모호성
  >
* 예시 코드
```C++
<참조 매개 변수로 인한 함수 중복의 모호성>
#include <iostream>
using namespace std;

int add(int a, int b) {
  return a + b;
}

int add(int a, int &b) {
  b = b + a;
  return b;
}

int main() {
  int s=10, t=20;
  cout << add(s, t);  //컴파일 오류, 참조 매개 변수로 인해 함수 호출이 모호함
```
</div>
</details>

___

<details>
<summary>3. 디폴트 매개 변수로 인한 모호성</summary>
<div markdown="1">       

* **디폴트 매개 변수**를 가진 함수가 보통 매개 변수를 가진 함수와 중복 작성될 때, 모호성이 존재할 수 있음
  >void msg(int id) {
  >
  >void msg(int id, string s=””)
  >
  >msg(6);  //컴파일 오류. 하수 호출 모호
  * 앞의 중복된 두 msg() 함수 중 어떤 함수를 호출해도 무관하므로, 컴파일 오류가 발생함
 
* 예시 코드
```C++
<디폴트 매개 변수로 인한 함수 중복의 모호성>
#inclde <iostream>
#include <string>
using namespace std;

void msg(int id) {
  cout << id << endl;
}

void msg(int id, string s="") {
  cout << id << ":" << s << endl;
}

int main() {
  msg(5, "Goom Morning"); //정상 컴파일, 두번째 msg() 호출
  msg(6); //함수 호출 모호, 컴파일 오류
```
</div>
</details>
