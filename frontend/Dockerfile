FROM node:21-alpine AS builder

ARG NODE_ENV

ENV DOCKER_CONTENT_TRUST=1

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

ENV NODE_ENV=${NODE_ENV}
ENV VUE_APP_API_URL=/api
ENV NODE_OPTIONS=--openssl-legacy-provider

RUN npm run build

FROM nginxproxy/nginx-proxy:1.7.1-alpine

ARG NODE_ENV

ENV DOCKER_CONTENT_TRUST=1

ENV NODE_ENV=${NODE_ENV}

COPY nginx.tmpl /app/nginx.tmpl

COPY --from=builder --chown=appuser:appgroup /app/dist /usr/share/nginx/html

RUN apk update && \
    apk add --no-cache curl &&\
    addgroup -g 1001 -S appgroup && \
    adduser -u 1001 -S appuser -G root &&\
    chown -R appuser:root /var/cache/nginx && \
    chown -R appuser:root /var/log/nginx && \
    chown -R appuser:root /etc/nginx/conf.d && \
    touch /var/run/nginx.pid && \
    chown -R appuser:root /var/run/nginx.pid &&\
    chown -R appuser:root /etc/nginx &&\
    chown -R appuser:root /tmp

USER appuser

EXPOSE 80

# healthcheck
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1

ENTRYPOINT ["/app/docker-entrypoint.sh"]
CMD ["forego", "start", "-r"]