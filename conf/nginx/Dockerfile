FROM nginx:stable

RUN apt-get update && apt-get install -y --no-install-recommends curl \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /tmp/nginx && mkdir -p /tmp/nginx/cache && mkdir -p /tmp/nginx/cache \
	&& usermod -u 1000 -d /data -s /bin/bash www-data \
	&& mkdir /data && chmod -R 775 /data && find /data -type d -exec chmod 775 {} \;

COPY conf /etc/nginx

WORKDIR /data
CMD ["nginx"]
