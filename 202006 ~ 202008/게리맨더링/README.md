
## 게리맨더링(문제 _ [17471](https://www.acmicpc.net/problem/17471))

백준시의 시장 최백준은 지난 몇 년간 게리맨더링을 통해서 자신의 당에게 유리하게 선거구를 획정했다. 견제할 권력이 없어진 최백준은 권력을 매우 부당하게 행사했고, 심지어는 시의 이름도 백준시로 변경했다. 이번 선거에서는 최대한 공평하게 선거구를 획정하려고 한다.

백준시는 N개의 구역으로 나누어져 있고, 구역은 1번부터 N번까지 번호가 매겨져 있다. 구역을 두 개의 선거구로 나눠야 하고, 각 구역은 두 선거구 중 하나에 포함되어야 한다. 선거구는 구역을 적어도 하나 포함해야 하고, 한 선거구에 포함되어 있는 구역은 모두 연결되어 있어야 한다. 구역 A에서 인접한 구역을 통해서 구역 B로 갈 수 있을 때, 두 구역은 연결되어 있다고 한다. 중간에 통하는 인접한 구역은 0개 이상이어야 하고, 모두 같은 선거구에 포함된 구역이어야 한다.

공평하게 선거구를 나누기 위해 두 선거구에 포함된 인구의 차이를 최소로 하려고 한다. 백준시의 정보가 주어졌을 때, 인구 차이의 최솟값을 구해보자.

**입력**
첫째 줄에 구역의 개수 N이 주어진다. 둘째 줄에 구역의 인구가 1번 구역부터 N번 구역까지 순서대로 주어진다. 인구는 공백으로 구분되어져 있다.

셋째 줄부터 N개의 줄에 각 구역과 인접한 구역의 정보가 주어진다. 각 정보의 첫 번째 정수는 그 구역과 인접한 구역의 수이고, 이후 인접한 구역의 번호가 주어진다. 모든 값은 정수로 구분되어져 있다.

구역 A가 구역 B와 인접하면 구역 B도 구역 A와 인접하다. 인접한 구역이 없을 수도 있다.

**출력**
첫째 줄에 백준시를 두 선거구로 나누었을 때, 두 선거구의 인구 차이의 최솟값을 출력한다. 두 선거구로 나눌 수 없는 경우에는 -1을 출력한다.
 
## 예제

	6
	5 2 3 4 1 2
	2 2 4
	4 1 3 6 5
	2 4 2
	2 1 3
	1 2
	1 2

	answer : 1


## 풀이

n개의 구역을 두개의 선거구로 나누기 위해서 1 선거구와 2 선거구에 뽑을 구역의 수를 정했다.

만약 n이 5일때, 다음과 같이 구역의 수를 나눌 수 있다.

| 1선거구 구역의 수 | 2선거구 구역의 수  |
|--|--|
| 1 | 4 |
| 2 | 3 |
| 3 | 2 |
| 4 | 1 |

즉 for문을 돌아가며 n개중 1,2,3,4개를 뽑아 1선거구에 넣는다면 자연스럽게 1선거구와 2선거구는 나뉘게된다.

이때 각 선거구의 구역의 수를 2,3으로 나누는 것과 3,2로 나누는 것은 *동일한 결과를 나타낸다는 사실*을 알아챌 수 있다. (1,4와 4,1도 마찬가지)

그렇기 때문에 **n개 중 1~n/2개를 뽑아** 1선거구에 넣으면 중복으로 코드가 실행되는 것을 막을 수 있다.

이렇게 1선거구 구역을 뽑는다면, 뽑히지 않은 구역들은 자연스럽게 2선거구로 들어간다.




마지막으로 뽑은 각 선거구가 서로 연결되어 있는지 판단하고, 연결되어 있다면 인구수의 차이를 구하면 된다.

    구역구가 연결 되었는지 판단 -> dfs()
    


## 코드
```java

		// main 함수 일부코드 
		for (int i = 1; i <= n / 2; i++) {
			selected = new boolean[n + 1];
			func(0, 1, i); // 현재 뽑은 수 (0), 시작 인덱스(1), 뽑아야 하는 개수(i)
		}

		if (min == Integer.MAX_VALUE) // 두 선거구로 나눌 수 없는 경우
			System.out.println("-1");
		else
			System.out.println(min); // 인구 최소값 출력

	private static void func(int cur, int idx, int m) {
		if (cur == m) {
			if (check()) { // 만약 두 팀 다 연결되었다면
				min = Math.min(cal(), min); // 최소값 갱신
			}
			return;
		}

		for (int i = 1; i <= n; i++) {
			if (selected[i]) // 이미 뽑은 경우라면
				continue;

			selected[i] = true; // 선택
			func(cur + 1, i + 1, m); // 다음 구역 선택하러 출발
			selected[i] = false; // 선택 취소
		}
	}

	private static int cal() { .. 생략 .. } // 두 선거구 간 인구수 차이 구하기

	private static void dfs(int i, int type, boolean chk) { .. 생략 .. } // 그래프 탐색

	private static boolean check() { // dfs함수를 통해 선거구가 연결되었는지 판단
		for (int i = 1; i <= n; i++) {
			visited1[i] = false;
			visited2[i] = false;
		} // 초기화

		boolean flag1 = false, flag2 = false;

		for (int i = 1; i <= n; i++) {
			if (selected[i]) {
				if (!visited1[i]) {
					if (!flag1) { // 처음이라면
						dfs(i, 1, true); // 한번의 dfs로 뽑힌 선거구 모두 방문처리 됨
						flag1 = true;
					} else
						return false; // 연결되어 있지 않다면 (1 선거구)
				}
			} else {
				if (!visited2[i]) {
					if (!flag2) {// 처음이라면
						dfs(i, 2, false); // 한번의 dfs로 뽑힌 선거구 모두 방문처리 됨
						flag2 = true;
					} else
						return false; // 연결되어 있지 않다면 (2 선거구)
				}
			}
		}

		return true; // 잘 연결되어 있는 경우
	}
```
