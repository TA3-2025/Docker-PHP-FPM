FROM rockylinux:9


RUN dnf install httpd -y && \
    dnf clean all && \
    rm -rf /var/cache/dnf

RUN echo "" > /etc/httpd/conf.d/welcome.conf && \
    echo "" > /etc/httpd/conf.d/userdir.conf && \
    echo "" > /etc/httpd/conf.d/autoindex.conf

RUN cat <<EOF >> /etc/httpd/conf.d/default.conf
DirectoryIndex index.php index.html
ErrorLogFormat "[HTTPD_ERROR_LOG] [%{u}t] [%-m:%l] [pid %P:tid %T] %7F: %E: [client\ %a] %M% ,\ referer\ %{Referer}i"
ErrorLog /dev/stderr

LogFormat "[HTTPD_ACCESS_LOG] %h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
CustomLog /dev/stdout combined

<FilesMatch \.php$>
    SetHandler "proxy:fcgi://php-fpm:9000"
</FilesMatch>
EOF

#COPY .docker/Apache/Apache.conf /etc/httpd/conf.d/default.conf


CMD httpd -D FOREGROUND