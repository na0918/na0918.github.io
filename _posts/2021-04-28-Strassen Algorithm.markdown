---
layout: single
date: 2021-04-28 110401 +0900
title: "Strassen Algorithm"
---
# Strassen Algorithm

## Strassen Algorithm(슈트라센 알고리즘)

### 슈트라센 알고리즘의 특징

* 이론적으로만 봤을 때 시간복잡도가 적어 성능이 더 좋다
* P, Q, R 등 담아둘 변수들을 마련해야 하므로 행렬이 너무 커지면 메모리 할당량이 커진다.
* 일반 행렬의 곱셈보다 더 빠르게 수행되기 위해선 매우 큰 n이 필요하고 n이 너무 크기에 슈퍼 컴퓨터에 사용된다.
* 슈트라센 알고리즘은 정사각행렬에 대해서만 처리가 가능하기 때문에 행렬의 곱셈을 하기 위해서는 정사각행렬로 변경하는 작업이 필요하다.

### 슈트라센 알고리즘의 시간복잡도
![KakaoTalk_20210428_203321848](https://user-images.githubusercontent.com/80372995/116397864-12c39100-a862-11eb-9a41-1980aef8fe3b.jpg)


![KakaoTalk_20210428_203322073](https://user-images.githubusercontent.com/80372995/116397912-1f47e980-a862-11eb-838a-0ce6a5e7076e.jpg)


## 소스코드

### 소스코드 구현
```java
import java.util.Scanner;
import java.util.Random;

public class Strassen {
    
    //행렬의 합
    public int[][] add(int[][] A, int[][] B) {
        int n = A.length;
        int[][] C = new int[n][n];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                C[i][j] = A[i][j] + B[i][j];
        return C;
    }
    
    //행렬의 차
    public int[][] sub(int[][] A, int[][] B) {
        int n = A.length;
        int[][] C = new int[n][n];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                C[i][j] = A[i][j] - B[i][j];
        return C;
    }
    
    //행렬의 분배
    public void spl(int[][] G, int[][] C, int i, int j) {
        for(int i1 = 0, i2 = i; i1 < C.length; i1++, i2++)
            for(int j1 = 0, j2 = j; j1 < C.length; j1++, j2++)
                C[i1][j1] = G[i2][j2];
    }
    
    //행렬의 삽입
    public void put(int[][] C, int[][] G, int i, int j) {
        for(int i1 = 0, i2 = i; i1 < C.length; i1++, i2++)
            for(int j1 = 0, j2 = j; j1 < C.length; j1++, j2++)
                G[i2][j2] = C[i1][j1];
    }
    
    //행렬의 곱
    public int[][] mul(int[][] A, int[][] B) {
        int n = A.length;
        int[][] C = new int[n][n];

        if (n == 1)
            C[0][0] = A[0][0] * B[0][0];
        else {
            int[][] A11 = new int[n/2][n/2];
            int[][] A12 = new int[n/2][n/2];
            int[][] A21 = new int[n/2][n/2];
            int[][] A22 = new int[n/2][n/2];
            int[][] B11 = new int[n/2][n/2];
            int[][] B12 = new int[n/2][n/2];
            int[][] B21 = new int[n/2][n/2];
            int[][] B22 = new int[n/2][n/2];

            spl(A, A11, 0 , 0);
            spl(A, A12, 0 , n/2);
            spl(A, A21, n/2, 0);
            spl(A, A22, n/2, n/2);

            spl(B, B11, 0 , 0);
            spl(B, B12, 0 , n/2);
            spl(B, B21, n/2, 0);
            spl(B, B22, n/2, n/2);

            int [][] P = mul(add(A11, A22), add(B11, B22));
            int [][] Q = mul(add(A21, A22), B11);
            int [][] R = mul(A11, sub(B12, B22));
            int [][] S = mul(A22, sub(B21, B11));
            int [][] T = mul(add(A11, A12), B22);
            int [][] U = mul(sub(A21, A11), add(B11, B12));
            int [][] V = mul(sub(A12, A22), add(B21, B22));


            int [][] C11 = add(sub(add(P, S), T), V);
            int [][] C12 = add(R, T);
            int [][] C21 = add(Q, S);
            int [][] C22 = add(sub(add(P, R), Q), U);


            put(C11, C, 0 , 0);
            put(C12, C, 0 , n/2);
            put(C21, C, n/2, 0);
            put(C22, C, n/2, n/2);
        }
        return C;
    }

    public static void main (String[] args) {
        Scanner scan = new Scanner(System.in);
        Random random = new Random();
        Strassen s = new Strassen();

        System.out.println("행과 열의 개수(n) :");
        int N = scan.nextInt();

        System.out.println("행렬 1");
        int[][] A = new int[N][N];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                A[i][j] = random.nextInt(9);
                System.out.print(A[i][j] +" ");
            }
            System.out.println();
        }

        System.out.println("\n행렬 2");
        int[][] B = new int[N][N];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                B[i][j] = random.nextInt(9);
                System.out.print(B[i][j] +" ");
            }
            System.out.println();
        }

        int[][] C = s.mul(A, B);

        System.out.println("\n행렬의 곱(A와B) : ");
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++)
                System.out.print(C[i][j] +" ");
            System.out.println();
        }
    }
}
```
### 소스코드 입력
* 행과 열(n)을 입력받아 **nxn** 크기의 행렬를 만든다.

`*행과 열의 개수(n)* : 4`

### 소스코드 출력
* Random 클래스를 활용하여 행렬 1과 행렬 2에 0~9사이의 숫자들을 랜덤하게 출력한다.
* 행렬 1과 행렬 2을 곱하여 출력한다.


`행렬 1
0 5 3 5 
3 4 8 0 
5 2 0 1 
9 5 3 7 

행렬 2
4 9 7 7 
3 6 2 7 
2 2 9 3 
2 4 6 4 

행렬의 곱(A와B) : 
31 56 67 64 
40 67 101 73 
28 61 45 53 
71 145 142 135`