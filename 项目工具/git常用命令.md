### 1. 撤销commit  
- git reset --soft HEAD~1 撤回最近一次的commit(撤销commit，不撤销git add)  
- git reset --mixed HEAD~1 撤回最近一次的commit(撤销commit，撤销git add)  
- git reset --hard HEAD~1 撤回最近一次的commit(撤销commit，撤销git add,还原改动的代码)  

### 2. 提交分支  
git push origin test-ly

### 3. 撤销错误add操作  
git status 先看一下add 中的文件
git reset HEAD 如果后面什么都不跟的话 就是上一次add 里面的全部撤销了
git reset HEAD XXX.py 就是对某个py文件进行撤销了
git reset HEAD file  即使对file文件夹进行撤销

### 4. rebase操作  
