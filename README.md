1. 在arm鸡同一个号子下面，开一台amd小鸡
2. 切换root并安装必要依赖
sudo -i
## For CentOS/RHEL
yum install wget -y
## For Fedora
dnf install wget -y
## For Ubuntu/Debian
apt-get update -y && apt-get install wget -y
3. 登录小鸡，下载DD包
wget http://安装包地址（.gz）
4. 等待DD包下载完毕后，解压DD包
tar xzvf *.tar.gz
gunzip *.gz
5.进入甲骨文后台，点开arm机器的页面，点击Stop（停止）按钮，将arm机器关机
![2022-02-23T08_26_59](https://user-images.githubusercontent.com/52071436/166200787-6ff65e51-5f8a-4c68-8f50-75efca1cd065.png)
6.向下滚动页面，选择左下角的Boot volume切换至引导卷详情
![2022-02-23T08_27_22](https://user-images.githubusercontent.com/52071436/166200841-9b54b20f-01d9-4905-baff-8615a23da9fa.png
7.点击引导卷右边三个小圆点，选择Detach boot volume（分离引导卷），将引导卷分离
![2022-02-23T08_28_03](https://user-images.githubusercontent.com/52071436/166200886-9d89b7a7-2c12-4f29-add5-2d21f06cb211.png)
8.等待分离完成切换到AMD小鸡的详情页面，点击左下角Attached block volumes，并点击右侧的Attach block volume. 然后在弹出页面中选择刚才分离的arm鸡的引导卷，并点击attach。
![2022-02-23T08_28_26](https://user-images.githubusercontent.com/52071436/166200942-089e1c3c-7a78-47b5-929c-d18f11665c0d.png)
![2022-02-23T08_28_42](https://user-images.githubusercontent.com/52071436/166200950-b65f266b-e5fe-4770-9078-4ea38474315f.png)
9.等待引导卷附加完成点击引导卷详情右侧的三个小黑点，选择iSCSI commands & information.
![2022-02-23T08_29_06](https://user-images.githubusercontent.com/52071436/166200983-c1bf3439-f745-43b1-8914-73aba5a3a584.png)
10.在弹出的对话框中，复制Connect命令和Disconnect命令，并分开保存
![2022-02-23T08_29_24](https://user-images.githubusercontent.com/52071436/166201030-975cf8fe-6e32-4a70-9cfb-a44ee4699299.png)
 命令示例：
 #Connect:
sudo iscsiadm -m node -o new -T ***.***.oracle.boot:uefi -p ***.***.***.***:3260
sudo iscsiadm -m node -o update -T ***.***.oracle.boot:uefi -n node.startup -v automatic
sudo iscsiadm -m node -T ***.***.oracle.boot:uefi -p ***.***.***.***:3260 -l

#Disconnect:
sudo iscsiadm -m node -T ***.***.oracle.boot:uefi -p ***.***.***.***:3260 -u
sudo iscsiadm -m node -o delete -T ***.***.oracle.boot:uefi -p ***.***.***.***:3260
11.附加硬盘登录amd小鸡，然后在小鸡上逐条执行Connect命令
12.DD系统命令如下，等待DD过程完成：
dd if=安装包.img of=/dev/安装位置sdb bs=10M status=progress
13.分离arm引导卷
在AMD小鸡上逐条执行上面的Disconnet命令，完成后，进入甲骨文后台回到AMD小鸡详情页的Attached block volumes 页面，点击邮编三个小黑点，选择Detach。
![2022-02-23T08_31_29](https://user-images.githubusercontent.com/52071436/166201199-0252aeaf-a2d9-4770-9ae8-4e81c161b601.png)
14.将引导卷重新挂在至arm鸡
回到arm鸡的详情页面，点开左下角Boot volume，右侧三个小黑点，Attach boot volum。
![2022-02-23T08_31_56](https://user-images.githubusercontent.com/52071436/166201222-d32c3289-c6d1-4f48-93ec-73c4294ad67d.png)
![2022-02-23T08_32_11](https://user-images.githubusercontent.com/52071436/166201232-80399f06-0573-440a-a635-d0cac0ea4591.png)
15.点击Start启动arm鸡
![2022-02-23T08_32_32](https://user-images.githubusercontent.com/52071436/166201266-96547ef4-d4fc-43fb-9547-e105ad6e6209.png)
16.重新登录失联的arm机器
启动成功后，即可重新登录失联的arm机器，用户名root，密码 CoiaPrant#CentOS7 ，密钥 >>点击下载<< ，登录后强烈建议第一时间修改密码并删除默认密钥，保证机器安全。
#修改Root密码
passwd root
#删除Root私钥
rm -f /root/.ssh/authorized_keys


