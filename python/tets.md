#### Django框架

```
安装

     pip  install django
     

Sync数据库表

    python manage.py makemigrations
    
    python manage.py migrate
    
    



部署Nginx + uwsgi

   pip install uwsgi

   vim start.ini
   
    [uwsgi]
    chdir= your_project_path
    module=ops_system.wsgi:application
    pidfile=/tmp/project.pid
    vacuum=True
    max-requests=10000
    daemonize=/tmp/project.log
    socket=127.0.0.1:9999
    master=True
    
    
    vim  nginx.conf
    
     location / {
          uwsgi_pass 127.0.0.1:9999;
          include uwsgi_params;
        }



```



