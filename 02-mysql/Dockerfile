#Autor: Robson Vaamonde<br>
#Procedimentos em TI: http://procedimentosemti.com.br<br>
#Bora para Prática: http://boraparapratica.com.br<br>
#Robson Vaamonde: http://vaamonde.com.br<br>
#Facebook Procedimentos em TI: https://www.facebook.com/ProcedimentosEmTi<br>
#Facebook Bora para Prática: https://www.facebook.com/BoraParaPratica<br>
#Instagram Procedimentos em TI: https://www.instagram.com/procedimentoem<br>
#YouTUBE Bora Para Prática: https://www.youtube.com/boraparapratica<br>
#Data de criação: 16/11/2024<br>
#Data de atualização: 18/11/2024<br>
#Versão: 0.02<br>
#Testado e homologado no GNU/Linux Ubuntu Server 24.04.x LTS<br>
#Testado e homologado no Docker-CE (Community Edition) 24.x<br>
#Testado e homologado na Imagem do Docker Hub MySQL 9.1.x<br>

#utilizando a Imagem Oficial do Docker Hub do MySQL para criar nossa imagem personalizada
#Imagem do Docker Hub: https://hub.docker.com/_/mysql
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#from
#FROM (Create a new build stage from a base image)
FROM mysql:latest

#utilizando os Rótulos (labels) para identificar a imagem do MySQL personalizada
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#label
#Documentação do Docker-CE: https://docs.docker.com/extensions/extensions-sdk/extensions/labels/
#Documentação do OpenContainer: https://specs.opencontainers.org/image-spec/annotations/
#LABEL (Add metadata to an image)
LABEL org.opencontainers.image.authors="Robson Vaamonde"
LABEL org.opencontainers.image.vendor="Bora para Prática"
LABEL org.opencontainers.image.description="Servidor Database MySQL do Projeto VavaInfra Docker-CE"
LABEL org.opencontainers.image.url="https://github.com/vaamonde/ubuntu-2404"
LABEL org.opencontainers.image.source="https://github.com/vaamonde/vava-docker"
LABEL org.opencontainers.image.documentation="https://github.com/vaamonde/ubuntu-2404/blob/main/05-services/01-vava-infra.md"

#criando o diretório de trabalho padrão do MySQL para os banco de dados
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#workdir
#WORKDIR (Change working directory)
WORKDIR /var/lib/mysql/

#criando o ponto de montagem do volume da imagem padrão do MySQL
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#volume
#VOLUME (Create volume mounts)
VOLUME /var/lib/mysql/

#criando as variáveis de ambiente para a criação do novo usuário do MySQL
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#env
#ENV (Set environment variables)
ENV MYSQL_ROOT_PASSWORD=pti@2018
ENV MYSQL_USER=admin
ENV MYSQL_PASSWORD=pti@2018
ENV MYSQL_DATABASE=vavadb

#criando o usuário Admin e setando os privilégios de acesso remoto no MySQL
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#run
#RUN (Execute build commands)
#Documentação do MySQL Server: https://dev.mysql.com/doc/refman/8.4/en/mysql.html
#Documentação do MySQL Server: https://dev.mysql.com/doc/refman/8.4/en/mysql-command-options.html
#opção do comando mysql: -u --user (MySQL user name to use when connecting to server),
#-p --password (Password to use when connecting to server), -e --execute (Execute the
#statement and quit)
#Documentação do MySQL Server: https://dev.mysql.com/doc/refman/8.4/en/create-user.html
#Documentação do MySQL Server: https://dev.mysql.com/doc/refman/8.4/en/grant.html
#Documentação do MySQL Server: https://dev.mysql.com/doc/refman/8.4/en/flush.html
#RUN mysql -u root -p$MYSQL_ROOT_PASSWORD -e "CREATE USER '$MYSQL_USER'@'%' IDENTIFIED BY '$MYSQL_PASSWORD';"
#RUN mysql -u root -p$MYSQL_ROOT_PASSWORD -e "GRANT ALL PRIVILEGES ON *.* TO '$MYSQL_USER'@'%';"
#RUN mysql -u root -p$MYSQL_ROOT_PASSWORD -e "FLUSH PRIVILEGES;"

#expondo a porta de acesso remoto do MySQL no Dockerfile
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#expose
#EXPOSE (Describe which ports your application is listening on)
EXPOSE 3306

#especificando o comando padrão do MySQL na imagem do Dockerfile
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#cmd
#Documentação do NGINX: http://nginx.org/en/docs/switches.html
#CMD (Specify default commands)
CMD ["mysqld"]