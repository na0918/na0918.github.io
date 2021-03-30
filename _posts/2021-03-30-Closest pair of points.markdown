---
layout: single
date: 2021-03-30 110401 +0900
title: "Closest pair of points"
---

## 컴퓨터알고리즘 과제 1

```java
import java.util.Random;

class pair {
    int x;
    int y;
    public pair(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
public class Main {

    private void merge(int[] A, int p, int k, int q) {
        int[] C = new int[q-p+1];

        int i = p, j = k+1, n=0;
        while(i<=k && j<=q) C[n++] = A[i] < A[j] ? A[i++] : A[j++];
        while(i<=k) C[n++] = A[i++];
        while(j<=q) C[n++] = A[j++];

        for (int a=p, b=0; a<=q; a++) A[a] = C[b++];
    }

    public void mergeSort(int[] A, int p, int q) {
        if (p >= q) return;
        int k = (p+q) /2;
        mergeSort(A, p, k);
        mergeSort(A, k+1, q);
        merge(A, p, k, q);
    }

    public double ClosestPair_l(int[] A, int[] B, int p, int k) {
        double[] C = new double[k/2];
        C[0] =  Math.sqrt((A[0] - A[1]) + (B[0] - B[1])^2);
        C[1] = Math.sqrt((A[1] - A[2])^2 + (B[1] - B[2])^2);
        C[2] = Math.sqrt((A[2] - A[0])^2 + (B[2] - B[0])^2);

        double min = C[p];
        for(int i=0;i<C.length;i++) {
            if(min>C[i]) {
                min = C[i];
            }
        }
        return min;
    }

    public double ClosestPair_r(int[] A, int[] B, int p, int k) {
        double[] C = new double[k/2];
        C[3] = Math.sqrt((A[3] - A[4])^2 + (B[3] - B[4])^2);
        C[4] = Math.sqrt((A[4] - A[5])^2 + (B[4] - B[5])^2);
        C[5] = Math.sqrt((A[5] - A[3])^2 + (B[5] - B[3])^2);
        double min = C[p];
        for(int i=0;i<C.length;i++) {
            if(min>C[i]) {
                min = C[i];
            }
        }
        return min;
    }

    public static void main(String[] args) {
        Random r = new Random();

        int[] A = new int[6];
        int[] B = new int[6];

        for (int i=0; i<A.length; i++) {
            A[i] = r.nextInt(10);
            B[i] = r.nextInt(10);
        }

        Main sorter = new Main();
        sorter.mergeSort(A, 0, A.length-1);

        pair XY[] = new pair[6];
        for (int i=0; i<A.length; i++) {
            XY[i] = new pair(A[i], B[i]);
        }
        for (int i=0; i<A.length; i++) {
            System.out.printf("(%d, %d) ", A[i], B[i]);
        }
        System.out.println();

        double[] C = new double[A.length/2];
        C[0] = Math.sqrt((A[0] - A[1])*(A[0] - A[1]) + (B[0] - B[1])*(B[0] - B[1]));
        C[1] = Math.sqrt((A[1] - A[2])*(A[1] - A[2]) + (B[1] - B[2])*(B[1] - B[2]));
        C[2] = Math.sqrt((A[2] - A[0])*(A[2] - A[0]) + (B[2] - B[0])*(B[2] - B[0]));

        double min_l = C[0];
        for(int i=0;i<C.length;i++) {
            if(min_l>C[i]) {
                min_l = C[i];
            }
        }

        double[] D = new double[A.length/2];
        D[0] = Math.sqrt((A[3] - A[4])*(A[3] - A[4]) + (B[3] - B[4])*(B[3] - B[4]));
        D[1] = Math.sqrt((A[4] - A[5])*(A[4] - A[5]) + (B[4] - B[5])*(B[4] - B[5]));
        D[2] = Math.sqrt((A[5] - A[3])*(A[5] - A[3]) + (B[5] - B[3])*(B[5] - B[3]));
        double min_r = D[0];
        for(int i=0;i<D.length;i++) {
            if(min_r>D[i]) {
                min_r = D[i];
            }
        }
        double d = Math.min(min_l, min_r);
        System.out.println(d);               //두개의 영역만 비교했을때의 최근접 거리

        double le = A[A.length/2 - 1] - d;
        double ri = A[A.length/2] + d;
        double mid = -1;

        for(int i=0; i < (A.length/2) - 1; i++ ) {
            for(int j = A.length/2; j < A.length; j++) {
                if(A[i] >= le && A[j] <= ri) {
                    mid = Math.sqrt((A[i] - A[j])*(A[i] - A[j]) + (B[i] - B[j])*(B[i] - B[j]));
                }
            }
        }
        System.out.println(Math.min(d, mid));  //중간영역까지 포함했을 때의 최근접 거리
    }
}
```

PPT에서 제시되어있는 것처럼 완벽하게 구현하지는 못했습니다.(점이 6개일 경우만 구현했습니다 ㅠㅠ)

* 출력

1. 점의 좌표
2. 두 영역에서의 거리 d
3. 중간 영역을 포함했을 때의 거리 mid

특별하게 3번의 출력이 **-1**인 경우는 mid가 존재하지 않는 것으로 판단하여 2번이 최단 거리라고 생각하시면 됩니다. 