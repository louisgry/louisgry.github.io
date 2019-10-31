---
title: 'Backtracking: LeetCode 79. Word Search'
date: 2019-10-30 15:59:23
categories: 算法
tags: 
- backtracking
---
- backtracking
    - 79 Word Search：https://leetcode.com/problems/word-search/
        - 查询二维字符数组中，连起来是否有word
        - Input: board=[['A','B','C','E'],['S','F','C','S'],['A','D','E','E']]，word="ABCCED"
        - Output: true
        <!-- more -->
    - 思路：回溯法，递归树中搜索上下左右四个方向
    - 时间复杂度：O(m*n*m*n)
    - 空间复杂度：O(m*n)
    ```java
    private boolean[][] visited;
    private int[][] d = {{-1,0},{0,1},{1,0},{0,-1}};
    int m, n;
    public boolean exist(char[][] board, String word) {
        m = board.length;
        n = board[0].length;
        visited = new boolean[m][n];
        for(int i=0; i<m; i++) {
            for(int j=0; j<n; j++) {
                if(searchWord(board, word, 0, i, j) == true) {
                    return true;
                }
            }
        }
        return false;
    }
    private boolean searchWord(char[][] board, String word, int index, int startx, int starty) {
        if(index == word.length()-1) {
            return board[startx][starty] == word.charAt(index);
        }
        if(board[startx][starty] == word.charAt(index)) {
            visited[startx][starty] = true;
            // 向四个方向寻找
            for(int i=0; i<4; i++) {
                int newx = startx + d[i][0];
                int newy = starty + d[i][1];
                if(inArea(newx, newy) && !visited[newx][newy]) {
                    if(searchWord(board, word, index+1, newx, newy) == true) {
                        return true;
                    }
                }
            }
            visited[startx][starty] = false;
        }
        return false;
    }
    private boolean inArea(int x, int y) {
        return x>=0 && x<m && y>=0 && y<n;
    }
    ```