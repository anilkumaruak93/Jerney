FROM node:24-alpine AS build

WORKDIR /app

COPY package.json package-lock.json* ./

RUN npm ci && npm cache clean --force

COPY . .

RUN npm run build 

#--------production--------

FROM nginx:27-alpine AS production 

RUN rm -rf /etc/nginx/con.f/default.conf /usr/share/nginx/html/*

COPY nginx.conf /etc/nginx/con.f/default.conf

COPY --from=build /app/dist /usr/share/nginx/html/*

RUN chown -R nginx:nginx /usr/share/nginx/html && \
    chown -R nginx:nginx /var/cache/nginx && \
    chown -R nginx:nginx /var/log/nginx && \
    touch /var/run/nginx.pid && \
    chown -R nginx:nginx /var/run/nginx.pid

USER nginx

EXPOSE 80

CMD ["nginx", "-g", "daemon off"]