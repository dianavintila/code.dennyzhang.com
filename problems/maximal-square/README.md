# Leetcode: Maximal Square     :BLOG:Medium:


---

Maximal Square  

---

Similar Problems:  
-   [Unique Paths](https://code.dennyzhang.com/unique-paths)
-   [Review: Dynamic Programming Problems](https://code.dennyzhang.com/review-dynamicprogramming)
-   Tag: [#dynamicprogramming](https://code.dennyzhang.com/tag/dynamicprogramming), [#inspiring](https://code.dennyzhang.com/tag/inspiring)

---

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.  

For example, given the following matrix:  

    1 0 1 0 0
    1 0 1 1 1
    1 1 1 1 1
    1 0 0 1 0

Return 4.  

Github: [challenges-leetcode-interesting](https://github.com/DennyZhang/challenges-leetcode-interesting/tree/master/maximal-square)  

Credits To: [leetcode.com](https://leetcode.com/problems/maximal-square/description/)  

Leave me comments, if you have better ways to solve.  

---

    // Blog link: https://code.dennyzhang.com/maximal-square
    // Basic Ideas: dynamic programming
    // dp[i][j]: length of the rectangle
    //    dp[i][j] = min(dp[i-1][j-1], dp[i][j-1], dp[i-1][j]) + 1
    //
    // We can do it with one pass
    // Let's assume we can change the original matrix
    // Thus we don't need extra space
    //
    // Complexity: Time O(n*m), Space O(1)
    func maximalSquare(matrix [][]byte) int {
        row_count := len(matrix)
        if row_count == 0 { return 0 }
        col_count := len(matrix[0])
        res := 0
        // initialize counter for corner case
        for i:=0; i<row_count; i++ {
            if matrix[i][0] == '1' { res = 1 }
        }
        for j:=0; j<col_count; j++ {
            if matrix[0][j] == '1' { res = 1 }
        }
        for i:=1; i<row_count; i++ {
            for j:=1; j<col_count; j++ {
                if (matrix[i][j] == '1') {
                    v := int(matrix[i-1][j-1])-48
                    if v>int(matrix[i-1][j])-48 { v=int(matrix[i-1][j])-48 }
                    if v>int(matrix[i][j-1])-48 { v=int(matrix[i][j-1])-48 }
                    v += 1
                    matrix[i][j] = byte(v+48)
                    if v > res { res = v }
                }
            }
        }
        return res*res
    }