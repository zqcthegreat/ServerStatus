FROM debian:latest as builder

MAINTAINER cppla https://cpp.la

WORKDIR /ServerStatus/server

RUN apt-get update && \
    apt-get -y install gcc g++ make git wget && \
    git clone https://github.com/zqcthegreat/ServerStatus-1.git

RUN make
RUN pwd && ls -a

# glibc env run 
FROM nginx:latest

RUN mkdir -p /ServerStatus/server/

COPY --from=builder /ServerStatus/server /ServerStatus/server/
COPY --from=builder /ServerStatus/web /usr/share/nginx/html/

EXPOSE 80 35601

CMD nohup sh -c 'wget -P /ServerStatus/server/ https://raw.githubusercontent.com/zqcthegreat/ServerStatus-1/master/autodeploy/config.json && /etc/init.d/nginx start && /ServerStatus/server/sergate --config=/ServerStatus/server/config.json --web-dir=/usr/share/nginx/html'
