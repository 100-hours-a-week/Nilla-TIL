# 그리디 알고리즘

### 탐욕법: 현재 상황에서 지금 당장 좋은 것만 고르는 방법
- 정당성 분석이 중요하다. (반복 선택만으로 최적의 해를 구할 수 있는가?)
- 예시 : 가장 최소의 동전 갯수를 사용한 거스름돈 문제 (500원,100원,50원)
>왜 최적의 해를 보장할까? : **큰 단위는 항상 작은 단위의 배수**이기에, 다른 해가 나올 수 없음! (800원을 거슬러 주어야 하는데 500원, 400원, 100원이 있는 경우라면 이야기가 다를 것이다.)

#### 거스름돈 문제
```java
int array[] = {500,100,50,10};
int money = 1340;
int num = 0;

for(int i:array){
    num += array/i;
    money %= i;
}

system.out.println(money);
```
- 화폐의 종류가 k개 있으면 복잡도는 O(k)

#### 나누기 혹은 -1
```java
import java.util.Scanner;

Scanner sc = new Scanner(system.in);
int n = sc.nextInt();
int k = sc.nextInt();
int num = 0;


while True{
    //이렇게 하면 나누고 남은 나머지를 한방에 뺄 수가 있다.
    //나머지가 0이었다면 당연히 remain은 0.
    int remain = n%k;
    int now = n-remain;
    num += remain;
    if(n<k){break;} //조건문 빼먹지 않게 주의
    n = now/k;
    num += 1;
}
num += n-1; //1만 남기고 다 빼준다.
system.out.println(num);

```

#### 곱하기 혹은 더하기
- 각 자리 숫자를 왼쪽-오른쪽 순으로 확인하며 x또는 +연산을 진행, 결과적으로 만들어질 수 있는 가장 큰 수를 구하여라.

```java
import System.util.Scanner;

Scanner sc = new Scanner(System.in);
String num = sc.nextLine();
int result = 0;

for(char c:num.toCharArray())
{
    int n = c-'0';
    if(c<2||result<2){result+=c;}
    else{result*=c;}
}

System.out.println(result);

```

#### 모험가 길드
- 한 마을에 모험가가 n명 있고, 모험가마다 '공포도'가 존재한다. '공포도'가 높은 모험가는 쉽게 공포를 느껴 위험 상황에서 대처 능력이 떨어진다.
- 공포도가 x인 모험가는 안전을 위해 반드시 x명 이상으로 구성한 모험가 그룹게 참여해야 여행을 떠날 수 있다.
- N명의 모험가에 대한 정보가 주어질 때 여행을 떠날 수 있는 그룹 수의 최댓값을 구하라
- 몇 명의 모험가는 마을에 남아있어도 괜찮다. 모든 모험가가 특정 그룹에 들어갈 필요는 없다. 
- 풀이 시간 30분 | 시간 제한 1초 | 메모리 제한 128mb | 난이도 1
- 첫줄에 모험가의 수 N(1<=N<=100,000)이, 둘째 줄에 모험가의 공포도값이 공백으로 구분되어 주어진다. 공포도는 N이하의 자연수다. 
  
  > 🤔 풀이 전 생각해보기<br>
  남아있는 모험가가 있어도 괜찮다면, 공포도가 적은 사람은 최소만 채워서 넣는게 좋을 것 같다.
  너무 큰 수의 모험가는 제외시키는게 나을지도. <br>
  그렇다면 공포도가 1인 모험가는 찾아서 무조건 혼자 넣어버려야함. <br>
  이후에 2,3...이렇게 채우는게 좋을 것 같다. > 그럼 숫자를 세어서 나머지 연산 때려버리면 되겠군!

1. 맨 처음 풀이
  ```java
    import java.util.Scanner;
    import java.util.HashMap;

    Scanner sc = new Scanner(System.in);
    int n = sc.nextInt();
    HashMap<Integer,Integer> count = new HashMap<Integer,Integer>();
    int result = 0;


    while(sc.hasNext()){
        int k = sc.nextInt();
        if(count.containsKey(k)){count.put(k,count.get(k)+1);}
        else{count.put(k,1);}
    }
    
    int prevremain = 0; 
    for(int i=1;i<n+1;i++){ //불필요한 반복이 일어날 가능성 있음. 
        int now = (count.get(i)+prevremain); //prevreamin 사용이 불안정
        result += now/i;
        prevremain = now%i;
    }

  ```
  - 이렇게하면 비효율적이다... 
  - 왜? : 그냥 한 번 루프 돌면서 count값만 바꿔서 해결할 수 있는 문제인데, 굳이 hash map을 사용하며 put 메소드 사용까지 하는 중이다. 
  
  2. 개선된 풀이 확인 (sort 사용)
  ```java
    import java.util.Scanner;
    import java.util.Arrays;

    Scanner sc = new Scanner(System.in);
    int n = sc.nextInt();
    int count = new int[n];
    for(int i=0;i<n;i++){
        count[i] = sc.nextInt();
    }
    int result = 0;
    int group = 0;

    Arrays.sort(count);

    for(int i:count){
        group++;
        if(group>=i){
            result++;
            count = 0;
        }
    }

    system.out.println(result);


  ```
  - 사실 이것도 ArrayList<Inteager>와 Collections 사용하면 add를 통해 숫자 추가하는 식으로도 풀 수가 있다. 