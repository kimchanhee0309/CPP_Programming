# 함수의 인자 전달 방식 리뷰

#### 값에 의한 호출(call by value)
* 호출하는 코드에서 넘겨주는 실인자 값이 함수의 매개 변수에 복사되어 전달되는 방식
```C++
#include <iostream>
using namespace std;

void swap(int a, int b) {
  int tmp;

  tmp = a;
  a = b;
  b = tmp;
}

int main() {
  int m=2, n=9;
  swap(m, n);
  cout << m << ' ' << n;
}

<실행 결과>
2 9
```
* 매개 변수 a, b가 swap() 함수의 `스택`에 생성되고 m, n의 값이 a, b에 `복사`됨
* a와 b 값이 서로 교환되고, swap() 함수가 종료하면 swap() 스택과 함께 a, b도 사라지지만,
* main() 스택에 m, n 은 변함없이 2, 9의 값을 유지함

#### 주소에 의한 호출(call by address)
* 주소를 직접 포인터 타입의 매개 변수에 전달하는 방식
```C++
#include <iostream>
using namespace std;

void swap(int *a, int *b) {
  int tmp;

  tmp = *a;
  *a = *b;
  *b = tmp;
}

int main() {
  int m=2, n=9;
  swap(&m, &n);
  cout << m << ' ' << n;
}

<실행 결과>
9 2
```
* 포인터 매개 변수 a, b가  swap()의 스택에 생성되고 m, n의 주소가 a, b에 전달되어 포인터 a, b는 m, n의 공간을 각각 가리킴
* swap() 함수에 의해 포인터 a와 b가 가리키는 값이 서로 교환되면, 그 결과 m과 n의 값이 교환됨
* swap() 함수가 종료하면 a, b가 사라지고 main() 스택의 m, n은 서로 교환된 채 남아있게 됨
* 배열이 전달되는 경우, 배열의 이름이 전달되므로 자연스럽게 ‘주소에 의한 호출’이 이루어짐

#### '값에 의한 호출'과 '주소에 의한 호출'의 특징
* `값에 의한 호출`
  * 실인자의 값을 복사하여 전달하므로, 함수 내에서 실인자를 손상시킬 수 없다는 장점
  * So, 함수 호출에 따른 부작용(side-effect)은 없음
* `주소에 의한 호출`
  * 실인자의 주소를 넘겨주어 의도적으로 함수 내에서 실인자의 값을 변경하고자 할 때 이용됨
