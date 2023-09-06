# 인라인 함수

<details>
<summary>1. 함수 호출에 따른 시간 오버헤드</summary>
<div markdown="1">       

* 함수 호출과 실행을 마치고 돌아오는 과정에서 시간 소모가 발생함
* **함수 호출 오버헤드(overhead)** 시간이 무시할 수 없는 경우도 있음
```C++
#include <iostream>
using namespace std;

int odd(int x) {
  return (x%2);
}

int main() {
  int sum = 0;

  //1에서 10000까지의 홀수의 합 계산
  for(int i=1; i<=10000; i++) {
    if(odd(i))
      sum += i;
  }
  cout << sum;
}
```
* odd()함수의 코드 x%2를 계산하는 시간보다, odd() 함수의 호출과 리턴에 따른 오버헤드 시간이 더 큼
* 10000번이나 odd() 함수를 호출하기 때문에 함수 호출 오버헤드는 더욱 가중됨
* 짧은 코드를 함수로 만들면 함수 호출의 오버헤드가 상대적으로 커서 프로그램 실행 시간이 길어지는 원인이 됨
* 이러한 함수에 대해 호출 오버헤드를 줄여 프로그램의 실행 속도를 개선할 수 있는 방법이 인라인 함수
</div>
</details>

___

<details>
<summary>2. 인라인 함수(inline function)</summary>
<div markdown="1">    

* 짧은 코드로 구성된 함수에 대해, 함수 호출 오버헤드로 인한 프로그램의 실행 속도 저하를 막기 위해 도입된 기능
* 함수 앞에 inline 키워드를 이용하여 선언 가능함
```C++
inline int odd(int x) { //odd 함수를 인라인 함수로 선언
  return(x%2);
}
```
* 인라인 함수를 호출하는 곳에 인라인 함수의 코드를 그대로 삽입하여 함수 호출이 일어나지 않게 함
  * 실행 속도가 빨라지게 됨
```C++
//인라인 함수 선언과 인라인 함수의 확장
#include <iostream>
using namespace std;

inline int odd(int x) {
  return (x%2);
}

int main() {
  int sum = 0;

  for(int i=1; i<=10000; i++) {
    if(odd(i))
      sum += i;
  }
  cout << sum;
}

------------------------------------------------------------------------------------------------
<컴파일러에 의해 inline 함수의 코드 확장 삽입>
#include <iostream>
using namespace std;

int main() {
  int sum = 0;

  for(int i=1; i<=10000; i++) {
    if((i%2))
      sum += i;
  }
  cout << sum;
}
```
* 인라인 함수의 장단점
  * 장점 : C++ 프로그램의 실행 속도를 향상시킬 수 있음
  * 단점 : 호출하는 곳이 여러 군데 있으면 그 만큼 전체 크기가 늘어나게 됨
* 인라인 함수의 제약 사항
  * 컴파일러는 함수의 크기나 효율을 따져 불필요한 경우, inline 선언을 무시할 수 있음
</div>
</details>

___

<details>
<summary>3. 멤버 함수의 인라인 선언과 자동 인라인</summary>
<div markdown="1">       

* 생성자를 포함하여 클래스의 모든 멤버 함수가 인라인으로 선언될 수 있음
* 멤버 함수의 크기가 작은 경우, **클래스의 선언부에 직접 구현**하여도 무방함
* 클래스의 선언부에 구현된 멤버 함수들에 대해서 inline 선언이 없어도 인라인 함수로 자동 처리함
```C++
//(a) 멤버 함수를 inline으로 선언하는 경우
class Circle {
private:
  int radius;
public:
  Circle();
  Circle(int r);
  double getArea();
};

inline Circle::Circle() { //inline 멤버 함수
  radius = 1;
}

Circle::Circle(int r) {
  radius = r;
}

inline double Circle::getArea() { //inline 멤버 함수
  return 3.14*radius*radius;
}
```
```C++
//(b) 자동 inline으로 처리되는 경우
class Circle {
private:
  int radius;
public:
  Circle() { //자동 인라인 함수
    radius = 1;
  }

  Circle(int r);
  double getArea() { //자동 인라인 함수
    return 3.14*radius*radius;
  }
};

Circle::Circle(int r) {
  radius = r;
}
```
* (a), (b) 코드는 사실상 같은 코드임
</div>
</details>
