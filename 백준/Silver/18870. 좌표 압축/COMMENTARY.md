 ### 백준 18870 좌표 압축

 **정렬, HashMap, StringBuilder를 이용한 문제**

 처음에는 문제 자체가 이해가 되지 않아서 질문 게시판을 참고하였다. 이 문제에서 압축이란 뜻이 본인보다 작은 수가 몇개 있는지를 나타내는 말이고 **중복을 제거**해야 하는 것이 핵심이다. 

 처음에 문제를 풀 때는 ArrayList로 배열을 입력 받아 중복을 제거하기 위해 Set을 사용하여 연결리스트에서 중복을 삭제한 뒤 정렬을 위해 새로운 연결리스트를 할당 해 정렬을 한 뒤 이분탐색으로 문제를 해결했다.

 틀린 풀이
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashSet;
import java.util.List;
import java.util.Set;
import java.util.StringTokenizer;

public class Main {
	private static List<Integer> firstArr=new ArrayList<>(); //중복을 포함한 원본 숫자 저장
	private static List<Integer> arr=new ArrayList<>(); //중복을 제거하여 정렬된 숫자 저장
	private static Set<Integer> set; //중복을 제거하기 위한 Set 객체
	private static int binarySearch(int key, int low, int high) {
		int mid;

		if(low <= high) {
			mid = (low + high) / 2;

			if(key == arr.get(mid))  // 탐색 성공 
				return mid;
			
			// 왼쪽 부분 arr[0]부터 arr[mid-1]에서의 탐색 
			else if(key < arr.get(mid)) 
				return binarySearch(key ,low, mid-1);  
			
			// 오른쪽 부분 - arr[mid+1]부터 arr[high]에서의 탐색 
			else 
				return binarySearch(key, mid+1, high); 		
		}

		return -1; // 탐색 실패 
	}
	
	public static void main(String[] args) throws IOException{
		BufferedReader br =new BufferedReader(new InputStreamReader(System.in));
		int size=Integer.parseInt(br.readLine());
		StringTokenizer st=new StringTokenizer(br.readLine()," ");
		
		for(int i=0;i<size;i++) {
			firstArr.add(Integer.parseInt(st.nextToken()));
		}
		//중복을 허용하지 않는 Set을 통해 firstArr에 담긴 중복 제거
		set=new HashSet<Integer>(firstArr); 
		//정렬을 위해 Set을 다시 ArrayList 타입의 arr로 변환하고 정렬
		arr=new ArrayList<>(set);
		Collections.sort(arr);
		
		int high=arr.size()-1;
		for(int i=0;i<size;i++) 
			System.out.print(binarySearch(firstArr.get(i),0,high)+" ");
		
		br.close();
	}
	
}
```
시간초과가 나지 않을 까 예상하기는 했지만 역시 시간초과로 인해 통과되지 못했다. 문제를 풀면서 HashMap에 숫자와 인덱스 좌표를 저장하면 되지 않을까 생긱하긴 했었는데 풀이가 잘 떠오르지 않아 이분탐색으로 진행했는데 여기서 문제가 생긴 것 같다.

정답풀이
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.HashMap;
import java.util.StringTokenizer;

public class Main {
	
	public static void main(String[] args) throws IOException{
		BufferedReader br =new BufferedReader(new InputStreamReader(System.in));
		int size=Integer.parseInt(br.readLine());
		StringTokenizer st=new StringTokenizer(br.readLine()," ");
	
		int[] list=new int[size]; //정렬을 위한 리스트
		for(int i=0;i<size;i++) {
			list[i]=Integer.parseInt(st.nextToken());
		}
		int[] cloneList=list.clone(); //중복을 포함한 원본 숫자 저장, 얕은 복사
		Arrays.sort(list); //list 정렬
	
		HashMap<Integer,Integer> map=new HashMap<>(); //중복을 제거하기 위한 map
		int index=0; //중복을 제외한 인덱스
		for(int number:list) {
			//중복이 아닌 경우에 map에 (숫자, 인덱스) 저장, 중복일 경우에는 최초 숫자, 인덱스 쌍 저장
			if(!map.containsKey(number)) 
				map.put(number,index++);
		}
		
		StringBuilder sb = new StringBuilder();
		for(int key:cloneList) 
			sb.append(map.get(key)).append(' ');
		System.out.println(sb);
		
		br.close();
	}
}
```

먼저 입력을 배열 list에 저장하고 원본 리스트가 출력을 위해 필요하니 clone 메소드를 통해 복제된 리스트 cloneList에 깊은 복사를 진행한다. **Object.clone()** 메소드는 인스턴스 객체의 복제를 위한 메소드로, 해당 인스턴스를 복제하여 새로운 인스턴스를 생성해 그 참조값을 반환한다. 즉 **깊은 복사**를 진행한다. 그런 다음 원본 리스트 list를 정렬한 뒤 중복된 숫자가 아니라면 map에 인덱스를 늘리며 (숫자, 인덱스) 를 저장한다. **이 때 리스트가 이미 정렬되어 있으니 인덱스를 하나씩 늘리면서 저장하면 된다.** map을 통해 인덱스를 어떻게 저장해야 할 지 떠오르지 않아서 map을 이용해 문제를 풀 지 못했었다.  중복일 경우 최초 하나의 숫자와 인덱스만 저장된다. 

정렬, map과 더불어 이 문제에서 중요한 점은 출력할 때 StringBuilder를 사용하는 것이였다. 항상 출력할 때 **'+' 연산**을 이용해 문자열을 합쳐서 출력했는데 '+' 연산의 경우 기존 문자열에 새로운 문자열을 더해 새로운 문자열을 만드는 것을 반복하기 때문에 시간복잡도가 시간 복잡도가 늘어나게 된다. 대략적으로 1억번의 단순 연산에서 1초 정도 시간이 소모된다. 따라서 **StringBuilder** 클래스를 이용해야 한다. StringBuilder는 미리 일정한 크기의 배열을 잡아두고 거기에 붙여나가는 형식으로 Vector나 ArrayLilst와 같이 배열이 가득찼을 경우 기존보다 2배 더 크게 새로운 배열을 만들어가는 형태이다.  StringBuilder가 시간복잡도가 훨씬 작기 때문에 이 문제는 자바로 풀면 반드시 StringBuilder를 이용해야 했다.

다른 분들이 C++로 푼 풀이를 보니 C++에서는 중복을 제거하는 함수를 이용한 뒤 이분 탐색을 해도 시간 초과가 나지 않은 것 같다..
