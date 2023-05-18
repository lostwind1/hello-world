# hello-world
random code
//Your code begin.
//示例仅供参考，你也可以自行修改设计
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#define MAX 10000

typedef struct ListNode            //结点结构，哈夫曼树与频度链表共用
{
    char      c;                    //结点的字符
    int      frequency;            // 字符的频度
    char     *code;            // 字符的编码(对哈夫曼树结点有效)
    struct ListNode *parent;            //结点的双亲结点(对哈夫曼树结点有效)
    struct ListNode *left;                //结点的左子树(对哈夫曼树结点有效)
    struct ListNode *right;                // 结点的右子树(对哈夫曼树结点有效)
    struct ListNode *next;                // 结点的后继结点(对频度链表结点有效)
}ListNode,HuffmanTree;

void make_struct(ListNode *l,ListNode *h);
int findnum(ListNode * l);
void sort(ListNode * l,int num);
int search(ListNode *l,char ch);
void createtree(ListNode * l,ListNode * tail,int num);
void Huffmancode(ListNode * l,int num);
void count(ListNode * l,int num);
int main()
{
	ListNode *h=NULL;
	h=(ListNode*)malloc(sizeof(ListNode));
	h->next=NULL;
	ListNode *l=h;
	make_struct(l,h);
	int num=findnum(l);
    sort(l,num);
    for(h=l->next;h->next!=NULL;h=h->next);
    createtree(l->next,h,num);
    Huffmancode(l,num);
    int i=0;
    ListNode * turn;
	for(turn=l->next;i<num;i++,turn=turn->next){
        if(turn->c=='\n') 
        	printf("'\\n' %d %s\n",turn->frequency,turn->code);
        else 
        	printf("'%c' %d %s\n",turn->c,turn->frequency,turn->code);   
    }
    count(l->next,num);
    return 0;
}
void make_struct(ListNode *l,ListNode *h){
	char ch;
	while((ch=getchar())!=EOF){			//逐步读取单个字符
		if(l->next&&search(l->next,ch)) continue;			//检验字符是否已被存入链表
		ListNode *p=(ListNode*)malloc(sizeof(ListNode));
		h->next=p;
        p->c=ch;
		p->frequency=1;
		h=p;
		h->next=NULL;
	}
}
int findnum(ListNode * l){
	int num=0;
	ListNode *p=l->next;
	while(p!= NULL){
        num++;
        p->parent=NULL;
        p=p->next;
    }
    return num;
}
void sort(ListNode * l,int num){
    int i,num1;
	ListNode *p,*tail,*q;
	p=l;
	for(i=0;i<num-1;i++){
		q=l->next;
		p=q->next;
		tail=l;
        num1=num-i-1;
		while(num1--){
			if(p->frequency > q->frequency){
				q->next=p->next;
				p->next=q;
				tail->next=p;
			}
			tail=tail->next;
			q=tail->next;
			p=q->next;
		}
	}
}
int search(ListNode *l,char ch){
	while(l!=NULL){
		if(ch==l->c){
			l->frequency+=1;
			return 1;
		}
		l=l->next;
	}
	return 0;
}
void createtree(ListNode * l,ListNode * tail,int num){
	int min_1,min_2;
	ListNode *son_1,*son_2,*pa,*t;
	for(int i=1;i<num;i++){
		min_1=MAX,min_2=MAX;
		for(t=l;t!=NULL;t=t->next){
		if(t->frequency < min_1 && t->parent==NULL){
			min_1=t->frequency;
			son_1=t;	
		}
	}
			pa=(ListNode*)malloc(sizeof(ListNode));
			son_1->parent=pa;
			pa->left=son_1;
			son_1->code="0";
		for(t=l;t!=NULL;t=t->next){
		if(t->frequency < min_2 && t->parent==NULL){
			min_2=t->frequency;
			son_2=t;
		}
	}
			son_2->parent=pa;
			pa->right=son_2;
			son_2->code="1";
		pa->frequency=son_1->frequency+son_2->frequency;
		pa->parent=NULL;
		tail->next=pa;
		tail=pa;
		tail->next=NULL;
	}
}
void Huffmancode(ListNode * l,int num){
	ListNode * p,* pon;
	int i=0;
	for(p=l->next;i<num;i++,p=p->next){
		for(pon=p->parent;pon->parent!=NULL;pon=pon->parent){
			char *result=NULL;
			result=(char*)malloc(1 * sizeof(char));
			strcpy(result,pon->code);
			strcat(result,p->code);
			p->code=result;
		}
	}
}
void count(ListNode * l,int num){
	int sum=0,i=0,len;
	for(;i<num;i++,l=l->next){
		len=strlen(l->code);
		sum+=len*l->frequency;
	}
	printf("%d",sum);
}
//Your code end.
