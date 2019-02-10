FROM python:3-alpine

EXPOSE 1080 8118

RUN set -ex && \
    country=$(wget -qO- ipinfo.io/country) && \
    if [ $country == "CN" ]; then \
        pipmirrorarg="-i https://pypi.doubanio.com/simple/" && \
        sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories ; \
    fi && \
	apk --update add --no-cache privoxy curl && \
	echo 'listen-address 0.0.0.0:8118' > /etc/privoxy/config && \
	echo 'forward-socks5 / 127.0.0.1:1080 .' >> /etc/privoxy/config && \
	pip install --no-cache-dir https://github.com/shadowsocks/shadowsocks/archive/master.zip $pipmirrorarg

HEALTHCHECK --interval=60s --timeout=5s --retries=10 CMD curl -x http://127.0.0.1:8118 https://www.google.com/gen_204 || exit 1

CMD privoxy /etc/privoxy/config && nohup sslocal -v -b 0.0.0.0 -c /etc/ss-client.json > /var/log/shadowsocks-nohup.log 2>&1
