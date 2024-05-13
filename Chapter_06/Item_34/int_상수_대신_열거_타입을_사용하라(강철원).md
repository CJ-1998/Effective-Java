프로그래밍을 하다 보면 여러 값 중 하나를 선택해야 할 때가 있습니다. 예를 들어, 계절, 요일, 메뉴 옵션 등을 코드 내에서 다루어야 하는 경우가 그러합니다. 이러한 선택지들을 코드 내에서 표현하는 방법 중 하나가 'int 상수'를 사용하는 것입니다. 하지만 이 방법은 여러 가지 한계와 문제점을 가지고 있습니다. 이에 비해 자바에서 제공하는 '열거 타입(enum)'은 이러한 한계를 극복하고 더욱 강력하고 유연한 방법을 제공합니다.


## int 상수의 한계

int 상수를 사용하는 방법은 간단합니다. 예를 들어, 요일을 표현하기 위해 일요일부터 토요일까지를 1부터 7까지의 숫자로 할당할 수 있습니다. 하지만 이 방법은 타입 안전성이 부족하고, 의미 있는 이름을 제공하지 못하는 문제가 있습니다. 숫자만으로는 해당 값이 무엇을 의미하는지 코드를 읽는 사람이 쉽게 이해하기 어렵습니다. 또한, 잘못된 값이 사용될 위험이 있으며, 이러한 오류는 런타임에야 발견되기 때문에 디버깅이 어려워질 수 있습니다.

열거타입이 없을때는  int 상수를 어떻게 표현했을까?

아래는 정수 열거 패턴입니다.  

```java
public static final int APPLE_FUJI = 0;
public static final int APPLE_PIPPIN = 1;
public static final int APPLE_GRANNY_SMITH = 2;

public static final int ORANGE_NAVEL = 0;
public static final int ORANGE_TEMPLE = 1;
public static final int ORANGE_BOOD = 2;
```

이러한 정수 열거 패턴에는 단점이 존재합니다. 
1. 타입 안전을 보장하지 않습니다.
2. 표현력이 좋지 않습니다.
3. 문자열로 출력하기가 다소 까다롭다.

### 1. 타입 안전을 보장하지 않습니다.

타입 안전이라는 것은 프로그램에서 자료형의 오류를 미리 잡아내어 실행 중인 프로그램이 예기치 않은 동작을 하지 않도록 막는 것을 말합니다. 정수 열거 패턴을 사용할 때는 이러한 안전이 보장되지 않습니다.
```java
public static final int SEASON_SPRING = 0;
public static final int SEASON_SUMMER = 1;
public static final int SEASON_FALL = 2;
public static final int SEASON_WINTER = 3;

public static void wearClothes(int season) {
    if (season == SEASON_SPRING) {
        System.out.println("봄 옷을 입습니다.");
    }
}

```

여기서 `wearClothes` 함수에 `5` 같은 잘못된 값을 넣어도 컴파일러는 에러를 표시하지 않습니다. 이는 값 `5`가 유효한 계절을 나타내지 않지만, 정수 타입이기 때문에 에러로 감지되지 않는 것입니다.


### 2. 표현력이 좋지 않습니다.

숫자로 표현하면 누군가 코드를 볼 때 각 숫자가 무엇을 의미하는지 쉽게 알아차리기 어렵습니다.  
```java
wearClothes(2);
```

### 3. 문자열로 출력하기가 다소 까다롭다.  

그 값을 출력하거나 디버거로 살펴보면 (의미가 아닌) 단지 숫자로만 보여서 도움이 되지 않습니다. 
또한 그 안에 상수가 몇 개인지도 알 수 없습니다.  


## 💎 열거 타입의 등장

자바는 이러한 열거 패턴의 단점을 말끔히 씻어주는 동시에 여러 장점을 안겨주는 대안을 제시했습니다.
바로 `열거 타입`입니다.  

```java
public enum Apple { FUJI, PIPPIN, GRANNY_SMITH}
public enum Orange { NAVEL, TEMPLE, BLOOD }
```

자바의 열거 타입은 완전한 형태의 클래스라서 다른 언어의 열거 타입보다 훨씬 강력합니다. 


## 💎 자바 열거 타입 특징


열거 타입 자체는 클래스이며, 상수 하나당 자신의 인스턴스를 하나씩 만들어 `public static fianl` 필드로 공개합니다. 열거 타입은 밖에서 접근할 수 있는 생성자를 제공하지 않으므로 사실상 final입니다. 따라서 클라이언트가 인스턴스를 직접 생성하거나 확장 할 수 없으니 열거 타입 선언으로 만들어진 인스턴스들은 딱 하나씩만 존재합니다.

### 1️⃣ 열거 타입은 컴파일타임 타입 안전성을 제공합니다.

```java
public enum Apple { PUJI, PIPPIN, GRANNY_SMITH}
```

