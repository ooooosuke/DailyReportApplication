# 0. ubuntu:22.04をベースにする
FROM ubuntu:22.04

# 環境変数を設定して、タイムゾーン選択を自動化
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Tokyo

# パッケージリストを更新し、必要なパッケージをインストールする
RUN apt-get update && \
    apt-get -y --fix-missing install vim language-pack-ja tzdata apache2 wget apt-transport-https gnupg sudo

# 日本語対応
RUN update-locale LANG=ja_JP.utf8
ENV LANG ja_JP.utf8

# 日本時間（JST）の設定
RUN echo 'Asia/Tokyo' > /etc/timezone
ENV TZ Asia/Tokyo

# Apache2の設定
RUN a2enmod rewrite ssl proxy proxy_ajp
RUN a2ensite default-ssl
RUN sed -i -e "s/^<\/VirtualHost>/RewriteEngine on/" /etc/apache2/sites-available/000-default.conf
RUN echo 'RewriteCond %{HTTPS} off' >> /etc/apache2/sites-available/000-default.conf
RUN echo 'RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L]' >> /etc/apache2/sites-available/000-default.conf
RUN echo '</VirtualHost>' >> /etc/apache2/sites-available/000-default.conf

# JDKのインストール
RUN wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | apt-key add -
RUN echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
RUN apt-get update && apt-get -y --fix-missing install temurin-17-jdk=17.0.5.0.0+8

# Javaアプリケーションの設定
RUN mkdir /var/www/java
COPY DailyReportSystemApplication-0.0.1-SNAPSHOT.jar /var/www/java/
RUN echo "ProxyPass / ajp://localhost:8009/" >> /etc/apache2/apache2.conf

# 一般ユーザmyuserの作成およびmyuserでログイン
RUN useradd -s /bin/bash -G sudo -m myuser
RUN echo "myuser:mypass" | chpasswd
RUN echo "root:rootpass" | chpasswd

# 起動時コマンドの指定
COPY startup.sh /startup.sh
RUN chmod 744 /startup.sh
CMD /startup.sh