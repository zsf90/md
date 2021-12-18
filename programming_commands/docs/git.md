# git 命令

## 创建 github 仓库后给出的使用提示

```shell
echo "# md" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:zsf90/md.git
git push -u origin main
```