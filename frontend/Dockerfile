FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --legacy-peer-deps
COPY . .
RUN npm run build -- --configuration=production --output-path=dist --output-hashing=all

FROM nginx:1.27
RUN rm -rf /usr/share/nginx/html/*
COPY --from=builder /app/dist/frontend/ /usr/share/nginx/html
EXPOSE 80
