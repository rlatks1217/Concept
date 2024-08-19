### ThreadLocal
- ThreadLocal은 자바 class로서 해당 class Type의 변수를 만들어서 사용함
- 한 Thread 내부 어디서나 접근할 수 있는 변수를 만들기 위해 씀

a라는 ThreadLocal Type의 변수를 선언했을 때, a라는 변수가 Thread1과 Thread2라는 2개의 Thread에서 사용된다고 하면 각 Thread 내부에서 개별적인 a라는 변수로 동작함. 
즉, 이름만 같은 변수 2개가 각 Thread 내부에서 사용된다고 생각하면 됨

### ThreadLocalMap
- ThreadLocal이 Thread의 갯수만큼 있는 것처럼 마찬가지로 ThreadLocalMap도 Thread의 갯수만큼 있음
- ThreadLocal 변수를 통해 실제로 저장공간인 ThreadLocalMap에 접근하여 사용하게 됨
- 해당 Map은 key가 ThreadLocal이고 Value는 Thread마다 저장하려는 값임
- ThreadLocalMap를 참조하게 되는 threadLocals라는 변수는 해당 Thread의 필드이기 때문에 t.threadLocals와 같이 작성하여 ThreadLocalMap에 접근할 수 있음

### ThreadLocal 클래스의 주요 메소드들
- this는 당연히 해당 ThreadLocal을 의미함
```java
void createMap(Thread t, T firstValue) {
	t.threadLocals = new ThreadLocalMap(this, firstValue);
	// 해당 Thread에 대한 map을 만듦 
}

public void set(T value) {
	Thread t = Thread.currentThread();
	ThreadLocalMap map = getMap(t);
 
	if (map != null) {
		map.set(this, value);
	} else {
		createMap(t, value);
	}
}

public T get() {
	Thread t = Thread.currentThread();
	ThreadLocalMap map = getMap(t);
	
	if (map != null) {
		ThreadLocalMap.Entry e = map.getEntry(this);
		if (e != null) {
			@SuppressWarnings("unchecked")
			T result = (T)e.value;
			return result;
		}
	}
	return setInitialValue(); 
	// 해당 key에 대한 value가 없을 땐 null 반환하도록 함
}

private T setInitialValue() {
	T value = initialValue();
	Thread t = Thread.currentThread();
	ThreadLocalMap map = getMap(t);
	
	if (map != null) {
		map.set(this, value);
	} else {
		createMap(t, value);
	}
	
	if (this instanceof TerminatingThreadLocal) {
		TerminatingThreadLocal.register((TerminatingThreadLocal<?>) this);
	}
	return value;
}

```

- ThreadLocal.withlnitial(()-> 원하는 value값) : ThreadLocalMap에 처음 들어갈 entry의 value값을 초기화(key값은 해당 threadLocal 객체임) + 동시에 value의 Type도 지정하는 꼴이 됨
- ThreadLocal 변수.set(value값): 해당 ThreadLocal이 key인 entry의 value를 원하는 값으로 수정할 수 있음
- 하나의 Thread에서 여러 ThreadLocal을 만들어서 사용할 수 있음(이때는 해당 Thread의 ThreaddocalMap에 두 개의 entry가 들어가게 됨)
- 실행되고 있는 thread(A) 내부에서 새로운 thread(B)를 생성하면 A는 B의 부모가 됨
- 부모 thread에서 childValue()를 override하게 되면 해당 메소드가 return하는 값이 자식 thread의 threadLocal의 value로 나오게 됨

생성자에서 super(이름)으로 해당 thread의 이름을 정해줄 수 있음

### 주의 사항
Thread pool 환경에서 ThreadLocal을 사용하는 경우 ThreadLocal에 보관된 데이터는 사용이 끝나면 반드시 해당 데이터를 삭제 해주어야 함. 그렇지 않을 경우 Thread pool로 반납되었다가 재사용되는 Thread에 여전히 데이터가 남게 되어 오류의 원인이 될 수있음.
