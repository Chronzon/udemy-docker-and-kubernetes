FROM node:14-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install
COPY ./ ./

RUN npm run build

FROM nginx:1.28-alpine

COPY --from=builder /app/build /usr/share/nginx/html