# Добавление модулей nginx в Linux (Debian/Ubuntu/CentOS)

Выводит информацию по версии NGINX и конфигурационных параметрах. 

```
nginx -V
```

Установка nginx с той же версиeй, которая стоит на данный момент.

```
wget http://nginx.org/download/nginx-1.18.0.tar.gz
```

Распаковка архива.

```
tar –xvf nginx-1.12.0.tar.gz
```

Установка дополнительных пакетов архива.

```
apt install build-essential
apt install libpcre++-dev
apt install libssl-dev
apt install libgeoip-dev
apt install libxslt1-dev
```

После установки пакетов необходимо переписать конфигурацию nginx с добавлением модуля и выполнить его командой ./configure. 
Например:

```
./configure --prefix=/etc/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log
--http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi
--http-log-path=/var/log/nginx/access.log --http-proxy-temp-path=/var/lib/nginx/proxy --lock-path=/var/lock/nginx.lock
--pid-path=/var/run/nginx.pid --with-pcre-jit --with-http_gzip_static_module --with-http_ssl_module --with-ipv6
--without-http_browser_module --with-http_geoip_module --without-http_memcached_module --without-http_referer_module
--without-http_scgi_module --without-http_split_clients_module --with-http_stub_status_module --without-http_ssi_module
--without-http_userid_module --without-http_uwsgi_module
**--with-http_mp4_module**
```

**В процессе конфигурирования возможно будут появляться ошибки в связи отсутствия каких-то модулей.**

Далее необходимо собрать бинарник nginx.


```
make
```
```
make install
```

По окончании сборки проверяем, что nginx собрался с нужным нам модулем:

```
/etc/nginx/sbin/nginx -V
```

Осталось заменить текущий бинарник nginx новым, который мы только что собрали.

Останавливаем nginx:

```
service nginx stop
```

Переименовываем (на всякий случай) текущий nginx в nginx_back:

```
mv /usr/sbin/nginx /usr/sbin/nginx_back
```

Перемещаем на его место новый собраный бинарник:

```
mv /etc/nginx/sbin/nginx /usr/sbin/nginx
```

Удаляем ненужную больше папку /etc/nginx/sbin:

```
rm -r -f /etc/nginx/sbin
```

Проверяем ещё раз, что nginx у нас теперь тот, что нужно:

```
nginx -V
```

Запускаем nginx:

```
service nginx start
```

Source: https://firstvds.ru/technology/dobavlenie-moduley-nginx-v-linux-debianubuntucentos

