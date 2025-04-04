## 根据身高重建队列

<img width="567" alt="image" src="https://github.com/user-attachments/assets/d6dfcbcf-36a7-4ce8-bcef-8fc1e08a736d" />

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda x: (-x[0], x[1])) # 根据身高升序 同时根据第二个值降序
        n = len(people)
        # print(people)
        res = []
        for p in people:
            if len(res) <= p[1]:
                res.append(p)
            else:
                res.insert(p[1],p)
            
        return res

```
