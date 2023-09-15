# 함수 중복

<details>
<summary>1. 포인트</summary>
<div markdown="1">       

* 같은 이름의 함수를 여러 개 만들 수 있음, 이것을 **함수 중복(function overloading)** 이라고 부름
* **다형성**의 한 사례로, `전역 함수`와 `멤버 함수`에 모두 적용됨
* 상속 관계에 있는 `기본 클래스`와 `파생 클래스` 사이에도 허용됨
</div>
</details>

___

<details>
<summary>2. 중복 함수 조건</summary>
<div markdown="1">       

* 중복된 함수들의 **이름이 동일**하여야 함
* 중복된 함수들은 **매개 변수 타입**이나 **매개 변수 개수**가 달라야 함
* 함수 중복에 **리턴 타입**은 고려되지 않음
* 함수 중복의 성공 사례
```C++
int sum(int a, int b, int c) {
  return a + b + c;
}

double sum(double a, double b) {
  return a + b;
}

int sum(int a, int b) {
  return a + b;
}

int main() {
  cout << sum(2, 5, 33);
  cout << sum(12.5, 33.6);
  cout << sum(2, 6);
}
```
* 함수 중복의 실패 사례
```C++
int sum(int a, int b) {
  return a + b;
}

double sum(int a, int b) {
  return (double)(a + b);
}

int main() {
  cout << sum(2, 5);
}
```
  * 컴파일러는 중복된 함수를 구분할 때 리턴 타입은 전혀 고려하지 않음
  * 함수 이름, 매개 변수 타입과 개수가 모두 같고 리턴 타입만 다른 경우 함수 중복 성립 X
</div>
</details>

___

<details>
<summary>3. 함수 중복의 편리함</summary>
<div markdown="1">       

* 동일한 이름으로 함수를 중복하면 작성이 편리함
* 이름을 구분지어 기억할 필요가 없어 함수를 잘못 호출하는 실수를 줄일 수 있음
* 함수 중복 연습 코드
```C++
#include <iostream>
using namespace std;

int big(int a, int b) {
	if (a > b) return a;
	else return b;
}

int big(int a[], int size) {
	int res = a[0];
	for (int i = 1; i < size; i++)
		if (res < a[i]) res = a[i];
	return res;
}

int main() {
	int array[5] = { 1,9,-2,8,6 };
	cout << big(2, 3) << endl;
	cout << big(array, 5) << endl;
}

<실행 결과>
3
9
```

```C++
#include <iostream>
using namespace std;

int sum(int a, int b) {
	int s = 0;
	for (int i = a; i <= b; i++)
		s += i;
	return s;
}

int sum(int a) {
	int s = 0;
	for (int i = 0; i <= a; i++)
		s += i;
	return s;
}

int main() {
	cout << sum(3, 5) << endl;
	cout << sum(3) << endl;
	cout << sum(100) << endl;
}

<실행 결과>
12
6
5050
```
</div>
</details>

___

<details>
<summary>4. 생성자 함수 중복</summary>
<div markdown="1">       

* 생성자 함수도 중복 가능함
```C++
class Circle {
  int radius;
public:
  Circle(); //생성자 중복
  Circle(int r); // 생성자 중복
  . . . . .
};

int main() {
  Circle donut; //Circle() 생성자 호출
  Circle pizza(30); //Circle(int r) 생성자 호출
}
```

* 객체 생성 시, 매개 변수를 통해 다양한 형태로 초깃값을 전달하기 위해 생성자 함수를 중복 선언함
  * string 클래스의 주요 생성자
```C++
class string {
  .....
public:
  string(); //빈 문자열을 가진 스트링 객체 생성
  string(char* s); //'\0'로 끝나는 C-스트링 s를 스트링 객체로 생성
  string(string& str); //str을 복사한 새로운 스트링 객체 생성
  .....
};

-----------------------------------------------------------------------------------

<중복된 생성자를 활용하여 스트링 객체를 다양하게 생성하는 사례>
string str; //빈 문자열을 가진 스트링 객체
string address("서울시 성북구 삼선동 389");
string copyAddress(address); //address의 문자열을 복사한 별도의 copyAddress 생성
```
</div>
</details>

___

<details>
<summary>5. 소멸자 함수 중복</summary>
<div markdown="1">       

* 소멸자는 매개 변수를 가지지 않기 때문에 한 클래스에 오직 하나만 존재함
* 소멸자 함수의 중복은 근본적으로 불가능함
</div>
</details>
