# Laradock



```shell
$ docker-compose up -d nginx mysql
```



```shell
$ docker-compose up -d nginx mysql
Creating network "laradock_backend" with driver "bridge"
Creating network "laradock_frontend" with driver "bridge"
Creating network "laradock_default" with the default driver
Creating volume "laradock_mysql" with local driver
Creating volume "laradock_percona" with local driver
Creating volume "laradock_mssql" with local driver
Creating volume "laradock_postgres" with local driver
Creating volume "laradock_memcached" with local driver
Creating volume "laradock_redis" with local driver
Creating volume "laradock_neo4j" with local driver
Creating volume "laradock_mariadb" with local driver
Creating volume "laradock_mongo" with local driver
Creating volume "laradock_minio" with local driver
Creating volume "laradock_rethinkdb" with local driver
Creating volume "laradock_phpmyadmin" with local driver
Creating volume "laradock_adminer" with local driver
Creating volume "laradock_aerospike" with local driver
Creating volume "laradock_caddy" with local driver
Creating volume "laradock_elasticsearch-data" with local driver
Creating volume "laradock_elasticsearch-plugins" with local driver
Building mysql
Step 1/10 : ARG MYSQL_VERSION=latest
Step 2/10 : FROM mysql:${MYSQL_VERSION}
latest: Pulling from library/mysql
f2aa67a397c4: Pull complete
1accf44cb7e0: Pull complete
2d830ea9fa68: Pull complete
740584693b89: Pull complete
4d620357ec48: Pull complete
ac3b7158d73d: Pull complete
a48d784ee503: Pull complete
f122eadb2640: Pull complete
3df40c552a96: Pull complete
da7d77a8ed28: Pull complete
f03c5af3b206: Pull complete
54dd1949fa0f: Pull complete
Digest: sha256:d60c13a2bfdbbeb9cf1c84fd3cb0a1577b2bbaeec11e44bf345f4da90586e9e1
Status: Downloaded newer image for mysql:latest
 ---> a8a59477268d
Step 3/10 : LABEL maintainer="Mahmoud Zalt <mahmoud@zalt.me>"
 ---> Running in 7db9833bbb51
Removing intermediate container 7db9833bbb51
 ---> efce7d597f07
Step 4/10 : ARG TZ=UTC
 ---> Running in b11e3046a670
Removing intermediate container b11e3046a670
 ---> a1804787f39a
Step 5/10 : ENV TZ ${TZ}
 ---> Running in f995e0f9a64e
Removing intermediate container f995e0f9a64e
 ---> a3decd4e7aa0
Step 6/10 : RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
 ---> Running in f569ff40fd17
Removing intermediate container f569ff40fd17
 ---> 935441fd0939
Step 7/10 : RUN chown -R mysql:root /var/lib/mysql/
 ---> Running in 637ccde8909f
Removing intermediate container 637ccde8909f
 ---> 1e536067bd41
Step 8/10 : ADD my.cnf /etc/mysql/conf.d/my.cnf
 ---> f35e061de44d
Step 9/10 : CMD ["mysqld"]
 ---> Running in e88680c169dd
Removing intermediate container e88680c169dd
 ---> e2f268098601
Step 10/10 : EXPOSE 3306
 ---> Running in b46be24e0bec
Removing intermediate container b46be24e0bec
 ---> 61a7a3f27ca2
Successfully built 61a7a3f27ca2
Successfully tagged laradock_mysql:latest
WARNING: Image for service mysql was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Pulling applications (tianon/true:)...
latest: Pulling from tianon/true
7318119b0d21: Pull complete
Digest: sha256:1db73f3610a525e10f80c62326bfbab538d9eda3c893d3cdd4b91f65ed711998
Status: Downloaded newer image for tianon/true:latest
Building workspace
Step 1/169 : FROM laradock/workspace:2.0-72
2.0-72: Pulling from laradock/workspace
22ecafbbcc4a: Pull complete
580435e0a086: Pull complete
8321ffd10031: Pull complete
08b8f28a13c2: Pull complete
2b401702069a: Pull complete
a3ed95caeb02: Pull complete
eae027dcdc0e: Pull complete
93bc98227159: Pull complete
859e173c8271: Pull complete
43d810e6a73b: Pull complete
fcd7d46b9b66: Pull complete
9eaeb74b39a4: Pull complete
Digest: sha256:f14e611f559b94e5a6c77142e3b177c75e2d65f971deb51f993bb02d735efeb9
Status: Downloaded newer image for laradock/workspace:2.0-72
 ---> 69a57f6aca78
Step 2/169 : LABEL maintainer="Mahmoud Zalt <mahmoud@zalt.me>"
 ---> Running in 30be9831e053
Removing intermediate container 30be9831e053
 ---> 09b5c9dc1a8f
Step 3/169 : RUN rm /var/log/lastlog /var/log/faillog
 ---> Running in a1da85bb38ff
Removing intermediate container a1da85bb38ff
 ---> e296ca7bd7be
Step 4/169 : ARG PUID=1000
 ---> Running in 42485874e895
Removing intermediate container 42485874e895
 ---> 6c385321561c
Step 5/169 : ARG PGID=1000
 ---> Running in 1d96d3d6692d
Removing intermediate container 1d96d3d6692d
 ---> a08c50ee0325
Step 6/169 : ENV PUID ${PUID}
 ---> Running in cc167f5224dc
Removing intermediate container cc167f5224dc
 ---> 3393debe20cb
Step 7/169 : ENV PGID ${PGID}
 ---> Running in 4f967561805b
Removing intermediate container 4f967561805b
 ---> 70401f44f104
Step 8/169 : RUN groupadd -g ${PGID} laradock &&     useradd -u ${PUID} -g laradock -m laradock
 ---> Running in d26f5b7907b0
Removing intermediate container d26f5b7907b0
 ---> b6b4cb7b10b8
Step 9/169 : USER root
 ---> Running in 413037ddf34b
Removing intermediate container 413037ddf34b
 ---> 51b7f71df700
Step 10/169 : ARG INSTALL_SOAP=false
 ---> Running in 95ba879aa415
Removing intermediate container 95ba879aa415
 ---> bb24a92d4749
Step 11/169 : ENV INSTALL_SOAP ${INSTALL_SOAP}
 ---> Running in f938f172de12
Removing intermediate container f938f172de12
 ---> 63c9c153324f
Step 12/169 : RUN if [ ${INSTALL_SOAP} = true ]; then   add-apt-repository -y ppa:ondrej/php &&   apt-get update -yqq &&   apt-get -y install libxml2-dev php7.2-soap ;fi
 ---> Running in 902d64702ad8
Removing intermediate container 902d64702ad8
 ---> 9601051e3641
Step 13/169 : ARG INSTALL_LDAP=false
 ---> Running in 901effda1056
Removing intermediate container 901effda1056
 ---> 9b3776c5d704
Step 14/169 : ENV INSTALL_LDAP ${INSTALL_LDAP}
 ---> Running in 7ee1d8069e4d
Removing intermediate container 7ee1d8069e4d
 ---> 6167f3f7c09f
Step 15/169 : RUN if [ ${INSTALL_LDAP} = true ]; then     apt-get update -yqq &&     apt-get install -y libldap2-dev &&     apt-get install -y php7.2-ldap ;fi
 ---> Running in 177e34d1d74e
Removing intermediate container 177e34d1d74e
 ---> 131dc88ffe81
Step 16/169 : ARG INSTALL_IMAP=false
 ---> Running in 42c75395b458
Removing intermediate container 42c75395b458
 ---> 4d0d94226efe
Step 17/169 : ENV INSTALL_IMAP ${INSTALL_IMAP}
 ---> Running in 618c5d16990c
Removing intermediate container 618c5d16990c
 ---> 41f6609362b1
Step 18/169 : RUN if [ ${INSTALL_IMAP} = true ]; then     apt-get update -yqq &&     apt-get install -y php7.2-imap ;fi
 ---> Running in 151936dc067b
Removing intermediate container 151936dc067b
 ---> f2358b80dc79
Step 19/169 : ARG TZ=UTC
 ---> Running in 12679c4bb7ee
Removing intermediate container 12679c4bb7ee
 ---> a1a9e11ba05a
Step 20/169 : ENV TZ ${TZ}
 ---> Running in dd92301d38b6
Removing intermediate container dd92301d38b6
 ---> 79f554f00013
Step 21/169 : RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
 ---> Running in 9f8097311cac
Removing intermediate container 9f8097311cac
 ---> dad312a6f59f
Step 22/169 : COPY ./composer.json /home/laradock/.composer/composer.json
 ---> 32ad13df55da
Step 23/169 : RUN chown -R laradock:laradock /home/laradock/.composer
 ---> Running in 0609a96192fa
Removing intermediate container 0609a96192fa
 ---> 31592aafdc55
Step 24/169 : USER laradock
 ---> Running in e84f0f1d9abd
Removing intermediate container e84f0f1d9abd
 ---> f79764fe63c6
Step 25/169 : ARG COMPOSER_GLOBAL_INSTALL=false
 ---> Running in 3c931eae9a97
Removing intermediate container 3c931eae9a97
 ---> 8937f5db0f0a
Step 26/169 : ENV COMPOSER_GLOBAL_INSTALL ${COMPOSER_GLOBAL_INSTALL}
 ---> Running in 3d06a01fded0
Removing intermediate container 3d06a01fded0
 ---> 94619f1e46cd
Step 27/169 : RUN if [ ${COMPOSER_GLOBAL_INSTALL} = true ]; then     composer global install ;fi
 ---> Running in 613f496c4c5b
Removing intermediate container 613f496c4c5b
 ---> a9ed8c12a790
Step 28/169 : ARG COMPOSER_REPO_PACKAGIST
 ---> Running in 5832547fb078
Removing intermediate container 5832547fb078
 ---> c5ce3e9d326f
Step 29/169 : ENV COMPOSER_REPO_PACKAGIST ${COMPOSER_REPO_PACKAGIST}
 ---> Running in f3575f702c87
Removing intermediate container f3575f702c87
 ---> 97d53008414c
Step 30/169 : RUN if [ ${COMPOSER_REPO_PACKAGIST} ]; then     composer config -g repo.packagist composer ${COMPOSER_REPO_PACKAGIST} ;fi
 ---> Running in d702802f8d25
Removing intermediate container d702802f8d25
 ---> 25e9fa869799
Step 31/169 : USER root
 ---> Running in b05f1fec68c6
Removing intermediate container b05f1fec68c6
 ---> 0bf8e3d7f4f1
Step 32/169 : COPY ./crontab /etc/cron.d
 ---> e0d9f1ffd1fa
Step 33/169 : RUN chmod -R 644 /etc/cron.d
 ---> Running in 94ee17e56f45
Removing intermediate container 94ee17e56f45
 ---> f5e148671bfa
Step 34/169 : USER root
 ---> Running in 1402ee6f3dbc
Removing intermediate container 1402ee6f3dbc
 ---> a84f44f03a25
Step 35/169 : COPY ./aliases.sh /home/laradock/aliases.sh
 ---> e008a43f6c62
Step 36/169 : RUN echo "" >> ~/.bashrc &&     echo "# Load Custom Aliases" >> ~/.bashrc &&     echo "source /home/laradock/aliases.sh" >> ~/.bashrc && 	echo "" >> ~/.bashrc && 	sed -i 's/\r//' /home/laradock/aliases.sh && 	sed -i 's/^#! \/bin\/sh/#! \/bin\/bash/' /home/laradock/aliases.sh &&     chown laradock:laradock /home/laradock/aliases.sh
 ---> Running in 04ca2d0c5c26
Removing intermediate container 04ca2d0c5c26
 ---> fd77690c5c27
Step 37/169 : USER root
 ---> Running in 2cb4b9897492
Removing intermediate container 2cb4b9897492
 ---> bad37ba77bae
Step 38/169 : ARG INSTALL_XDEBUG=false
 ---> Running in a47de6a8eec8
Removing intermediate container a47de6a8eec8
 ---> 8d908b84c162
Step 39/169 : RUN if [ ${INSTALL_XDEBUG} = true ]; then     apt-get update &&     apt-get install -y --force-yes php7.2-xdebug &&     sed -i 's/^;//g' /etc/php/7.2/cli/conf.d/20-xdebug.ini &&     echo "alias phpunit='php -dzend_extension=xdebug.so /var/www/vendor/bin/phpunit'" >> ~/.bashrc ;fi
 ---> Running in e465d7a9a948
Removing intermediate container e465d7a9a948
 ---> a059d1d6b534
Step 40/169 : COPY ./xdebug.ini /etc/php/7.2/cli/conf.d/xdebug.ini
 ---> 7b87cac0470d
Step 41/169 : ARG INSTALL_BLACKFIRE=false
 ---> Running in 95e64f673f3a
Removing intermediate container 95e64f673f3a
 ---> 79eacd3fea79
Step 42/169 : ARG BLACKFIRE_CLIENT_ID
 ---> Running in cb0ed3ebb60a
Removing intermediate container cb0ed3ebb60a
 ---> a3b3e6cb7480
Step 43/169 : ARG BLACKFIRE_CLIENT_TOKEN
 ---> Running in 73fed87594f2
Removing intermediate container 73fed87594f2
 ---> af8f6e1484bc
Step 44/169 : ENV BLACKFIRE_CLIENT_ID ${BLACKFIRE_CLIENT_ID}
 ---> Running in 4b2baca06d5c
Removing intermediate container 4b2baca06d5c
 ---> 5412d6a537e9
Step 45/169 : ENV BLACKFIRE_CLIENT_TOKEN ${BLACKFIRE_CLIENT_TOKEN}
 ---> Running in fc87feeea197
Removing intermediate container fc87feeea197
 ---> ed2fb0fbd42a
Step 46/169 : RUN if [ ${INSTALL_XDEBUG} = false -a ${INSTALL_BLACKFIRE} = true ]; then     curl -L https://packagecloud.io/gpg.key | apt-key add - &&     echo "deb http://packages.blackfire.io/debian any main" | tee /etc/apt/sources.list.d/blackfire.list &&     apt-get update -yqq &&     apt-get install blackfire-agent ;fi
 ---> Running in d95f13c056f2
Removing intermediate container d95f13c056f2
 ---> 54e7a683ff2a
Step 47/169 : ARG INSTALL_WORKSPACE_SSH=false
 ---> Running in a840bca7109e
Removing intermediate container a840bca7109e
 ---> b258e85e0e4e
Step 48/169 : ENV INSTALL_WORKSPACE_SSH ${INSTALL_WORKSPACE_SSH}
 ---> Running in eb05426b8a16
Removing intermediate container eb05426b8a16
 ---> b88e8499b145
Step 49/169 : ADD insecure_id_rsa /tmp/id_rsa
 ---> 325872ed13d4
Step 50/169 : ADD insecure_id_rsa.pub /tmp/id_rsa.pub
 ---> a463f8da1bc6
Step 51/169 : RUN if [ ${INSTALL_WORKSPACE_SSH} = true ]; then     rm -f /etc/service/sshd/down &&     cat /tmp/id_rsa.pub >> /root/.ssh/authorized_keys         && cat /tmp/id_rsa.pub >> /root/.ssh/id_rsa.pub         && cat /tmp/id_rsa >> /root/.ssh/id_rsa         && rm -f /tmp/id_rsa*         && chmod 644 /root/.ssh/authorized_keys /root/.ssh/id_rsa.pub     && chmod 400 /root/.ssh/id_rsa     && cp -rf /root/.ssh /home/laradock     && chown -R laradock:laradock /home/laradock/.ssh ;fi
 ---> Running in e6b81a2eb30f
Removing intermediate container e6b81a2eb30f
 ---> f0d5f2e365d8
Step 52/169 : ARG INSTALL_MONGO=false
 ---> Running in 7c228342f9dd
Removing intermediate container 7c228342f9dd
 ---> a22b31a9ba3f
Step 53/169 : ENV INSTALL_MONGO ${INSTALL_MONGO}
 ---> Running in 53c3c1363777
Removing intermediate container 53c3c1363777
 ---> 7d36fa0c252f
Step 54/169 : RUN if [ ${INSTALL_MONGO} = true ]; then     pecl -q install mongodb &&     echo "extension=mongodb.so" >> /etc/php/7.2/mods-available/mongodb.ini &&     ln -s /etc/php/7.2/mods-available/mongodb.ini /etc/php/7.2/cli/conf.d/30-mongodb.ini ;fi
 ---> Running in 5b58cec73f8c
Removing intermediate container 5b58cec73f8c
 ---> a7c9dd1c5954
Step 55/169 : ARG INSTALL_AMQP=false
 ---> Running in 8c75589e8cd5
Removing intermediate container 8c75589e8cd5
 ---> 8b1bde4caff4
Step 56/169 : ENV INSTALL_AMQP ${INSTALL_AMQP}
 ---> Running in 917e9b8665f6
Removing intermediate container 917e9b8665f6
 ---> f67185897f31
Step 57/169 : RUN if [ ${INSTALL_AMQP} = true ]; then     apt-get install librabbitmq-dev -y &&     pecl -q install amqp &&     echo "extension=amqp.so" >> /etc/php/7.2/mods-available/amqp.ini &&     ln -s /etc/php/7.2/mods-available/amqp.ini /etc/php/7.2/cli/conf.d/30-amqp.ini ;fi
 ---> Running in fd0faa517942
Removing intermediate container fd0faa517942
 ---> c9f82f30bcf5
Step 58/169 : ARG INSTALL_PHPREDIS=false
 ---> Running in 6d9d91890ca2
Removing intermediate container 6d9d91890ca2
 ---> fdc92308081f
Step 59/169 : ENV INSTALL_PHPREDIS ${INSTALL_PHPREDIS}
 ---> Running in a9ecd4299805
Removing intermediate container a9ecd4299805
 ---> cc2810248345
Step 60/169 : RUN if [ ${INSTALL_PHPREDIS} = true ]; then     printf "\n" | pecl -q install -o -f redis &&     echo "extension=redis.so" >> /etc/php/7.2/mods-available/redis.ini &&     phpenmod redis ;fi
 ---> Running in 69bf87ac5c79
Removing intermediate container 69bf87ac5c79
 ---> f9df034e4e21
Step 61/169 : ARG INSTALL_SWOOLE=false
 ---> Running in 848ddd6d8139
Removing intermediate container 848ddd6d8139
 ---> f2f0e9704bf4
Step 62/169 : RUN if [ ${INSTALL_SWOOLE} = true ]; then     pecl -q install swoole &&     echo "extension=swoole.so" >> /etc/php/7.2/mods-available/swoole.ini &&     ln -s /etc/php/7.2/mods-available/swoole.ini /etc/php/7.2/cli/conf.d/20-swoole.ini ;fi
 ---> Running in 57dc8a3a45bc
Removing intermediate container 57dc8a3a45bc
 ---> ccc21a5ab6e0
Step 63/169 : USER root
 ---> Running in bd79816feb9a
Removing intermediate container bd79816feb9a
 ---> 7f074e7e6421
Step 64/169 : ENV DRUSH_VERSION 8.1.2
 ---> Running in 57681fb96569
Removing intermediate container 57681fb96569
 ---> 9c07a36bca7b
Step 65/169 : ARG INSTALL_DRUSH=false
 ---> Running in a8d493e532dd
Removing intermediate container a8d493e532dd
 ---> a0ba14dc1c11
Step 66/169 : ENV INSTALL_DRUSH ${INSTALL_DRUSH}
 ---> Running in 31af309c932a
Removing intermediate container 31af309c932a
 ---> f6a30e86bca0
Step 67/169 : RUN if [ ${INSTALL_DRUSH} = true ]; then     apt-get update -yqq &&     apt-get -y install mysql-client &&     curl -fsSL -o /usr/local/bin/drush https://github.com/drush-ops/drush/releases/download/$DRUSH_VERSION/drush.phar | bash &&     chmod +x /usr/local/bin/drush &&     drush core-status ;fi
 ---> Running in 476620c74140
Removing intermediate container 476620c74140
 ---> 1fe50e58f3e8
Step 68/169 : USER root
 ---> Running in cdfa240b14eb
Removing intermediate container cdfa240b14eb
 ---> 3aee6f1b7578
Step 69/169 : ARG INSTALL_DRUPAL_CONSOLE=false
 ---> Running in ca779478e1c8
Removing intermediate container ca779478e1c8
 ---> 0c2a220f2492
Step 70/169 : ENV INSTALL_DRUPAL_CONSOLE ${INSTALL_DRUPAL_CONSOLE}
 ---> Running in 3d46a93bfe4d
Removing intermediate container 3d46a93bfe4d
 ---> 3966a75e588c
Step 71/169 : RUN if [ ${INSTALL_DRUPAL_CONSOLE} = true ]; then     apt-get update -yqq &&     apt-get -y install mysql-client &&     curl https://drupalconsole.com/installer -L -o drupal.phar &&     mv drupal.phar /usr/local/bin/drupal &&     chmod +x /usr/local/bin/drupal ;fi
 ---> Running in cf2f4c3a2239
Removing intermediate container cf2f4c3a2239
 ---> 086c57bb938e
Step 72/169 : USER laradock
 ---> Running in b6e0aa9c132a
Removing intermediate container b6e0aa9c132a
 ---> ac03ba1ee32d
Step 73/169 : ARG NODE_VERSION=stable
 ---> Running in ff8cfad1ae44
Removing intermediate container ff8cfad1ae44
 ---> cb9404379a58
Step 74/169 : ENV NODE_VERSION ${NODE_VERSION}
 ---> Running in bff093be8fdb
Removing intermediate container bff093be8fdb
 ---> 5183171fe380
Step 75/169 : ARG INSTALL_NODE=false
 ---> Running in 916943eeff5b
Removing intermediate container 916943eeff5b
 ---> e65ffeb9558d
Step 76/169 : ENV INSTALL_NODE ${INSTALL_NODE}
 ---> Running in c4fbbba0508d
Removing intermediate container c4fbbba0508d
 ---> c2327a5451cd
Step 77/169 : ENV NVM_DIR /home/laradock/.nvm
 ---> Running in e57acecea7c3
Removing intermediate container e57acecea7c3
 ---> f0168d750a23
Step 78/169 : ARG NPM_REGISTRY
 ---> Running in 2dffa844be0f
Removing intermediate container 2dffa844be0f
 ---> 041b89cb82a6
Step 79/169 : ENV NPM_REGISTRY ${NPM_REGISTRY}
 ---> Running in 11fefe69ae12
Removing intermediate container 11fefe69ae12
 ---> ee08a9a5c34d
Step 80/169 : RUN if [ ${INSTALL_NODE} = true ]; then     curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash &&         . $NVM_DIR/nvm.sh &&         nvm install ${NODE_VERSION} &&         nvm use ${NODE_VERSION} &&         nvm alias ${NODE_VERSION} &&         if [ ${NPM_REGISTRY} ]; then         npm config set registry ${NPM_REGISTRY}         ;fi &&         npm install -g gulp bower vue-cli ;fi
 ---> Running in e0e59e13cad2
Removing intermediate container e0e59e13cad2
 ---> cc3ed361c54f
Step 81/169 : RUN if [ ${INSTALL_NODE} = true ]; then     echo "" >> ~/.bashrc &&     echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc &&     echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc ;fi
 ---> Running in ef64d21af0fb
Removing intermediate container ef64d21af0fb
 ---> 45b66d13d215
Step 82/169 : USER root
 ---> Running in 59467b8f8bba
Removing intermediate container 59467b8f8bba
 ---> 9ad0e5dbdda4
Step 83/169 : RUN if [ ${INSTALL_NODE} = true ]; then     echo "" >> ~/.bashrc &&     echo 'export NVM_DIR="/home/laradock/.nvm"' >> ~/.bashrc &&     echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc ;fi
 ---> Running in 43843b62178e
Removing intermediate container 43843b62178e
 ---> e471f55440b7
Step 84/169 : ENV PATH $PATH:$NVM_DIR/versions/node/v${NODE_VERSION}/bin
 ---> Running in 3e6e167271d7
Removing intermediate container 3e6e167271d7
 ---> 1d8a0fe8bab0
Step 85/169 : RUN if [ ${NPM_REGISTRY} ]; then     . ~/.bashrc && npm config set registry ${NPM_REGISTRY} ;fi
 ---> Running in 5306611b5ca4
Removing intermediate container 5306611b5ca4
 ---> c473bf764948
Step 86/169 : USER laradock
 ---> Running in 0ef2717f2d6c
Removing intermediate container 0ef2717f2d6c
 ---> d7606710ea33
Step 87/169 : ARG INSTALL_YARN=false
 ---> Running in 214cff59a5d5
Removing intermediate container 214cff59a5d5
 ---> b1d9f2041c8a
Step 88/169 : ENV INSTALL_YARN ${INSTALL_YARN}
 ---> Running in a4150ec0aa1c
Removing intermediate container a4150ec0aa1c
 ---> d69e43eaca6b
Step 89/169 : ARG YARN_VERSION=latest
 ---> Running in 2502a748d092
Removing intermediate container 2502a748d092
 ---> ffd078a156da
Step 90/169 : ENV YARN_VERSION ${YARN_VERSION}
 ---> Running in 500b226d1cd5
Removing intermediate container 500b226d1cd5
 ---> 8f7659bb9ef1
Step 91/169 : RUN if [ ${INSTALL_YARN} = true ]; then     [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" &&     if [ ${YARN_VERSION} = "latest" ]; then         curl -o- -L https://yarnpkg.com/install.sh | bash;     else         curl -o- -L https://yarnpkg.com/install.sh | bash -s -- --version ${YARN_VERSION};     fi &&     echo "" >> ~/.bashrc &&     echo 'export PATH="$HOME/.yarn/bin:$PATH"' >> ~/.bashrc ;fi
 ---> Running in 95d2e2805aff
Removing intermediate container 95d2e2805aff
 ---> d1752b4288fb
Step 92/169 : USER root
 ---> Running in 5577042044a2
Removing intermediate container 5577042044a2
 ---> fd3d796fd663
Step 93/169 : RUN if [ ${INSTALL_YARN} = true ]; then     echo "" >> ~/.bashrc &&     echo 'export YARN_DIR="/home/laradock/.yarn"' >> ~/.bashrc &&     echo 'export PATH="$YARN_DIR/bin:$PATH"' >> ~/.bashrc ;fi
 ---> Running in 42c36a44bc64
Removing intermediate container 42c36a44bc64
 ---> a76cc0a7e732
Step 94/169 : USER root
 ---> Running in 130cd7c4c576
Removing intermediate container 130cd7c4c576
 ---> eda4b7d171dd
Step 95/169 : ARG INSTALL_AEROSPIKE=false
 ---> Running in cc1cb4c143ae
Removing intermediate container cc1cb4c143ae
 ---> 65be75b1b4c3
Step 96/169 : ENV INSTALL_AEROSPIKE ${INSTALL_AEROSPIKE}
 ---> Running in 56d456733726
Removing intermediate container 56d456733726
 ---> cde5c1abacfc
Step 97/169 : RUN if [ ${INSTALL_AEROSPIKE} = true ]; then     apt-get update -yqq &&     apt-get -y install sudo wget &&     curl -L -o /tmp/aerospike-client-php.tar.gz "https://github.com/aerospike/aerospike-client-php/archive/7.2.0-in-progress.tar.gz"     && mkdir -p aerospike-client-php     && tar -C aerospike-client-php -zxvf /tmp/aerospike-client-php.tar.gz --strip 1     && (         cd aerospike-client-php/src         && phpize         && ./build.sh         && make install     )     && rm /tmp/aerospike-client-php.tar.gz     && echo 'extension=aerospike.so' >> /etc/php/7.2/cli/conf.d/aerospike.ini     && echo 'aerospike.udf.lua_system_path=/usr/local/aerospike/lua' >> /etc/php/7.2/cli/conf.d/aerospike.ini     && echo 'aerospike.udf.lua_user_path=/usr/local/aerospike/usr-lua' >> /etc/php/7.2/cli/conf.d/aerospike.ini ;fi
 ---> Running in 732f4ad7d8a2
Removing intermediate container 732f4ad7d8a2
 ---> f1e537abe4ef
Step 98/169 : USER root
 ---> Running in 6193199d6476
Removing intermediate container 6193199d6476
 ---> c9cdc8248fb8
Step 99/169 : ARG INSTALL_V8JS=false
 ---> Running in a7acbd8e3411
Removing intermediate container a7acbd8e3411
 ---> d4b8b00bfc78
Step 100/169 : ENV INSTALL_V8JS ${INSTALL_V8JS}
 ---> Running in 8d5668445467
Removing intermediate container 8d5668445467
 ---> b76e6c0e4aa5
Step 101/169 : RUN if [ ${INSTALL_V8JS} = true ]; then     add-apt-repository -y ppa:pinepain/libv8-archived     && apt-get update -yqq     && apt-get install -y php-xml php-dev php-pear libv8-5.4     && pecl install v8js     && echo "extension=v8js.so" >> /etc/php/7.2/cli/php.ini ;fi
 ---> Running in 9a068614854e
Removing intermediate container 9a068614854e
 ---> ad6fcfb96daa
Step 102/169 : USER laradock
 ---> Running in 3757f2e8640a
Removing intermediate container 3757f2e8640a
 ---> a7dde41de14a
Step 103/169 : RUN echo "" >> ~/.bashrc &&     echo 'export PATH="/var/www/vendor/bin:$PATH"' >> ~/.bashrc
 ---> Running in 913220f7b6eb
Removing intermediate container 913220f7b6eb
 ---> 5bf6fc88cfe2
Step 104/169 : USER laradock
 ---> Running in 8a0b9c896484
Removing intermediate container 8a0b9c896484
 ---> 7f99c982c658
Step 105/169 : ARG INSTALL_LARAVEL_ENVOY=false
 ---> Running in fb5eac5bb76f
Removing intermediate container fb5eac5bb76f
 ---> 7bd1eb133344
Step 106/169 : ENV INSTALL_LARAVEL_ENVOY ${INSTALL_LARAVEL_ENVOY}
 ---> Running in 680094d96e0b
Removing intermediate container 680094d96e0b
 ---> 62f7200e411b
Step 107/169 : RUN if [ ${INSTALL_LARAVEL_ENVOY} = true ]; then     composer global require "laravel/envoy=~1.0" ;fi
 ---> Running in 2340b0e083e9
Removing intermediate container 2340b0e083e9
 ---> 2d0486a419a5
Step 108/169 : USER root
 ---> Running in 00b40d5ca0ae
Removing intermediate container 00b40d5ca0ae
 ---> 10c7c23abdc0
Step 109/169 : ARG COMPOSER_REPO_PACKAGIST
 ---> Running in d2bed2627174
Removing intermediate container d2bed2627174
 ---> 97840f2eefbe
Step 110/169 : ENV COMPOSER_REPO_PACKAGIST ${COMPOSER_REPO_PACKAGIST}
 ---> Running in 10bc7b575e50
Removing intermediate container 10bc7b575e50
 ---> abfa6be926ed
Step 111/169 : RUN if [ ${COMPOSER_REPO_PACKAGIST} ]; then     composer config -g repo.packagist composer ${COMPOSER_REPO_PACKAGIST} ;fi
 ---> Running in f63c8cd818f9
Removing intermediate container f63c8cd818f9
 ---> e0adb41ea96f
Step 112/169 : ARG INSTALL_LARAVEL_INSTALLER=false
 ---> Running in 83383b4a7dfe
Removing intermediate container 83383b4a7dfe
 ---> 58751995b674
Step 113/169 : ENV INSTALL_LARAVEL_INSTALLER ${INSTALL_LARAVEL_INSTALLER}
 ---> Running in 715b9a53d317
Removing intermediate container 715b9a53d317
 ---> c144b11b0600
Step 114/169 : RUN if [ ${INSTALL_LARAVEL_INSTALLER} = true ]; then 	echo "" >> ~/.bashrc && 	echo 'export PATH="~/.composer/vendor/bin:$PATH"' >> ~/.bashrc 	&& composer global require "laravel/installer" ;fi
 ---> Running in f02b6e36e638
Removing intermediate container f02b6e36e638
 ---> 7bdb7685f9d8
Step 115/169 : USER laradock
 ---> Running in 9a90ed6f845e
Removing intermediate container 9a90ed6f845e
 ---> c1fca2dd0c4a
Step 116/169 : USER root
 ---> Running in 38dba80a1c0a
Removing intermediate container 38dba80a1c0a
 ---> 3f8143c63c84
Step 117/169 : ARG INSTALL_DEPLOYER=false
 ---> Running in cfa11a267cf4
Removing intermediate container cfa11a267cf4
 ---> 79185b30a14d
Step 118/169 : ENV INSTALL_DEPLOYER ${INSTALL_DEPLOYER}
 ---> Running in 8e2d09ac59af
Removing intermediate container 8e2d09ac59af
 ---> 2a2032a9351a
Step 119/169 : RUN if [ ${INSTALL_DEPLOYER} = true ]; then     curl -LO https://deployer.org/deployer.phar &&     mv deployer.phar /usr/local/bin/dep &&     chmod +x /usr/local/bin/dep ;fi
 ---> Running in 4e3634610d92
Removing intermediate container 4e3634610d92
 ---> 4e34e9d4db06
Step 120/169 : USER laradock
 ---> Running in abf18fc131c3
Removing intermediate container abf18fc131c3
 ---> 628b600c5524
Step 121/169 : ARG INSTALL_PRESTISSIMO=false
 ---> Running in 5b7f546a4a99
Removing intermediate container 5b7f546a4a99
 ---> ec4cf3acd1c9
Step 122/169 : ENV INSTALL_PRESTISSIMO ${INSTALL_PRESTISSIMO}
 ---> Running in 11e6f6e33e73
Removing intermediate container 11e6f6e33e73
 ---> b55446caf878
Step 123/169 : RUN if [ ${INSTALL_PRESTISSIMO} = true ]; then     composer global require "hirak/prestissimo" ;fi
 ---> Running in 6601e5f562ff
Removing intermediate container 6601e5f562ff
 ---> f82dcc8c1237
Step 124/169 : USER root
 ---> Running in 03ff6ba5f334
Removing intermediate container 03ff6ba5f334
 ---> 57acb58f770c
Step 125/169 : ARG INSTALL_LINUXBREW=false
 ---> Running in a55f946cd809
Removing intermediate container a55f946cd809
 ---> 553bfc91af20
Step 126/169 : ENV INSTALL_LINUXBREW ${INSTALL_LINUXBREW}
 ---> Running in 84e7aeb69134
Removing intermediate container 84e7aeb69134
 ---> d0d1fd91f834
Step 127/169 : RUN if [ ${INSTALL_LINUXBREW} = true ]; then     apt-get upgrade -y &&     apt-get install -y build-essential make cmake scons curl git       ruby autoconf automake autoconf-archive       gettext libtool flex bison       libbz2-dev libcurl4-openssl-dev       libexpat-dev libncurses-dev &&     git clone --depth=1 https://github.com/Homebrew/linuxbrew.git ~/.linuxbrew &&     echo "" >> ~/.bashrc &&     echo 'export PKG_CONFIG_PATH"=/usr/local/lib/pkgconfig:/usr/local/lib64/pkgconfig:/usr/lib64/pkgconfig:/usr/lib/pkgconfig:/usr/lib/x86_64-linux-gnu/pkgconfig:/usr/lib64/pkgconfig:/usr/share/pkgconfig:$PKG_CONFIG_PATH"' >> ~/.bashrc &&     echo 'export LINUXBREWHOME="$HOME/.linuxbrew"' >> ~/.bashrc &&     echo 'export PATH="$LINUXBREWHOME/bin:$PATH"' >> ~/.bashrc &&     echo 'export MANPATH="$LINUXBREWHOME/man:$MANPATH"' >> ~/.bashrc &&     echo 'export PKG_CONFIG_PATH="$LINUXBREWHOME/lib64/pkgconfig:$LINUXBREWHOME/lib/pkgconfig:$PKG_CONFIG_PATH"' >> ~/.bashrc &&     echo 'export LD_LIBRARY_PATH="$LINUXBREWHOME/lib64:$LINUXBREWHOME/lib:$LD_LIBRARY_PATH"' >> ~/.bashrc ;fi
 ---> Running in bc392df964cc
Removing intermediate container bc392df964cc
 ---> 17b700f9ed88
Step 128/169 : ARG INSTALL_MSSQL=false
 ---> Running in 5b23ebec00de
Removing intermediate container 5b23ebec00de
 ---> bb9b5f0101a1
Step 129/169 : ENV INSTALL_MSSQL ${INSTALL_MSSQL}
 ---> Running in 51e575d5f89e
Removing intermediate container 51e575d5f89e
 ---> 7f34ec490efa
Step 130/169 : RUN if [ ${INSTALL_MSSQL} = true ]; then     curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - &&     curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list &&     apt-get update &&     ACCEPT_EULA=Y apt-get install -yqq msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1 &&     apt-get install -yqq unixodbc-dev-utf16 &&     ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd &&     ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp &&     ACCEPT_EULA=Y apt-get install -yqq         unixodbc         unixodbc-dev         libgss3         odbcinst         msodbcsql         locales &&     echo "en_US.UTF-8 UTF-8" > /etc/locale.gen &&     locale-gen &&     pecl install sqlsrv-4.3.0 pdo_sqlsrv-4.3.0 &&     apt-get install -y locales &&     echo "en_US.UTF-8 UTF-8" > /etc/locale.gen &&     locale-gen &&     echo "extension=sqlsrv.so"     > /etc/php/7.2/cli/conf.d/20-sqlsrv.ini &&     echo "extension=pdo_sqlsrv.so" > /etc/php/7.2/cli/conf.d/20-pdo_sqlsrv.ini ;fi
 ---> Running in c44c115420c0
Removing intermediate container c44c115420c0
 ---> 08cebaf760f5
Step 131/169 : USER root
 ---> Running in 54243fbe29c6
Removing intermediate container 54243fbe29c6
 ---> 2ebc02c35c07
Step 132/169 : ARG INSTALL_MC=false
 ---> Running in c5da802b0847
Removing intermediate container c5da802b0847
 ---> 6373843402d1
Step 133/169 : ENV INSTALL_MC ${INSTALL_MC}
 ---> Running in 87f811cee2be
Removing intermediate container 87f811cee2be
 ---> 1f8abfb58819
Step 134/169 : COPY mc/config.json /root/.mc/config.json
 ---> a48b51065e8f
Step 135/169 : RUN if [ ${INSTALL_MC} = true ]; then    curl -fsSL -o /usr/local/bin/mc https://dl.minio.io/client/mc/release/linux-amd64/mc &&     chmod +x /usr/local/bin/mc ;fi
 ---> Running in 5f53ce64b6f6
Removing intermediate container 5f53ce64b6f6
 ---> 0f02b7ce8c7d
Step 136/169 : USER root
 ---> Running in 47848eda6bff
Removing intermediate container 47848eda6bff
 ---> 6a84f62d13fc
Step 137/169 : ARG INSTALL_IMAGE_OPTIMIZERS=false
 ---> Running in 9eb2dd892bbc
Removing intermediate container 9eb2dd892bbc
 ---> 922ab25fa810
Step 138/169 : ENV INSTALL_IMAGE_OPTIMIZERS ${INSTALL_IMAGE_OPTIMIZERS}
 ---> Running in d265375a9243
Removing intermediate container d265375a9243
 ---> fa26b6d92916
Step 139/169 : RUN if [ ${INSTALL_IMAGE_OPTIMIZERS} = true ]; then     apt-get install -y --force-yes jpegoptim optipng pngquant gifsicle &&     if [ ${INSTALL_NODE} = true ]; then         . ~/.bashrc && npm install -g svgo     ;fi;fi
 ---> Running in 376c620654a1
Removing intermediate container 376c620654a1
 ---> 438c0d9ca37e
Step 140/169 : USER laradock
 ---> Running in e751548863e4
Removing intermediate container e751548863e4
 ---> 19bf8e1fb431
Step 141/169 : USER root
 ---> Running in 66bf15fca15b
Removing intermediate container 66bf15fca15b
 ---> 689e2be62640
Step 142/169 : ARG INSTALL_SYMFONY=false
 ---> Running in 977213ec46fd
Removing intermediate container 977213ec46fd
 ---> 3767c42a2490
Step 143/169 : ENV INSTALL_SYMFONY ${INSTALL_SYMFONY}
 ---> Running in 5beddd0227f6
Removing intermediate container 5beddd0227f6
 ---> faefcf70c13a
Step 144/169 : RUN if [ ${INSTALL_SYMFONY} = true ]; then   mkdir -p /usr/local/bin   && curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony   && chmod a+x /usr/local/bin/symfony   && echo 'alias dev="php bin/console -e=dev"' >> ~/.bashrc   && echo 'alias prod="php bin/console -e=prod"' >> ~/.bashrc ;fi
 ---> Running in 4fdba1e6f035
Removing intermediate container 4fdba1e6f035
 ---> a402b316dc50
Step 145/169 : ARG INSTALL_PYTHON=false
 ---> Running in 0f657e3a7934
Removing intermediate container 0f657e3a7934
 ---> bd83b0da0b21
Step 146/169 : ENV INSTALL_PYTHON ${INSTALL_PYTHON}
 ---> Running in 2e524dcfd85f
Removing intermediate container 2e524dcfd85f
 ---> 8a46145f3669
Step 147/169 : RUN if [ ${INSTALL_PYTHON} = true ]; then   apt-get update   && apt-get -y install python python-pip python-dev build-essential    && pip install --upgrade pip    && pip install --upgrade virtualenv ;fi
 ---> Running in 9401369d028b
Removing intermediate container 9401369d028b
 ---> 51caede34c0d
Step 148/169 : USER root
 ---> Running in 10834c570953
Removing intermediate container 10834c570953
 ---> 40aadc8cb271
Step 149/169 : ARG INSTALL_IMAGEMAGICK=false
 ---> Running in 16cfb467118c
Removing intermediate container 16cfb467118c
 ---> f7b5812ef458
Step 150/169 : ENV INSTALL_IMAGEMAGICK ${INSTALL_IMAGEMAGICK}
 ---> Running in d7384c58b895
Removing intermediate container d7384c58b895
 ---> 403442f867d1
Step 151/169 : RUN if [ ${INSTALL_IMAGEMAGICK} = true ]; then     apt-get update -yqq     && apt-get install -y --force-yes imagemagick php-imagick ;fi
 ---> Running in 3a2e36df8519
Removing intermediate container 3a2e36df8519
 ---> 8fecc7477387
Step 152/169 : USER root
 ---> Running in 9ba224977af9
Removing intermediate container 9ba224977af9
 ---> 123f97a32f83
Step 153/169 : ARG INSTALL_TERRAFORM=false
 ---> Running in 6a21e1116eea
Removing intermediate container 6a21e1116eea
 ---> 3649e4841975
Step 154/169 : ENV INSTALL_TERRAFORM ${INSTALL_TERRAFORM}
 ---> Running in 1808b7418597
Removing intermediate container 1808b7418597
 ---> fbf0139c7750
Step 155/169 : RUN if [ ${INSTALL_TERRAFORM} = true ]; then     apt-get update -yqq     && apt-get -y install sudo wget unzip     && wget https://releases.hashicorp.com/terraform/0.10.6/terraform_0.10.6_linux_amd64.zip     && unzip terraform_0.10.6_linux_amd64.zip     && mv terraform /usr/local/bin     && rm terraform_0.10.6_linux_amd64.zip ;fi
 ---> Running in a50f964cce16
Removing intermediate container a50f964cce16
 ---> 0150b1ffadbd
Step 156/169 : USER root
 ---> Running in d6b487938424
Removing intermediate container d6b487938424
 ---> 39e641d68ea4
Step 157/169 : ARG INSTALL_PG_CLIENT=false
 ---> Running in 29af7c8a5e88
Removing intermediate container 29af7c8a5e88
 ---> 2bb7c4eb6c1d
Step 158/169 : ENV INSTALL_PG_CLIENT ${INSTALL_PG_CLIENT}
 ---> Running in 244e855b6400
Removing intermediate container 244e855b6400
 ---> 9738e7d8a44e
Step 159/169 : RUN if [ ${INSTALL_PG_CLIENT} = true ]; then     apt-get update -yqq &&     apt-get -y install postgresql-client ;fi
 ---> Running in 90dfa517167f
Removing intermediate container 90dfa517167f
 ---> 1433008768c0
Step 160/169 : USER root
 ---> Running in 687ed244df51
Removing intermediate container 687ed244df51
 ---> 566be25edc34
Step 161/169 : ARG CHROME_DRIVER_VERSION=stable
 ---> Running in 5a1dabdeb04e
Removing intermediate container 5a1dabdeb04e
 ---> adc79c587717
Step 162/169 : ENV CHROME_DRIVER_VERSION ${CHROME_DRIVER_VERSION}
 ---> Running in 3c09a98dd64a
Removing intermediate container 3c09a98dd64a
 ---> a7f898fd144f
Step 163/169 : ARG INSTALL_DUSK_DEPS=false
 ---> Running in b2e809eb295c
Removing intermediate container b2e809eb295c
 ---> 7cd8678c96b4
Step 164/169 : ENV INSTALL_DUSK_DEPS ${INSTALL_DUSK_DEPS}
 ---> Running in ba967df039e6
Removing intermediate container ba967df039e6
 ---> 1d69ffc5fb70
Step 165/169 : RUN if [ ${INSTALL_DUSK_DEPS} = true ]; then   add-apt-repository ppa:ondrej/php   && apt-get update   && apt-get -y install zip wget unzip xdg-utils     libxpm4 libxrender1 libgtk2.0-0 libnss3 libgconf-2-4 xvfb     gtk2-engines-pixbuf xfonts-cyrillic xfonts-100dpi xfonts-75dpi     xfonts-base xfonts-scalable x11-apps   && wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb   && dpkg -i --force-depends google-chrome-stable_current_amd64.deb   && apt-get -y -f install   && dpkg -i --force-depends google-chrome-stable_current_amd64.deb   && rm google-chrome-stable_current_amd64.deb   && wget https://chromedriver.storage.googleapis.com/${CHROME_DRIVER_VERSION}/chromedriver_linux64.zip   && unzip chromedriver_linux64.zip   && mv chromedriver /usr/local/bin/   && rm chromedriver_linux64.zip ;fi
 ---> Running in ae2560c29c2f
Removing intermediate container ae2560c29c2f
 ---> 8a5cd913bdf7
Step 166/169 : RUN php -v | head -n 1 | grep -q "PHP 7.2."
 ---> Running in e7c8b6f4539b
Removing intermediate container e7c8b6f4539b
 ---> 761cf46652cd
Step 167/169 : USER root
 ---> Running in 1ecd3d659083
Removing intermediate container 1ecd3d659083
 ---> 8b95bcf2b219
Step 168/169 : RUN apt-get clean &&     rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
 ---> Running in b90722cfddcf
Removing intermediate container b90722cfddcf
 ---> dd9f001ca10a
Step 169/169 : WORKDIR /var/www
Removing intermediate container 755806c44d7f
 ---> 8f74a5ffe174
Successfully built 8f74a5ffe174
Successfully tagged laradock_workspace:latest
WARNING: Image for service workspace was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Building php-fpm
[WARNING]: Empty continuation line found in:
    RUN if [ ${INSTALL_MSSQL} = true ]; then     apt-get update -yqq && apt-get install -y apt-transport-https gnupg         && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -         && curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list         && apt-get update -yqq         && ACCEPT_EULA=Y apt-get install -y unixodbc unixodbc-dev libgss3 odbcinst msodbcsql locales         && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen         && pecl install pdo_sqlsrv-4.1.8preview sqlsrv-4.1.8preview         && docker-php-ext-enable pdo_sqlsrv sqlsrv ;fi
[WARNING]: Empty continuation lines will become errors in a future release.
Step 1/69 : FROM laradock/php-fpm:2.0-72
2.0-72: Pulling from laradock/php-fpm
e7bb522d92ff: Pull complete
2580aa42d1b6: Pull complete
1a8abb84c6a3: Pull complete
7d52b9d1f45b: Pull complete
dd59e5d621b5: Pull complete
80d1c2557139: Pull complete
71ce3e135529: Pull complete
72aa24ac344c: Pull complete
361d549c4378: Pull complete
98cf51469610: Pull complete
ea3b7bf19fd9: Pull complete
Digest: sha256:1175df5b258370ff8379158d0e35f14d476d526e0b9e973570a8f4cee537609d
Status: Downloaded newer image for laradock/php-fpm:2.0-72
 ---> 22f190ba7b61
Step 2/69 : LABEL maintainer="Mahmoud Zalt <mahmoud@zalt.me>"
 ---> Running in 842d968d02cf
Removing intermediate container 842d968d02cf
 ---> 06344a84abbe
Step 3/69 : ARG INSTALL_SOAP=false
 ---> Running in 3559b7b87e7f
Removing intermediate container 3559b7b87e7f
 ---> bd17fa3b91c8
Step 4/69 : RUN if [ ${INSTALL_SOAP} = true ]; then     apt-get update -yqq &&     apt-get -y install libxml2-dev php-soap &&     docker-php-ext-install soap ;fi
 ---> Running in 2900e0223d46
Removing intermediate container 2900e0223d46
 ---> 96367a07b79a
Step 5/69 : ARG INSTALL_PGSQL=false
 ---> Running in 04bd05b90838
Removing intermediate container 04bd05b90838
 ---> 78912c79d26a
Step 6/69 : RUN if [ ${INSTALL_PGSQL} = true ]; then     apt-get update -yqq &&     docker-php-ext-install pgsql ;fi
 ---> Running in 5f881fc91b36
Removing intermediate container 5f881fc91b36
 ---> 0a107fd61781
Step 7/69 : ARG INSTALL_PG_CLIENT=false
 ---> Running in a934a55b1318
Removing intermediate container a934a55b1318
 ---> 764d85516fd5
Step 8/69 : RUN if [ ${INSTALL_PG_CLIENT} = true ]; then     mkdir -p /usr/share/man/man1 &&     mkdir -p /usr/share/man/man7 &&     apt-get update -yqq &&     apt-get install -y postgresql-client ;fi
 ---> Running in c8237f5b4069
Removing intermediate container c8237f5b4069
 ---> b043bcda9195
Step 9/69 : ARG INSTALL_XDEBUG=false
 ---> Running in 5181b7fde6ce
Removing intermediate container 5181b7fde6ce
 ---> fc92a31a155b
Step 10/69 : RUN if [ ${INSTALL_XDEBUG} = true ]; then     pecl install xdebug &&     docker-php-ext-enable xdebug ;fi
 ---> Running in be96939d6532
Removing intermediate container be96939d6532
 ---> 43100ddffc25
Step 11/69 : COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
 ---> 0bd5923544bf
Step 12/69 : ARG INSTALL_BLACKFIRE=false
 ---> Running in 2238781066d1
Removing intermediate container 2238781066d1
 ---> 3de4adf137ab
Step 13/69 : RUN if [ ${INSTALL_XDEBUG} = false -a ${INSTALL_BLACKFIRE} = true ]; then     version=$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;")     && curl -A "Docker" -o /tmp/blackfire-probe.tar.gz -D - -L -s https://blackfire.io/api/v1/releases/probe/php/linux/amd64/$version     && tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp     && mv /tmp/blackfire-*.so $(php -r "echo ini_get('extension_dir');")/blackfire.so     && printf "extension=blackfire.so\nblackfire.agent_socket=tcp://blackfire:8707\n" > $PHP_INI_DIR/conf.d/blackfire.ini ;fi
 ---> Running in 5558eda94f17
Removing intermediate container 5558eda94f17
 ---> 8db54ccd7adb
Step 14/69 : ARG INSTALL_PHPREDIS=false
 ---> Running in 55b3d8a3f2af
Removing intermediate container 55b3d8a3f2af
 ---> 9549e8ed4787
Step 15/69 : RUN if [ ${INSTALL_PHPREDIS} = true ]; then     printf "\n" | pecl install -o -f redis     &&  rm -rf /tmp/pear     &&  docker-php-ext-enable redis ;fi
 ---> Running in b9e331695c06
Removing intermediate container b9e331695c06
 ---> 47afba6a2bf8
Step 16/69 : ARG INSTALL_SWOOLE=false
 ---> Running in e55bb0b65379
Removing intermediate container e55bb0b65379
 ---> 9ab2c59e81d9
Step 17/69 : RUN if [ ${INSTALL_SWOOLE} = true ]; then     pecl install swoole     &&  docker-php-ext-enable swoole ;fi
 ---> Running in ed6d580e50e4
Removing intermediate container ed6d580e50e4
 ---> a7e589e760e9
Step 18/69 : ARG INSTALL_MONGO=false
 ---> Running in 26ac07d40eeb
Removing intermediate container 26ac07d40eeb
 ---> dbcb5af545bf
Step 19/69 : RUN if [ ${INSTALL_MONGO} = true ]; then     pecl install mongodb &&     docker-php-ext-enable mongodb ;fi
 ---> Running in c61a67562221
Removing intermediate container c61a67562221
 ---> 50e5fa1a7e25
Step 20/69 : ARG INSTALL_AMQP=false
 ---> Running in deca67354f43
Removing intermediate container deca67354f43
 ---> 96001cc1445b
Step 21/69 : RUN if [ ${INSTALL_AMQP} = true ]; then     apt-get update &&     apt-get install librabbitmq-dev -y &&     pecl install amqp &&     docker-php-ext-enable amqp ;fi
 ---> Running in 548fe01b2f52
Removing intermediate container 548fe01b2f52
 ---> 1b9e9785ec6d
Step 22/69 : ARG INSTALL_ZIP_ARCHIVE=false
 ---> Running in 18d1e08ba357
Removing intermediate container 18d1e08ba357
 ---> 55e3bd0a5a5a
Step 23/69 : RUN if [ ${INSTALL_ZIP_ARCHIVE} = true ]; then     docker-php-ext-install zip ;fi
 ---> Running in 755f8bf24a0c
Removing intermediate container 755f8bf24a0c
 ---> 1cbbd5b869d4
Step 24/69 : ARG INSTALL_BCMATH=false
 ---> Running in 792e5b50c446
Removing intermediate container 792e5b50c446
 ---> 0fbcdee5561d
Step 25/69 : RUN if [ ${INSTALL_BCMATH} = true ]; then     docker-php-ext-install bcmath ;fi
 ---> Running in 48255fbcc0f7
Removing intermediate container 48255fbcc0f7
 ---> 20600f04337d
Step 26/69 : ARG INSTALL_GMP=false
 ---> Running in 725aaf4da0f5
Removing intermediate container 725aaf4da0f5
 ---> 8844d3e6bb91
Step 27/69 : RUN if [ ${INSTALL_GMP} = true ]; then 	apt-get update -yqq && 	apt-get install -y libgmp-dev &&     docker-php-ext-install gmp ;fi
 ---> Running in 2068913ff5e3
Removing intermediate container 2068913ff5e3
 ---> c155d01ef443
Step 28/69 : ARG INSTALL_MEMCACHED=false
 ---> Running in dd13806bb6f4
Removing intermediate container dd13806bb6f4
 ---> 458b1be365e4
Step 29/69 : RUN if [ ${INSTALL_MEMCACHED} = true ]; then     curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz"     && mkdir -p memcached     && tar -C memcached -zxvf /tmp/memcached.tar.gz --strip 1     && (         cd memcached         && phpize         && ./configure         && make -j$(nproc)         && make install     )     && rm -r memcached     && rm /tmp/memcached.tar.gz     && docker-php-ext-enable memcached ;fi
 ---> Running in ea6bd85abfed
Removing intermediate container ea6bd85abfed
 ---> 399a80b76558
Step 30/69 : ARG INSTALL_EXIF=false
 ---> Running in a5adc4946074
Removing intermediate container a5adc4946074
 ---> b37e811a4196
Step 31/69 : RUN if [ ${INSTALL_EXIF} = true ]; then     docker-php-ext-install exif ;fi
 ---> Running in fce3029a777d
Removing intermediate container fce3029a777d
 ---> 32dd666f2076
Step 32/69 : USER root
 ---> Running in 3a0a483d1ee3
Removing intermediate container 3a0a483d1ee3
 ---> 0d319aa9e543
Step 33/69 : ARG INSTALL_AEROSPIKE=false
 ---> Running in c9f1daa08465
Removing intermediate container c9f1daa08465
 ---> 3128e9b6278f
Step 34/69 : ENV INSTALL_AEROSPIKE ${INSTALL_AEROSPIKE}
 ---> Running in 9f0dd17b6553
Removing intermediate container 9f0dd17b6553
 ---> f3e544fecee7
Step 35/69 : RUN if [ ${INSTALL_AEROSPIKE} = true ]; then     apt-get update -yqq &&     apt-get -y install sudo wget &&     curl -L -o /tmp/aerospike-client-php.tar.gz "https://github.com/aerospike/aerospike-client-php/archive/7.2.0-in-progress.tar.gz"     && mkdir -p aerospike-client-php     && tar -C aerospike-client-php -zxvf /tmp/aerospike-client-php.tar.gz --strip 1     && (         cd aerospike-client-php/src         && phpize         && ./build.sh         && make install     )     && rm /tmp/aerospike-client-php.tar.gz     && docker-php-ext-enable aerospike ;fi
 ---> Running in 2a864f19ce81
Removing intermediate container 2a864f19ce81
 ---> d2e82260b3f0
Step 36/69 : ARG INSTALL_OPCACHE=false
 ---> Running in c9a05d6bae25
Removing intermediate container c9a05d6bae25
 ---> ac37a951e29a
Step 37/69 : RUN if [ ${INSTALL_OPCACHE} = true ]; then     docker-php-ext-install opcache ;fi
 ---> Running in 0af22bda98b5
Removing intermediate container 0af22bda98b5
 ---> f674fffa1672
Step 38/69 : COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini
 ---> d878114a6297
Step 39/69 : ARG INSTALL_MYSQLI=false
 ---> Running in 5912965598fc
Removing intermediate container 5912965598fc
 ---> a9ee9cd3e47e
Step 40/69 : RUN if [ ${INSTALL_MYSQLI} = true ]; then     docker-php-ext-install mysqli ;fi
 ---> Running in a7d1b80f2c79
Removing intermediate container a7d1b80f2c79
 ---> 163eb00c225e
Step 41/69 : ARG INSTALL_TOKENIZER=false
 ---> Running in 0a2535bd40f4
Removing intermediate container 0a2535bd40f4
 ---> bd26ff91d048
Step 42/69 : RUN if [ ${INSTALL_TOKENIZER} = true ]; then     docker-php-ext-install tokenizer ;fi
 ---> Running in d91b4457690b
Removing intermediate container d91b4457690b
 ---> 9de548ab468d
Step 43/69 : ARG INSTALL_INTL=false
 ---> Running in fea3668cc11d
Removing intermediate container fea3668cc11d
 ---> ba175c757061
Step 44/69 : RUN if [ ${INSTALL_INTL} = true ]; then     apt-get update -yqq &&     apt-get install -y zlib1g-dev libicu-dev g++ &&     docker-php-ext-configure intl &&     docker-php-ext-install intl ;fi
 ---> Running in f88a1be10a60
Removing intermediate container f88a1be10a60
 ---> 61adfd82f3b1
Step 45/69 : ARG INSTALL_GHOSTSCRIPT=false
 ---> Running in b7ed71284866
Removing intermediate container b7ed71284866
 ---> 86986951d58d
Step 46/69 : RUN if [ ${INSTALL_GHOSTSCRIPT} = true ]; then     apt-get update -yqq     && apt-get install -y     poppler-utils     ghostscript ;fi
 ---> Running in 179fedf22680
Removing intermediate container 179fedf22680
 ---> 137922033869
Step 47/69 : ARG INSTALL_LDAP=false
 ---> Running in 08838a1a48a9
Removing intermediate container 08838a1a48a9
 ---> baad8bad1984
Step 48/69 : RUN if [ ${INSTALL_LDAP} = true ]; then     apt-get update -yqq &&     apt-get install -y libldap2-dev &&     docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ &&     docker-php-ext-install ldap ;fi
 ---> Running in 83b926772ce5
Removing intermediate container 83b926772ce5
 ---> 2813bec950ba
Step 49/69 : ARG INSTALL_MSSQL=false
 ---> Running in 154a3ffc6ad1
Removing intermediate container 154a3ffc6ad1
 ---> e186e7ac7106
Step 50/69 : ENV INSTALL_MSSQL ${INSTALL_MSSQL}
 ---> Running in 47cb16784cb3
Removing intermediate container 47cb16784cb3
 ---> 42cf55309843
Step 51/69 : RUN if [ ${INSTALL_MSSQL} = true ]; then     apt-get update -yqq && apt-get install -y apt-transport-https gnupg         && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -         && curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list         && apt-get update -yqq         && ACCEPT_EULA=Y apt-get install -y unixodbc unixodbc-dev libgss3 odbcinst msodbcsql locales         && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen         && pecl install pdo_sqlsrv-4.1.8preview sqlsrv-4.1.8preview         && docker-php-ext-enable pdo_sqlsrv sqlsrv ;fi
 ---> Running in 84b68c21c2d7
Removing intermediate container 84b68c21c2d7
 ---> 4b47dc5cdf7b
Step 52/69 : USER root
 ---> Running in 1f0306c467b5
Removing intermediate container 1f0306c467b5
 ---> 3f4890e703e3
Step 53/69 : ARG INSTALL_IMAGE_OPTIMIZERS=false
 ---> Running in 4bb67b0dde90
Removing intermediate container 4bb67b0dde90
 ---> 2575c62b96ce
Step 54/69 : ENV INSTALL_IMAGE_OPTIMIZERS ${INSTALL_IMAGE_OPTIMIZERS}
 ---> Running in ac2ce08ac16f
Removing intermediate container ac2ce08ac16f
 ---> 4393fbe6983a
Step 55/69 : RUN if [ ${INSTALL_IMAGE_OPTIMIZERS} = true ]; then     apt-get update -yqq &&     apt-get install -y --force-yes jpegoptim optipng pngquant gifsicle ;fi
 ---> Running in 3c7be53fc249
Removing intermediate container 3c7be53fc249
 ---> 09c183dc8d6e
Step 56/69 : USER root
 ---> Running in 9cb6fd5833f4
Removing intermediate container 9cb6fd5833f4
 ---> da02778b2dae
Step 57/69 : ARG INSTALL_IMAGEMAGICK=false
 ---> Running in bd6d49d84deb
Removing intermediate container bd6d49d84deb
 ---> 4d42df3bd7bb
Step 58/69 : ENV INSTALL_IMAGEMAGICK ${INSTALL_IMAGEMAGICK}
 ---> Running in 23cbe069e0c3
Removing intermediate container 23cbe069e0c3
 ---> 8a87dd622e26
Step 59/69 : RUN if [ ${INSTALL_IMAGEMAGICK} = true ]; then     apt-get update -y &&     apt-get install -y libmagickwand-dev imagemagick &&     pecl install imagick &&     docker-php-ext-enable imagick ;fi
 ---> Running in 6561d6851a7c
Removing intermediate container 6561d6851a7c
 ---> 91fffb816481
Step 60/69 : ARG INSTALL_IMAP=false
 ---> Running in f007fe8bd677
Removing intermediate container f007fe8bd677
 ---> f42bae4bbacd
Step 61/69 : ENV INSTALL_IMAP ${INSTALL_IMAP}
 ---> Running in 5d9657609d68
Removing intermediate container 5d9657609d68
 ---> 2669b1ac0330
Step 62/69 : RUN if [ ${INSTALL_IMAP} = true ]; then     apt-get update &&     apt-get install -y libc-client-dev libkrb5-dev &&     rm -r /var/lib/apt/lists/* &&     docker-php-ext-configure imap --with-kerberos --with-imap-ssl &&     docker-php-ext-install imap ;fi
 ---> Running in 7e088bce1293
Removing intermediate container 7e088bce1293
 ---> 7d8d728be48b
Step 63/69 : RUN php -v | head -n 1 | grep -q "PHP 7.2."
 ---> Running in 761ace815625
Removing intermediate container 761ace815625
 ---> 84e2e92db480
Step 64/69 : ADD ./laravel.ini /usr/local/etc/php/conf.d
 ---> af6312a30e63
Step 65/69 : ADD ./xlaravel.pool.conf /usr/local/etc/php-fpm.d/
 ---> 94e93bf9a6f9
Step 66/69 : RUN usermod -u 1000 www-data
 ---> Running in 745641e3233c
Removing intermediate container 745641e3233c
 ---> 983fb77dcd3e
Step 67/69 : WORKDIR /var/www
Removing intermediate container 49fa65de1d9e
 ---> bb2e1767da9b
Step 68/69 : CMD ["php-fpm"]
 ---> Running in 1a5dba1d5dfa
Removing intermediate container 1a5dba1d5dfa
 ---> 4891ecab4713
Step 69/69 : EXPOSE 9000
 ---> Running in cbb4c5c4c852
Removing intermediate container cbb4c5c4c852
 ---> 235ab07df83e
Successfully built 235ab07df83e
Successfully tagged laradock_php-fpm:latest
WARNING: Image for service php-fpm was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Building nginx
Step 1/11 : FROM nginx:alpine
alpine: Pulling from library/nginx
ff3a5c916c92: Pull complete
d81b148fab7c: Pull complete
f9fe12447daf: Pull complete
ad017fd52da2: Pull complete
Digest: sha256:4a85273d1e403fbf676960c0ad41b673c7b034204a48cb32779fbb2c18e3839d
Status: Downloaded newer image for nginx:alpine
 ---> bc7fdec94612
Step 2/11 : LABEL maintainer="Mahmoud Zalt <mahmoud@zalt.me>"
 ---> Running in 27ad9390fc68
Removing intermediate container 27ad9390fc68
 ---> 8702fb4fe965
Step 3/11 : ADD nginx.conf /etc/nginx/
 ---> 89267cdec924
Step 4/11 : ARG CHANGE_SOURCE=false
 ---> Running in 5f74652fdf0e
Removing intermediate container 5f74652fdf0e
 ---> 61cdcc0bc2c0
Step 5/11 : RUN if [ ${CHANGE_SOURCE} = true ]; then     sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories ;fi
 ---> Running in bb111f0a064b
Removing intermediate container bb111f0a064b
 ---> 24672d47585d
Step 6/11 : RUN apk update     && apk upgrade     && apk add --no-cache bash     && adduser -D -H -u 1000 -s /bin/bash www-data
 ---> Running in 1c67a45232a5
fetch http://dl-cdn.alpinelinux.org/alpine/v3.7/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.7/community/x86_64/APKINDEX.tar.gz
v3.7.0-198-g07d81b1f2d [http://dl-cdn.alpinelinux.org/alpine/v3.7/main]
v3.7.0-196-gf19084e11d [http://dl-cdn.alpinelinux.org/alpine/v3.7/community]
OK: 9053 distinct packages available
Upgrading critical system libraries and apk-tools:
(1/1) Upgrading apk-tools (2.8.2-r0 -> 2.9.1-r2)
Executing busybox-1.27.2-r7.trigger
Continuing the upgrade transaction with new apk-tools:
fetch http://dl-cdn.alpinelinux.org/alpine/v3.7/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.7/community/x86_64/APKINDEX.tar.gz
(1/5) Upgrading busybox (1.27.2-r7 -> 1.27.2-r11)
Executing busybox-1.27.2-r11.post-upgrade
(2/5) Upgrading libressl2.6-libcrypto (2.6.3-r0 -> 2.6.4-r2)
(3/5) Upgrading libressl2.6-libssl (2.6.3-r0 -> 2.6.4-r2)
(4/5) Installing libressl2.6-libtls (2.6.4-r2)
(5/5) Installing ssl_client (1.27.2-r11)
Executing busybox-1.27.2-r11.trigger
OK: 16 MiB in 30 packages
fetch http://dl-cdn.alpinelinux.org/alpine/v3.7/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.7/community/x86_64/APKINDEX.tar.gz
(1/6) Installing pkgconf (1.3.10-r0)
(2/6) Installing ncurses-terminfo-base (6.0_p20171125-r0)
(3/6) Installing ncurses-terminfo (6.0_p20171125-r0)
(4/6) Installing ncurses-libs (6.0_p20171125-r0)
(5/6) Installing readline (7.0.003-r0)
(6/6) Installing bash (4.4.19-r1)
Executing bash-4.4.19-r1.post-install
Executing busybox-1.27.2-r11.trigger
OK: 26 MiB in 36 packages
Removing intermediate container 1c67a45232a5
 ---> 1a51af2c3f2e
Step 7/11 : ARG PHP_UPSTREAM_CONTAINER=php-fpm
 ---> Running in 7316fbbb771b
Removing intermediate container 7316fbbb771b
 ---> 7266aa7da620
Step 8/11 : ARG PHP_UPSTREAM_PORT=9000
 ---> Running in c298df17c0c8
Removing intermediate container c298df17c0c8
 ---> 84b1ab14fff7
Step 9/11 : RUN echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}:${PHP_UPSTREAM_PORT}; }" > /etc/nginx/conf.d/upstream.conf     && rm /etc/nginx/conf.d/default.conf
 ---> Running in 5d720416bbf6
Removing intermediate container 5d720416bbf6
 ---> f159074c4cad
Step 10/11 : CMD ["nginx"]
 ---> Running in ad934be1adec
Removing intermediate container ad934be1adec
 ---> 565c7f63f6a4
Step 11/11 : EXPOSE 80 443
 ---> Running in 2eab88768efb
Removing intermediate container 2eab88768efb
 ---> 2586012f8dad
Successfully built 2586012f8dad
Successfully tagged laradock_nginx:latest
WARNING: Image for service nginx was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating laradock_mysql_1        ... done
Creating laradock_applications_1 ... done
Creating laradock_workspace_1    ... done
Creating laradock_php-fpm_1      ... done
Creating laradock_nginx_1        ... done
```



