- InputStream은 바이트 입력 스트림의 최상위 클래스임
### Method
- read() : 1바이트를 읽은 후 return(읽을 바이트가 없다면 -1을 return -> 이를 이용해서 반복문 활용가능(-1이 return된다면 반복문을 멈추게끔 코드를 작성할 수 있음))
- readbyte(byte[]) : 읽은 바이트를 매개값으로 주어진 배열에 저장한 후 저장한 바이트의 `수`를 return(얘도 읽을 바이트가 없다면 -1을 return)
	+ 최대로 저장할 수 있는 바이트 갯수는 배열의 길이만큼임
	+ 읽은 바이트의 갯수가 배열의 길이보다 작을 땐 읽은 바이트만 배열에 저장함
- close() : 스트림 닫는 용도(사용 후엔 닫아야 함)

### 1바이트씩 읽기

```java
InputStream input;

try {
	input = new FileInputStream("C:/ddd.txt");
	int byteCount = 0;
	
	while(true) {
		int a = input.read();
		if (a == -1) {
			break;
		}
		System.out.println(a);
		byteCount++;
	}
	System.out.println("3글자는 총 몇 바이트? " + byteCount);
	input.close();
} catch (IOException e) {
// FileNotFoundException도 IOException을 상속하고 있기 때문에 try/catch 한 번만 작성해도 됨
	e.printStackTrace();
}
```
### 배열 단위로 받아서 출력하기

```java
InputStream input;

try {

	input = new FileInputStream("C:/ddd.txt");
	// 바이트 갯수가 많은 파일
	byte[] arr = new byte[10];
	input.read(arr);
	for (int i = 0; i < arr.length; i++) {
		System.out.print(arr[i] + " ");
	}
	
	System.out.println();
	
	input = new FileInputStream("C:/aaa.txt");
	// 바이트 갯수가 적은 파일
	input.read(arr);
	for (int i = 0; i < arr.length; i++) {
		System.out.print(arr[i] + " ");
	}

	input.close();
} catch (IOException e) {

e.printStackTrace();

}

// 출력 결과
// -20 -104 -120 0 0 0 0 0 0 0
// 49 -104 -120 0 0 0 0 0 0 0
```

- 새로운 파일의 바이트를 기존의 배열에 저장하게 되면 인덱스 0부터 저장하게 됨을 알 수 있음
- 새로운 파일의 바이트들이 저장된 자리를 제외하곤 이전의 읽은 파일(바이트 갯수가 많은 파일)의 바이트가 여전히 배열에 남아있는 것을 알 수 있음
### 파일 복사 예제

```java
String originalFileName = "C:/aaa.txt";
String targetFileName = "C:/Users/82106/Desktop/bbb.txt";

InputStream input;
OutputStream output;
try {
	input = new FileInputStream(originalFileName);
	output = new FileOutputStream(targetFileName);
	byte[] data = new byte[9];

	while (true) {
		int num = input.read(data);
	
		if (num == -1) {
			break;
		}
	
		output.write(data, 0, num);
		// (바이트 배열, 시작 인덱스, 끝 인덱스 + 1)
		// 버퍼에다 배열에 저장된 개수만큼만 담도록 작성
	}
	output.flush();
	// flush()를 사용하지 않아도 OutputStream가 내부적으로 알아서 버퍼를 비우는 기능을 함
	// 그래서 flush()없이 write()만 써도 파일 저장은 될 것임  있음
	// 하지만 프로그램이 강제로 종료될 경우, 모든 데이터가 파일에 제대로 저장되지 않을 수 있     으므로 flush()를 사용하는 것이 좋은 습관임
	
	input.close();
	output.close();
	// close() 사용 시 버퍼에 남아 있는 게 있다면 다 비워내고 스트림을 닫음
	// 그래서 flush()없이 close()만 사용해도 파일 저장은 될 것임
} catch (IOException e) {
	e.printStackTrace();
}
```

- 실행 시 aaa 파일에 담긴 내용을 data라는 바이트 배열에 저장한 후 새로운 파일(bbb라는 파일)을 만들어서 바탕화면에 저장하게 됨. 즉, 파일 형태로 출력을 하게 되는 것임
 -> 파일을 복사하여 바탕화면에 저장하는 꼴이 됨
 
- 만일 파일의 바이트 갯수가 배열의 길이로 딱 나눠 떨어지지 않는다면 **마지막에** 버퍼에 담길 바이트 갯수는 배열의 길이보다 작을 것임 
 -> 이럴 경우 배열의 나머지 자리는 이전에 버퍼에 담았던 내용들이 고대로 남아있을 것이므로 애초에 write()를 사용할 때 예제처럼 저장된 길이만큼만 담기도록 작성해야 함

- 해당 경로에 출력할 파일의 이름과 같은 파일이 이미 있는 경우 지금 출력하는 파일의 내용으로 덮어쓰게 됨