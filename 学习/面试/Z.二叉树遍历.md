### 二叉树思想
前序遍历的方式是：先访问根节点，然后访问左树，最后访问右树。
中序遍历的方式是：先访问左树，接着访问根结点，最后访问右树。
后序遍历的方式是：先访问左树，接着访问右树，最后访问根结点。

### 创建二叉树
```java
class TreeNode{
     TreeNode left;   //左子树
     TreeNode right;   //右子树
     int data;    //存储的数据
}
```


### 二叉树前序遍历
递归
```java
//递归前序遍历
    public static void pre(TreeNode head){
        if(head==null){
            return;
        }
        System.out.println(head.data);
        pre(head.left);
        pre(head.right);
 }

``` 

非递归
```java
    //非递归前序遍历
    public static void preOrderUnRecur(TreeNode head){
        if(head==null){
            return;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(head);
        while (!stack.isEmpty()){
            head=stack.pop();
            System.out.println(head.data);
            if(head.right!=null){
                stack.push(head.right);
            }
            if(head.left!=null){
                stack.push(head.left);
            }
        }
    }

```



### 二叉树中序遍历
递归
``` java
    //中序遍历
    public static void in(TreeNode head){
        if(head==null){
            return;
        }
        in(head.left);
        System.out.println(head.data);
        in(head.right);
    }

```
非递归

```java
    //非递归中序遍历
    public static void inOrderUnRecur(TreeNode head){
        if(head==null){
            return;
        }
        Stack<TreeNode> stack=new Stack<>();
        while (!stack.isEmpty()||head!=null){
            if(head!=null){
                stack.push(head);
                head=head.left;
            }else {
                head=stack.pop();
                System.out.println(head.data);
                head=head.right;
            }
        }
    }
```




### 二叉树后序遍历
递归
``` java
    //递归后序遍历
    public static void afterOrderRecur(TreeNode head){
        if(head==null){
            return;
        }
        after(head.left);
        after(head.right);
        System.out.println(head.data);
    }
```

非递归
```java
    //非递归后序遍历
    public static void aftOrderUnRecur(TreeNode head){
        if(head==null){
            return;
        }
        Stack<TreeNode> s1=new Stack<>();
        Stack<TreeNode> s2=new Stack<>();
        s1.push(head);
        while (!s1.isEmpty()){
            head=s1.pop();
            s2.push(head);
            if(head.left!=null){
                s1.push(head.left);
            }
            if(head.right!=null){
                s1.push(head.right);
            }
        }
        while (!s2.isEmpty()){
            System.out.println(s2.pop().data);
        }
    }
```