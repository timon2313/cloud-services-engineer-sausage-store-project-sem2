FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:1.28.0-alpine
RUN apk update && apk upgrade && apk add --no-cache curl
RUN addgroup -S nginxgrp && adduser -S -G nginxgrp nginxusr
COPY --from=builder /app/dist/frontend /usr/share/nginx/html
COPY nginx.tmpl /etc/nginx/templates/default.conf.template
RUN \
  mkdir -p /run /var/cache/nginx /var/log/nginx && \
  chown -R nginxusr:nginxgrp /run /var/cache/nginx /var/log/nginx /usr/share/nginx/html /etc/nginx/templates
USER nginxusr
EXPOSE 80

HEALTHCHECK \
  --interval=30s \
  --timeout=5s \
  --start-period=10s \
  --retries=3 \
  CMD curl -f http://localhost:80 || exit 1

ENTRYPOINT ["nginx", "-g", "daemon off;"]