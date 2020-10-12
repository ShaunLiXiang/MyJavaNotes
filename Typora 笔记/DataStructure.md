Data Structure Notes

# 一、稀疏数组（SparseArray）

定义：[rows]=sum+1 [cols]=3 [value]=sum

第一行 二维数组的行数，二维数组的列数，二维数组非0的值的个数

```java
int chessArray[][] = new int[11][11];
chessArray[1][2] = 1;
chessArray[2][3] = 2;
//遍历二维数组
for (int row[]: chessArray){
    for (int data : row){
            System.out.print(data+"\t");
    }
    System.out.println();
}

//统计非0的值的个数
int sum = 0;
for (int i = 0; i <11 ; i++) {
    for (int j = 0; j <11; j++) {
        if(chessArray[i][j] != 0){
            sum++;
        }
    }
}
System.out.println(sum);
//定义稀疏数组
int sparseArray [][] = new int[sum+1][3];
sparseArray [0][0] =11;
sparseArray [0][1] =11;
sparseArray [0][2]=sum;
int count = 0;
for (int i = 0; i <chessArray.length ; i++) {
    for (int j = 0; j <chessArray[i].length ; j++) {
        if(chessArray[i][j] != 0){
            count++;
            sparseArray[count][0]=i;
            sparseArray[count][1]=j;
            sparseArray[count][2]=chessArray[i][j];
        }
    }
}

System.out.println("=========");
for (int i = 0; i <sparseArray.length ; i++) {
    System.out.print(sparseArray[i][0]+"\t");
    System.out.print(sparseArray[i][1]+"\t");
    System.out.print(sparseArray[i][2]+"\t");
    System.out.println();
}
```



```java
//将SparseArray转化为二维数组
int chessArray2[][]=new int[sparseArray[0][0]][sparseArray[0][1]];
//将SparseArray中的值赋值给二维数组
//注意：
	=====》稀疏数组第一列只是描述二维数组
for (int i = 1; i <sparseArray.length ; i++) {
    chessArray2[sparseArray[i][0]][sparseArray[i][1]] = sparseArray[i][2];
}
for (int[] rows : chessArray2) {
    for (int data : rows) {
        System.out.print(data+"\t");
    }
    System.out.println();
}
```

## 稀疏数组存档

```java
//写入到map.data
        try (ObjectOutputStream bw = new ObjectOutputStream(new FileOutputStream("map.data"));){
            Object ob = sparseArray;
            bw.writeObject(ob);
            bw.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
//读取文件
        System.out.println("====++0.0++=====");
		//序列化
        try(ObjectInputStream ois = new ObjectInputStream(new FileInputStream("map.data"))){
            int sparseArray2 [][] = (int[][]) ois.readObject();
            for (int[] rows : sparseArray2) {
                for (int data : rows) {
                    System.out.print(data+"\t");
                }
                System.out.println();
            }
        }
```



# 二、数组模拟队列

```java
package com.li.sparseArray.queue;

public class ArrayQueue {
    public static void main(String[] args) {
        QueueArray queueArray = new QueueArray(5);
        queueArray.addQueue(2);
        queueArray.addQueue(3);
        queueArray.showQueue();
    }
}

class QueueArray{
    private int maxSize;//表示数组的最大容量
    private int front;//队列头
    private int rear;//队列尾部
    private int [] arr;//该数据用于存放数据，模拟队列

    //创建队列的构造器

    public QueueArray(int maxSize) {
        this.maxSize = maxSize;
        arr = new int[maxSize];
        front = -1; //===》指向队列头的【前一个位置】
        rear = -1;  //===》【直接】指向队列尾的数据
    }

    //判断队列是否满
    public boolean isFull(){
        return rear == maxSize - 1;
    }

    //判断队列是否为空
    public boolean isEmpty(){
        return front == rear;
    }

    //将数据添加到队列
    public void addQueue(int data){
        //判断队列是否满
        if(isFull()){
            System.out.println("Sorry,列满");
            return;
        }else{
            rear++;//让rear后移一个单位
            arr[rear] = data;
            //或者 直接 ===》 arr[++rear] = data;
        }
    }

    //出队列
    public int getQueue(){
        //判断队列是否为空
        if(isEmpty()){
            throw new RuntimeException("队列为空");
        }
        front ++;
        return arr[front];
    }

    //显示队列的所有非空的数据
    public void showQueue(){
        if(!isEmpty()){
            for (int data : arr) {
                System.out.println(data);
            }
        }
    }
    //显示队列的头数据，注意不是取数据
    public int headQueue(){
        if(!isEmpty()){
            return arr[front+1];
        }else{
            throw new RuntimeException("队列为空");
        }
    }
}
```

## 2.1数组实现环形队列

front	   ：===》0

rear	    ：===》预留空间，即最后一个元素的后一个位置

maxSize ：数组长度

牺牲数组的最后一个空间来实现环形队列，注意：这里 【rear+1】回到队列头

**最大有效数据为：**(rear+maxSize-front)%maxSize

**断数组是否满：**===》(rear+1+maxSize)%maxSize=front

**判断数组是否为空：**front == rear【空】



## 2.2按照编号的顺序添加

2.2.1.首先找到新添加的节点的位置，是通过辅助（变量）遍历

2.2.2新节点的next = 原temp.next

2.2.3将现temp.next = 新的节点

```java
//按编号顺序来添加
public void addByOrder(HeroNode heroNode){
        HeroNode temp = head;
        boolean flag = false; //判定flag的标志的编号是否存在，默认false
        while(true){
            if(temp.next == null){
                break;
            }
            if(temp.next.no >heroNode.no){ //next的编号 域大于 将要插入的节点的编号
                //插入数据
                break;
            }else if(temp.next.no == heroNode.no){
                //已存在。抛出异常，或sout
                flag = true;
                break;
            }
            temp = temp.next;
        }
        //判断flag的值
        if(flag){
            //不能添加，编号已存在
            System.out.println("准备插入的数据的编号已存在"+heroNode.no);
        }else{
            heroNode.next = temp.next;
            temp.next = heroNode;
        }
    }
```

2.3.4栈逆序打印

```java
//逆序打印
public void reversePrint(HeroNode head){
    if(head == null){
        System.out.println("链表为空不能打印");
        return;
    }
    HeroNode current = head.next;
    Stack<HeroNode> stack = new Stack<>();
    while(current != null){
        stack.push(current);
        System.out.println(current+"==========");
        current = current.next;
    }
    //将stack中的各个节点打印出来
    while(stack.size() >0){
        System.out.println(stack.pop());
    }
}
```