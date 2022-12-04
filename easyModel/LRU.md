```go
type LRUCache struct {
    n int
    L,R *Node
    key2Node map[int]*Node
}


func Constructor(capacity int) LRUCache {
    tL:=NewNode(-1,-1)
    tR:=NewNode(-1,-1)
    tL.right=tR
    tR.left=tL
    return LRUCache{
        n:capacity,
        L:tL,
        R:tR,
        key2Node:make(map[int]*Node),
    }
}


func (this *LRUCache) Get(key int) int {
    //不存在
    if _,ok := this.key2Node[key];!ok{
        return -1
    }else{
        //存在
        p:=this.key2Node[key];
        this.delete_node(p);
        this.insert_node(p);
        return p.val;
    }
}

func (this *LRUCache) delete_node(p *Node){
    p.left.right=p.right;
    p.right.left=p.left;
}

func (this *LRUCache) insert_node(p *Node){
    p.left=this.L
    p.right=this.L.right
    this.L.right.left=p
    this.L.right=p
}

func (this *LRUCache) Put(key int, value int)  {
    //不存在
    if _,ok := this.key2Node[key];!ok{
        //容量已满
        if(len(this.key2Node)>=this.n){
            p:= this.R.left;
            this.delete_node(p);
            delete(this.key2Node,p.key)
        }
        //新建插入
        p:=NewNode(key,value)
        this.insert_node(p);
        this.key2Node[key]=p;
    }else{
        //存在则更新
        p:=this.key2Node[key];
        p.val=value
        this.delete_node(p)
        this.insert_node(p)
    }
}


type Node struct{
    key,val int
    left,right *Node
}

func NewNode(key,val int) *Node{
    return &Node{
        key:key,
        val:val,
    }
}
```