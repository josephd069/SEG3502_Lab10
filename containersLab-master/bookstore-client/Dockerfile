# Construction de l'application Angular
FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN npm install --force

COPY . .

RUN npm run build -- --configuration production

# Configuration du serveur Web Nginx
FROM nginx:alpine AS image

RUN rm -rf /usr/share/nginx/html/*

RUN apk add --no-cache gettext

COPY --from=build /app/dist/book-store/browser /usr/share/nginx/html

COPY --from=build /app/src/assets/env.template.json /usr/share/nginx/html/assets/env.template.json

# Configuration du routage Angular
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Configuration du script de démarrage
COPY entrypoint.sh /entrypoint.sh

RUN chmod +x /entrypoint.sh

EXPOSE 80

ENTRYPOINT ["/entrypoint.sh"]