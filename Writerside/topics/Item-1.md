# Item 1. 생성자 대신 정적 팩터리 메서드를 고려하라

## 장단점 미리보기 
<tabs>
<tab title="장점">
    <ul>
      <li>이름을 가질 수 있다.</li>
      <li>호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.</li>
      <li>반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.</li>
      <li>입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.</li>
      <li>정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.</li>
    </ul>
</tab>
    <tab title="단점">
        <ul>
          <li>상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.</li>
          <li>정적 팩터리 메서드는 프로그래머가 찾기 어렵다.</li>
        </ul>
    </tab>
</tabs>

## init
- 자바는 생성자의 매개변수의 타입만 보고, 생성자의 매개변수의 이름과는 상관없이 타입이 동일하면 생성이 불가하다.
- 보통 아래의 코드와 같이 순서를 바꾸어서 생성자를 만들어야 한다.
- 하지만 이러한 경우 생성자의 이름을 표현할 수가 없다. 클래스 명과 동일해야한다. 즉 생성자가 여러개여도 모두 같은 이름이다(구분하기 힘들다)

<compare>
    <code-block lang="java">
    public class Order {
            private boolean prime;
            private boolean urgent;
            private Product product;
            public Order(Product product, boolean prime) {
                    this.product = product;
                    this.prime = prime;
            }
            public Order(Product product, boolean urgent) {
                    this.product = product;
                    this.urgent = urgent;
            }
    }
    </code-block>
    <code-block lang="java">
        // 매개변수의 순서만 바꿔서 생성 
        public class Order {
		private boolean prime;
		private boolean urgent;
		private Product product;
		public Order(Product product, boolean prime) {
				this.product = product;
				this.prime = prime;
		}
		public Order(boolean urgent, Product product) {
				this.product = product;
				this.urgent = urgent;
		}
}
    </code-block>
</compare>

## 장점


## 단점
- 상속을 사용하려면 public 이나 protected 생성자가 필요하니 정적팩터리 메서드만 제공하면 하위클래스 x (생성자를 다 막아놨을 경우)
- 정적 팩터리 메서드는 프로그래머가 찾기 어렵다. (생성자는 new 키워드로 찾을 수 있지만, 정적 팩터리 메서드는 이름을 가질 수 있기 때문에 찾기 어렵다)
<code-block lang="java">
    public Settings(boolean useAutoSteering, boolean useABS, boolean Difficulty difficulty) {
        this.useAutoSteering = useAutoSteering;
        this.useAutoAcceleration = useAutoAcceleration;
        this.useAutoBrake = useAutoBrake;
    }
    public static Settings getInstance(){
        return SETTINGS
}
</code-block>
- javadoc 에서 찾을 수는 있겠지만, 많아지면 좀 힘들다 
- 네이밍으로 of, newInstance, getInstance ~... 등등 으로 해결할 수 있지만, 이것도 한계가 있다.
- 자바독이 알아서 처리해주는 날이 올 때까자 <b>API 문서</b>를 잘써놓고 메서드 이름도 널리 알려진 규약을 따라 짓는 식으로 문제를 완화해주자

// 팁
<code-block lang="java">
    /**
    * 이 클래스의 인스턴스는 #getInstance() 메서드로만 생성할 수 있습니다.
    * @see #getInstance()
    */
    public class Settings {
        private static final Settings SETTINGS = new Settings();
        private boolean useAutoSteering;
        private boolean useABS;
        private Difficulty difficulty;
        ...
    }
// 이런식으로 하면 쉽게 찾기 가능 정적 팩터리 메서드를 사용하는 클래스에는 이런식으로 주석을 달아놓으면 좋다.
</code-block>

## 참고
- [flyweight pattern](https://en.wikipedia.org/wiki/Flyweight_pattern)
- [flyweight pattern blog](https://lee1535.tistory.com/106)
- 플라이 웨이트 패턴이 언급되는 이유는, 정적 패터리 메소드는 플라이웨이트 패턴처럼 인스턴스를 통제하기 떄문에 -> (같은 객체가 자주 요청되는 상황)사용하는 객체들을 미리 만들어 놓고, 필요할 때마다 반환하는 방식 
 