# Etapa 1: Preparación de archivos (Cumpliendo requisito multietapa)
FROM alpine AS builder
WORKDIR /app
COPY index.html app.js default.conf ./

# Etapa 2: Imagen minimalista de Producción
FROM nginx:alpine
# Limpia contenido por defecto
RUN rm -rf /usr/share/nginx/html/*
# Copia los archivos desde la etapa anterior
COPY --from=builder /app/index.html /app/app.js /usr/share/nginx/html/
COPY --from=builder /app/default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80