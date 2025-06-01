FROM nginx:stable-alpine

# Eliminamos el default.conf original
RUN rm /etc/nginx/conf.d/default.conf

# Copiamos el nuestro
COPY default.conf /etc/nginx/conf.d/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
