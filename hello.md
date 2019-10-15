```
$ sudo add-apt-repository ppa:graphics-dirvers/ppa
$ ubuntu-drivers devices
$ sudo apt-get install nvidia-driver-430
```

```
# 运行CUDA安装脚本
$ sudo sh cuda_10.0.130_410.48_linux.run

# 在~/.bashrc文件添加如下几行，配置环境变量
export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
export CUDA_HOME=/usr/local/cuda

# 使配置的环境变量生效
$ source ~/.bashrc
```

```
# 解压CUDNN压缩包
$ tar -zxvf cudnn-10.0-linux-x64-v7.6.3.30.tgz

# 将解压后的文件拷贝至/usr/local/cuda目录下的相应目录
$ sudo cp cuda/include/cudnn.h    /usr/local/cuda/include      
$ sudo cp cuda/lib64/libcudnn*    /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/ libcudnn*
```

```
$ pip install virtualenv

# 测试安装是否成功
$ virtualenv –version
```

```
$ pip install virtualenvwrapper

# 在~/.bashrc文件最后添加如下一行，配置环境变量
export WORKON_HOME=~/Envs

# 使环境变量生效
$ source ~/.bashrc
$ source /usr/local/bin/virtualenvwrapper.sh
```

```
# 使用-p参数指定Python解释器版本为Python3
$ mkvirtualenv -p python3 vagrs-env
```

```
$ workon vagrs-env
(vagrs-env) $ pip install -r requirements.txt
```

```
(vagrs-env) $ pip install gunicorn
```

```
(vagrs-env) $ gunicorn --workers=4 --bind=localhost:8000 wsgi:app
```

```
$ sudo apt update
$ sudo apt install nginx
```

```
# 删除默认的配置文件
$ sudo rm /etc/nginx/sites-enabled/default
# 新建一个跟本项目关联的配置文件
$ sudo vim /etc/nginx/sites-enabled/vagrs
```

```
server {
    listen 80 default_server;
    server_name _;
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;
    location / {
        proxy_pass http://localhost:8000;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

```
$ sudo nginx -t

# 如果输出如下两行，则配置正确
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

```
$ sudo nginx restart
```