위와 같이 Apple을 선언했다면  건네받은 참조는 Apple의 세 가지 값 중 하나임이 확실합니다.  
다른 타입의 값을 넘기려 하면 컴파일 오류가 납니다.  

### 2️⃣ 이름이 같은 상수도 평화롭게 공존합니다.

- 열거 타입에는 각자의 이름공간이 있어서 이름이 같은 상수도 평화롭게 공존합니다.  


```java
public enum Color { RED, GREEN, BLUE } 
public enum Fruit { APPLE, BANANA, ORANGE, RED // 다른 열거 타입에서 RED 사용 }
```

`Color`와 `Fruit`은 서로 다른 타입이므로, 각각의 `RED`는 다른 네임스페이스에 있어서 충돌이 일어나지 않습니다. 하지만 같은 열거 타입 내에서는 이름이 중복되어선 안 됩니다. 예를 들어, `Color` 열거 타입 내에서 두 번 `RED`를 선언하려고 하면 컴파일 에러가 발생합니다. 이렇게 열거 타입을 사용하면 이름 충돌을 피하면서도 코드의 명확성과 안전성을 유지할 수 있습니다.

### 3️⃣ 열거 타입에 새로운 상수를 추가하거나 순서를 바꿔도 다시 컴파일하지 않아도 됩니다.

공개되는 것이 오직 필드의 이름뿐이라, 정수 열거 패턴과 달리 상수 값이 클라이언트로 컴파일되어 각인되지 않기 때문입니다.  

여기서 말하는 '상수 값'은 메모리 주소를 의미하는 것이 아니라, 상수가 실제로 가지고 있는 데이터 값 자체를 의미합니다. 예를 들어, 정수 열거 패턴에서 상수들은 특정 정수 값에 연결되어 있고, 이 값이 프로그램에 하드코딩되어 사용됩니다.

예를 들어, 고전적인 정수 열거 패턴에서는 다음과 같이 상수를 정의할 수 있습니다:
```java
public static final int COLOR_RED = 0;
public static final int COLOR_GREEN = 1;
public static final int COLOR_BLUE = 2;
```

이 경우, `COLOR_RED`, `COLOR_GREEN`, `COLOR_BLUE`는 각각 0, 1, 2라는 정수 값을 가집니다. 만약 이 값들이 클라이언트 코드에 사용된다면, 클라이언트 코드는 이 정수 값을 직접 사용합니다. 이 정수 값들이 프로그램 코드 여러 곳에 퍼져 있게 되면, 나중에 이 값을 변경하게 될 경우 모든 참조 지점을 찾아서 일일이 수정해야 할 수도 있습니다.

반면, 열거 타입을 사용할 경우, 각 상수는 이름으로 참조됩니다. 예를 들어 Java의 열거 타입을 사용하면 다음과 같이 됩니다
```java
public enum Color {
    RED, GREEN, BLUE
}
```

여기서 `RED`, `GREEN`, `BLUE`는 각각 `Color` 타입의 인스턴스입니다. 프로그램 내에서 이들을 사용할 때는 `Color.RED`, `Color.GREEN`, `Color.BLUE`와 같이 타입과 함께 참조하게 되므로, 실제 정수 값이 무엇인지는 중요하지 않습니다.


### 4️⃣ 여러가지 사전 구현

toString, Object 메서드, Comparable, Serializable을 사전에 구현해놓았으며 그 직렬화 형태도 웬만큼 변형을 가해도 문제없이 동작하게끔 구현해놨습니다.


## ❓ 열거 타입에 메서드나 필드는 언제 필요한 기능인가?


```java
public enum Planet {
    MERCURY(3.302e+23, 2.439e6),
    VENUS  (4.869e+24, 6.052e6),
    EARTH  (5.975e+24, 6.378e6),
    MARS   (6.419e+23, 3.393e6),
    JUPITER(1.899e+27, 7.149e7),
    SATURN (5.685e+26, 6.027e7),
    URANUS (8.683e+25, 2.556e7),
    NEPTUNE(1.024e+26, 2.477e7);

    private final double mass;           // 질량(단위: 킬로그램)
    private final double radius;         // 반지름(단위: 미터)
    private final double surfaceGravity; // 표면중력(단위: m / s^2)

    // 중력상수(단위: m^3 / kg s^2)
    private static final double G = 6.67300E-11;

    // 생성자
    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
        surfaceGravity = G * mass / (radius * radius);
    }

    public double mass()           { return mass; }
    public double radius()         { return radius; }
    public double surfaceGravity() { return surfaceGravity; }

    public double surfaceWeight(double mass) {
        return mass * surfaceGravity;  // F = ma
    }
}
```

열거 타입 상수 각각을 특정 데이터와 녀결지으려면 생성자에서 데이터를 받아 인스턴스 필드에 저장하면 됩니다.

- 열거 타입은 근본적으로 불변이라 모든 필드는 final이어야 합니다.
	- 필드를 public으로 선언해도 되지만, private으로 두고 별도의 public 접근자 메서드를 두는 게 낫습니다.  

