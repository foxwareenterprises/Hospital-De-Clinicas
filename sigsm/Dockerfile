FROM openeuler/openeuler:24.03-lts-sp3

ENV APACHE_DOCUMENT_ROOT=/var/www/html \
    APP_FORCE_HTTPS=false \
    APP_ENV=development

RUN set -eux; \
    dnf -y update && dnf -y install \
        httpd \
        mod_ssl \
        php \
        php-cli \
        php-common \
        php-gd \
        php-pdo \
        php-mysqlnd \
        php-sqlite3 \
        mariadb \
        openssl \
        cronie \
    && dnf clean all \
    && rm -rf /var/cache/dnf

RUN groupadd -g 33 www-data || true && \
    useradd -u 33 -g www-data -M -s /sbin/nologin www-data || usermod -g www-data apache

RUN mkdir -p /etc/httpd/conf.d /etc/httpd/ssl

RUN sed -ri "s!/var/www/html!${APACHE_DOCUMENT_ROOT}!g" /etc/httpd/conf/httpd.conf \
    && sed -i 's/AllowOverride None/AllowOverride All/g' /etc/httpd/conf/httpd.conf \
    && sed -i 's/Listen 80/Listen 80\nListen 443/' /etc/httpd/conf/httpd.conf

RUN printf '%s\n' \
    '<VirtualHost *:80>' \
    '    ServerName _default_' \
    '    DocumentRoot /var/www/html' \
    '    Alias /sigsm /var/www/html/sigsm' \
    '    <Directory /var/www/html>' \
    '        Options FollowSymLinks' \
    '        AllowOverride All' \
    '        Require all granted' \
    '    </Directory>' \
    '    <Directory /var/www/html/sigsm>' \
    '        Options FollowSymLinks' \
    '        AllowOverride All' \
    '        Require all granted' \
    '    </Directory>' \
    '</VirtualHost>' > /etc/httpd/conf.d/000-default.conf

RUN printf '%s\n' \
    '<VirtualHost *:443>' \
    '    ServerName _default_' \
    '    DocumentRoot /var/www/html' \
    '    Alias /sigsm /var/www/html/sigsm' \
    '    SSLEngine on' \
    '    SSLCertificateFile /etc/httpd/ssl/server.crt' \
    '    SSLCertificateKeyFile /etc/httpd/ssl/server.key' \
    '    RedirectMatch 302 ^/$ /sigsm/' \
    '    <Directory /var/www/html>' \
    '        Options FollowSymLinks' \
    '        AllowOverride All' \
    '        Require all granted' \
    '    </Directory>' \
    '    <Directory /var/www/html/sigsm>' \
    '        Options FollowSymLinks' \
    '        AllowOverride All' \
    '        Require all granted' \
    '    </Directory>' \
    '</VirtualHost>' > /etc/httpd/conf.d/default-ssl.conf

COPY config/php-custom.ini /etc/php.d/custom.ini
COPY config/apache-custom.conf /etc/httpd/conf.d/servername.conf

WORKDIR /var/www/html
COPY . /var/www/html/sigsm

RUN printf '%s\n' '<?php' 'header("Location: /sigsm/");' 'exit;' '?>' > /var/www/html/index.php \
    && mkdir -p /var/www/html/sigsm/uploads \
    /var/www/html/sigsm/temp_qr \
    /var/www/html/sigsm/config/reset_codes \
    && chown -R apache:apache /var/www/html \
    && chmod -R 775 /var/www/html/sigsm/uploads \
    /var/www/html/sigsm/temp_qr \
    /var/www/html/sigsm/config \
    && chmod 755 /var/www/html \
    /var/www/html/index.php \
    /var/www/html/sigsm

COPY entrypoint.sh /usr/local/bin/entrypoint.sh
COPY backup_task.sh /usr/local/bin/sigsm-backup.sh
RUN chmod +x /usr/local/bin/entrypoint.sh /usr/local/bin/sigsm-backup.sh \
    && (crontab -l 2>/dev/null; echo "0 2 * * * /usr/local/bin/sigsm-backup.sh >> /var/log/sigsm_backups.log 2>&1") | crontab -

EXPOSE 80 443
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
