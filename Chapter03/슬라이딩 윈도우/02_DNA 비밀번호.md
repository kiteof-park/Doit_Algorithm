### DNA 비밀번호
#### 문제
평소에 문자열을 가지고 노는 것을 좋아하는 민호는 DNA 문자열을 알게 되었다.  
DNA 문자열은 모든 문자열에 등장하는 문자가 {‘A’, ‘C’, ‘G’, ‘T’} 인 문자열을 말한다.  
예를 들어 “ACKA”는 DNA 문자열이 아니지만 “ACCA”는 DNA 문자열이다.  
이런 신비한 문자열에 완전히 매료된 민호는 임의의 DNA 문자열을 만들고 만들어진 DNA 문자열의 부분문자열을 비밀번호로 사용하기로 마음먹었다.  
하지만 민호는 이러한 방법에는 큰 문제가 있다는 것을 발견했다.  
임의의 DNA 문자열의 부분문자열을 뽑았을 때 “AAAA”와 같이 보안에 취약한 비밀번호가 만들어 질 수 있기 때문이다.  
그래서 민호는 부분문자열에서 등장하는 문자의 개수가 특정 개수 이상이여야 비밀번호로 사용할 수 있다는 규칙을 만들었다.   
임의의 DNA문자열이 “AAACCTGCCAA” 이고 민호가 뽑을 부분문자열의 길이를 4라고 하자.  
그리고 부분문자열에 ‘A’ 는 1개 이상, ‘C’는 1개 이상, ‘G’는 1개 이상, ‘T’는 0개 이상이 등장해야 비밀번호로 사용할 수 있다고 하자.  
이때 “ACCT” 는 ‘G’ 가 1 개 이상 등장해야 한다는 조건을 만족하지 못해 비밀번호로 사용하지 못한다.  
하지만 “GCCA” 은 모든 조건을 만족하기 때문에 비밀번호로 사용할 수 있다. 
민호가 만든 임의의 DNA 문자열과 비밀번호로 사용할 부분분자열의 길이, 그리고 {‘A’, ‘C’, ‘G’, ‘T’} 가 각각 몇번 이상 등장해야  
비밀번호로 사용할 수 있는지 순서대로 주어졌을 때 민호가 만들 수 있는 비밀번호의 종류의 수를 구하는 프로그램을 작성하자.  
단 부분문자열이 등장하는 위치가 다르다면 부분문자열이 같다고 하더라도 다른 문자열로 취급한다.

#### 입력
첫 번째 줄에 민호가 임의로 만든 DNA 문자열 길이 |S|와 비밀번호로 사용할 부분문자열의 길이 |P| 가 주어진다. (1 ≤ |P| ≤ |S| ≤ 1,000,000)  
두번 째 줄에는 민호가 임의로 만든 DNA 문자열이 주어진다.  
세번 째 줄에는 부분문자열에 포함되어야 할 {‘A’, ‘C’, ‘G’, ‘T’} 의 최소 개수가 공백을 구분으로 주어진다.  
각각의 수는 |S| 보다 작거나 같은 음이 아닌 정수이며 총 합은 |S| 보다 작거나 같음이 보장된다.
```
4 2
GATA
1 0 0 1
```

#### 출력
첫 번째 줄에 민호가 만들 수 있는 비밀번호의 종류의 수를 출력해라.
```
2
```

---
```java
import java.util.*;
import java.io.*;

public class Main{
    static int[] minCount = new int[4];
    static int[] myCount = new int[4];
    
    public static void main(String[] args) throws IOException{
        // 1. S(문자열 길이), P(부분 문자열 길이)
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int S = Integer.parseInt(st.nextToken());
        int P = Integer.parseInt(st.nextToken());
        
        // 2. DNA 문자열 -> 문자배열
        String str = br.readLine();
        char[] dna = str.toCharArray();
        
        // 3. A,C,G,T 최소 개수
        st = new StringTokenizer(br.readLine());
        for(int i = 0; i < 4; i++){
            minCount[i] = Integer.parseInt(st.nextToken());
        }
        
        // 4. 슬라이딩 윈도우
            // (1) 문자배열을 윈도우 사이즈로 제한
            // (2) 조건 검사
            // (3) 다음 윈도우로 이동
        int cnt = 0;
        
        for(int i = 0; i < P; i++){
            add(dna[i]);
        }
        if(isValid()){
            cnt++;
        }
        
        for(int i = P; i < S; i++){
            add(dna[i]);
            remove(dna[i - P]);
            if(isValid()){
                cnt++;
            }
        }
        
        System.out.println(cnt);
    }
    
    static void add(char c){
        switch(c){
            case 'A' : myCount[0]++; break;
            case 'C' : myCount[1]++; break;
            case 'G' : myCount[2]++; break;
            case 'T' : myCount[3]++; break;
        }
    }
    
    static boolean isValid(){
        for(int i = 0; i < 4; i++){
            if(myCount[i] < minCount[i]){
                return false;
            }
        }
        return true;
    }
    
    static void remove(char c){
        switch(c){
            case 'A' : myCount[0]--; break;
            case 'C' : myCount[1]--; break;
            case 'G' : myCount[2]--; break;
            case 'T' : myCount[3]--; break;
        }
    }   
}
```

### 정리
- 인덱스 범위 지정이 헷갈린다!
- `i` : 윈도우의 오른쪽 끝
- `i = P` : 이전 윈도우에서 왼쪽 끝, 즉 빠져나갈 문자
