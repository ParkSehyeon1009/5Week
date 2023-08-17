# 5Week
# 13장 클래스의 상속
**간단한 기초 클래스**<br>
**파생클래스**<br>
파생클래스는 말 그대로 파생되어 나온 클래스를 뜻한다.<br>
즉 새로운 클래스를 정의할때 이미 정의된 클래스의 특성을 재활용 혹은 기능을 추가하여 변형된 클래스를 만드는 것이다.<br>
아래는 예시이다.
```cpp
class SampleBass
{
  ... //베이스, 부모 클래스
}
class Sampledervied:public SampleBass
{
  ... //파생, 자식 클래스
}
```
위의 예시 코드에서는 부모 클래스를 public으로 파생시켜 부모 클래스의 모든 공용 멤버를 자식 클래스의 공용 멤버처럼 사용가능하고<br>
모든 전용 멤버는 자식 클래스의 전용 멤버처럼 사용 가능하다.<br>
이때 public를 접근 지정자라 뜻하는데. 접근 지정자는 상속시 public,private로만 지정 가능하다.<br>
만약 private로 접근 지정자를 지정한다면 부모 클래스의 모든 공용 부분의 멤버가 자식 클래스의 전용 멤버처럼 사용 가능하다.<br>
즉 아래와 같다.
```cpp
public으로 선언시

부모(private) -> 자식(private)
부모(public) -> 자식(public)

private로 선언시

부모(public) -> 자식(private)
```

**멤버 초기자 리스트**<br>
C++에서는 상수인 클래스 멤버를 초기화시키기 위하여 특별한 문법을 제공한다.<br>
일반적으로는 상수인 클래스 멤버를 생성자를 통화여 초기화 하지 못함으로 객체가 생성될때 초기화해야 한다.<br>
즉 아래의 코드와 같이 사용할수 없다는 것이다.<br>
```cpp
class Sample
{
  const int Test;
  Sample(int T)
  {
    Test = T; // 허용안됨
  };
}
```
이러한 불편함을 해결하기 위해 나온것이 **멤버 초기자 리스트**다.
```cpp
class Sample
{
  const int Test;
  Sample(int T) : Test(T){ //허용됨
  };
}
```
사용 방법은 위 코드에서 알수 있듯이 아래와 같다.
```cpp
생성자(매개변수) : 클래스멤버(값){...}
```
또한 하나가 아닌 여러개도 가능하다.
```cpp
class Sample
{
  const int Test;
  int Test2;
  Sample(int T) : Test(T),Test2(10){ //허용됨
  };
}
```
멤버 초기자 리스트를 사용할때 주의사항은 아래와 같다.<br>
1. 생성자에서만 사용 가능
2. 참조 데이터 형식을 초기화 해야할때도 멤버 초기자 리스트를 사용한다.

<br>

**상속 is-a 관계** <br>
is-a 관계는 파생 클래스의 상속 조건이다.<br>
즉 ~은 ~이다 라는 관계를 성립시키자 라는 것이다.<br>
예를 들어 "바나나 는 과일이다" 는 성립한다. 하지만 반대로 "과일은 바나나이다" 는 성립하지 않는다.<br>
그 이유는 문장 그대로 모든 과일이 바나나는 아니기 때문이다.<br>
즉 우리가 여러 과일에 관한 클래스를 만들때<br>
과일들이 공통적으로 가지고 있는 특징을 추상화 하여 기본 클래스를 만들고<br>
그 기본 클래스를 상속받아 여러 과일 클래스를 만드는데 이것이 파생 클래스가 되는것이다.<br>
그렇기에 is-a관계를 성립시키는 것이 상속을 하는 조건에서 반드시 필요로 하는 이유이다.<br>
**public 다형 상속**<br>
파생 클래스에서 기초 클래스의 메서드를 다르게 사용해야 될 때가 있다. 호출하는 객체에 따라<br>
메서드의 행동이 달라질 수도 있는데 이 때 다형상속이 필요하다.<br>
아래는 다형 상속의 방법이다.
```cpp
class Test
{
  virtual void Print() const;
};
class Test2
{
  virtual void Print() const;
}

void main()
{
  Test A(123,"A");
  Test2 B(12, "B");
  Test & t1 = A;
  Test & t2 = B;

  t1.Print(); // Test::Print() 사용
  t2.Print(); // Test2::Print() 사용
}
```
위와 같은 식으로 호출하는 객체에 따라 불러올 함수를 다르게 불러올수 있다.<br>
<br>

