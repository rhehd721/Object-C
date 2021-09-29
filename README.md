# Object-C

## 기본정보
- 작명규칙
    - 클래스 : 대문자로 시작하는 명사형
    - 인스턴스/멤버면수 : 소문자로 시작하는 명사형
    - 메소드/함수 : 소문자로 시작하는 동사형
- 상속으로 메서드를 제거하거나 삭제할 순 없지만 '재정의'하여 메서드의 정의를 변경할 수는 있다.
    - 메소드를 호출 할 경우 해당 클래스에서 메서드를 찾은 후 찾지 못한 경우 상위 클래스에서 메서드를 찾아 호출하기 때문
- objc 장점
    - 다형성(Override)이 가능하다. : 동일한 메서드 이름을 사용할 수 있다.
    - 동적 타이핑이 가능하다. : 객체가 속한 클래스를 알아내는 단계를 프로그램이 실행될 때로 미룬다.
    - 동적 바인딩이 가능하다. : 객체에 호출되는 실제 메서드를 알아내는 시기를 프로그램 실행 중으로 미룬다.

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

## @selector

## @try
```objc
// Ex
int main(){
    NSAutoreleasePool * pool = [[NSAutoreleasePool alloc]init];
    Fraction *f = [[Fraction alloc]init];
    
    @try {  // try실패시 프로그램 종료가 아닌 catch 실행
        [f noSuchMethod];
    }

    @catch (NSException * e) {
        NSLog(@"Error: %@%@", [e name], [e reason]);
    }
    @finally {
        NSLog(@"Finally executes no matter what");
    }
    
    [f release];
    [pool drain];
    return 0;
```
- @finally : @try 블록에서 예외의 싱행 유무와 상관없이 실행 할 코드를 작성할 수 있다.
- @throw : 직접 만든 예외를 던질 수 있다.

## 인스턴스 변수의 범위
- @protected : 어떤 클래스에서 인스턴스 변수가 정의되었을 때, 그 클래스와 그 서브클래스에 정의된 메서드는 이 인스턴스 변수에 바로 접근할 수 있다. 이것이 기본값 이다.
- @private : 클래스에 정의된 메서드는 인스턴스 변수에 바로 접근할 수 있지만, 서브클래스의 메서드는 바로 접근할 수 없다.
- @public : 인스턴스 변수가 정의된 클래스와 그 밖에 클래스 그리고 모듈에 정의된 메서드라면, 인스턴스 변수에 바로 접근할 수 있다.
- @package : 64비트 이미지의 경우, 그 클래스를 구현하는 이미지 안에서든 어디든 인스턴스 변수에 접근할 수 있다.

## 식별자
- extern
    - 전역변수를 다른 프로그램 파일에서도 함께 사용할 수 있도록 해주는 선언 방법 == 외부변수
```objc
extern int gMoveNumber;
```
- static
    - 정적변수
```objc
static int gGlobalVar = 0;
```
- const
    - 값이 변하지 않는 변수 == 상수
- volatile
    - 값이 변할 예정인 변수

## 열거 데이터 형
```objc
enum direction{up, down, left = 10, right };
// up == 0
// down == 1
// left == 10
// right == 11
```

## typedef
```objc
typedef Number *NumberObject;   // typedef 기존이름 새이름
// Ex
typedef int Counter;
Counter j;  // == int j
```

## 카테고리
- 클래스 정의를 그룹짓거나, 연관된 메서드를 카테고리로 쉽게 모듈러화할 수 있게 해준다. 또한 원본 소스코드에 접근하거나 서브클래스 생성 없이 현존한느 클래스의 정의를 쉽게 확장하는 방법이다.
```objc
// 정의부
//NSString+Reorder
#import <Foundation/NSString.h>
@interface NSString (Reorder)
    -(NSString *)reversedString;
@end
// 구현부
#import "NSString+Reorder.h"
#import "NSString+PathComp.h"
@implementation NSString
-(NSString *)reversedString
{
    ...
}
@end
```
- 카테고리는 클래스에 인스턴스 변수를 추가할 수 없으며 메소드만 추가할 수 있습니다. 대신 클래스 메소드와 인스턴스 메소드를 모두 선언할 수 있습니다.

## 익스텐션(익명 카테고리)
- 익스텐션을 사용하면 클래스의 @interface 부분을 나누어 사용할 수 있지만 @implementation 부분은 분리할 수 없습니다. 일부 메소드의 프로토타입을 인터페이스로 공개하지 않게 구성할 때 유용합니다.
```objc
// 익스텐션은 @interface에 카테고리 이름을 지정하지 않으며 원래 클래스의 @implementation 부분에 구현해야 합니다.
// Ship.h
#import <Foundation/Foundation.h>
#import "Person.h"
 
@interface Ship : NSObject
 
@property (strong, readonly) Person *captain;
 
-(id)initWithCaptain:(Person *)captain;
 
@end

// Ship.m
#import "Ship.h"
 
// The class extension.
@interface Ship()
 
@property (strong, readwrite) Person *captain;
 
@end
 
 
// The standard implementation.
@implementation Ship
 
@synthesize captain = _captain;
 
-(id)initWithCaptain:(Person *)captain {
    self = [super init];
    if (self) {
        // This WILL work because of the extension.
        [self setCaptain:captain];
    }
    return self;
}
 
@end
```

## 프로토콜
- “선언만 되고 구현되지 않은” 메소드
    - 다른 객체가 구현해주면 되는 메소드를 선언
    - 클래스를 숨기고 인터페이스만 선언하고자 할 때
    - 상속관계가 아니지만 비슷한 인터페이스를 만들고자 할 때
```objc
@protocol NSCopying <protocolName>
-(id)copyWithZone:(NSZone*)zone;
// @required : 반드시 구현해야 하는 메소드
// @optional : 구현해도 되고 안해도 되는 메소드
@end
////////////////////////////////
@interface AddressBook: NSObject <NSCopying, ...>
```
- 객체가 프로토콜에 따르는지 알아보는 방법
```objc
id currentObject;
    ...
if ([currentObject conformsToProtocol: @protocol (Drawing)] == YES){
    // currentObject에 존재하는 프로토콜 인스턴스에 메세지를 보낸다.
    ...
}
```
- 프로토콜을 따르는 변수 표기
```objc
id <Drawing, NSCopying> myDocument;
```

## 전처리기
- #define
- #연산자 ##연산자
- #import
- #ifdef, #endif, #else, #ifndef
- #if, #elif
- #undef

## 구조체
```objc
strct date  [
    int month;
    int day;
    int year;
}Date;

//strct date today;
Date today;
today.day = 21;

## 공용체
```objc
union mixed{
    char c;
    float f;
    int i;
};
```

```
