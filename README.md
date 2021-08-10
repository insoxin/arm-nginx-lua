# arm-nginx-lua
```
文件存放位置是/www/server/panel/install

安装命令是bash nginx.sh install openresty

把lua的环境添加到系统目录中
        ln -s /www/server/nginx/lualib/resty/ /usr/local/lib
        ln -s /www/server/nginx/lualib /usr/local/lib/lua
        再安装防火墙即可
安装完后，修改防火墙的配置文件
        /www/server/panel/vhost/nginx/btwaf.conf
        把lua_package_path补全：lua_package_path "/www/server/nginx/lualib/resty/?.lua;;/www/server/nginx/lualib/ngx/?.lua;;/www/server/btwaf/?.lua";



新版宝塔绕过登录亲测可用 

sed -i "s|if (bind_user == 'True') {|if (bind_user == 'REMOVED') {|g" /www/server/panel/BTPanel/static/js/index.js

rm -rf /www/server/panel/data/bind.pl




1.打开目录/www/server/panel/class找到并编辑panelplugin.py文件

2.搜索并找到softList['list'] = tmpList这段代码，在其下方添加如下代码：
softList['pro'] = 1
        for soft in softList['list']:
            soft['endtime'] = 0
3.重启面板


修改完成后重启面板，重启完成后就可以直接安装收费的插件了，Nginx防火墙也可以直接安装使用

网站监控报表

如果需要使用网站监控报表还需另外修改一次代码：

安装好网站监控报表插件后打开/www/server/panel/plugin/total目录并编辑total_main.py文件

使用Ctrl+F搜索并找到if 'bt_total' in session: return public.returnMsg(True,'OK!');这段代码

在这段代码前加上#将其注释掉，并在其下方加入以下代码：

        session['bt_total'] = True

        return public.returnMsg(True,'OK!');

示例：

然后再次重启面板，即可使用网站监控报表插件了；
