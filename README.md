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
</pre>

* Run the playbook
<pre>
[root@apps-dev-2837710 ~]# ansible-playbook -i ./hosts  nginx-uwsgi.yml  --limit "helloworld2.com"
</pre>


## Host variables referrence
* uid: The username you want to use to run the application, ansible will create it if not exists  
* gid: The group you want to use to run the application, ansible will create it if not exists  
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
