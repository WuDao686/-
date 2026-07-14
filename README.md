这是我的一个个人博客，使用的是GitHub简单部署
添加文章方式：
# 进入博客目录
cd 盘符:\hugo\myblog

# 创建新文章
盘符:\hugo\hugo.exe new post/文章标题.md

# 编辑 content/post/文章标题.md
# 把 draft: true 改为 draft: false
# 在 +++ 下面写正文

# 提交推送
git add .

git commit -m "新文章"

git pull --rebase

git push

博客模板套用的是hugo
借助的是此博主主题：https://d-sketon.github.io
