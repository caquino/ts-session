FROM ubuntu:16.04
MAINTAINER Cassiano Aquino "caquino@zendesk.com"
STOPSIGNAL SIGQUIT

ADD certs/nginx-repo.crt /etc/ssl/nginx/
ADD certs/nginx-repo.key /etc/ssl/nginx/

RUN echo 'debconf debconf/frontend select Noninteractive' \
| debconf-set-selections && \
cd /tmp && \
sha256sum --ignore-missing -c consul-template_0.19.5_SHA256SUMS && \
tar -zxvpf consul-template_0.19.5_linux_amd64.tgz -C /usr/bin && \
chmod +x /usr/bin/consul-template && \
apt-get update && \
apt-get -y upgrade && \
apt-get install -y -q wget apt-transport-https lsb-release ca-certificates --allow-unauthenticated && \
wget -q -O - http://nginx.org/keys/nginx_signing.key | apt-key add - && \
wget -q -O /etc/apt/apt.conf.d/90nginx https://cs.nginx.com/static/files/90nginx && \
printf "deb https://plus-pkgs.nginx.com/ubuntu `lsb_release -cs` nginx-plus\n" >/etc/apt/sources.list.d/nginx-plus.list && \
apt-get update && apt-get install -y nginx-plus nginx-plus-module-ndk nginx-plus-module-lua nginx-plus-module-njs lua-cjson && \
apt-get install -y jq curl tcpdump ngrep git vim && \
cd /opt && \
git clone https://github.com/bsm/lua-resty-http && \
ln -sf /dev/stdout /var/log/nginx/access.log && \
ln -sf /dev/stderr /var/log/nginx/error.log 

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
