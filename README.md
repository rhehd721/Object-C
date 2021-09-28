# Object-C

## 기본정보
- 작명규칙
    - 클래스 : 대문자로 시작하는 명사형
    - 인스턴스/멤버면수 : 소문자로 시작하는 명사형
    - 메소드/함수 : 소문자로 시작하는 동사형

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
```
- implementation 영역
```objc
@implementation Example
@synthesize value;
@end
```
