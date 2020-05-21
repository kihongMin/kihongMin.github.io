# LeetCode daily challenge - 3주차 BST / inorder traversal

## <week 3 - Kth Smallest Element in a BST>

Given a binary search tree, write a function `kthSmallest` to find the **k**th smallest element in it. 

**Example 1:**

```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```

**Example 2:**

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

**Follow up:**
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

 

**Constraints:**

- The number of elements of the BST is between `1` to `10^4`.
- You may assume `k` is always valid, `1 ≤ k ≤ BST's total elements`.

전통적인 트리 순회(Tree Traversal) 문제로 이진 트리의 속성을 잘 알고있다면 접근하기 어렵지 않은 문제이다. 중위순회(Inorder Traversal) 방식으로 접근하면 오름차순으로 데이터에 접근할 수 있으며 이 순서를 기억해 k번째 작은 수를 확인하면 이를 반환하면 된다.

이 문제는 처음에 재귀 함수로 접근했지만 재귀에는 약해서.. 몇 가지 이슈가 존재했다. 처음에 짠 코드는 다음과 같다.

```python
def search(tree,k,count=0):
	if tree.left:
		search(tree.left)
	count+=1
	if count == k:
		return tree.val
	if tree.right:
		search(tree.right)
```

1. 재귀가 올바르게 return을 반환하지 못하는 점![스크린샷 2020-05-21 오후 3.45.19](/Users/mingihong/Library/Application Support/typora-user-images/스크린샷 2020-05-21 오후 3.45.19.png)
2. count를 올바르게 세지 못한 점

![image-20200521154557105](/Users/mingihong/Library/Application Support/typora-user-images/image-20200521154557105.png)

위와 같은 문제로 인해 마음편하게 class 변수를 사용해 문제를 해결하기로 했다.

1번 이슈는 조건을 만족했을 때 정답을 클래스 변수에 할당하는 것으로 해결했고, 2번 이슈는 count를 클래스 변수로 사용하여 해결했다. 이럴 때는 포인터를 사용하는 C를 쓰고 싶은 마음이 들기는 한다...

```python
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        self.count = 0
        self.search(root,k)
        return self.answer
        
    def search(self,tree,k):
        if self.count > k: # 이상의 탐색 진행X
            return
        if tree.left:
            self.search(tree.left,k)        
        
        self.count+=1
        if self.count == k:
            self.answer = tree.val
            return

        if tree.right:
            self.search(tree.right,k)
```

