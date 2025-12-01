# docker 单机部署

## 仓库代码

1. 部署仓库地址
```
https://github.com/CLIM02/tsdd

git clone -b main0_https --single-branch git@github.com:CLIM02/tsdd.git

```
2. 客服工作台仓库地址
```
git clone -b 123edu --single-branch git@github.com:bearxiao9756/TangSengDaoDaoWeb.git
```
3. 客户A链仓库地址
```
git clone -b 123edu_B --single-branch git@github.com:bearxiao9756/TangSengDaoDaoWeb.git
```
4. 后台管理系统仓库地址
```
git clone -b 123edu --single-branch git@github.com:bearxiao9756/TangSengDaoDaoManager.git
```
5. 后台服务仓库地址

大群

```
git clone -b 123edum --single-branch git@github.com:bearxiao9756/TangSengDaoDaoServer.git
```
单群

```
git clone -b 123edus --single-branch git@github.com:bearxiao9756/TangSengDaoDaoServer.git

```
无群
```
git clone -b 123edux --single-branch git@github.com:bearxiao9756/TangSengDaoDaoServer.git

```

## 拉取代码与配置
### 拉取配置
```
git@github.com:CLIM02/tsdd.git
```
## 安装 docker
## 安装 compose
## 运行
```
docker-compose -f docker-compose.yaml up -d
```
放行端口  
```
客户web 82 
后台管理 83
文件服务 9000
文件管理 9001
APP TCP 长链接 5100
Web TCP 长链接 5200
API 端口 8090

```

# 开发调试


### 拉取服务端代码
```
git clone  git@github.com:bearxiao9756/TangSengDaoDaoServer.git
```
### 拉取客户端代码
```
git clone  git@github.com:bearxiao9756/TangSengDaoDaoWeb.git
```
### 拉取服务端代码
```
git clone git@github.com:bearxiao9756/TangSengDaoDaoManager.git
```

### 编译镜像
编译后台服务
```
docker build -t  tsddserver .
```
编译web 
```
docker build -t tsddweb .
```
编译改变web，暂定为 kehu
```
docker build -t tsddkehu .

```
编译后台
```
docker build -t tsddmanager .
```

### git 处理

同步远程分支信息
```
git fetch --all
```
切换分支
```
git checkout   branch name
```