**정적 결합과 동적 결합**<br>
정적 결합 : 가르키고 있는 타입이 아니라 스스로, 변수의 타입만 보고 어떤 함수로 갈지를 결정 한 것<br>
예) 함수 오버로딩<br>
```cpp
class A
{
  public:
    int num;
};
class B : public A
{
};
A operator+(const A& x, const A& y)
{
  A a;
  a.num = x.num + y.num;
  return a;
}
B operator+(const B& x, consst B& y) // 곱셈 연산이 덧셈 연산보다 복잡하기 때문에 구체적인 구현을 제공함
{
  B b;
  b.num = x.num * y.num;
  return b;
}
void main()
{
  B a0, a1;
  a0.num = 10;
  a1.num = 20;
  A a2 = a0 + a1;
  cout << a2.num;
}
```
결과는 아래와 같다
```
200
```
위와 같이 더 구체적인 구현을 하는 함수를 사용한다.<br>
아래 또한 비슷한 예제이다
```cpp
class A
{
public:
    int num;
};
class B : public A
{

};

A operator+(const A& x, const A& y)
{
    A a;
    a.num = x.num + y.num;
    return a;
}

B operator+(const B& x, const B& y)
{
    B b;
    b.num = x.num * y.num;
    return b;
}


int main()
{
    B b0, b1;
    b0.num = 10;
    b1.num = 20;

    A& a0 = b0;
    A& a1 = b1;

    A a2 = a0 + a1;
    cout << a2.num << endl;
}
```
실행 결과는 아래와 같다.
```
30
```
결과값이 다르게 나오는 이유는 변수의 타입만 보고 어떤 함수로 갈지 결정한 정적결합 특징이다.<br>
<br>
동적 결합 : 런타입 바인딩, 런타임 도중에 타입이 결정되는 것을 동적 결합 이라고 한다.<br>
예) 함수 오버라이딩<br>
```cpp
class Animal
{
public:
    virtual void eat() const
    {
        cout << "냠" << endl;
    }
};

class Cat : public Animal
{
public:
    virtual void eat() const override
    {
        cout << "냥" << endl;
    }
};

class Dog : public Animal
{
public:
    virtual void eat() const override
    {
        cout << "멍" << endl;
    }
};


int main()
{
    Animal* a = new Dog();
    a->eat(); // 가리키고 있는 객체의 함수를 호출함
    // 런타임 도중에 매번 가리키고 있는 함수를 찾음 (런타임 바인딩임.)
}
```
아래는 동적결합의 예제이다
```cpp
class Animal
{
public:
    int* p;
    virtual void eat() const
    {
        cout << "냠" << endl;
    }
    virtual void foo()
    {
        cout << "foo " << endl;
    }
};

class Cat : public Animal
{
public:
    virtual void eat() const override
    {
        cout << "냥" << endl;
    }
};

class Dog : public Animal
{
public:
    virtual void eat() const override
    {
        cout << "멍" << endl;
    }
};


int main()
{
    Dog d;
    Animal* animal = new Cat; // 부모가 자식을 가르킬때는 가상함수 메카니즘에 의해 가상함수 테이블을 통해 런타임 도중에 함수를 찾는다
    animal->eat(); // 런타임 도중에 최적의 함수인 오버라이딩에 의해 "냥"을 출력한다.
```
**접근 제어 : protected**<br>
pritacted는 간단하게 자식클래스의 멤버함수에서만 접근가능하게 해준다.
```cpp
class A {
public:
    int num1;

    A() : num1(5), num2(6), num3(7) {}
    void pri() { cout << "hi" << endl; };

protected:
    int num2;
private:
    int num3;

};

class B : protected A {
public:
     
    void setNum() {
        num1 = 10;   //컴파일 가능!!
        num2 = 100;  //컴파일 가능!!
        num3 = 1000; //컴파일 불가!!
    }
};


int main(void) {

    B b;
    b.pri();                //컴파일 불가!
    cout << b.num1 << endl; //컴파일 불가!!
    cout << b.num2 << endl; //컴파일 불가!!
    cout << b.num3 << endl; //컴파일 불가!!

}​
```
**추상화 기초 클래스**<br>
추상화 기초 클래스 : 공통된 부분이 많지만, 한 객체가 어느 한 쪽에 속하지는 않는 관계일 때,<br>
공통된 멤버들만으로 정의한 클래스를 뜻한다. <br>
예를 들어 A를 구현하고 나머지 부분은 각 클래스마다 다르게 구현한후 A를 상속받는 방법으로 정의하는 방식<br>
아래는 예제이다.
```cpp
class Fruit {
public:
    Fruit(const string& n) : name(n) {}

    virtual string getName() const {
        return name;
    }

    virtual void eat() const = 0; // 순수 가상 함수

private:
    string name;
};

// 사과 클래스
class Apple : public Fruit {
public:
    Apple() : Fruit("Apple") {}

    void eat() const override {
        cout << "Eating an apple." << endl;
    }
};

// 바나나 클래스
class Banana : public Fruit {
public:
    Banana() : Fruit("Banana") {}

    void eat() const override {
        cout << "Peeling and eating a banana." << endl;
    }
};

int main() {
    Apple apple;
    Banana banana;

    Fruit* fruits[] = { &apple, &banana };

    for (const Fruit* fruit : fruits) {
        cout << "Fruit: " << fruit->getName() << endl;
        fruit->eat();
    }

    return 0;
}
```
**상속과 동적 메모리 대입**<br>
파생 클래스의 성질마다 상속은 동적 메모리 대입과 관계가 다르다.<br>
아래와 같은 기본 클래스가 있다고 가정해보자.
```cpp
class BaseClass
{
  public:
    BaseClass(const char* str = "null", int r = 0); //생성자
    BaseClass(const BassClass& rs); //복사 생성자
    virtual ~BaseClass(); //소멸자
    BaseClass& operator=(const BaseClass& rs); //대입 연산자
}
```
만약 BaseClass 에서 파생하는 클래스가 new를 사용하지 않고 다른 특별한 처리도 요구하지 않는다고 가정해보자<br>
파생 클래스를 위해 명시적으로 소멸자,복사 생성자, 대입연산자를 정의를 하지 않아도 된다.<br>
소멸자의 경우 명시적으로 소멸자를 정의하지 않으면 컴파일러가 기본 소멸자를 정의해주며<br> 
파생 클래스의 소멸자는 기초 클래스의 소멸자를 항상 호출한다.<br>
<br>
복사 생성자의 경우, 기본 복사 생성자는 멤버별 복사를 한다. 멤버별 복사는 동적 메모리 대입에는 사용할 수 없고,<br>
새로운 파생 클래스의 멤버에 대해서는 멤버별 복사를 사용할 수 있다.<br> 
클래스 멤버 또는 기초 클래스의 클래스 성분을 복사하는 것은 해당 클래스의 복사 생성자를 통하여 이루어진다.<br>
파생 클래스의 기본 복사 생성자는 기초 클래스 성분을 복사하기 위해 명시적인 기초 클래스의 복사 생성자를 이용하므로,<br> 
파생 클래스의 기본 복사 생성자는 새로운 파생 클래스 멤버에 대해 사용할 수 있고<br>
상속받은 기초 클래스 객체에 대해서도 사용할 수 있다.<br>
대입 연산자 또한 복사 생성자와 같은 논리가 적용된다.<br>
<br>
파생 클래스가 new를 사용한다고 가정해보자<br>
이 경우에는 파생클래스를 위해 명시적으로 정의를 해주어야 한다.
```cpp
class SampleClass : public BaseClass
{
  public:
  ~SampleClass(); //소멸자
  SampleClass(const SampleClass& rh); //복사 생성자
  SampleClass& operator=(const SampleClass rh); //대입 연산자

  private:
    char* style;
};
SampleClass::~SampleClass()//소멸자
{
  delete[] style;
}
SampleClass::SampleClass(const SampleClass & rh) //복사 생성자
‌: BaseClass(rh)
{
‌style = new char[strlen(rh.style) + 1];
‌strcpy(style, rh.style);
}

SampleClass& SampleClass::operator=(const SampleClass & rh) //대입 연산자
{
‌if (this == &rh)
‌‌return *this;

‌BaseClass::operator=(rh); //rh의 기초 클래스 복사
‌delete[] style;
‌style = new char[strlen(rh.style) + 1];
‌strcpy(style, rh.style);

‌return *this;
}
```
아래 예제는 동적 메모리 대입과 프렌드를 이용하는 상속이다.
```cpp
class BaseClass
{
public:
‌...

‌friend ostream& operator<<(ostream& os, const BaseClass& rs);

private:
‌char* label;
‌int rating;

};

class SampleClass : public BaseClass
{
public:
‌...

‌friend ostream& operator<<(ostream& os, const SampleClass& rh);

private:
‌char* style;

};

ostream& operator<<(ostream& os, const BaseClass& rs)
{
‌os << "상표 : " << rs.label << endl;
‌os << "등급 : " << rs.rating << endl;

‌return os;
}

ostream& operator<<(ostream& os, const SampleClass& rh)
{
‌os << (const BaseClass&)rh; //강제 데이터형 변환
‌os << "스타일 : " << rh.color << endl;

‌return os;
}
```
파생클래스가 SampleClass의 friend이기 때문에 sstyle멤버에 접근이 가능하지만 기초 클래스의<br>
friend가 아님으로 label과 rating멤버에 접근 불가하다. 이를 해결하기 위해 기초 클래스에도 friend를 사용한다.
