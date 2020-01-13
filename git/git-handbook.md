#### Git Property
- 创始人：Linus Torvalds
- 诞生: 2005
- 完全分布式
- 对非线性开发模式的强力支持

#### Difference with other CVCS
- 对所有文件制作一个快照来保存信息，没有变化的文件在快照中只保留一个链接之前之前存储的文件
- 本地有项目的完整历史，基本所有操作都可以本地执行，不需要网络开销，速度快
- 在数据存储前会做校验和（SHA-1散列），当作索引

#### Config
- git config --list 查看配置列表
- .git/config , ~/.gitconfig, /etc/gitconfig 一层层覆盖


#### Problem & Solution
- [没有改变文件， 但却是已修改状态](https://dzone.com/articles/git-showing-file-modified-even)