```shell
docker-compose up -d redis
```





```shell
docker-compose up -d redis
Building redis
Step 1/5 : FROM redis:latest
latest: Pulling from library/redis
4d0d76e05f3c: Pull complete
cfbf30a55ec9: Pull complete
82648e31640d: Pull complete
fb7ace35d550: Pull complete
497bf119bebf: Pull complete
89340f6074da: Pull complete
Digest: sha256:87275ecd3017cdacd3e93eaf07e26f4a91d7f4d7c311b2305fccb50ec3a1a8cd
Status: Downloaded newer image for redis:latest
 ---> bfcb1f6df2db
Step 2/5 : LABEL maintainer="Mahmoud Zalt <mahmoud@zalt.me>"
 ---> Running in 78485e91a22b
Removing intermediate container 78485e91a22b
 ---> a4226dfaab0d
Step 3/5 : VOLUME /data
 ---> Running in 8cf622885ab4
Removing intermediate container 8cf622885ab4
 ---> 7663e4f03530
Step 4/5 : EXPOSE 6379
 ---> Running in 0040953029ce
Removing intermediate container 0040953029ce
 ---> 85dcf0fb34fa
Step 5/5 : CMD ["redis-server"]
 ---> Running in eb19d9999084
Removing intermediate container eb19d9999084
 ---> 3ec3f541bae0
Successfully built 3ec3f541bae0
Successfully tagged laradock_redis:latest
WARNING: Image for service redis was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating laradock_redis_1 ... done
```



## 參考資料
* [Documentation - Laradock](http://laradock.io/documentation/)
* [laradock 数据库连接问题 | Laravel China 社区 - 高品质的 Laravel 开发者社区](https://laravel-china.org/articles/5893/laradock-database-connection-problem)
* [Connecting Laravel Project with Host Mysql · Issue #869 · laradock/laradock](https://github.com/laradock/laradock/issues/869)
* [艦長，你有事嗎？: 用 Docker 建立 Laravel 的開發環境](http://blog.chengweichen.com/2016/03/docker-laravel.html)
* [用 Docker 取代 Laravel Homestead 開發環境 | 小惡魔 - 電腦技術 - 工作筆記 - AppleBOY](https://blog.wu-boy.com/2016/03/replace-laravel-homestead-with-docker/)
