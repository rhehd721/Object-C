# Object-C

## 기본정보
- 작명규칙
    - 클래스 : 대문자로 시작하는 명사형
    - 인스턴스/멤버면수 : 소문자로 시작하는 명사형
    - 메소드/함수 : 소문자로 시작하는 동사형
- 상속으로 메서드를 제거하거나 삭제할 순 없지만 '재정의'하여 메서드의 정의를 변경할 수는 있다.
    - 메소드를 호출 할 경우 해당 클래스에서 메서드를 찾은 후 찾지 못한 경우 상위 클래스에서 메서드를 찾아 호출하기 때문
- objc 장점
    - 다형성(Override)이 가능하다.
    - 동적 타이핑, 동적 바인딩이 가능하다.

## nil
- 아무것도 가르키지 않는 상태

## NS 원시형 
- NSInteger
- NSUInteger

## 객체, 구조체 반환
- 객체
```objc
-(NSString *)uppercaseString;
```
- 구조체
```objc
-(NSString)uppercaseString;
```
- EX
```objc
NSString *str = @"Hello ObjC";
NSUInteger len = [str length];
NSLOG(@"Length of str is : %lu",len);   // @문자열 == NSString 객체
```

## 객체생성
```objc
// Origin
[["ClassName" alloc] init]
// ex
NSObject *obj = [[NSObject alloc] init];
NSLog(@"obj : %@", obj);    // 주소 출력
```

## 파라미터가 정의된 함수(메소드) 호출
```objc
NSString *str = @"1";
NSComparisonResult result = [str compare:@"9"];
NSLog(@"Result : %ld", result);
```

## 클래스

### 선언
- 형식
```objc
@interface ClassName : SuperClass
@end
```
- Test.h
```objc
// @interface에선 상속, 변수선언, 함수선언등의 작업이 이루어 진다.
@interface Rectangle : NSObject{
    int height;
}
-(int)size;
@end
```

### 구현
- 형식
```objc
@implementation ClassName
@end
```
- Test.m
```objc
// @implementation에선 선언했던 함수의 구현이 이루어 진다.
#import "Test.h"
@implementation Rectangle{
    int width;
}
-(int)size{
    return width*height;
}
@end
```

## 인스턴스 메소드 VS 클래스 메소드

||인스턴스 메소드|클래스 메소드|
|------|---|---|
|구별하기|- 기호로 시작|+ 기호로 시작|
|메세지 리시버|객체|클래스|
|멤버 변수 접근|가능|불가능|
|선언 예제|-(void)method1;|+(void)method2;|
|사용 예제|[obj method1]|[Class method2]|

### 인스턴스 메소드
- 인스턴스 메소드 선언
```objc
@interface ClassName : SuperClass
-(void)instanceMethod;
@end
```
- 인스턴스 메소드 사용, 객체 생성 필요
```objc
Rectangle *rect = [[Rectangle alloc] init];
int area = [rect size];
```
- 멤버변수 접근 가능

```objc
-(int)size{
    return width*height;
}
```

### 클래스 메소드
- 클래스 메소드 선언
```objc
@interface MyClass : NSObject
+(void)classMethod;
@end
```
- 클래스 메소드 사용, 객체 생성 불필요
```objc
[MyClass classMethod];
```
- 멤버변수 접근 불가 ERROR
```objc
+(int)size{
    return width*height;    // ERROR
}
```

## 프로퍼티(인스턴스 변수 접근)
- interface 영역
```objc
@interface Example : NSObject{
   float value;
}
@property float value;
@end
```
- implementation 영역
```objc
@implementation Example
@synthesize value;  // 자료형을 제외한 변수명만 작성한다.
@end
```
- 사용
```objc
int mian(){
    Example *ex = [[Example alloc]init];
    ex.value = 3;
    
    return 0;
}
```

##  self
- 자기 자신의 인스턴스 메소드 호출하기
```objc
// EX
@implementation Minus

-(void)minusAB{
    NSLog(@"A - B : %i", a-b);
    [self plusAB];
}
-(void)plusAB{
    NSLog(@"plus AB!");
}

@end
```

## @class 지시어
```objc
#import XYPoint.h == @class XYPoint
```

## 동적바인딩과 id형
- id는 어떤 클래스 객체든 저장하여 사용할 수 있다.
```objc
id dataValue;   // *를 붙이지 않는다.
Fraction *f1 = [[Fraction alloc]init];
Complex *c1 = [[Complex alloc]init];

[f1 setTo:2 over:5];
[c1 setReal:10.0 andImaginary:2.5];

dataValue = f1;
[dataValue print];

dataValue = c1;
[dataValue print];
```
