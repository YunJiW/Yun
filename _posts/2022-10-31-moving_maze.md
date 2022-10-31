---
layout: page
title: "백준 16954 - 움직이는 미로 탈출 "



| 시간 제한 | 메모리 제한 | 제출  | 정답 | 맞힌 사람 | 정답 비율 |
| :-------- | :---------- | :---- | :--- | :-------- | :-------- |
| 2 초      | 512 MB      | 10438 | 3326 | 2077      | 27.682%   |

## 문제

욱제는 학교 숙제로 크기가 8×8인 체스판에서 탈출하는 게임을 만들었다. 체스판의 모든 칸은 빈 칸 또는 벽 중 하나이다. 욱제의 캐릭터는 가장 왼쪽 아랫 칸에 있고, 이 캐릭터는 가장 오른쪽 윗 칸으로 이동해야 한다.

이 게임의 특징은 벽이 움직인다는 점이다. 1초마다 모든 벽이 아래에 있는 행으로 한 칸씩 내려가고, 가장 아래에 있어서 아래에 행이 없다면 벽이 사라지게 된다. 욱제의 캐릭터는 1초에 인접한 한 칸 또는 대각선 방향으로 인접한 한 칸으로 이동하거나, 현재 위치에 서 있을 수 있다. 이동할 때는 빈 칸으로만 이동할 수 있다.

1초 동안 욱제의 캐릭터가 먼저 이동하고, 그 다음 벽이 이동한다. 벽이 캐릭터가 있는 칸으로 이동하면 더 이상 캐릭터는 이동할 수 없다.

욱제의 캐릭터가 가장 오른쪽 윗 칸으로 이동할 수 있는지 없는지 구해보자.

## 입력

8개 줄에 걸쳐서 체스판의 상태가 주어진다. '`.`'은 빈 칸, '`#`'는 벽이다. 가장 왼쪽 아랫칸은 항상 벽이 아니다.

## 출력

욱제의 캐릭터가 가장 오른쪽 윗 칸에 도착할 수 있으면 1, 없으면 0을 출력한다.





## CODE

```java
package ReCommand_Lee;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

import java.util.Queue;
import java.util.LinkedList;


public class Moving_Maze_16954 {
	//욱제와 벽들의 좌표 체크
	static class Point{
		int row;
		int col;
		
		Point(int x, int y){
			this.row = x;
			this.col = y;
			
		}
		
		
	}
	
	static char[][] Map = new char[8][8];
	
	//욱제 캐릭 위치 체크용
	static Point body = new Point(7,0);
	
	//벽 개수
	static Point[] Wall;
	static int Wall_cnt = 0;
	
	static int dx[] = {1,1,1,0,0,0,-1,-1,-1};
	static int dy[] = {-1,0,1,-1,0,1,-1,0,1};
	
	public static void main(String[] args)throws IOException{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		
		//벽의 개수가 정해진게 아니기때문에 최대 개수로 저장
		Wall = new Point[8*8];
		
		for(int row =0; row <8; row++)
		{
			String Line = br.readLine();
			for(int col =0; col <8; col++)
			{
				Map[row][col] = Line.charAt(col);
				if(Map[row][col] == '#') {
					Wall[Wall_cnt++] = new Point(row,col);
				}
			}
		}
		
		if(Run()) {
			System.out.print(1);
		}else
			System.out.print(0);
		
		
	}
	
	private static void print() {
		
		for(int cnt =0; cnt < Wall_cnt;cnt++)
		{
			Map[Wall[cnt].row][Wall[cnt].col] = '#';
		}
		
		for(int row =0; row <8; row++)
		{
			for(int col =0; col <8; col++)
			{
				System.out.print(Map[row][col]);
			}
			System.out.println();
		}
		System.out.println();
		
	}

	private static boolean Run() {
		Queue<Point> q = new LinkedList<>();
		q.offer(body);
		
		
		boolean visited[][];
		
		while(!q.isEmpty())
		{
			int move = q.size();
			
			//중복으로 가는곳 제외시키기
			visited = new boolean[8][8];
			
			for(int index =0; index < move; index++)
			{
				Point cur = q.poll();
				
				if(Map[cur.row][cur.col] == '#')
					continue;
				if(cur.row == 0 && cur.col == 7)
					return true;
				
				//8방향 체크
				for(int dir = 0; dir < 9 ;dir++)
				{
					int m_row = cur.row + dx[dir];
					int m_col = cur.col + dy[dir];
					
					//바깥일경우 제외
					if(m_row < 0 || m_row >= 8 || m_col < 0 || m_col >= 8)
						continue;
					
					//벽이거나 이미 방문했으면 제외
					if(Map[m_row][m_col] == '#' || visited[m_row][m_col])
						continue;
					
					visited[m_row][m_col] = true;
					q.offer(new Point(m_row,m_col)); 
					
				}
			}
			//욱제 움직이고 벽 움직이기
			move_Wall();

		}
		return false;
	}

	private static void move_Wall() {
		for(int wall_cnt =0; wall_cnt < Wall_cnt; wall_cnt++)
		{
			Map[Wall[wall_cnt].row][Wall[wall_cnt].col] = '.';
			Point tmp = Wall[wall_cnt];
			//벽이 아래로 내려갈곳이 없을경우 사라짐
			tmp.row = tmp.row+1;
			if(tmp.row == 8) {
				Wall[wall_cnt] = Wall[--Wall_cnt];
				wall_cnt--;
				continue;
			}
			
			Wall[wall_cnt].row = tmp.row;
		}
		
		for(int index = 0; index <Wall_cnt;index++)
		{
			Map[Wall[index].row][Wall[index].col] = '#'; 
		}
		
	}

}

```
