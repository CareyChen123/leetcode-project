# 思路：先将s和t分别去掉#后放在两个栈中，再比较这两个栈是否相同。
```c
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        stack<char> stk_s;
        stack<char> stk_tt;
        s_to_stk(s,stk_s);
        s_to_stk(t,stk_tt);
        if(stk_s.size()!=stk_tt.size())
            return false;
        while(!stk_s.empty())
        {
            if(stk_s.top()==stk_tt.top())
            {
                stk_s.pop();
                stk_tt.pop();
            }else{
                return false;
            }
        }
        return true;
        
    }
    void s_to_stk(string s,stack<char> &stk)
    {
        for(char ch : s)
        {
            if(ch=='#')
            {
                if(!stk.empty())
                {
                    stk.pop();
                }
            }else{
                stk.push(ch);
            }
        }
    }
};
```
