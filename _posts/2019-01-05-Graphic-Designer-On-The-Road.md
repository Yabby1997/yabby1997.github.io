---
layout: post
title: "github.io 포스팅 테스트"
description: "markdown으로 작성된 포스트 테스트."
date: 2019-12-27
feature_image: images/road.jpg
tags: [compiter, programming language]
---
# C++

[위키독스](https://wikidocs.net/16468)

## cin cout override

    #include<iostream>
    
    void helloworld(void);
    void helloworld(int a, int b);
    
    int main(void){
        char temp[10];
        int a, b;
        std::cout << "input any string" << std::endl;
        std::cin >> temp;
        std::cout << "you typed : " << temp << std::endl;
        helloworld();
        std::cin >> a;
        std::cin >> b;
        helloworld(a, b);
        return 0;
    }
    
    void helloworld(void){
        std::cout << "HelloWorld" << std::endl;
        std::cout << "type two integer to add" << std::endl;
    }
    
    void helloworld(int a, int b){
        std::cout << "result : " << a + b << std::endl;
    }

## default parameter

    #include<iostream>
    
    int adder(int a = 1, int b = 2, int c = 3);
    
    int main(void){
        int a, b, c;
        std::cout << "type three integers to add" << std::endl;
        std::cin >> a;
        std::cin >> b;
        std::cin >> c;
        std::cout << "method with default values " << adder() << std::endl;
        std::cout << "method without default values " << adder(a, b, c) << std::endl;
        std::cout << "method with and without default values #1 " <<adder(a) << std::endl;
        std::cout << "method with and without default values #2 " <<adder(a, b) << std::endl;
        std::cout << "method with and without default values #3 " <<adder(a, b, c) << std::endl;
        return 0;
    }
    
    int adder(int a, int b, int c){
        return a + b + c;
    }

## define macro function

    #include<iostream>
    
    #define SQUARE(X) ((X)*(X))
    
    int main(void){
        int a;
        std::cout << "type any integer to square" << std::endl;
        std::cin >> a;
        std::cout << "result : " << SQUARE(a) << std::endl;
        return 0;
    }

## inline function

    #include<iostream>
    
    inline void helloWorld();
    
    int main(void){
        helloWorld();
        return 0;
    }
    
    inline void helloWorld(){
        std::cout << "Hello World!" << std::endl;
    }

인라인 함수는 실제로 함수를 실행하는 대신, 함수 안의 명령을 함수와 치환해서 실행한다. 
함수의 메모리영역인 스택영역 메모리를 참조하며 발생할 수 있는 시간낭비를 줄여줄 수 있음. 
매크로는 전처리기가 처리하지만 인라인함수는 컴파일러가 처리를 한다.
전달되는 인자도 차이가 있는데, 매크로는 자료형의 제한이 없지만 인라인 함수는 자료형을 정해줄 수 있다는 것이다.
요즘엔 컴파일러가 알아서 인라인시 성능 향상여부를 판단해 처리하기때문에 크게 의미있지는 않다.

## string and template

    #include<iostream>
    using std::string;
    
    template <typename T>
    
    T adder(T a, T b){
        return a + b;
    }
    
    int main(void){
        int a, b;
        string c, d;
        std::cout << "type two integers" << std::endl;
        std::cin >> a;
        std::cin >> b;
        std::cout << "you typed " << a << " and " << b << " the result is " << adder<int>(a, b) << std::endl;
        std::cout << "type two strings" << std::endl;
        std::cin >> c;
        std::cin >> d;
        std::cout << "you typed " << c << " and " << d << " the result is " << adder<string>(c, d) << std::endl;
        return 0;
    }

std 네임스페이스에 C++의 표준 함수가 들어가있다. 만약 using std를 처리하면 std::는 생략이 가능해짐. 그치만 권장되지는 않는다.
마찬가지로 string도 std안에 들어있어서 string을 사용하려면 std::string으로 해줘야함. 마찬가지로 권장되진 않지만 using std보단 using std::string이 낫다.

## namespace

    #include<iostream>
    
    using std::cin;
    using std::cout;
    using std::endl;
    
    namespace parents{
        void func();
        namespace son{
            void func();
        }
        namespace daughter{
            void func();
        }
    }
    
    std::string test = "GLOBAL";                                                    //VARIABLES CAN BE DEFINENED WITH SAME NAME BOTH GLOBALLY AND LOCALLY
    
    int main(void){
        std::string test = "LOCAL";
        
        cout << test << endl;
        cout << ::test <<endl; 
        
        parents::func();
        parents::son::func();
        parents::daughter::func();
        
        namespace seunghun = parents::son;                                          //NEW NAMESPACE CAN BE DEFINED
        seunghun::func();
        
        using namespace parents;
        func();
        son::func();
        daughter::func();
        
        return 0;
    }
    
    void parents::func(){
        cout << "PARENTS" << endl;
    }
    
    void parents::son::func(){
        cout << "SON" << endl;
    }
    
    void parents::daughter::func(){
        cout << "DAUGHTER" << endl;
    }

## basic reference

    #include<iostream>
    
    using std::cin;
    using std::cout;
    using std::endl;
    
    void swap(int& a, int& b);
    
    int main(void){
        int num1 = 10;
        int& num2 = num1;
        int num3 = 20;
        
        cout << num1 << endl;
        cout << num2 << endl;
        cout << num3 << endl;
        cout << &num1 << endl;
        cout << &num2 << endl;
        cout << &num3 << endl;
        
        num1++;
        cout << num1 << endl;
        cout << num2 << endl;
        cout << num3 << endl;
        cout << &num1 << endl;
        cout << &num2 << endl;
        cout << &num3 << endl;
        
        swap(num1, num3);
        cout << num1 << endl;
        cout << num2 << endl;
        cout << num3 << endl;
        cout << &num1 << endl;
        cout << &num2 << endl;
        cout << &num3 << endl;
        return 0;
    }
    
    void swap(int& a, int& b){
        int temp = b;
        b = a;
        a = temp;
    }

자료형 뒤에오는 &는 reference를 의미한다. 같은 메모리공간에 할당된 값에 다른 변수 명을 붙여줌. 이명이나 별명이라고 생각하면 됨.
두개의 변수지만 같은 메모리값을 참조하고, 참조되는 값이 변경되면 참조하는 값도 당연히 변한다.
주소연산자 &와는 다르다.
또 참조를 이용해 함수를 짜면 스택영역에 매개변수를 위한 메모리를 별도로 할당하지 않고 실제 변수에 직접 접근해서 더 효율적이다.

## return reference

    #include <iostream>
    
    using namespace std;
    
    int& RefRetFunc2(int& ref){
        ref++;
        return ref;
    }
    
    int main(void){
        int num1 = 1;
        int num2 = RefRetFunc2(num1);
        int& num3 = num1;
        int& num4 = num2;
        int num5 = num3;
    
        cout << "num1: " << num1 << endl;
        cout << "num2: " << num2 << endl;
        cout << "num3: " << num3 << endl;
        cout << "num4: " << num4 << endl;
        cout << "num5: " << num5 << endl;
        cout << "num1: " << &num1 << endl;
        cout << "num2: " << &num2 << endl;
        cout << "num3: " << &num3 << endl;
        cout << "num4: " << &num4 << endl;
        cout << "num5: " << &num5 << endl;
    
        num1 += 2;
        num2 += 100;
    
        cout << "num1: " << num1 << endl;
        cout << "num2: " << num2 << endl;
        cout << "num3: " << num3 << endl;
        cout << "num4: " << num4 << endl;
        cout << "num5: " << num5 << endl;
        cout << "num1: " << &num1 << endl;
        cout << "num2: " << &num2 << endl;
        cout << "num3: " << &num3 << endl;
        cout << "num4: " << &num4 << endl;
        cout << "num5: " << &num5 << endl;
    
        return 0;
    }

참조를 반환하거나 일반 변수에 참조변수를 집어넣으면 그 참조변수가 가진 값이 **복사**된다.

## conventional malloc and free methods

    #include<iostream>
    #include<string.h>
    
    char* createString(int length);
    
    using std::cout;
    using std::endl;
    
    int main(void){
        char* myString = createString(20);
        strcpy(myString, "I AM SO HAPPY");
        cout << myString << endl;
        cout << &myString << endl;
        free(myString);
        cout << myString << endl;
        cout << &myString << endl;
        return 0;
    }
    
    char* createString(int length){
        char* newString = (char*)malloc(sizeof(char) * length);
        return newString;
    }

## ... are replaceable with new and delete methods in C++

    #include<iostream>
    #include<string.h>
    
    char* createString(int length);
    
    using std::cout;
    using std::endl;
    
    int main(void){
        char* myString = createString(20);
        strcpy(myString, "I AM SO HAPPY");
        cout << myString << endl;
        cout << &myString << endl;
        delete[](myString);
        cout << myString << endl;
        cout << &myString << endl;
        return 0;
    }
    
    char* createString(int length){
        char* newString = new char[length];
        return newString;
    }

new []로 생성했으면 delete[]로 제거해준다.

## address , reference

    #include <iostream>
    
    using namespace std;
    
    
    int main(void)
    {
        int* ip = new int;                                                          //ADDRESS VARIABLE
        int& ref = *ip;                                                             //REFFERS ADDRESS VARIABLE
    
        *ip = 100;
    
        cout << "ip: " << ip << endl;                                               //ADDRESS
        cout << "ref: " << &ref << endl;                                            //VALUE(*) OF REF WHICH (INT&)REFFERS ADDRESS(&) OF IP -> VALUE OF IP
        cout << "*ip: " << *ip << endl;                                             //VALUE(*) OF ADDRESS OF IP
        cout << "ref: " << ref << endl;                                             //REFFERS ADDRESS OF IP
    
        delete ip;
        
        return 0;
    }

## structure & class

    #include<iostream>
    
    using std::cout;
    using std::cin;
    using std::endl;
        
    struct account{
        enum{
            NAME_LEN = 20,
            PW_LEN = 15,
        };
        char accountName[NAME_LEN];
        char accountPassword[PW_LEN];
        int accountNumber;
        int moneyInAccount;
        
        void deposit(int depositMoney);
        
        void changeName(){
            cin >> accountName;
        }
        
        inline void changePassword(){
            cin >> accountPassword;
        }
    };
    
    void showAllInformations(const account& thisAccount){
        cout << thisAccount.accountName << endl;
        cout << thisAccount.accountPassword << endl;
        cout << thisAccount.accountNumber << endl;
        cout << thisAccount.moneyInAccount << endl;
    }
    
    void account::deposit(int money){
        moneyInAccount += money;
    }
    
    int main(void){
        account seunghun { "seunghun", "abcdefg", 123456, 2100000000 };
        showAllInformations(seunghun);
        seunghun.changeName();
        showAllInformations(seunghun);
        seunghun.changePassword();
        showAllInformations(seunghun);
        seunghun.deposit(1);
        showAllInformations(seunghun);
        return 0;
    }

    #include<iostream>
    #include<string.h>
    
    using std::cout;
    using std::cin;
    using std::endl;
        
    class account{
    private :
        enum{
            NAME_LEN = 20,
            PW_LEN = 15,
        };
        char accountName[NAME_LEN];
        char accountPassword[PW_LEN];
        int accountNumber;
        int moneyInAccount;
    
    public :
        void createAccount(const char* newName, const char* newPW, int newAN, int newDeposit);
        void deposit(int depositMoney);
        void showAllInformations();
        
        void changeName(){
            cin >> accountName;
        }
        
        inline void changePassword(){
            cin >> accountPassword;
        }
    };
    
    void account::showAllInformations(){
        cout << accountName << endl;
        cout << accountPassword << endl;
        cout << accountNumber << endl;
        cout << moneyInAccount << endl;
    }
    
    void account::createAccount(const char* newName, const char* newPW, int newAN, int newDeposit){
        strcpy(accountName, newName);
        strcpy(accountPassword, newPW);
        accountNumber = newAN;
        moneyInAccount = newDeposit;
    }
    
    void account::deposit(int money){
        moneyInAccount += money;
    }
    
    int main(void){
        account seunghun;
        seunghun.createAccount("seunghun", "1234", 9988000188, 199);
        seunghun.showAllInformations();
        seunghun.changeName();
        seunghun.showAllInformations();
        seunghun.changePassword();
        seunghun.showAllInformations();
        seunghun.deposit(1);
        seunghun.showAllInformations();
        return 0;
    }

구조체와 클래스는 유사하면서도 다른 개념이다. 
...하지만 C++에서는 거의 같다고 볼 수 있는데, 선언이나 구현 모두 같고 다만 각 멤버에 대한 접근제한의 유무가 주요 차이점이 된다.

공통점 :
내부에서 enum, define, 함수를 정의할 수 있음
내부에서 함수 정의시 자동으로 inline함수가 되고, 외부에서 정의시 명시적으로 inlnine을 처리해주지 않으면 그냥 일반적인 함수가 됨.
다양한 방식으로 함수 구현이 가능함. 외부에서 선언/구현하고 const reference를 인자로 받는법,  구조체 내부에서 선언하고 구조체 외부에서 구현하는방법(인자없이 참조사용), 구조체 내부에서 일반/인라인 함수로 구현하는 방법이 있다.

차이점 :
구조체의 멤버는 모두에게 접근이 열려있는 public으로 고정되어있지만, 클래스에서는 각 멤버에 접근제한을 걸어줄 수 있다. (public, private, protected)

## header.h

    #ifndef HEADER_H
    #define HEADER_H
    
    //WRITE HEADER CODE HERE
    
    #endif

헤더파일 작성시 헤더파일이 여러번 include되면 코드가 꼬이게된다.
따라서 헤더파일의 중복 include를 피하기위해 헤더파일에 전처리를 해주는것이 일반적이다.
위와같이 전처리를 해주면 해당 헤더파일이 include되었는지 확인 하고 컴파일한다.
inline함수는 헤더 내에서 정의되어야한다.

## const function

    //IN HEADER
    void func() const;
    
    //IN CPP
    void func() const{
    		...
    }

헤더에서 함수를 const로 선언하게되면, 함수는 해당 객체의 멤버변수를 조정할 수 없으며, 내부에서 다른 함수를 호출해서 간접적으로 멤버 변수의 값을 변경할 위험이 있기때문에 함수 내에서 호출 가능한 함수도 const함수로 제한된다. (Read Only)

## constructor & destructer

    #include<iostream>
    
    class Player{
    private :
        int number;
        int age;
    public :
        Player();
        Player(int newNumber);
        Player(int newNumber, int newAge);
        void setNumber(int newNumber);
        void setAge(int newAge);
        void getNumber() const;
        void getAge() const;
        void showInfo() const;
        ~Player();
    };
    
    Player::Player(){
        number = 0;
        age = 20;
    }
    
    Player::Player(int newNumber){
        number = newNumber;
        age = 20;
    }
    
    Player::Player(int newNumber, int newAge){
        number = newNumber;
        age = newAge;
    }
    
    Player::~Player(){
        std::cout << "Destructor Called" << std::endl;
    }
    
    void Player::setNumber(int newNumber){
        number = newNumber;    
    }
    
    void Player::setAge(int newAge){
        age = newAge;
    }
    
    void Player::getNumber() const{
        std::cout << "getNumber" << std::endl;
        std::cout << number << std::endl;
        std::cout << "********" << std::endl;
    }
    
    void Player::getAge() const{
        std::cout << "getAge" << std::endl;
        std::cout << age << std::endl;
        std::cout << "********" << std::endl;
    }
    
    void Player::showInfo() const{
        std::cout << "showInfo" << std::endl;
        std::cout << number << std::endl;
        std::cout << age << std::endl;
        std::cout << "********" << std::endl;
    }
    
    int main(void){
        Player A;
        Player B(10);
        Player C(30, 50);
        A.showInfo();
        A.getAge();
        A.getNumber();
        B.showInfo();
        B.setNumber(999);
        C.showInfo();
        C.setAge(999);
        B.showInfo();
        C.getAge();
        A.~Player();
        A.showInfo();
        B.showInfo();
        C.showInfo();
        return 0;
    }

생성자를 변수랑 함수 짬뽕처럼 사용함.
생성자도 함수이기떄문에 오버라이드가 가능하다.
생성자와 동일한 이름으로 앞에 ~ 를 붙여주면 소멸자가 된다. (띠용)
`Player A = Player();`와 동일하게 생성자를 호출 할 수도 있다.

## this

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class House{
    private:
        int squareMeter;
        int level;
    public :
        House(){
            squareMeter = 30;
            level = 1;
        }
        House(int newSquare, int newLevel){
            this->squareMeter = newSquare;
            this->level = newLevel;
        }
        void showInfo(){
            cout << "address : " << this << endl;
            cout << "square meter : " << this->squareMeter << endl;
            cout << "level : " << this->level << endl;
        }
    };
    
    int main(void){
        House houseA;
        House houseB(100, 4);
        House& houseC = houseB;
        houseA.showInfo();
        houseB.showInfo();
        houseC.showInfo();
        cout << "Manual Address of house A : " << &houseA << endl;
        cout << "Manual Address of house B : " << &houseB << endl;
        cout << "Manual Address of house C : " << &houseC << endl;
        return 0;
    }

this를 사용해서 객체의 메모리 주소에 접근 가능.
객체도 레퍼런스 변수처럼 사용가능
파라미터 없는 생성자의 경우 ()를 써주면 함수의 선언과 같은 형태가 되므로 사용이 제한됨 (오류 발생)

## self-reference return

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class object{
    private : 
        int value;
    public :
        object(int newValue){
            this->value = newValue;
        }
        
        object& selfAdd(int operandValue){
            this->value = this->value + operandValue;
            return *this;
        }
        
        void showInfo(){
            cout << "ADDRESS : " << this << endl;
            cout << "VALUE : " << this->value <<endl;
        }
    };
    
    int main(void){
        object a(3);
        a.showInfo();
        object& b = a.selfAdd(5);
        a.showInfo();
        b.showInfo();
        a = b.selfAdd(8);
        a.showInfo();
        b.showInfo();
        return 0;
    }

클래스 메소드가 스스로의 레퍼런스를 반환할 수 있다.

## copy constructor & shallow copy

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class object{
    private : 
        int value;
    public :
        object(){
            this->value = 0;
        }
        object(int newValue){
            this->value = newValue;
        }
        //YOU CAN OVERRIDE COPY-CONSTROCTOR BY DEFINING EXPLICITLY
        object(const object &target){
            this->value = target.value + 1;
        }
        void showInfo(){
            cout<< "adrs : " << this << endl;
            cout<< "vlue : " << this->value<< endl;
        }
    };
    
    int main(void){
        object A(3);
        object B = object();
        object C(A);
        object D;
        D = B;
        A.showInfo();
        B.showInfo();
        C.showInfo();
        D.showInfo();
        return 0;
    }

객체에 객체를 대입하거나 객체를 생성할때 인자로 다른 객체를 넣어줌으로써 복사생성을 할 수 있다. 이경우는 멤버의 1대 1 복사가 되는것이므로 레퍼런스와는 다르게 다른 메모리 공간에 객체와 그 멤버 변수들이 복사가 되는것이다.
복사생성자를 명시적으로 정의해주지 않으면 암묵적으로 멤버 변수간의 복사가 이뤄지고, 명시적으로 정의시 입맛에 맞게 약간의 기능변경이 가능하다.
다만 그렇게 변경을해도 대입 방식의 복사생성에는 효력이 없음.

## explicit key-word

[불로그 : 네이버 블로그](https://blog.naver.com/kyed203/220172767137)

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class object{
    private :
        int value;
    public : 
        explicit object(int newValue){
            value = newValue;
        }
        void showInfo(){
            cout << "adrs : " << this << endl;
            cout << "vlue : " << this->value << endl;
        }
    };
    
    int main(void){
        object A(3);                    //NORMAL CONSTRUCTOR
        A.showInfo();
        object B(A);                    //COPY CONSTRUCTOR
        B.showInfo();
        object C = object(10);          //ANOTHER EXPRESSION
        C.showInfo();
        object D = (object)12;          //EXPLICIT
        D.showInfo();
        /*
        object E = 10;                   //IMPLICIT
        E.showInfo();
        */
        return 0;
    }

CPP에서 생성자는 다양한 방식으로 호출될 수 있는데, 특이하게도 (python 처럼) 변수의 형태로 호출이 가능하다. (암묵적 복사생성)
다만 CPP에서는 생성자를 다양한 매개변수에대해 오버라이드하면 컴파일러가 알아서 알맞는 변수에 따라 객체를 생성하기 때문에, 변수처럼 호출하는 경우 원하지 않는 값의 자료형으로 객체가 생성되는 경우가 발생할 수 있다. 이를 방지하기위해서 생성자의 앞에 explicit 키워드를 붙여주면 암묵적인 생성을 불가능하게 할 수 있다.

## shallow copy

## deep copy

    #include<iostream>
    #include<cstring>
    
    using std::cout;
    using std::endl;
    
    class student{
    private :
        char* name;
        int age;
    public :
        student(){
            name = "";
            age = 19;
        }
        
        student(char* newName, int newAge){
            int len = strlen(newName) + 1;
            name = new char[len];
            strcpy(name, newName);
            age = newAge;
        }
        /*
        student(const student& copy){
            int len = strlen(copy.name) + 1;
            name = new char[len];
            strcpy(name, copy.name);
            age = copy.age;
        }
        */
        void changeName(char* newName){
            delete [] name;
            int len = strlen(newName) + 1;
            name = new char[len];
            strcpy(name, newName);
        }
        
        void showInfo(){
            cout << "NAME : " << name << endl;
            cout << "AGE : " << age << endl;
        }
        
        ~student(){
            delete [] name;
            cout << "DESTRUCTOR CALLED" << endl;
        }
    };
    
    int main(void){
        student a("apple", 23);
        student b(a);
        a.showInfo();
        b.showInfo();
        a.changeName("lemon");
        a.showInfo();
        b.showInfo();
        return 0;
    }

> [출력]
NAME : apple
AGE : 23
NAME : apple
AGE : 23
NAME : lemon
AGE : 23
NAME : lemon
AGE : 23
DESTRUCTOR CALLED
*** Error in `./a.out': double free or corruption (fasttop): 0x0000000001d8ec20 ***
Aborted (core dumped)

    #include<iostream>
    #include<cstring>
    
    using std::cout;
    using std::endl;
    
    class student{
    private :
        char* name;
        int age;
    public :
        student(){
            name = "";
            age = 19;
        }
        
        student(char* newName, int newAge){
            int len = strlen(newName) + 1;
            name = new char[len];
            strcpy(name, newName);
            age = newAge;
        }
        
        student(const student& copy){
            int len = strlen(copy.name) + 1;
            name = new char[len];
            strcpy(name, copy.name);
            age = copy.age;
        }
        
        void changeName(char* newName){
            delete [] name;
            int len = strlen(newName) + 1;
            name = new char[len];
            strcpy(name, newName);
        }
        
        void showInfo(){
            cout << "NAME : " << name << endl;
            cout << "AGE : " << age << endl;
        }
        
        ~student(){
            delete [] name;
            cout << "DESTRUCTOR CALLED" << endl;
        }
    };
    
    int main(void){
        student a("apple", 23);
        student b(a);
        a.showInfo();
        b.showInfo();
        a.changeName("lemon");
        a.showInfo();
        b.showInfo();
        return 0;
    }

> [출력]
NAME : apple
AGE : 23
NAME : apple
AGE : 23
NAME : lemon
AGE : 23
NAME : apple
AGE : 23
DESTRUCTOR CALLED
DESTRUCTOR CALLED

객체 내부적으로 포인터 변수는 같은 메모리를 참조하고있다.
`changeName()`함수에서 기존 변수의 메모리공간을 free해주고 새로운값을 넣어주니 참조하는 값도 정상적으로 바뀐다. 
(이전에는 delete를 안해줬기떄문에 메모리공간이 추가적으로 할당되어 거기에 값이 저장된듯.)
이후에 copy constructor를 명시해서 포인터를 복사해주는게 아니라 값만 넣어주는식으로 복사를 하도록 하니 결과적으로 다른 메모리공간을 참조하게 되는듯.
결괏값에 오류가 나오는 이유는 이미 free된 공간을 참조하는 변수를 가진 객체를 다시 free하려고 하니 문제가 생긴 것.
디버거로 확인해보니 맞다. copy constructor를 정의하지 않은경우 객체는 새로운 메모리공간에 할당되지만, 내부적으로 그 값은 복사된 객체의 포인터값을 가지고 있다.

객체의 멤버의 주소를 알아내는건 그럼 어떻게..?

## object copy by function parameter

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class data{
    private :
        int value;
    public :
        data(int newValue){
            value = newValue;
        }
        
        ~data(){
            cout << "DESTRUCTOR CALLED" << endl;    
        }
        
        void showData(){
            cout << "VALUE : " << this->value << endl;
        }
    };
    
    void showData_Parameter(data dt){
        cout << "SHOW DATA OF PARAMETER" << endl;
        dt.showData();
    }
    
    int main(void){
        data a(3);
        a.showData();
        showData_Parameter(a);
        return 0;
    }

> VALUE : 3
SHOW DATA OF PARAMETER
VALUE : 3
DESTRUCTOR CALLED
DESTRUCTOR CALLED

소멸자가 두번 실행되는걸 확인할 수 있다.
객체가 외부의 함수의 파라미터로 전달되며 객체가 복사된다.

### reference return, copy by parameter etc..

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class data{
    private :
        int value;
    public :
        data(int newValue){
            value = newValue;
        }
        
        ~data(){
            cout << this << " DESTRUCTOR CALLED" << endl;    
        }
        
        void showData(){
            cout << this << " VALUE : " << this->value << endl;
        }
        
        data& addAndRefer(int operand){
            value = value + operand;
            return *this;
        }
    };
    
    data copyByParameter(data _data){
        return _data;
    }
    
    int main(void){
        data a(3);
        data b(a);
        data c = copyByParameter(b);
        data d = b.addAndRefer(5); 
        a.showData();
        b.showData();
        c.showData();
        d.showData();
        return 0;
    }

> 0x7ffcd26dd76c DESTRUCTOR CALLED
0x7ffcd26dd760 VALUE : 3
0x7ffcd26dd764 VALUE : 8
0x7ffcd26dd768 VALUE : 3
0x7ffcd26dd76c VALUE : 8
0x7ffcd26dd76c DESTRUCTOR CALLED
0x7ffcd26dd768 DESTRUCTOR CALLED
0x7ffcd26dd764 DESTRUCTOR CALLED
0x7ffcd26dd760 DESTRUCTOR CALLED

### const object and const m

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class data{
    private : 
        int value;
    public :
        data(int newValue){
            value = newValue;
        }
        
        void add(int operand){
            value = value + operand;
        }
        
        void showInfo() const{
            cout << value << endl;
        }
    };
    
    int main(void){
        data a(3);
        const data b(3);
        a.showInfo();
        b.showInfo();
        a.add(3);
        //b.add(3).
        a.showInfo();
        b.showInfo();
        return 0;
    }

const 객체는 const 함수 만 사용이 가능하다.
const 객체의 멤버 변수의 값을 바꿀 수 없다.

### friend class

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class one;
    class two;
    
    class one{
    friend class two;
    private : 
        int value;
    public :
        one(){
            value = 1;
        }
        void friendInfo(two& tw);
        void showInfo(){
            cout << this->value << endl;
        }
    };
    
    class two{
    private :
        int value;
    public :
        two(){
            value = 2;
        }
        void friendInfo(one& on);
        void showInfo(){
            cout << this->value << endl;
        }
    friend class one;
    };
    
    void one::friendInfo(two& tw){
        cout << tw.value << endl;
    }
    void two::friendInfo(one& on){
        cout << on.value << endl;
    }
    
    int main(void){
        one a;
        two b;
        a.showInfo();
        b.showInfo();
        a.friendInfo(b);
        b.friendInfo(a);
        return 0;
    }

클래스 내부 어디에서나 friend class를 지정해주면 해당 클래스에서 친구 클래스의 private에 접근할 수 있게된다.

### friend method

    #include<iostream>
    #include<cstring>
    
    using std::cout;
    using std::endl;
    
    class kitty{
    private :
        char * name;
        int age;
    public :
        kitty(char* newName, int newAge){
            int len = strlen(newName) + 1;
            name = new char[len];
            strcpy(name, newName);
            age = newAge;
        }
        
        friend void showInfo(kitty& cat);
    };
    
    void showInfo(kitty& cat){
        cout << cat.name << endl;
        cout << cat.age << endl;
    }
    
    int main(void){
        kitty a("Kiki", 2);
        showInfo(a);
        return 0;
    }

friend method는 절대 member function이 아니다. 
인자로 레퍼런스를 줘야함.  기능 구현도 클래스 외부에서 이뤄지는 등 아무튼 멤버 메소드랑 혼동하기 좋지만 멤버 메소드가 아님.
객체 내부의 멤버변수에 접근이 가능한것 뿐. 멤버 함수가 아닌것에 주의.

### static variable

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    void staticCounter(){
        static int count;
        cout << count++ <<endl;
    }
    
    void nonStaticCounter(){
        int count;
        cout << count++ <<endl;
    }
    
    int main(void){
        for(int i = 0; i < 5; i++){
            staticCounter();
            nonStaticCounter();
        }
        return 0;
    }

정적 변수는 함수/객체에 관여되지 않는다. 정적으로 메모리에 저장되어있어 함수가 종료되거나 객체가 소멸되도 남아있게된다. main함수의 종료와 함께 소멸된다.

### static object

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class test{
    public :
        test(){
            cout << this << " CREATED" <<endl;
        }
        ~test(){
            cout << this << " DESTROYED" <<endl;
        }
    };
    
    void createObject(){
        static test testObject;
    }
    
    int main(void){
        int x = 0;
        if(x == 0){
            test a;
            createObject();
        }    
        return 0;
    }

> 0x7fff0f0e286b CREATED
0x6021b9 CREATED
0x7fff0f0e286b DESTROYED
0x6021b9 DESTROYED

정적 객체는 바로 소멸되지 않는다. a오브젝트는 if문이 끝남과 동시에 소멸됐지만 정적 객체인 testObject는 main함수의 종료와 함께 소멸된다.

### static member variable

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class test{
    public :
        static int id;
        test(){
            cout<<this<<" CREATED"<<endl;
        }
        void showInfo(){
            cout<<this<<" "<<this->id<<endl;
        }
    };
    
    int test::id = 999;
    
    int main(void){
        test a;
        test b;
        test c;
        a.showInfo();
        b.showInfo();
        c.showInfo();
        return 0;
    }

정적 멤버 변수는 클래스의 선언에서 정적으로 선언된 멤버변수를 의미.
정적 멤버변수로 선언이되면, 모든 객체가 동일한 값을 갖는다. 생성자는 이걸 무시해도 됨.
선언은 클래스 안에서, 값을 정의하는건 클래스 밖에서.

### static method

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class test{
    public :
        static int id;
        static void idAdder(){
            id++;   
        }
        test(){
            cout<<this<<" CREATED"<<endl;
        }
        void showInfo(){
            cout<<this<<" "<<this->id<<endl;
        }
    };
    
    int test::id = 999;
    
    int main(void){
        test a;
        test b;
        test c;
        a.showInfo();
        b.showInfo();
        c.showInfo();
        test::idAdder();
        a.showInfo();
        b.showInfo();
        c.showInfo();
        return 0;
    }

정적 메소드는 정적 멤버 변수나 다른 정적 메소드에만 접근 가능하다.
당연히 this에도 접근 불가. 애초에 this라는 개념이 적용되는 범위가 아님.
함수를 호출할땐 클래스이름::정적메소드이름 으로 호출.

### mutable key-word

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class testObject{
    private :
        int valueA;
        mutable int valueB;
        int valueC;
    
    public : 
        testObject(int vA, int vB, int vC) : valueA(vA), valueB(vB), valueC(vC) {}
        
        void showData(){
            cout << valueA << endl;
            cout << valueB << endl;
        }
        
        void copyAB(){
            valueA = valueB;
        }
        
        void copyBC() const{
            valueB = valueC;
        }
    };
    
    int main(void){
        testObject a(1, 2, 3);
        a.showData();
        a.copyAB();
        a.showData();
        a.copyBC();
        a.showData();
        return 0;
    }

const함수 내에서는 const 변수를 읽을수만 있지만, mutable 키워드가 붙은 변수에 한해서는 값을 변경할 수 있다.

### differences between `::` `.` `->`

[What is the difference between "::" "." and "->" in c++](https://stackoverflow.com/questions/11902791/what-is-the-difference-between-and-in-c)

    ClassName::FieldName
    ClassInstance.FieldName
    ClassPointer->FieldName

클래스네임이나 네임스페이스에서 필드로 접근할떄 `::`
인스턴스화 된 객체에서 객체의 필드에 접근할때 `.`
포인터에서 해당 포인터 내부의 필드에 접근할때 `->`

## inheritance and range

    #include<iostream>
    #include<cstring>
    
    using std::cout;
    using std::cin;
    using std::endl;
    
    class Student{
    protected :
        char* name;
        int age;
    public : 
        Student(const char* newName, int newAge) : age(newAge) {
            int len = strlen(newName) + 1;
            name = new char[len];
            strcpy(name, newName);
            cout<<"[DEBUG] Student Created"<<endl<<"name : "<<name<<endl<<"age : "<<age<<endl;
        }
        ~Student(){
            cout<<"[DEBUG] Student Destroyed"<<endl<<"name : "<<name<<endl<<"age : "<<age<<endl;
            delete [] name;
        }
        void personalInfo(){
            cout<<"name : "<<name<<endl<<"age : "<<age<<endl;
        }
    };
    
    class Elementary : protected Student{
        char* schoolName;
        int grade;
    public :
        Elementary(const char* newName, int newAge, const char* newSchool, int newGrade) : Student(newName, newAge), grade(newGrade){
            int len = strlen(newSchool) + 1;
            schoolName = new char[len];
            strcpy(schoolName, newSchool);
            cout<<"[DEBUG] Elementary Created"<<endl<<"school : "<<schoolName<<endl<<"grade : "<<grade<<endl;
        }
        ~Elementary(){
            cout<<"[DEBUG] Elementary Destroyed"<<endl<<"school : "<<schoolName<<endl<<"grade : "<<grade<<endl;
            delete [] schoolName;
        }
        void studentInfo(){
            personalInfo();
            cout<<"test : "<<this->name<<endl;
            cout<<"school : "<<schoolName<<endl<<"grade : "<<grade<<endl;
        }
    };
    
    int main(void){
        Student a("Peter", 21);
        a.personalInfo();
        Elementary b("Peterson", 13, "Bibong", 6);
        b.studentInfo();
    }

private 상속의 경우 접근 불가
protected 상속의 경우 접근 (public, protected)가능, 어떤 형태로 접근하냐에 따라 접근 범위가 달라짐 
public 상속의 경우 접근 (public, protected)가능, 어떤 형태로 접근하냐에 따라 접근 범위가 달라짐
기본적으로 public상속을 가장 많이씀
public은 객체와무관하게 클래스 외부에서도 접근이 가능한 차이가 있음.

## virtual keyword

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class Mother{
    public :
        virtual void whoAmI(){
            cout<<"mother"<<endl;
        }
    };
    
    class Child : public Mother{
    public :
        void whoAmI(){
            cout<<"child"<<endl;
        }
    };
    
    void introduce(Mother *_this){
        _this->whoAmI();
    }
    
    int main(void){
        Mother *a = new Mother;
        Child *b = new Child;
        a->whoAmI();
        b->whoAmI();
        introduce(a);
        introduce(b);
        return 0;
    }

자바에서는 상속시 함수 오버라이드에 대해 딱히 개발자가 신경써줄 부분이 없다.  C++에서도 자바처럼 상속을 하면 기본적인 함수의 오버라이드엔 문제가 없다.
다만 C의 특징인 포인터, 주소를 직접 가리킬 수 있다는 점 때문에 상속받은 객체를 포인터로 생성하고 오버라이드 된 함수를 다른 함수의 인자를 통해 간접적으로 실행하게되면, 
포인터가 가르키는 주소가 부모 클래스의 함수 주소이기때문에, 오버라이드를 했다고 해도 원하는 결과가 나오지 않게된다. 이를 방지하기위해 virtual키워드가 존재한다.
오버라이드 될 함수의 앞에 virtual키워드를 붙이게되면, 컴파일러가 문제를 해결한다.
위 코드에서는 `Child`클래스의 `whoAmI()`가 `Mother`클래스의 그것과 같기때문에 `Mother`클래스의 해당 함수 앞에 `virtual`키워드를 붙여줌으로써 해결했다.

## virtual destroyer

    #include<iostream>
    
    using std::cout;
    using std::endl;
    
    class Mother{
    public :
        virtual void whoAmI(){
            cout<<"mother"<<endl;
        }
        virtual ~Mother(){
            cout<<"MOTHER DESTROYED"<<endl;
        }
    };
    
    class Child : public Mother{
    public :
        void whoAmI(){
            cout<<"child"<<endl;
        }
        ~Child(){
            cout<<"CHILD DESTROYED"<<endl;
        }
    };
    
    void introduce(Mother *_this){
        _this->whoAmI();
    }
    
    int main(void){
        Mother *a = new Mother;
        Child *b = new Child;
        Mother *c = new Child;
        a->whoAmI();
        b->whoAmI();
        c->whoAmI();
        introduce(a);
        introduce(b);
        introduce(c);
        a->~Mother();
        b->~Mother();
        c->~Mother();
        return 0;
    }

소멸자를 virtual처리 해주지 않으면, 부모 객체만 소멸되고 자식 객체는 소멸되지 않는다 → 메모리 누수가 발생한다.

## does member methods included in class?

그렇지는 않다. 강지훈 컴프 때 했던 것처럼 함수에서 매개변수로 구조체의 포인터를 받아서 함수를 실행하던것과 비슷한방식으로 내부에서 동작된다. ← 같은 클래스 객체의 함수들, 상속받은 객체의 함수들은 결국 같은 위치를 참조한다는 것. 그렇다면 가상함수는? 그래서 가상함수 테이블이라는게 존재한다. 가상함수 테이블이 오버라이드 된 가상함수들의 테이블을 만들어, 함수가 호출될 때 적절한 함수가 호출될 수 있도록 관리하는데 사용한다. 
아마 C언어를 기반으로해서 객체지향이라는 개념이 없는걸 강지훈 컴프처럼 포인터와 키워드 전처리를 통해 정의한 것 같음.
그냥 그렇다고 생각하는게 이해에는 편할 것. 

배열을 템플릿으로 재정의 한 것.

## STL Container

### Container

동일한 타입의 객체의 저장/관리를 위해 만들어진 추상화 클래스. 각 컨테이너마다 멤버 함수가 있음.

### standard sequence container

표준시퀀스컨테이너(선형)
배열 기반의 컨테이너 vector, list, string, deque 하나의 메모리블럭에 데이터 저장, 단순 저장에 효율적, 검색에 비효율적, 일반 배열과 달리 자동으로 메모리 추가할당/제거로 메모리 관리에 유리함
표준연관컨테이너(비선형)
노드 기반의 컨테이너 set, map, multiset, multimap 여러 메모리블럭에 데이터 저장, 단순 저장에 비효율, 검색에 효율적, 
컨테이너어댑터
stack, queue, priority_queue(heap)

### iterator

위치를 나타내줌. 포인터랑 비슷하다?

## Vector

    #include<iostream>
    #include<vector>
    #include<algorithm>
    
    using std::endl;
    using std::cout;
    using std::cin;
    using std::vector;
    using std::greater;
    using std::less;
    
    void print(int n){
        cout<<"@ITERATOR@"<<n<<endl;
    }
    
    int main(void){
        vector<int> testVector;
        
        for(int i = 0; i < 10; i++){
            testVector.push_back(i);
            cout<<"@PUSH_BACK@ "<<endl;
            cout<<"@AT"<<i<<"@ "<<testVector.at(i)<<endl;
        }
        
        cout<<"@FRONT@ "<<testVector.front()<<endl;
        cout<<"@BACK@ "<<testVector.back()<<endl;
        cout<<"@SIZE@ "<<testVector.size()<<endl;
        cout<<"@EMPTY@ "<<testVector.empty()<<endl;
        cout<<"@DATA@ "<<testVector.data()<<endl;
        for_each(testVector.begin(), testVector.end(), print);
        cout<<"@GREATER@"<<endl;
        sort(testVector.begin(), testVector.end(), greater<int>());
        for_each(testVector.begin(), testVector.end(), print);
        cout<<"@LESS@"<<endl;
        sort(testVector.begin(), testVector.end(), less<int>());
        for_each(testVector.begin(), testVector.end(), print);
        return 0;
    }

## Deque

    #include<iostream>
    #include<deque>
    #include<algorithm>
    
    using std::cout;
    using std::endl;
    using std::cin;
    using std::deque;
    
    void print(int n){
        cout<<n<<endl;
    }
    
    void printDeque(deque<int> d){
        cout<<endl;
        cout<<"@SIZE@"<<d.size()<<endl;
        cout<<"@FRONT@"<<d.front()<<endl;
        cout<<"@BACK@"<<d.back()<<endl;
        for_each(d.begin(), d.end(), print);
    }
    
    int main(void){
        deque<int> d(5, 1);
        printDeque(d);
        d.push_back(1);
        printDeque(d);
        d.push_front(19);
        printDeque(d);
        for(int i = 0; i < d.size(); i++){
            d[i] = d[i] + i;
        }
        printDeque(d);
        sort(d.begin(), d.end());
        printDeque(d);
        sort(d.begin(), d.end(), std::greater<int>());
        printDeque(d);
    }

vector와 유사하지만 몇가지 다른점이 존제한다 :
push_front를 통해 배열 앞에 요소 삽입이 가능하다.
capacity 고갈 시 메모리 할당 방식이 다르다. (vector는 메모리 재할당, deque는 chunk를 생성)
vector와 달리 연속 메모리공간이 아니라 포인터 연산에 알맞지 않다.

## list

    #include<iostream>
    #include<list>
    #include<algorithm>
    
    using std::cout;
    using std::endl;
    using std::list;
    
    using std::cout;
    using std::endl;
    using std::list;
    
    bool rangeCheck(int num){
        return num>=100;
    }
    
    void showList(list<int> lt){
        list<int>::iterator iter;
        cout<<endl<<lt.size()<<endl;
        for(iter = lt.begin(); iter != lt.end(); iter++){
            cout<<*iter<<" ";
        }
        cout<<endl;
    }
    
    int main(){
        list<int> lt;
        lt.push_back(5);
        lt.push_back(13);
        lt.push_back(25);
        lt.push_front(60);
        lt.push_front(32);
        lt.push_front(44);
        lt.push_back(57);
        lt.push_front(60);
        lt.push_front(60);
        lt.push_back(71);
        lt.push_front(88);
        lt.push_back(92);
        lt.push_front(103);
        lt.push_front(111);
        lt.push_back(5);
        lt.push_front(5);
        showList(lt);
        
        lt.sort();
        showList(lt);
        
        lt.remove(60);
        showList(lt);
        
        lt.remove_if(rangeCheck);
        showList(lt);
        
        lt.unique();
        showList(lt);
        
        return 0;
    }
    

[[C++] list container 정리 및 사용법](https://blockdmask.tistory.com/76)

vector, deque와는 조금 사용법이 다르다.
deque는 배열이 아닌 DLL을 이용하기때문에 index를 통한 접근이 불가능하다. 

[vector, deque, list 간단 비교 정리](http://egloos.zum.com/sweeper/v/2817817)

    class Solution {
    public:
        int myAtoi(string str) {
            bool isNegative = false;
            str.erase(remove(str.begin(), str.end(), ' '), str.end());
            
            if(str[0] == '-'){
                str.erase(0, 1);
                isNegative = true;
            }
            
            if(str.size() == 0)
                return 0;
            
            else if(str[0] < 48 || str[0] > 57)
                return 0;
            
            for(std::string::iterator i = str.begin(); i < str.end(); i++){
                if(*i < 48 || *i > 57)
                    str.erase(i--);
            }
            
            double result = (isNegative == false) ? stod(str) : stod(str) * -1;
            if(result > 2147483647)
                return 2147483647;
            else if(result < -2147483648)
                return -2147483648;
            else
                return int(result);
        }
    };
