## **문제 요약**

> **양궁대회에서 라이언이 우승할 수 있는 방법들 중 가장 낮은 점수를 더 많이 맞힌 경우를 찾아 return, 없다면 \[-1\] return**

### **\[제한 조건\]** 

-   어피치와 라이언이 양궁대회 결승전에 올라왔다.


-   다양한 선수가 양궁대회에서 우승할 수 있도록 하기 위해, 운영위원회는 라이언에게 불리하도록 결승전 규칙을 정하였다.
    -   **k점에 여러발을 맞추었더라도, k점만 획득할 수 있다.**
    -   k점을 어피치가 a발, 라이언이 b발을 맞췄을 경우, a >= b 일 경우에 어피치가 k 점을 획득한다.


-   주어진 상황은 어피치가 화살 n발을 다 쏜 후, 라이언이 화살을 쏠 차례이다. 
    -   예를 들어 총 4발을 쏜다고 할 때, 어피치가 10점을 2발 쏘고 라이언이 10점을 3발 쏘았다면,
        -   어피치에게 남은 화살은 2발, 라이언에게 남은 화살은 1발이다.
        -   이 때, 라이언이 어피치를 이길 수 있는 경우는 존재하지 않는다.
            -   어피치가 9점에 2발을 쏘고 라이언이 남은 1발을 9점에 쏜다면, 라이언은 어피치와 동점이 된다.
            -   최종점수가 동점일 때는 어피치가 이긴 것으로 경기 규칙이 정해져 있으므로 라이언은 이길 수 없다.
            

### ****\[입출력\]****

1.  **n**: 각 양궁 선수가 화살을 쏠 수 있는 총 개수 (1 ≤ n ≤ 10)
2.  **info**: 어피치가 쏜 화살의 정보 (0 ≤ info의 원소 ≤ n, info의 원소 총합 = n) 
3.  라이언이 어피치를 이길 수 없는 경우에는 \[-1\] return
4.  라이언이 어피치를 이길 수 있다면 이길 수 있는 경우 중, 가장 낮은 점수를 더 많이 맞힌 경우 return

| **n** | **info** | **result** |
| --- | --- | --- |
| 5 | \[2,1,1,1,0,0,0,0,0,0,0\] | \[0,2,2,0,1,0,0,0,0,0,0\] |

-   info의 i 번째 원소는 과녁의 10-i 점을 맞힌 화살의 개수
-   즉, 0번째 원소의 점수는 10점
-   1번째 원소의 점수는 9점

---

## **문제 접근** 

-   깊이우선탐색(dfs)와 백트래킹을 이용하여 문제를 풀었습니다. 
-   높은 점수부터 라이언이 쏜 화살의 개수를 탐색하기 때문에, 라이언이 가장 큰 점수차이로 우승할 수 있는 방법 중 가장 낮은 점수를 더 많이 맞힌 경우가 최종 return 됩니다. 

> _자세한 접근 법은 코드의 주석을 확인하세요_ 🙂

```java
// 22. 06. 11. 토 - 2022 카카오 블라인드 - Lv.2 - 양궁대회

public class Solution {
    public static void main(String[] args) {
        solution(5, new int[]{2, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0});
    }

    public static int[] solution(int n, int[] info) {
        int[] answer = {};
        dfs(0, n, new int[11], info);
        answer = min_array;
        return answer;
    }


    static int[] min_array = {-1}; // 라이언이 쏜 화살의 합 중 라이언의 점수가 어피치 점수보다 낮거나 같으면 [-1]
    static int max_score = Integer.MIN_VALUE; // 라이언이 쏜 화살의 합 중 어피치 점수보다 높을 경우에만 갱신

    /**
     * 라이언이 어피치가 쏜 화살만큼 화살을 쏘고, 어피치보다 점수가 높은 경우들 중 가장 낮은 점수를 더 많이 맞힌 경우를 찾아낸다.
     *
     * @param level: 라이언이 쏜 화살의 개수
     * @param n: 어피치가 쏜 화살의 총 개수
     * @param scores: 라이언이 쏜 화살 정보를 보관하는 배열
     * @param info: 어피치가 쏜 화살 정보를 보관하는 배열
     */
    private static void dfs(int level, int n, int[] scores, int[] info) {
        if (level == n) {
            int total_score = 0;
            int opposite_score = 0;

            for (int i = 0; i <= 10; i++) {
                if (scores[i] > 0 || info[i] > 0) {
                    if (scores[i] > info[i]) total_score += 10 - i;
                    else opposite_score += 10 - i;
                }
            }

            if (total_score > opposite_score && total_score - opposite_score >= max_score) {
                // 가장 낮은 점수를 더 많이 맞힌 경우를 min_array에 넣기
                max_score = total_score - opposite_score;
                min_array = scores.clone();
            }
            return;
        }

        for (int i = 0; i <= 10; i++) {
            // 라이언이 쏜 화살의 개수가 어피치가 쏜 화살의 개수보다 많은 경우 더 살펴볼 필요가 없다.
            // 라이언이 어피치를 이기기 위해서는 각 점수대의 어피치가 쏜 화살의 개수와 같거나 개수보다 더 작게 쏘아야 하기 때문이다.
            // 즉, 총 개수 n발을 쏠 수 있을 때, 라이언은 각 점수대에서 어피치가 쏜 화살의 개수보다 같거나 작게 쏘아야 한다.
            if(scores[i] > info[i]) break;

            scores[i]++;
            dfs(level + 1, n, scores, info);
            scores[i]--;
        }
    }
}
```

---

> 문제 링크: [link](https://programmers.co.kr/learn/courses/30/lessons/92342)

 
