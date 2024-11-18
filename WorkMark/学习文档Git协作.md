## Git 协作

小组成员分领各自模块，每个模块开一条分支。

项目建立测试用类，配置给各成员，调用方式在开发时固定，小组成员开发完成功能后可通过此工具自行测试。

或使用测试框架进行单元测试。



* git checkout XXX 切换至已有分支

* git checkout -b XXX 创建新分支并切换进该分支中。
* 在分支上修改后，按正常提交方式提交至分支中。
* 分支内容完成后，需要将其合并到主分支中，先要通过git checkout master 切换至主分支，再用git merge XXX --no-ff -m"xxxxxx"进行合并分支， --no-ff的作用是禁用fast forward 模式，在合并时生成一次提交版本。

* 通常情况下，一个项目有开发版本 master支   和发布版本 release分支；开发过程整体在master上进行，在阶段性完成与程序bug修正后，可发布时由master传至release分支。
* 对于已完结的分支、bug修正后的分支，可通过git branch -d XXX 删除。