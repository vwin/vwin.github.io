# 博客
## 运行
```text
0. 本地配置git && github && node && npm yarn
1. 拉取git仓库 git clone git@github.com:vwin/vwin.github.io.git 进入src分支进行博客编写
2. 进入项目根目录，执行npm install 或者yarn install
3. 执行hexo clean && hexo s，在本地访问localhost:4000调试
4. 执行sh deploy.sh 将博客发布
5. 执行sh git.sh "commit info"将变更提交到src分支
6. npm install nodeppt,使用: ./node_modules/nodeppt/bin/nodeppt build xx.md
node ppt使用：https://daiqianying.ac.cn/2021/03/19/hexo%E5%8D%9A%E5%AE%A2%E5%B5%8C%E5%85%A5nodeppt/

常用hexo命令：
1. 创建新文章：hexo new "文章名"
2. 清理缓存：hexo clean 
3. 本地调试：hexo s
4. 部署：hexo d
5. 生成：hexo g
```