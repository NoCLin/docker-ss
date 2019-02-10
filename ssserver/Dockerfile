FROM python:3-alpine

RUN set -ex && \
    country=$(wget -qO- ipinfo.io/country) && \
    if [ $country == "CN" ]; then \
        pipmirrorarg="-i https://pypi.doubanio.com/simple/" && \
    fi && \
    pip install --no-cache-dir https://github.com/shadowsocks/shadowsocks/archive/master.zip $pipmirrorarg
ENTRYPOINT ["ssserver", "-c", "/etc/shadowsocks.json", "start", "-d"]
