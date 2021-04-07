FROM nginx:latest

COPY hw.conf /etc/nginx/conf.d/default.conf
COPY homework.html /app/homework.html
RUN chmod -R go+rw /var/cache/nginx && \
    chmod -R go+rw /var/run

EXPOSE 8000

CMD ["nginx", "-g", "daemon off;"]

USER 1001
