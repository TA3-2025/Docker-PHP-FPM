FROM rockylinux:9


RUN dnf install php php-fpm -y && \
    dnf clean all && \
    rm -rf /var/cache/dnf


RUN cat <<EOF > /etc/php-fpm.conf
include=/etc/php-fpm.d/*.conf
[global]
pid =  0
error_log = /proc/self/fd/2
log_level = notice
log_buffering = no
daemonize = no
EOF

RUN cat <<EOF > /etc/php-fpm.d/www.conf
[www]
access.log = /proc/self/fd/2
catch_workers_output = yes
user = apache
group = apache
listen = 0.0.0.0:9000
listen.acl_users = apache,nginx
pm = dynamic
pm.max_children = 50
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 35
php_admin_value[error_log] = /var/log/php-fpm/www-error.log
php_admin_flag[log_errors] = on
php_value[session.save_handler] = files
php_value[session.save_path]    = /var/lib/php/session
EOF

#COPY .docker/PHP/php-fpm.conf /etc/php-fpm.conf
#COPY .docker/PHP/www.conf /etc/php-fpm.d/www.conf


CMD php-fpm --nodaemonize