# 第3章Linux的用户与权限

## 1、root用户

![image-20240704224854931](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240704224854931.png)

![image-20240704224632656](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240704224632656.png)



## 2、su、exit命令（切换、退出用户的命令）

![image-20240704224759579](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240704224759579.png)



## 3、sudo命令（临时获取root用户权限的命令）

![image-20240704225022056](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240704225022056.png)

### notes：为普通用户itheima配置sudo认证

![image-20240704225159957](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240704225159957.png)





## 4、用户和用户组

 ![image-20240705162612443](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240705162612443.png)



### notes：`groupadd、groupdel`命令（用户组管理的命令）

![image-20240705162807021](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240705162807021.png)



### notes：`useradd、userdel、usermod -aG`命令（用户管理的命令）

![image-20240705162822569](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240705162822569.png)

![image-20240705180019914](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240705180019914.png)





## 5、查看文件/文件夹权限（就是查看各个用户对于当前文件的操作权限）

![image-20240705164234603](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240705164234603.png)

![image-20240705164302676](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240705164302676.png)

![image-20240705164331130](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240705164331130.png)





## 6、chmod命令（修改各个用户对于当前文件/文件夹的操作权限）

![image-20240705164414207](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240705164414207.png)

### notes：权限的数字表示（和二进制表示差不多，稍微了解一下）

![image-20240705164427047](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240705164427047.png)



## 7、chown命令（修改文件/文件夹所属用户、用户组的命令）

![image-20240705164504688](%E7%AC%AC3%E7%AB%A0Linux%E7%9A%84%E7%94%A8%E6%88%B7%E4%B8%8E%E6%9D%83%E9%99%90.assets/image-20240705164504688.png)

