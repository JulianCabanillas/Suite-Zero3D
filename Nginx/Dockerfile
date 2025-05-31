# Usamos una imagen estable y oficial de NGINX
FROM nginx:stable-alpine

# Eliminamos confi inicial para evitar problemas:
RUN rm /etc/nginx/conf.d/default.conf

# Y pasamos a copiar nuestro confi inicial:
COPY default.conf /etc/nginx/conf.d/

# Exponemos el puerto donde va a funcionar Nginx:
EXPOSE 80

# Por ultimo arrancamos nuestro servicio:
CMD ["nginx", "-g", "daemon off;"]