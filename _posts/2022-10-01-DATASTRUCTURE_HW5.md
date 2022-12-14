---
title: REVIEW-Data_Structure_hw5
categories: [STUDY, DATASTRUCTURE]
tags: [DATASTRUCTURE,TIL]     # TAG names should always be lowercase
---

아래, 문제의 코드를 보자.

```cpp
#include <iostream>
using namespace std;

int main(int argac, char** argv)
{
  int y=1;
  int *& ry = &y;
  cout <<ry<<endl;
  return 0;
}
```

<aside>
💡 error가 발생한다 !!!

</aside>
![image](https://user-images.githubusercontent.com/105411918/193461212-f7ed8838-a8cb-4a39-81d1-7aace1e88209.png)

오늘, C++ 공부를 하다가 의문이 들어 다시 펼쳐보았다.

아래의 포인터변수의 참조자 예시를 봅자.

![image](https://user-images.githubusercontent.com/105411918/193461298-b1dfa7da-ce05-46ca-bfc4-248d52aae39b.png)

여기서 

```cpp
*&pref = ptr; 
```

이것은 

```cpp
int* (&pref) = ptr;
```

과 같은 표기라고 하며

열혈 C++ 교재에는

<aside>
💡 포인터 변수의 참조자 선언도 & 연산자를 하나 더 추가하는 형태로 진행이 된다. 이미 잘 아는 10행의 참조자 선언과 비교하기 바란다.

</aside>

라고 적혀있다.

---

즉, 

오류가 발생하는 줄인

```cpp
int *& ry = &y;
```

이 부분에서 

ry는 포인터 변수의 참조자 선언이라고 볼 수가 있는것이다.

![image](https://user-images.githubusercontent.com/105411918/193461307-2df23487-3b95-43f9-a180-7fd04a98cc91.png)

이렇게 했을 때 오류가 나니까 다시 한 번 확인할 수 있는 것.

여기까지 동의하시나요 ?
---

But !! 

&y는 포인터변수가 아니지만 ! &y를 포인터 변수에 대입하면 값이 똑같기때문에 이것도 문제가 없지 않나 ? 라는 의문이 들 수 있겠다

---

error코드를 살펴보자.

![image](https://user-images.githubusercontent.com/105411918/193461328-803d6eaf-73ed-4a2d-a840-2b757e83f40b.png)

일단 reference to type ‘int*’이라는 말을 보면 포인터변수에 대한 참조자라는 것은 확실하다.

그런데 !!!!

여기서 Lvalue가 무엇인지, non-const value라는 말은 왜 나오는지, temporary라는 용어는 왜 나오는지 볼 필요가 있다.

아래 사이트들을 한번씩 찬찬히 살펴보고 Lvalue와 Rvalue, &와 &&에 대해 알아보았으면 좋겠다.

[http://hellocbc.blogspot.com/2021/01/c-lvalue-rvalue.html](http://hellocbc.blogspot.com/2021/01/c-lvalue-rvalue.html) (이 사이트가 제일 도움이 될 것.)

[https://m.blog.naver.com/nortul/197601619](https://m.blog.naver.com/nortul/197601619)

[https://effort4137.tistory.com/entry/Lvalue-Rvalue](https://effort4137.tistory.com/entry/Lvalue-Rvalue)

한번보면 이해 안돼도 이렇게 여러개 보면 대충 무슨 말인지 느낌이 올 것이다.

그럼 이제 아래에 설명을 차근 차근 따라가보자.

---
(출처는 첫 번째 사이트 !)

![image](https://user-images.githubusercontent.com/105411918/193461342-83aa3ce8-f080-47f4-9fc6-c673ceee33ec.png)

Lvalue는 표현식 이후에 없어지지 않고 지속되는 객체로, 말하자면 선언식 중 왼쪽에 있는거, 예를 들면 선언된 변수 ! 를 이야기하는 것으로 추정됨 !

Rvalue는 표현식 종료 후 더 이상 존재하지 않는 임시적인 값으로, 말하자면 선언식 중 오른쪽에 있는 것, 예를 들면 변수선언에 이용되는 값 일부 ?!? 로 생각.

![image](https://user-images.githubusercontent.com/105411918/193461365-03d1daf0-4a74-4cfd-b9ca-06c49d39de33.png)

그런데 Lvalue와 Rvalue의 참조자 선언은 다르게 생겼다는 점 !

---

우리 과제를 다시 한 번 보자.

![image](https://user-images.githubusercontent.com/105411918/193461374-c9f82719-d952-4327-879e-f86a057b2b1d.png)

이제 이상한 부분이 보이지 않는가 ?  !

일단 포인터변수를 참조하기 위해 `*&ry`를 사용했는데 오른쪽에 할당된 값은 `&y` 이다 !!!!!!!!!!!

&y는 표현 식 종료 후 더 이상 존재하지 않는 임시적인 값으로 볼 수 있지 않을까? 그건 Rvalue라 위에서 그랬는데 !!!

그럼 `&y` 를 Rvalue라 생각하고 이 줄을 고쳐보자.


---
1. Rvalue의 참조자 선언인 &&

```cpp
#include <iostream>
using namespace std;

int main(int argac, char** argv)
{
  int y=1;
  int *&& ry = &y;
  cout <<ry<<endl;
  return 0;
}
```

![image](https://user-images.githubusercontent.com/105411918/193461396-dc3432b1-76fa-4fe4-b520-ecdc8ae4c825.png)

 

헉 ! 오류가 나타나지 않고 실행이 잘 된다 !

2. 이번에는 Lvalue로 만들어볼게용

```cpp
#include <iostream>
using namespace std;

int main(int argac, char** argv)
{
  int y=1;
  int *k=&y;
  int *& ry = k;
  cout <<ry<<endl;
  return 0;
}
```

![image](https://user-images.githubusercontent.com/105411918/193461413-180c5178-c07f-49fb-b2ef-e67d0857854a.png)

 !!!!!!!

포인터 변수 k를 선언해 y의 주소값을 받은 뒤 포인터변수의 참조자에 포인터변수를 할당해주면..

오류가 나타나지 않고 실행이 잘 되는 것을 확인할 수 있다.

---

즉, 위에서의 오류가 나는 원인은

#### 1. Rvalue와 Lvalue의 개념 때문이다
#### 2. *&ry는 포인터변수의 참조자로, Rvalue를 참조해야하는데
#### 3. &y는 Rvalue입니다.
#### 4. 따라서 우리는 Rvalue의 참조자인 *&&ry로 고쳐주거나
#### 5. &y를 Lvalue로 만들어 포인터 변수를 만들어 참조를 하게되면
#### 6. 우리는 문제를 해결할 수 있겠네요

<aside>
💡 참조자 선언은 상수가 아닌 변수를 대상으로만 가능하다

</aside>
