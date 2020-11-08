FROM openfaas/of-watchdog:0.8.1 as watchdog

COPY nginx.conf /etc/nginx/nginx.conf
COPY static /home/www-data/app

FROM nginx:1.19

RUN mkdir -p /home/app

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

COPY --from=watchdog /etc/nginx/nginx.conf /etc/nginx/nginx.conf
COPY --from=watchdog /home/www-data/app /home/www-data/app
# Add non root user

WORKDIR /home/app

ENV fprocess="nginx"
ENV mode="http"
ENV upstream_url="http://127.0.0.1:8201"
ENV write_debug="true"

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]