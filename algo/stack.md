자료구조
1. 선형자료구조 : 자료 간의 관계가 1대1 관계
2. 비선형자료구조 : 자료 간의 관계가 1대N 관계(ex : 트리, 그래프)

stack
- 자료를 쌓아 올린 형태의 선형자료구조
- 후입선출(LIFO - Last In First Out)

주요 연산
- push : 저장소에 자료를 저장(삽입)
- pop : 저장소에서 자료를 꺼낸다(삭제)
- peek : 스택의 top에 있는 원소를 반환(값이 빠지진 않음)
- isEmpty : 저장소가 비어있는지 확인
- size : 저장소의 size 반환
- get(index) : 해당 인덱스의 데이터 반환

스택 사용 예시
- 괄호 검사 : 여는 괄호가 나오면 스택에 쌓고 닫는 괄호가 나오면 스택 맨 위에 괄호와 짝이 맞는지 확인
- Function call : 가장 마지막에 호출된 함수가 가장 먼저 실행을 완료하고 복귀하는 후입선출 구조
- 계산기 : 중위 표기법의 수식을 후위표기법으로 변경하여 계산(스택 사용)
> 중위표기법 : 연산자를 피연산자의 가운데 표기하는 방법 (A+B)
> 후위표기법 : 연산자를 피연산자 뒤에 표기하는 방법 (AB+)

중위표기식 -> 후위표기식 변환
1. 수식의 각 연산자에 대해서 우선순위에 따라 괄호를 사용하여 다시 표현
2. 각 연산자를 그에 대응하는 오른쪽괄호의 뒤로 이동
3. 괄호 제거

ex
```
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

```
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
