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

    

#### 新分支流程

以分支`testA`为例

* 创建新分支并切换

    ```cpp
    git checkout -b testA
    ```

* 在分支上进行内容更新

#### 分支合并

以分支`testA`为例

* 在主分支上进行分支合并（需要先拉取主分支版本至最新）

    ```cpp
    git merge testA --no-ff -m"合并分支testA"
    ```

    * 存在冲突，先解决冲突，然后重新添加提交

* 更新主分支版本。

* 对于已经完结的分支，需要进行删除。

    ```cpp
    git branch -d testA
    ```

    管理项目，需要对远程仓库中的分支进行删除。

    ```cpp
    git push origin --delete testA
    ```



#### 协作开发某分支

在已经将项目克隆至本地的前提下，使用git fetch 拉取仓库中所有分支。

```cpp
git fetch
```

然后进行分支切换即可。



#### 分支同步主分支代码

以分支`testA`为例

* 在主分支上进行拉取，确保本地的当前主分支代码为最新版本。

* 切换至自己的分支。

* 使用`merge`命令将主分支代码合并至自己的分支上。

    ```cpp
    git merge master
    ```

* 记得更新自己分支的远程仓库代码。

    

#### 多仓库管理

使用`submodule`会存在实时依赖，所以建议是将三方项目克隆至局域网，再使用该命令引用项目。

* 以`testProject1`为例，我们需要在该项目下的`thirdPart`目录中引入`testProject2`项目。

    ```cpp
    git submodule add "http://192.168.11.11:880/zhoupeifeng/testProject2.git" thirdPart/testProject2   
    ```

* 记得提交此版本。

在克隆项目时，若项目中存在引用仓库，直接克隆会出现该部分为空情况，需要使用 `--recursive` 参数进行克隆。

* 以`testProject1`为例，在该项目的`thirdPart`目录下有个引用仓库`testProject2`，直接`clone`时会出现该部分为空。

    ```cpp
    git clone --recursive http://192.168.11.11:880/zhoupeifeng/testProject1.git
    ```

* 或者，在直接克隆后，进入该目录下，执行`submodule update`命令

    ```cpp
    git submodule update --init  --recursive
    ```

在引用的三方库更新时（外部更新），在项目目录进行全局更新。

```cpp
 git submodule update --remote
```

或指定更新。

```cpp
 git submodule update --remote thirdPart/testProject2
```

在更新完成后，在本地会形成一个添加记录，需要推送至远程仓库。

