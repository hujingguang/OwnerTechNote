#### SaltStack

开源自动化运维工具,使用Python开发编写,底层使用zeromq通信,架构模式为Master--Minion

centos下安装: yum install salt salt-master salt-minion -y

---

#### SLS文件语法

1. Logstash客户端安装sls编写范列

```
{%set logstash_dir= '/usr/local/logstash' %}

mkdir_logstash:
  cmd.run:
    - name: mkdir -p {{logstash_dir}}
    - unless: test -e {{logstash_dir}}

install_jdk:
  file.managed:
    - name: /tmp/jdk-8u5-linux-x64.tar.gz
    - source: salt://logstash/files/jdk-8u5-linux-x64.tar.gz
    - unless: test -e /tmp/jdk-8u5-linux-x64.tar.gz
  cmd.run:
    - name: cd /tmp/ && tar zxf /tmp/jdk-8u5-linux-x64.tar.gz && mv jdk1.8.0_05 {{logstash_dir}}/jdk1.8
    - unless: test -e {{logstash_dir}}/jdk1.8/bin

install_logstash:
  file.managed:
    - source: salt://logstash/files/logstash-6.2.2.zip
    - name: /tmp/logstash-6.2.2.zip
    - unless: test -e /tmp/logstash-6.2.2.zip
  cmd.run: 
    - name: cd /tmp && unzip logstash-6.2.2.zip &>/dev/null && mv logstash-6.2.2/*  {{logstash_dir}}/
    - unless: test -e {{logstash_dir}}/bin

upload_conf:
  file.managed:
    - name: {{logstash_dir}}/log.conf
    - template: jinja
    - source: salt://logstash/files/log.conf
    - unless: test -e {{logstash_dir}}/log.conf

add_regex:
  file.append:
    - name: {{logstash_dir}}/vendor/bundle/jruby/2.3.0/gems/logstash-patterns-core-4.1.2/patterns/grok-patterns
    - text:
      - MYDATE 20%{DATE} %{TIME}
      - SECURE_DATE %{MONTH} %{MONTHDAY} %{HOUR}:%{MINUTE}:%{SECOND}
    - unless: grep MYDATE {{logstash_dir}}/vendor/bundle/jruby/2.3.0/gems/logstash-patterns-core-4.1.2/patterns/grok-patterns &>/dev/null


/etc/init.d/logstash:
  file.managed:
    - source: salt://logstash/files/logstash.sh
    - template: jinja
    - mode: 775
    - unless: test -e /etc/init.d/logstash
    - defaults:
        logstash_dir: {{logstash_dir}}

add_monit_logstash:
  file.managed:
    - source: salt://logstash/files/monit_logstash
    - name: /etc/monit.d/logstash.conf
    - unless: test -e /etc/monit.d/logstash.conf
```

  

