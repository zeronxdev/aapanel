# aaPanel-6.8.37

### Hướng dãn sử dụng：

✨Cài đặt:
```
     bash <(curl -Ls https://raw.githubusercontent.com/zeronxdev/aapanel/main/install.sh)
```


✨Thêm cổng

            ufw allow 20/tcp
            ufw allow 21/tcp
            ufw allow 22/tcp
            ufw allow 888/tcp
            ufw allow 39000:40000/tcp

### Cài đặt Nginx 


Tệp tin nginx.sh đã được sửa đổi dựa trên tài liệu chính thức của aaPanel. Có giải thích chi tiết trong tài liệu. Mục đích chính là tối ưu hóa và tăng cường bảo mật mô-đun brotli,

Viết trên cơ sở debian 12, tương thích với ubuntu


✨Cách sử dụng nginx.sh：

     rm -f /www/server/panel/install/nginx.sh && wget -O /www/server/panel/install/nginx.sh https://raw.githubusercontent.com/zeronxdev/aapanel/main/nginx.sh -T 20 && bash /www/server/panel/install/nginx.sh install 1.24

Chú ý sửa lại số phiên bản ở cuối lệnh, nginx 1.24 được cài đặt mặc định

♦ Các phiên bản được hỗ trợ:

tengine='3.1.0'

nginx_108='1.8.1'

nginx_112='1.12.2'

nginx_114='1.14.2'

nginx_115='1.15.10'

nginx_116='1.16.1'

nginx_117='1.17.10'

nginx_118='1.18.0'

nginx_119='1.19.8'

nginx_120='1.20.2'

nginx_121='1.21.4'

nginx_122='1.22.1'

nginx_123='1.23.4'

nginx_124='1.24.0'

nginx_125='1.25.5' #Thử nghiệm

nginx_126='1.26.1' #Thử nghiệm

openresty='1.25.3.1'



<br><br><br>

### ModSecurity-nginx.sh

✨ModSecurity-nginx.sh bổ sung tường lửa ModSecurity (OWASP CRS) dựa trên nginx.sh

+ Sửa chữa các thư mục không xác định trong /www/server/nginx/src

+ Thêm file cấu hình cho một số quy tắc từ chối phổ biến cho wordpress, đường dẫn: /www/server/nginx/owasp/conf/nginx-wordpress.conf

+ Xóa ipv6 

+ Xóa mô-đun webdav tích hợp ${ENABLE_WEBDAV}

+ Thêm các thông số tối ưu hóa: --with-threads --with-file-aio  --with-cc-opt='-O2 -fPIE --param=ssp-buffer-size=4 -fstack-protector -Wformat -Werror=format-security -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-E -Bsymbolic-functions -fPIE -pie -Wl,-z,relro -Wl,-z,now'

+ Thêm mô-đun ngx_brotli --add-module=/www/server/nginx/src/ngx_brotli

+ Thêm mô-đun tĩnh ModSecurity-nginx --add-module=/www/server/nginx/owasp/ModSecurity-nginx. Nếu bạn cần biên dịch nó thành mô-đun động, hãy sửa đổi nó thành --add-dynamic-module=/www/server/nginx/owasp/ModSecurity-nginx mô-đun động cần nhập tệp .so theo tài liệu chính thức. Vui lòng đọc hướng dẫn sau để biết chi tiết.


Lưu ý: ModSecurity-nginx.sh không cài đặt phần phụ thuộc tương ứng trên các hệ thống không phải hệ thống ubuntu/debian.

✨ Cách sử dụng ModSecurity-nginx.sh: 

     rm -f /www/server/panel/install/nginx.sh && wget -O  /www/server/panel/install/nginx.sh https://raw.githubusercontent.com/zeronxdev/aapanel/main/ModSecurity-nginx.sh -T 20 && bash /www/server/panel/install/nginx.sh install 1.24


Đường dẫn ModSecurity：/www/server/nginx/owasp/ModSecurity

Mã nguồn：https://github.com/SpiderLabs/ModSecurity

ModSecurity-nginx这个是nginx连接器

Đường dẫn：/www/server/nginx/owasp/ModSecurity-nginx

Mã nguồn：https://github.com/SpiderLabs/ModSecurity-nginx

Phiên bản mới nhất của tệp quy tắc OWASP CRS được tải xuống theo mặc định

Lưu trữ tệp trong /www/server/nginx/owasp/owasp-rules

Mã nguồn：https://github.com/coreruleset/coreruleset/releases

####使用说明####

【【【默认会编译成静态模块下面这句无需添加，只有编译成动态模块时候才会需要引入.so文件】】】

根据<a href="https://www.netnea.com/cms/nginx-tutorial-6_embedding-modsecurity/"  target="_blank">官方文档</a>步骤五在nginx.conf文件添加引入。将以下代码添加在worker_rlimit_nofile 51200;下面即可引入

     load_module /www/server/nginx/modules/ngx_http_modsecurity_module.so;

根据<a href="https://www.netnea.com/cms/nginx-tutorial-6_embedding-modsecurity/"  target="_blank">官方文档</a>骤5建议在http模块内添加以下代码全局开启

     modsecurity on;


编辑规则全局引入文件。这里面可以引入你需要的规则
文件路径： /www/server/nginx/owasp/conf/main.conf

在你的网站配置文件内添加以下代码

     modsecurity on;
     modsecurity_rules_file /www/server/nginx/owasp/conf/main.conf;

然后编辑/www/server/nginx/owasp/conf/main.conf文件在里面引入你需要的规则文件即可

所有国则文件都在/www/server/nginx/owasp/owasp-rules/rules里面

/www/server/nginx/owasp/conf/nginx-wordpress.conf文件是针对wordpress程序的一些常用拒绝规则，需要在网站的nginx配置文件里面引入


注意修改命令尾部的版本号，默认安装 nginx 1.24

支持版本：

tengine='3.1.0'

nginx_108='1.8.1'

nginx_112='1.12.2'

nginx_114='1.14.2'

nginx_115='1.15.10'

nginx_116='1.16.1'

nginx_117='1.17.10'

nginx_118='1.18.0'

nginx_119='1.19.8'

nginx_120='1.20.2'

nginx_121='1.21.4'

nginx_122='1.22.1'

nginx_123='1.23.4'

nginx_124='1.24.0'

nginx_125='1.25.5' # 未测试是否可用

nginx_126='1.26.1' # 未测试是否可用

openresty='1.25.3.1'

# 关于降级
不建议高版本降级低版本，如果你真想降级，备份/www目录然后删除他重新安装即可

# 关于错误
这个脚本测试过很多次，保证没有错误，如果安装过程中出现错误或者后台出现错误大部分原因是因为宝塔服务器出现了问题，请过20分钟到一小时后重新安装即可
