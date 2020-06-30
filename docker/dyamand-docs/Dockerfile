FROM nginx:stable-alpine
EXPOSE 8080
## Copy our default nginx config
COPY nginx/default.conf /etc/nginx/conf.d/

## Change paths so that nginx can be run as non-root
RUN sed -i '/^user/d' /etc/nginx/nginx.conf
RUN sed -i 's/\/var\/run\/nginx.pid/\/tmp\/nginx.pid/' /etc/nginx/nginx.conf
RUN sed -i -e '/http {/r paths.txt' /etc/nginx/nginx.conf

COPY build/mkdocs/unspecified /app
RUN chmod -R 777 /app

ENTRYPOINT ["nginx","-g","daemon off;"]