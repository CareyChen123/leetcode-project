# 思路 遍历原来的字串，我们把前半个括号的值放在一个栈（新的数组）里，当遇到后半个括号的值时，就看新建栈内是否为空，若为空返回false；若不为空，则看栈顶元素与次后半部括号类型是否匹配，匹配的话
就把栈顶指针减一，以此循环。当最后栈里元素为空时，返回true，否则返回false。
```c
bool isValid(char * s){
    int n =strlen(s);
    if(n%2!=0) // 奇数个不符合条件
        return false;
    char stk[n+1]; // 可变长度不能初始化
    int top = 0;
    for(int i=0; i<n; i++)
    {
        if(s[i] == '(' || s[i] == '{' || s[i] == '[')
        {
            stk[top] = s[i]; // 栈顶元素如果找到匹配的后半部分就会被下一个前半部分(下一个栈顶元素（（，{，或[）)覆盖
            top++;
        }
        else
        {
            if(top == 0 || (s[i] == ')'&&stk[top-1] != '(') || (s[i] == '}'&&stk[top-1] != '{') || (s[i] == ']'&&stk[top-1] != '['))
            {
                // printf("%d,%c,%c\n",top,s[i],stk[top-1]);
                return false;
            }
            else
            {
                top--;
            }
        }
    }
    // printf("%d\n",top);
    return top == 0;

}
```
# 思路：c++,用map容器和stack容器。
```c
class Solution {
public:
    bool isValid(string s) {
        int n=s.size();
        if(n%2!=0)
            return false;
        map<char,char> pairs={
            {')','('},
            {'}','{'},
            {']','['}
        };
        stack<char> stk;
        for(char ch:s)
        {
            if(pairs.count(ch)) //查找是否含有ch键
            {
                if(stk.empty() || pairs[ch]!=stk.top())
                    return false;
                else{
                    stk.pop();
                }
            }else{
                stk.push(ch);
            }
        }
        return stk.empty();
    }
};
```
