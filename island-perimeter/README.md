# Leetcode: Island Perimeter     :BLOG:Medium:


---

Island Perimeter  

---

You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water. Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells). The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.  

    Example:
    
    [[0,1,0,0],
     [1,1,1,0],
     [0,1,0,0],
     [1,1,0,0]]
    
    Answer: 16

Github: [challenges-leetcode-interesting](https://github.com/DennyZhang/challenges-leetcode-interesting/tree/master/island-perimeter)  

Credits To: [leetcode.com](https://leetcode.com/problems/island-perimeter/description/)  

Leave me comments, if you have better ways to solve.  

    ## Blog link: http://brain.dennyzhang.com/island-perimeter
    class Solution(object):
        def islandPerimeter(self, grid):
            """
            :type grid: List[List[int]]
            :rtype: int
            """
            row_count = len(grid)
            if row_count==0: return 0
            col_count = len(grid[0])
            cell_count, neighbor_count = 0, 0
            for i in xrange(row_count):
                for j in xrange(col_count):
                    if grid[i][j] == 1:
                        cell_count += 1
                        # only count the right and down
                        if j != col_count-1 and grid[i][j+1] == 1: neighbor_count += 1
                        if i != row_count-1 and grid[i+1][j] == 1: neighbor_count += 1
            return cell_count*4 - 2*neighbor_count
    
        ## Basic Idea:  Get how many 1 cells. Let's say it's m
        ##              Find how many 1-1 pair which is adjacent, let's say it's n
        ##              The result is 4*m-2*n
        ## Complexity: Time O(m*n), Space O(1)
        def islandPerimeter_v1(self, grid):
            """
            :type grid: List[List[int]]
            :rtype: int
            """
            self.row_count = len(grid)
            if self.row_count==0: return 0
            self.col_count = len(grid[0])
    
            cell_count = 0
            for i in xrange(self.row_count):
                for j in xrange(self.col_count):
                    if grid[i][j] == 1:
                        cell_count += 1
    
            self.adjacent_count = 0
            for i in xrange(self.row_count):
                for j in xrange(self.col_count):
                    if grid[i][j] == 1:
                        self.DFSMark(grid, i, j)
                        return 4*cell_count - self.adjacent_count
    
        def DFSMark(self, grid, i, j):
            if self.isValidIndex(i, j) is False: return
            if grid[i][j] != 1: return
    
            grid[i][j] = 2
            self.adjacent_count += self.GetAdjacentCellCount(grid, i, j)
            self.DFSMark(grid, i-1, j)
            self.DFSMark(grid, i+1, j)
            self.DFSMark(grid, i, j-1)
            self.DFSMark(grid, i, j+1)
    
        def isValidIndex(self, i, j):
            return not(i<0 or i>=self.row_count or \
                        j<0 or j>=self.col_count)
    
        def GetAdjacentCellCount(self, grid, i, j):
            count = 0
            if self.isValidIndex(i-1, j) and grid[i-1][j] != 0: count += 1
            if self.isValidIndex(i+1, j) and grid[i+1][j] != 0: count += 1
            if self.isValidIndex(i, j-1) and grid[i][j-1] != 0: count += 1
            if self.isValidIndex(i, j+1) and grid[i][j+1] != 0: count += 1
            # print i, j, count
            return count