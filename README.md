    这是一个使用caddy v2做前端代理，registry web+registry做后端来部署的docker registry服务。本项目改自：https://github.com/mkuchin/docker-registry-web-examples
    本项目依赖于docker、docker-compose运行。
            
## 这是个使用caddy做前端部署的[docker-registry-web](https://github.com/mkuchin/docker-registry-web)的示例配置
    这是一个使用caddy v2做前端代理，registry web+registry做后端来部署的docker registry服务。本项目改自：https://github.com/mkuchin/docker-registry-web-examples
    本项目依赖于docker、docker-compose运行。

### 项目介绍
    在本配置里，caddy反向代理registry-web的8080端口用于实现用户认证、WEB管理，以及反向代理registry的5000端口用于docker client使用login、push、pull等客户端命令访问仓库，为了简单化，把caddy、registry-web、registry部署在了同一个docker命名网络空间。如果需要把该服务整体与前端隔离开，可以用nginx+registr-web和registry部署在独立的网络命名空间，并在docker-compose编排文件里把nginx跨caddy的网络部署。

#### 准备工作: 安装docker-compose。这里使用了最新版本的docker-compose，你可以根据需要选择合适自己平台的版本。已经安装的可以跳过此步骤。

#### 运行:

    curl -L https://github.com/peace4j/caddy-registry-web/releases/download/0.1.0-rc1/examples.tar.gz | tar -xzv
    cd examples/nginx-auth-enabled/
    ./generate-keys.sh
    docker-compose up
    


#### 管理员登录创建用户:
    使用缺省管理员账号： 用户admin/密码admin 在浏览器里登录 https://your.domain，按照下面的图示建立一个新用户，并赋予该用户read和write的权限，这样就可以使用该用户进行docker login、push、pull。
![Creating user](https://raw.githubusercontent.com/mkuchin/docker-registry-web-examples/master/images/create-test.gif)

#### Push、Pull镜像

    docker login your.domain
    docker pull hello-world
    docker tag hello-world your.domain/hello-world:latest
    docker tag hyper/docker-registry-web:latest your.domain/docker-registry-web:latest
    docker push your.domain/hello-world:latest
    docker push your.domain/docker-registry-web:latest

	
#### 从浏览器登录查看仓库
![Repo](https://raw.githubusercontent.com/mkuchin/docker-registry-web-examples/master/images/repo.gif)
