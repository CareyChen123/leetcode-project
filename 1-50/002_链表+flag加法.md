# 方法一 思路
将两个链表的数据转换成int型数据，然后int相加得到目标数，然后再将目标数拆分成数字放在列表里。
# 缺陷
即使换成long long太大的数字也存不下
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */


struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2){
    long long num1=0,num2=0,num3=0;
    int num[200]={0};
    int i = 0,j=0,k=0;
    struct ListNode *tmp;
    tmp = l1;
    while(tmp != NULL)
    {
        num1=num1+tmp->val*pow(10,i);
        i++;
        tmp = tmp->next;
    }
    tmp = l2;
    while(tmp != NULL)
    {
        num2=num2+tmp->val*pow(10,j);
        j++;
        tmp = tmp->next;
    }
    num3 = num1+num2;
    do // 0+0至少要循环一次，不然k值不累加，导致后面申请0空间
    {
        num[k]=num3%10;
        num3=num3/10;
        k++;
    }while(num3!=0);
    // printf("%d\n",k);
    struct ListNode *p=(struct ListNode*)malloc(sizeof(struct ListNode)*k); // malloc空间要强制转换类型
    for(i=0;i<k-1;i++)
    {
        p[i].val=num[i]; // p[i]是实例,用.
        p[i].next=p+i+1; // p[i].next=&(p[i+1]);要加取地址符
    }
    p[k-1].val=num[k-1];
    p[k-1].next=NULL; // 一定要指向null,申请空间后里面内容随机
    // struct ListNode* s=p;
    // while(s!=NULL)
    // {

    //     printf("%d\n",s->val);
    //     s=s->next;
    // }
    return p;

}
```

# 方法二 思路
将两个链表的数字分别存在两个数组中并统计个数，用位数多的数组长度作为累加的循环边界，累加要置flag位，并且若最高位相加大于9，则目标链表长度还需要加一,最后将数字放在另一个数组；最后申请链表空间，并把数组中的值赋给链表节点值。
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */


struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2){
    int num1[100]={0},num2[100]={0},flag=0;
    int num[200]={0};
    int i = 0,j=0,k=0;
    struct ListNode *tmp;
    // printf("%d",num1[3]);
    tmp = l1;
    while(tmp != NULL)
    {
        num1[i]=tmp->val;
        i++;
        tmp = tmp->next;
    }
    tmp = l2;
    while(tmp != NULL)
    {
        num2[j]=tmp->val;
        j++;
        tmp = tmp->next;
    }
    if(i>=j)
        k=i;
    else
        k=j;
    for(i=0;i<k;i++)
    {
        // num[i]=num1[i]+num2[i]+flag;
        if((num1[i]+num2[i]+flag)>9)
        {
            num[i] = (num1[i]+num2[i]+flag)%10;
            flag = 1;
        }
        else
        {
            num[i] = num1[i]+num2[i]+flag;
            flag = 0;
        }
    }
    if((num1[k-1]+num2[k-1]+flag)>9)
    {
        num[k] = 1;
        k=k+1;
    }
    // num3 = num1+num2;
    // do
    // {
    //     num[k]=num3%10;
    //     num3=num3/10;
    //     k++;
    // }while(num3!=0);
    // printf("%d\n",k);
    struct ListNode *p=(struct ListNode*)malloc(sizeof(struct ListNode)*k);
    for(i=0;i<k-1;i++)
    {
        p[i].val=num[i];
        p[i].next=&(p[i+1]);
    }
    p[k-1].val=num[k-1];
    p[k-1].next=NULL;
    // struct ListNode* s=p;
    // while(s!=NULL)
    // {

    //     printf("%d\n",s->val);
    //     s=s->next;
    // }
    return p;

}
```
# c++ 时间复杂度：O(max(m,n))
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        vector<int> num1;
        vector<int> num2;
        int flag=0;

        while(l1!=NULL)
        {
            num1.push_back(l1->val);
            l1=l1->next;
        }

        while(l2!=NULL)
        {
            num2.push_back(l2->val);
            l2=l2->next;
        }

        ListNode *head=new ListNode();
        ListNode *tmp=head;

        (num1.size()>num2.size()) ? num2.resize(num1.size(),0) : num1.resize(num2.size(),0);

        for(int i=0; i<max(num1.size(),num2.size()); i++)
        {
            tmp->next=new ListNode();
            tmp->next->val=(num1[i]+num2[i]+flag)%10;
            flag=(num1[i]+num2[i]+flag)/10;
            tmp=tmp->next;
        }

        if(flag>0)
        {
            tmp->next=new ListNode();
            tmp->next->val=flag;
        }

        return head->next;
    }
};
```
