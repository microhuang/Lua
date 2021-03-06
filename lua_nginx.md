# lua-nginx-module 安装

安装luajit：
```
#http://luajit.org/install.html
wget http://luajit.org/download/LuaJIT-2.0.2.tar.gz
tar zxvf LuaJIT-2.0.2.tar.gz
cd LuaJIT-2.0.2
make
make install
```

调整nginx编译参数，并编译安装：
```
        --with-ld-opt='-Wl,-rpath,/usr/local/lib' \
        --add-module=/root/lua-nginx-module-0.9.15 \
```

配置lua库加载路径：
```
echo "/usr/local/lib" > /etc/ld.so.conf.d/usr_local_lib.conf
lddconf
```

#rpm安装nginx可能需要添加--nodeps
```
rpm -Uvh /root/rpmbuild/RPMS/x86_64/nginx-1.6.2-1.el6.ngx.x86_64.rpm --nodeps
```

测试：
```
location /lua {
    set $test "hello, world.";
    content_by_lua '
        ngx.header.content_type = "text/plain";
        ngx.say(ngx.var.test);
    ';
}
curl http://127.0.0.1/lua
```

UUID：
```
io.open('/proc/sys/kernel/random/uuid','r'):read(36)
```
