# 동적 메모리 할당 및 반환

<details>
<summary>1. 동적 메모리 할당</summary>
<div markdown="1">       

* 실행 중에 필요한 만큼 메모리를 할당받고 필요 없을 때 반환하는 ‘동적 메모리 할당/반환 메커니즘’이 필요
* C언어에서는 malloc()/free() 함수를 이용하지만, C++에서는 `new`와 `delete` 연산자를 이용함
  * **new 연산자** : `힙(heap)` 이라는 공간으로부터 메모리를 할당받음
  * **delete 연산자** : 할당받은 메모리를 힙으로 반환
</div>
</details>

___

<details>
<summary>2. new와 delete 연산자</summary>
<div markdown="1">       

#### new와 delete의 기본 활용
* 기본 형식
  >데이터타입 *포인터변수 = new 데이터타입;
  >
  >delete 포인터변수;
* ‘데이터타입’은 int, char, double 등 기본 타입뿐 아니라 구조체(struct), 클래스(class)도 포함함
* **new 연산자**
  * ‘데이터타입’의 크기만큼 힙으로부터 메모리를 할당받고 주소를 리턴함
  * 그 결과 ‘포인터변수’는 할당받은 **메모리의 주소**를 가짐
* **delete 연산자**
  * ‘포인터변수’가 가리키는 메모리를 힙으로 반환함
```C++
<동적 메모리를 할당받고 반환하는 간단한 코드 예시>
int *pInt = new int; //int 타입의 정수 공간 할당
char *pChar = new char; //char 타입의 문자 공간 할당
Circle *pCircle = new Circle(); //Circle 클래스 타입의 객체 할당

delete pInt; //할당받은 정수 공간 반환
delete pChar; //할당받은 문자 공간 반환
delete pCircle; //할당받은 객체 공간 반환
```
* 힙 메모리가 부족하면 new는 `NULL`을 리턴하므로, new의 리턴 값이 NULL인지 검사하는 것이 좋음
* 예시 코드(int 타입의 정수 공간 한 개를 할당받고 반환하는 코드)
```C++
int *p = new int; //힙으로부터 int 타입의 정수 공간 할당
if(!p) { //if(p==NULL)과 동일. p가 NULL이면 
  return; //메모리 할당받기 실패
}

*p = 5; //할당받은 정수 공간에 5 기록
int n = *p; //할당받은 정수 곤간에서 값 읽기, n = 5
delete p; //할당받은 정수 공간 반환
```

#### 동적 할당 메모리 초기화
* new를 이용하여 메모리를 할당받을 때, ‘초깃값’을 지정하여 초기화할 수 있음
  >데이터타입 *포인터변수 = new 데이터타입(초깃값);
```C++
int *pInt = new int(20); //20으로 초기화된 int 공간 할당
char *pChar = new char('a'); //'a'로 초기화된 char 공간 할당
```

#### delete 사용 시 주의
* 동적으로 할당받지 않는 메모리를 반환하면 실행 오류가 발생함
```C++
int n;
int *p = &n;
delete p; //실행 오류, p가 가리키는 메모리는 동적 할당받은 것이 아님
```
* 동일한 메모리를 두 번 반환하면 실행 오류가 발생함
```C++
int *p = new int;
delete p; //정상적인 메모리 반환
delete p; //실행 오류, 이미 반환한 메모리를 중복 반환할 수 없음
```

#### 예시 코드(정수형 공간의 동적 할당및 반환)
```C++
#include <iostream>
using namespace std;

int main() {
	int* p;

	p = new int; //int 타입 1개 할당
	if (!p) {
		cout << "메모리를 할당할 수 없습니다.";
		return 0;
	}

	*p = 5; //할당 받은 정수 공간에 5 기록
	int n = *p;
	cout << "*p = " << *p << endl;
	cout << "n = " << n << endl;

	delete p;
}
```
</div>
</details>

___

<details>
<summary>3. 배열의 동적 할당 및 반환</summary>
<div markdown="1">       

#### 배열의 동적 할당/반환의 기본 형식
* 배열의 동적 할당/반환의 기본 형식
  >데이터타입 *포인터변수 = new 데이터타입 [배열의 크기]; //배열의 동적 할당
  >
  >delete [] 포인터변수; //배열 메모리 반환
  >
  >>new 연산자는 ‘배열의 크기’만한 배열을 할당받아 주소를 리턴함
  >>
  >>delete 연산자는 ‘포인터변수’가 가리키는 배열 메모리를 반환함
```C++
int *p = new int [5]; //크기가 5인 정수형 배열의 동적 할당
if(!p)
  return; //메모리 할당 실패

for(int i=0; i<5; i++)
  p[i] = i; //배열에 순서대로 0, 1, 2, 3, 4를 기록한다.
  // *(p+i) = i; 로도 표현 가능함

delete [] p; //배열 메모리 반환
```
```C++
<정수형 배열의 동적 할당 및 반환 예시 코드>

#include <iostream>
using namespace std;

int main() {
	cout << "입력할 정수의 개수는?";
	int n;
	cin >> n; //정수의 개수 입력
	if (n <= 0) return 0;
	int* p = new int[n]; //n개의 정수 배열 동적 할당
	if (!p) {
		cout << "매모리를 할당할 수 없습니다.";
		return 0;
	}

	for (int i = 0; i < n; i++) {
		cout << i + 1 << "번째 정수: "; //프롬프트 출력
		cin >> p[i]; //키보드로부터 정수 입력
	}

	int sum = 0;
	for (int i = 0; i < n; i++)
		sum += p[i];
	cout << "평균 = " << sum / n << endl;

	delete[] p; //배열 매모리 반환
}
```

#### 배열을 초기화할 때 주의 사항
* new로 배열을 동적 할당받을 때 다음과 같이 생성자를 통해 직접 ‘초깃값’을 지정할 수 없음
  >int *pArray = new int [10](20); //구문 오류, 배열의 초기화는 불가
  >
  >int *pArray = new int(20)[10]; //구문 오류
  >
  >다음과 같이 초깃값을 지정할 수 있음
  >
  >int *pArray = new int [] {1, 2, 3, 4}  //1, 2, 3, 4로 초기화된 정수 배열 생성

#### 배열을 delete할 때 주의 사항
>int *p = new int [10];
>
>delete p; //비정상 반환, delete  [] p; 로 하여야 함
>
>int *q = new int;
>
>delete [] q; //비정상 반환, delete q; 로 하여야 함

* malloc(), free()는 잊어버리기!
  * malloc()은 할당받은 크기를 지정해야 하고, 리턴되는 포인터를 형 변환해야 하는 불편함이 있지만, new는 이런 불편함이 없음
  * malloc(), free()를 사용하기 위해서는 <cstdlib>를 include해야 하지만, new와 delete는 그럴 필요가 없음

</div>
</details>
