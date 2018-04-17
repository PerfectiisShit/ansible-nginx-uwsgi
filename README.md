# ansible-nginx-uwsgi
Automation for nginx and uWSGI applications setup. Based on my pyweb tutorial and Ansible. This project suspect you have already setup uWSGI emperor with emperor-tyrant mode enabled

# How to deploy my application
* Get the latest code
<pre>
[root@apps-dev-2837710 ~]# git clone https://github.com/PerfectiisShit/ansible-nginx-uwsgi.git
[root@apps-dev-2837710 ~]# cd ansible-nginx-uwsgi
</pre>

* Edit the host file as below
<pre>
[root@apps-dev-2837710 ~]# cat hosts
helloworld2.com ansible_connection=local
</pre>

* Edit the host_vars file according to your application structure
<pre>
[root@apps-dev-2837710 ~]# cd host_vars
[root@apps-dev-2837710 ~]# cp example helloworld2.com
[root@apps-dev-2837710 ~]# vim helloworld2.com
---
uid: test3
gid: test3
packages:
  - atop
app_name: helloworld2
app_package: flaskapp
app_callable: application
base_dir: '{{ websites_dir }}/{{ app_name }}'
appdata_dir: '{{ base_dir }}'
nginx_server_name: '{{ app_name }}.com'
nginx_http_port: 80
uwsgi_module: '{{ app_package }}:{{ app_callable }}'
uwsgi_plugins: python
uwsgi_processes: 1
uwsgi_threads: 1
uwsgi_env: []
# Example for Django:
# uwsgi_env:
# - 'DJANGO_SETTINGS_MODULE={{ app_package }}.settings'
git_repo: https://github.com/Kwpolska/flask-demo-app
git_branch: master

</pre>

* Run the playbook
<pre>
[root@apps-dev-2837710 ~]# ansible-playbook -i ./hosts  nginx-uwsgi.yml  --limit "helloworld2.com"
</pre>


## Host variables referrence
* uid: The username you want to use to run the application, ansible will create it if not exists  
* gid: The group you want to use to run the application, ansible will create it if not exists  
* packages: The os packages your application requires
* app_name: Usually the top level of your domain name, for instance, helloworld2 of helloworld.com  
* app_package: Your application file name  
* app_callable: The app object in your application file  
* base_dir: Your websites directory  
* appdata_dir: The directory to store your app soure code  
* nginx_server_name: Whatever domain you want to use  
* nginx_http_port: Nginx http port  
* uwsgi_module: Usually '{{ app_package }}:{{ app_callable }}'  
* uwsgi_plugins: uWSGI plugins your application requires  
* uwsgi_processes: The number of workes uWSGI will create  
* uwsgi_threads: The thread your application will generate  
* uwsgi_env: Your custom uWSGI environment   
* git_repo: The git repository which stores your application source code  
* git_branch: The branch of the git repository your want to deploy  

## uWSGI emperor configuration file
[root@apps-dev-2837710 ansible-nginx-uwsgi]# cat /etc/uwsgi.ini
[uwsgi]
uid = uwsgi
gid = uwsgi
safe-pidfile = /run/uwsgi/emperor.pid
emperor = /etc/uwsgi.d
logto = /var/log/uwsgi/emperor.log
touch-reload = true
emperor-tyrant = true
emperor-stats = /run/uwsgi/emperor-stats.sock
cap = setgid,setuid


