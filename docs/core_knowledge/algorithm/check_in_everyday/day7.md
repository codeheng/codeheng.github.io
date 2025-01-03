---
comments: true
--- 

## [344.反转字符串](https://leetcode.cn/problems/reverse-string/)

1、直接调用C++库函数来进行反转
```c++ linenums="1"
void reverseString(vector<char>& s) {
    reverse(s.begin(), s.end());
}
```
PS: **如果题目关键部分直接用库函数可以实现，那么尽量别用**

2、双指针，前后字符进行交换

=== "Java"

    ```java linenums="1"
    class Solution {
        public void reverseString(char[] s) {
            for(int i = 0, j = s.length - 1;  i < s.length / 2; i ++ , j --) {
                char t = s[i];
                s[i] = s[j];
                s[j] = t;
            }        
        }
    }
    ```

=== "C++"

    ```c++ linenums="1"
    void reverseString(vector<char>& s) {
        for (int i = 0, j = s.size() - 1; i < s.size() / 2; i ++, j --) 
            swap(s[i], s[j]);
    }
    ``` 

## [541.反转字符串II](https://leetcode.cn/problems/reverse-string-ii/)

直接进行模拟即可，利用reverse反转符合要求的

> 当需要固定规律一段一段去处理字符串的时候，想想在for循环上可实现否?

=== "Java"

    ```java linenums="1"
    class Solution {
        public String reverseStr(String s, int k) {
            char[] c = s.toCharArray();
            for(int i = 0;  i < c.length; i += 2 * k) {
                reverse(c, i, Math.min(i + k - 1, c.length - 1));
            } 
            return new String(c);
        }
        public void reverse(char[] c, int l, int r) {
            while(l < r) {
                char t = c[l];
                c[l] = c[r];
                c[r] = t;
                l ++; r --;
            }
        }
    }
    ```

=== "C++"

    ```c++ linenums="1" hl_lines="2"
    string reverseStr(string s, int k) {
        for(int i = 0; i < s.size(); i += 2 * k) {
            if (i + k > s.size()) reverse(s.begin() + i , s.end());
            else reverse(s.begin() + i, s.begin() + i + k);
        }
        return s;
    }
    ```

## [替换数字](https://kamacoder.com/problempage.php?pid=1064)

> 给定一个字符串 `s`，它包含小写字母和数字字符，请编写一个函数，将字符串中的字母字符保持不变，而将每个数字字符替换为`number`。 
> 
> e.g. 对于输入字符串 `a1b2c3`，函数应该将其转换为 `anumberbnumbercnumber`

??? Note "代码"

    ```java linenums="1"
    import java.util.*;  
    public class Main {
        public static void main(String[] args) {
            Scanner sc = new Scanner(System.in);
            String s = sc.next();
            int l = s.length();
            for(int i = 0; i < s.length(); i ++)
                if(s.charAt(i) >= '0' && s.charAt(i) <= '9') l += 5;
            char [] res = new char[l];
            for(int i = 0; i < s.length(); i ++) res[i] = s.charAt(i);
            for(int i = s.length() - 1, j = l - 1; j >= 0; i --) {
                if(res[i] >= '0' && res[i] <= '9') {
                    res[j --] = 'r';
                    res[j --] = 'e';
                    res[j --] = 'b';
                    res[j --] = 'm';
                    res[j --] = 'u';
                    res[j --] = 'n';
                } else res[j --] = res[i];
            }
            System.out.println(res);
        }
    }
    ```

## [151.反转字符串里的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

要求原地$O(1)$空间复杂度

- 整体反转，再单词再反转，空格怎么办? --> 直接用不是空格的字符进行填充
- [图片参考](https://assets.leetcode.com/users/images/b62b1a27-3688-41eb-b294-c26a5ba11d19_1634751350.7162225.png)

=== "C++"

    ```c++ linenums="1"
    string reverseWords(string s) {
        reverse(s.begin(), s.end());
        int l = 0, r = 0, i = 0;
        while( i < s.size() ) {
            while( i < s.size() && s[i] == ' ' ) i ++;
            while( i < s.size() && s[i] != ' ' ) s[r ++] = s[i ++];
            if (l < r) {
                reverse(s.begin() + l, s.begin() + r);
                s[r ++] = ' ';
                l = r;
            }
        }
        s.resize(r - 1);
        return 0;
    }
    ```
