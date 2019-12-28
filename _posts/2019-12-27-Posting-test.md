---
layout: post
title: "github.io 포스팅 테스트"
description: "markdown으로 작성된 포스트 테스트."
date: 2019-12-27
feature_image: images/test_image.png
tags: [compiter, programming language]
---
# C++

[위키독스](https://wikidocs.net/16468)

## cin cout override

typora를 썻을 때 과연 잘 될까 ?


```C++
\#include<iostream>

   

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
```

   
    
## default parameter

```C++
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
```

## define macro function

```C++
#include<iostream>
    
#define SQUARE(X) ((X)*(X))
    
int main(void){
        int a;
        std::cout << "type any integer to square" << std::endl;
        std::cin >> a;
        std::cout << "result : " << SQUARE(a) << std::endl;
        return 0;
}
```
## inline function

```C++
#include<iostream>
    
inline void helloWorld();
    
int main(void){
    helloWorld();
    return 0;
}
    
inline void helloWorld(){
    std::cout << "Hello World!" << std::endl;
}
```

`인라인` 함수는 실제로 함수를 실행하는 대신, 함수 안의 명령을 함수와 치환해서 실행한다. 
함수의 메모리영역인 스택영역 메모리를 참조하며 발생할 수 있는 시간낭비를 줄여줄 수 있음. 
매크로는 전처리기가 처리하지만 인라인함수는 컴파일러가 처리를 한다.
>전달되는 인자도 차이가 있는데, 매크로는 자료형의 제한이 없지만 인라인 함수는 자료형을 정해줄 수 있다는 것이다.
요즘엔 컴파일러가 알아서 인라인시 성능 향상여부를 판단해 처리하기때문에 크게 의미있지는 않다.

First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

😆🙂

~~호애애애액~~

- [  ] ㅎㅇㅎㅇ
- [x] ㅎㅇㅎㅇㅎㅇ


```javascript
function fancyAlert(arg) {
  if(arg) {
    $.facebox({div:'#foo'})
  }
}
```
# swift

```Swift
print()
enum Rank: Int{
    case ace = 1;                                                                           //explicitly set Integer value of enum element
    case two, three, four, five, six, seven, eight, nine, ten, jack, queen, king
    
    func description() -> String{
        switch self{
        case .ace:
            return "ace"
        case .jack:
            return "jack"
        case .queen:
            return "queen"
        case .king:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
    
    func isSpecial() -> String{
        switch self{
        case .ace, .jack, .queen, .king:
            return "special"
        default:
            return "not special"
        }
    }
}

let card1 = Rank.three
    print("\(card1.description()) and it is \(card1.isSpecial())")

if let card2 = Rank(rawValue: 11) {                                                             //rawValue is naive value of element in enum, it returns optional
    print("\(card2.description()) and it is \(card2.isSpecial())")
}
else{
    print("value is nil!!")
}

if let card3 = Rank(rawValue: 14){
    print("\(card3.description()) and it is \(card3.isSpecial())")
}
else{
    print("value is nil!!")
}

let card4: Rank? = Rank(rawValue: 12)
print("\(card4!.description()) and it is \(card4!.isSpecial())")                                  //it may be nil and if it is, it occurs fatal error
```
>스위프트 코드는 과연 잘 나올 것인가?!
잘 나오니..?
