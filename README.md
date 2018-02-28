# Supervisor Installation and Maintenance Guide
## Installation
1. Install **_python-setuptools_**
```shell
$ sudo yum install python-setuptools python-setuptools-devel
```

2. Install **_superpvisor_**
```shell
$ sudo easy_install supervisor
```

3. Configure **_superpvisord_**
```shell
$ sudo vi /etc/supervisord.conf
```
Add these lines at the bottom of the file
```
[program:laravel-worker]
;command=php /var/www/ku_uni_cms/artisan queue:listen database --sleep=3 --tries=3
command=php /var/www/ku_uni_cms/artisan queue:listen database --timeout=600 --sleep=60 --tries=10
process_name=%(program_name)s_%(process_num)02d
autostart=true
autorestart=true
user=centos
numprocs=32
stderr_logfile=/var/log/laraqueue.err.log
stdout_logfile=/var/log/laraqueue.out.log
```

4. Restart **_superpvisord_** to start queue workers
```shell
$ systemctl start supervisord
$ supervisorctl reread ; supervisorctl update ; supervisorctl restart laravel-worker:*
```

5. Start **_superpvisord_** when CentOS starts
```shell
systemctl enable supervisord
```

6. If you want to read worker outputlogs, run this command
```shell
tail -f /var/log/laraqueue.out.log
```

7. If you want to read worker error logs, run this command
```shell
tail -f /var/log/laraqueue.err.log
```
