### 详细步骤：

#### 一. 在 IDEA 中创建自己的项目

我创建了一个最普通的 Java 项目用于演示

![d7d97ece2f8e4d5f97c80281f1cdc4bf.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/d7d97ece2f8e4d5f97c80281f1cdc4bf.png)

#### 二. 在 IDEA 中创建本地仓库

**2.1** 点击上方的 **VCS**，再点击 **创建 Git 仓库**

![44c45856231643b39564261431c880ac.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/44c45856231643b39564261431c880ac.png)

**2.2** 在跳出的界面中选择当前项目（这里是 `github_t2`）并点击确认,这里是选择上传的文件夹,也可以上传最高层的根目录

![d8d00894e2864f67aefaa4f54353b5c0.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/d8d00894e2864f67aefaa4f54353b5c0.png)

**2.3** 此时已经创建完本地仓库，会发现idea中的文件变为红色，这是正常的，因为这些文件还没有保存到github中，所以显示红色（未提交到 GitHub 的状态）

![02c612e9889b4733a829ed2a5f6ae414.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/02c612e9889b4733a829ed2a5f6ae414.png)

#### 三. 提交代码到本地仓库

**3.1** 点击右上角的绿色对勾按钮或左侧的 **Commit** 进入提交界面,也可以对着项目目录右键选择git -> Commit Directory

![d67a7e1670954a8892fa8e2deb57e896.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/d67a7e1670954a8892fa8e2deb57e896.png)![4e4d8b1775004f8d828b39f2e3f1ad15.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/4e4d8b1775004f8d828b39f2e3f1ad15.png)

**3.2** 选择所有文件（或部分文件），填写提交信息后点击 **Commit**

![d24f7d68e6054f7794ee82dd4d8cd4b1.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/d24f7d68e6054f7794ee82dd4d8cd4b1.png)

**3.3** 提交完成后，右下角提示文件已提交

![9823ff1098384fadb0780b3c2056b3c1.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/9823ff1098384fadb0780b3c2056b3c1.png)

#### 四. 本地仓库连接远程仓库

**4.1** 复制 GitHub 仓库的 SSH 地址（点击 **Code** → **SSH**）或者**https**

![3f566652ba3a467283fb0808ccfca945.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/3f566652ba3a467283fb0808ccfca945.png)

**4.2** 在 IDEA 中点击 **Git** → **管理远程(Manage Remotes)**

![4a7a14926b434e5db5d052fcb92353ff.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/4a7a14926b434e5db5d052fcb92353ff.png)



**4.3** 添加远程仓库(点击加号并粘贴刚刚的SSH信息)地址并确认,就已经连接远程仓库了

![3783718d4a4e4b7c90cf66560827307c.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/3783718d4a4e4b7c90cf66560827307c.png)

#### 五. 推送本地代码至 GitHub 远程仓库

5.1点击右上角的绿色箭头推送代码(或者直接git -> push)

![32cbe1e2dd19421e84ce4d30a01c83f3.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/32cbe1e2dd19421e84ce4d30a01c83f3.png)



**5.2** 在跳出的界面中出现了已经保存在本地仓库的代码,确认推送文件后点击 **Push**

![f6f64f1838134b1d9f9728172eefed5c.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/f6f64f1838134b1d9f9728172eefed5c.png)



**5.3** 推送成功提示（若未报错，直接到 GitHub 检查即可）



**5.4** 推送被拒绝的解决方案

- **5.4.1** 右下角出现推送被拒，一般会发生在第一次提交代码到远程仓库中，是因为远程仓库中已经有一些更改，这些更改你本地还没有同步



![75da86ae717a4ed2affd3a870d7f4d70.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/75da86ae717a4ed2affd3a870d7f4d70.png)

- **5.4.2** 点击下方的终端执行 `git pull origin master` 拉取远程仓库的信息



![6c6a05b2957846668235fed4e0aa2c30.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/6c6a05b2957846668235fed4e0aa2c30.png)

- **5.4.3** 做完这步，点击右上角的推送重复推送步骤，要是推送成功，那就ok,但是要是还是不行，那就继续往下看把



- **5.4.4** 现在已经拉取了远程仓库的信息，但是还是无法成功推送，那你可以在终端中输入:

  ```
  git push origin master --force
  ```

​		实现强制推送

​		**注意！！！**：这个方法会覆盖你远程仓库中的所有信息，谨慎使用！！！![d5c3d37eb6d542e2a2a854df4fe00aef.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/d5c3d37eb6d542e2a2a854df4fe00aef.png)

#### 六. GitHub 仓库检查

现在github仓库中已经出现了你刚刚上传的代码

![91537cb302d44bd4a73476bd506e78f6.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/91537cb302d44bd4a73476bd506e78f6.png)

#### 七. 拓展使用：增量提交代码

上面已经在idea中将一个项目上传至github中，要是你还是在这个项目的位置写代码，还想上传到github中，那会十分的方便，只需要三步即可，看我下面演示

**7.1** 新增文件后，文件显示为红色（未提交状态）

我这里创建了一个新的类，显示红色，表示还没上传到远程仓库

![03089c37de52408b85f7ba2cd1d989bc.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/03089c37de52408b85f7ba2cd1d989bc.png)

**7.2** 提交到本地仓库（选择文件并填写提交信息）

点击右侧的提交，选中新创建的类，填写推送信息并点击提交

![664bcc1f4a0c49d5a04e573d90ee5516.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/664bcc1f4a0c49d5a04e573d90ee5516.png)

右下角显示提交成功

![8acb8f720ce94635be7eeacc7e8ae0ce.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/8acb8f720ce94635be7eeacc7e8ae0ce.png)

**7.3** 推送至远程仓库

点击右上角的推送，在推送界面出现刚刚提交到本地仓库的代码，点击推送

![8c7dc446672b493592b2dc9a63cf1c2e.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/8c7dc446672b493592b2dc9a63cf1c2e.png)

右下角显示推送成功

![b590b3f80eb24e3c80cf40120d89133c.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/b590b3f80eb24e3c80cf40120d89133c.png)