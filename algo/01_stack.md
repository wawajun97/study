# 자료구조

1. 선형 자료구조: 자료 간의 관계가 1:1인 구조
2. 비선형 자료구조: 자료 간의 관계가 1:N인 구조
   예: 트리, 그래프

## Stack

- 자료를 차곡차곡 쌓아 올린 형태의 선형 자료구조이다.
- 후입선출(LIFO, Last In First Out) 구조를 가진다.

## 주요 연산

- `push`: 스택에 데이터를 저장한다.
- `pop`: 스택에서 데이터를 꺼내며 제거한다.
- `peek`: 스택의 top에 있는 원소를 반환한다. 값은 제거되지 않는다.
- `isEmpty`: 스택이 비어 있는지 확인한다.
- `size`: 스택의 크기를 반환한다.
- `get(index)`: 해당 인덱스의 데이터를 반환한다.

## 스택 사용 예시

- 괄호 검사: 여는 괄호가 나오면 스택에 넣고, 닫는 괄호가 나오면 스택의 맨 위 원소와 짝이 맞는지 확인한다.
- 함수 호출: 가장 나중에 호출된 함수가 가장 먼저 실행을 마치고 복귀하는 후입선출 구조를 가진다.
- 계산기: 중위 표기식을 후위 표기식으로 바꾼 뒤 계산할 때 스택을 사용한다.

> 중위 표기법: 연산자를 피연산자 사이에 표기하는 방법 (`A+B`)
> 후위 표기법: 연산자를 피연산자 뒤에 표기하는 방법 (`AB+`)

## 중위 표기식 -> 후위 표기식 변환

1. 수식의 각 연산자에 대해 우선순위를 고려하여 괄호를 사용해 다시 표현한다.
2. 각 연산자를 자신에 대응하는 오른쪽 괄호 뒤로 이동한다.
3. 괄호를 제거한다.

## 예제

```java
public static void main(String[] args) {
		Stack<String> stack = new Stack<>();

		stack.push("A");
		stack.push("B");
		stack.push("C");
		stack.push("D");
		stack.push("E");
		stack.push("F");

//		int size = stack.size();
//
//		for(int i = 0;i<size;i++) { //i<stack.size()로 하면 스택의 값이 빠지면서 stack.size()가 계속 변경되어서 정상동작 x
//			System.out.println(stack.pop());
//			System.out.println(stack);
//		}

		while(!stack.isEmpty()) {
			System.out.println(stack.pop());
			System.out.println(stack);
		}
	}
```

```java
public static void main(String[] args) {
		String expression = "6528-*2/+";
		Stack<Integer> stack = new Stack<>();

		for (int i = 0, size = expression.length(); i < size; i++) {
			char temp = expression.charAt(i);

			if (Character.isDigit(temp)) {
				int k = temp - '0';
				stack.push(k);
			} else {
				int value2 = stack.pop();
				int value1 = stack.pop();
				int result = 0;

				switch (temp) {
					case '+':
						result = value1 + value2;
						break;
					case '-':
						result = value1 - value2;
						break;
					case '*':
						result = value1 * value2;
						break;
					case '/':
						result = value1 / value2;
						break;
				}

				stack.push(result);
			}
		}
		System.out.println(stack.pop());
	}
```
