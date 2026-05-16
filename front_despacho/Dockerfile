# ─── STAGE 1: Build ──────────────────────────────────────────
FROM node:20-alpine AS builder
WORKDIR /app

# Instalar dependencias primero (mejor cache de capas)
COPY package*.json ./
RUN npm ci --silent

# Copiar fuentes y construir
COPY . .
RUN npm run build

# ─── STAGE 2: Serve con Nginx ─────────────────────────────────
FROM nginx:alpine

# Usuario no root
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copiar build desde stage anterior
COPY --from=builder /app/dist /usr/share/nginx/html

# Configuración Nginx para React Router (SPA)
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