- planet의 생성자에서 표면중력을 계산해 저장한 이유는 단순히 최적화를 위해서입니다.
	- 사실 질량과 반지름이 있으니 표면중력은 언제든 계산할 수 있습니다.



여기서 상수가 더 다양한 기능을 제공해줬으면 할 때도 있습니다.  
상수마다 동작이 달라져야 하는 상황

## 열거타입 상수마다 동작이 달라지는 상수


### 1️⃣ switch 문을 이용해 상수의 값에 따라 분기하는 방법

```java
enum Operation {
    PLUS, MINUS, TIMES, DIVIDE;

    public double apply(double x, double y) {
        switch (this) {
            case PLUS -> {
                return x+y;
            }
            case MINUS -> {
                return x-y;
            }
            case TIMES -> {
                return x*y;
            }
            case DIVIDE -> {
                return x/y;
            }
            default -> throw new IllegalStateException();
        }
    }
}

@Test
public void operationApplyTest() {
    double x = 99;
    double y = 33;

    for (Operation value : Operation.values()) {
        System.out.printf("%f %s %f = %f%n", x, value, y, value.apply(x, y));
    }
}
```

- 단점
	- 마지막의 throw 문은 실제로는 도달할 일이 없지만 기술적으로는 도달할 수 있기 때문에 생략하면 컴파일조차 되지 않습니다.  
	- 새로운 상수를 추가하면 해당 case 문도 추가해야합니다.
		- 혹시라도 깜빡한다면, 컴파일은 되지만 새로 추가한 연산을 수행하려 할 때 "알수 없는 연산"이라는 런타임 오류를 내며 프로그램이 종료됩니다.


### 2️⃣ 추상메서드 활용하기


> 열거 타입에 apply라는 추상 메서드를 선언하고 각 상수별 클래스 몸체, 즉 각 상수에서 자신에 맞게 재정의하는 방법입니다.  
```java
enum Operation {
    PLUS {
        @Override
        public double apply(double x, double y) {
            return x+y;
        }
    },
    MINUS {
        @Override
        public double apply(double x, double y) {
            return x-y;
        }
    },
    TIMES {
        @Override
        public double apply(double x, double y) {
            return x*y;
        }
    },
    DIVIDE {
        @Override
        public double apply(double x, double y) {
            return x/y;
        }
    };

    public abstract double apply(double x, double y);
}

@Test
public void operationApplyTest() {
    double x = 99;
    double y = 33;

    for (Operation value : Operation.values()) {
        System.out.printf("%f %s %f = %f%n", x, value, y, value.apply(x, y));
    }
}
```

- apply가 추상 메서드이므로 재정의하지 않았다면 컴파일 오류로 알려줍니다.  

### 3️⃣ 생성자 이용 및 공통 메서드 재정의

```java
enum Operation {
    PLUS ("+") {
        @Override
        public double apply(double x, double y) {
            return x+y;
        }
    },
    MINUS ("-") {
        @Override
        public double apply(double x, double y) {
            return x-y;
        }
    },
    TIMES ("*") {
        @Override
        public double apply(double x, double y) {
            return x*y;
        }
    },
    DIVIDE ("/") {
        @Override
        public double apply(double x, double y) {
            return x/y;
        }
    };

    private final String symbol;

    Operation(String symbol) {
        this.symbol = symbol;
    }

    public abstract double apply(double x, double y);

    @Override
    public String toString() {
        return symbol;
    }
}

@Test
public void operationApplyTest() {
    double x = 10;
    double y = 15;

    for (Operation value : Operation.values()) {
        System.out.printf("%f %s %f = %f%n", x, value, y, value.apply(x, y));
    }
}
```


## 📢 정리


- 열거 타입은 필요한 원소를 컴파일타임에 다 알 수 있는 상수 집합이라면 항상 열거 타입을 사용하는 것을 권장합니다.
	- 태양계 행성, 한 주의 요일, 체스 말처럼 본질적으로 열거타입인 타입은 당연히 포함
	- 메뉴 아이템, 연산 코드, 명령줄 플래그 등 혀용하는 값 모두를 컴파일타임에 이미 알고 있을 때 사용
- 열거 타입에 정의된 상수 개수가 영원히 고정 불변일 필요는 없습니다.
	- 나중에 상수가 추가돼도 바이너리 수준에서 호환되도록 설계되었습니다.


열거 타입은 확실히 정수 상수보다 뛰어납니다. 더 읽기 쉽고 안전하고 강력합니다.   대다수 열거 타입이 명시적 생성자나 메서드 없이 쓰이지만, 각 상수를 특정 데이터와 연결짓거나 상수마다 다르게 동작하게 할 때는 필요합니다.

드물게는 하나의 메서드가 상수별로 다르게 동작해야 할 때도 있습니다. 이런 열거 타입에서는 switch 문 대신 상수별 메서드를 구현하여 사용하면 좋습니다. 
