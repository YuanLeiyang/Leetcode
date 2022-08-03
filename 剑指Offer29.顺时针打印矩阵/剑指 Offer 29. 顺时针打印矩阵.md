### 剑指 Offer 29. 顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。


解题思路：上下左右各用一个指针作为边界线。使用指针顺时针遍历元素。


```
public class SpiralOrder {
    public int[] spiralOrder(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return new int[0];
        }
        int[] result = new int[matrix.length * matrix[0].length];
        int index = 0;
        int top = 0;
        int right = matrix[0].length - 1;
        int bottom = matrix.length - 1;
        int left = 0;
        while (index < result.length) {
            int temp = left;
            while (index < result.length && temp <= right) {
                result[index++] = matrix[top][temp];
                temp++;
            }
            top++;
            temp = top;
            while (index < result.length& temp <= bottom) {
                result[index++] = matrix[temp][right];
                temp++;
            }
            right--;
            temp = right;
            while (index < result.length& temp >= left) {
                result[index++] = matrix[bottom][temp];
                temp--;
            }
            bottom--;
            temp = bottom;
            while (index < result.length& temp >= top) {
                result[index++] = matrix[temp][left];
                temp--;
            }
            left++;
        }
        return result;
    }
```

使用index作为统计元素是否遍历完成，同时在遍历过程中记录数组位置。
