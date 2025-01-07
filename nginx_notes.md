# Nginx:

- Nginx is an open source web server that is often used as reverse proxy or HTTP cache. Reverse proxy server is typically a server that will accept incomming requesting traffic and send that to appropriate backend server and sending response on the behalf of those backend servers. It can also used as a load balancer which means the nginx server will balance the load of incomming requst traffic between multiple servers.

- use `ps -ax | grep nginx` to get all running nginx processes.

- Nginx consists of modules which are controlled by directives.

- There are 2 types of directives(space saperated values of name and parameters):
    - simple directives (ends with ;)
    - block directives (ends with {})
        - if block directive can have other directives inside {} then its called context.

- Directives placed inside the configuration file outside of any context are considered to be in `main` context.

    ---

    

    ### Tips:

- use `ctrl + shift + r` to hard reload, it will remove cache.

- read about the user `www-data` from [here](https://askubuntu.com/questions/873839/what-is-the-www-data-user).

- When nginx serves static contenet it uses the user defined in `user` directive (in this case its www-data)  to have pre-defined permissions of files.

- By default `www-data` cannot access files from other user's home directory, to allow use for example`user mod -aG soham www-data`. now nginx will be able to serve all files that `soham` has access to.

- In order to serve from a perticular directory statically, the worker process of nginx needs the `read` permission. So when you `cd /home` and run `ls -l .` it will show that `other`s  do not have read permission in all the children of home. This means that `other` users cannot read anything from that directory even if inner children of that directories are readable, its like if a node as no permission to read then subtree of that node is not readable as whole.

- In `/var` however, `other` users can read.

- read additional information from: [The Architecture of Open Source Applications](The Architecture of Open Source Applications).
