#Autor: Robson Vaamonde<br>
#Procedimentos em TI: http://procedimentosemti.com.br<br>
#Bora para Prática: http://boraparapratica.com.br<br>
#Robson Vaamonde: http://vaamonde.com.br<br>
#Facebook Procedimentos em TI: https://www.facebook.com/ProcedimentosEmTi<br>
#Facebook Bora para Prática: https://www.facebook.com/BoraParaPratica<br>
#Instagram Procedimentos em TI: https://www.instagram.com/procedimentoem<br>
#YouTUBE Bora Para Prática: https://www.youtube.com/boraparapratica<br>
#Data de criação: 16/11/2024<br>
#Data de atualização: 16/11/2024<br>
#Versão: 0.01<br>
#Testado e homologado no GNU/Linux Ubuntu Server 24.04.x LTS<br>
#Testado e homologado no Docker-CE (Community Edition) 24.x<br>
#Testado e homologado na Imagem do Docker Hub NGINX 1.27.x<br>

#utilizando a Imagem Oficial do Docker Hub do NGINX para criar nessa imagem personalizada
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#from
#FROM (Create a new build stage from a base image)
FROM nginx:latest

#utilizando os Rótulos (labels) para identificar a imagem do NGINX personalizada
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#label
#Documentação do Docker-CE: https://docs.docker.com/extensions/extensions-sdk/extensions/labels/
#Documentação do OpenContainer: https://specs.opencontainers.org/image-spec/annotations/
#LABEL (Add metadata to an image)
LABEL org.opencontainers.image.authors="Robson Vaamonde"
LABEL org.opencontainers.image.vendor="Bora para Prática"
LABEL org.opencontainers.image.description="Servidor Web NGINX do Projeto VavaInfra Docker-CE"
LABEL org.opencontainers.image.url="https://github.com/vaamonde/ubuntu-2404"
LABEL org.opencontainers.image.source="https://github.com/vaamonde/vava-docker"
LABEL org.opencontainers.image.documentation="https://github.com/vaamonde/ubuntu-2404/blob/main/05-services/01-vava-infra.md"

#criando argumentos na imagem padrão do NGINX quando utilizar o comando apt
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#arg
#ARG (Use build-time variables)
#Documentação do Debconf: https://manpages.debian.org/jessie/debconf-doc/debconf.7.en.html
#opção da variável DEBIAN_FRONTEND: noninteractive (This is the anti-frontend. It never 
#interacts with you at all, and makes the default answers be used for all questions.)
ARG DEBIAN_FRONTEND=noninteractive

#utilizar o comando apt para atualizar a imagem do NGINX
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#run
#RUN (Execute build commands)
#opção do comando apt-get: update (Resynchronize the package index files from their sources), 
#upgrade (Install the newest versions of all packages currently installed on the system), 
#-y (yes confirmation)
#opção do operador lógico &&: E lógico (AND)
RUN apt-get update && apt-get upgrade -y

#instalando os comandos vim, git e wget na imagem do NGINX
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#run
#RUN (Execute build commands)
#opção do comando apt-get: install (install is followed by one or more package names), -y 
#(yes confirmation)
RUN apt-get install vim git wget -y

#copiando o arquivo de configuração do site padrão do NGINX do diretório de projeto
#para o diretório de configuração padrão do NGINX
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#copy
#Documentação do NGINX: http://nginx.org/en/docs/beginners_guide.html
#COPY: (Copy files and directories)
COPY default /etc/nginx/sites-available/default

#criando o diretório de trabalho padrão do NGINX para a hospedagem de sites
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#workdir
#WORKDIR (Change working directory)
WORKDIR /var/www/html/

#criando o ponto de montagem do volume da imagem padrão do NGINX
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#volume
#VOLUME (Create volume mounts)
VOLUME /var/www/html/

#copiando o arquivo de html padrão para o diretório Document Root do NGINX
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#copy
#Documentação do NGINX: http://nginx.org/en/docs/beginners_guide.html
#COPY: (Copy files and directories)
COPY index.html ./

#clonando os sites desenvolvidos em HTML do Github para o diretório de Document Root do NGINX
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#run
#RUN (Execute build commands)
#Documentação do Git: https://git-scm.com/docs/git-clone
#opção do comando git: clone (Clone a repository into a new directory), --depth=1 (Create a 
#shallow clone with a history truncated to the specified number of commits)
RUN git clone --depth=1 https://github.com/andradejao/outlawgames ./
RUN git clone --depth=1 https://github.com/camilajodai/protectpetz ./

#alterando as permissões de Dono e Grupo dos diretórios dos sites clonados do Github
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#run
#RUN (Execute build commands)
#opção do comando chown: -R (recursive), -f (silent), -v (verbose), www-data (user), www-data (group)
RUN chown -Rfv www-data.www-data ./outlawgames/
RUN chown -Rfv www-data.www-data ./protectpetz/

#alterando as permissões de Leitura e Gravação dos diretórios dos sites clonados do Github
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#run
#RUN (Execute build commands)
#opção do comando find: . (path), -type d (directory), , type f (file), -exec (execute command)
#opção do comando chmod: -v (verbose), 2775 (Set-GID=2, Dono=RWX, Grupo=RWS, Outros=R-X)
#opção do comando chmod: -v (verbose), 2664 (Set-GID=2, Dono=RW-, Grupo=RWS, Outros=R--)
#opção do comando {} \;: executa comandos em lote e aplica as permissões nos arquivo/diretório em loop
RUN find ./outlawgames/. -type d -exec chmod -v 2775 {} \;
RUN find ./outlawgames/. -type f -exec chmod -v 2664 {} \;
RUN find ./protectpetz/. -type d -exec chmod -v 2775 {} \;
RUN find ./protectpetz/. -type f -exec chmod -v 2664 {} \;

#alterando as opções globais do git nos diretórios clonados do Github
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#run
#RUN (Execute build commands)
#Documentação do Git: https://git-scm.com/docs/git-config
#opções do comando git: config (Get and set repository or global options), --global 
#(write to global ~/.gitconfig), --add (Adds a new line to the option without altering 
#any existing values), safe.directory (These config entries specify Git-tracked directories 
#that are considered safe even if they are owned by someone other than the current user)
RUN git config --global --add safe.directory ./outlawgames/
RUN git config --global --add safe.directory ./protectpetz/

#expondo a porta de acesso ao protocolo HTTP do NGINX no Dockerfile
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#expose
#EXPOSE (Describe which ports your application is listening on)
EXPOSE 80

#especificando qual o executável do NGINX na imagem do Dockerfile
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#entrypoint
#ENTRYPOINT (Specify default executable.)
ENTRYPOINT ["/usr/sbin/nginx"]

#especificando os comandos padrão do NGINX na imagem do Dockerfile
#Documentação do Docker-CE: https://docs.docker.com/reference/dockerfile/#cmd
#Documentação do NGINX: http://nginx.org/en/docs/switches.html
#CMD (Specify default commands)
#opções do comando nginx: -g (directives), daemon off (Determines whether nginx should 
#become a daemon)
CMD ["-g", "daemon off;"]