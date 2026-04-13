# === ЭТАП 1: Сборка ===
FROM node:16-alpine AS build

WORKDIR /app

# Копируем только package.json (без pnpm-lock.yaml)
COPY package*.json ./

# Устанавливаем зависимости через npm
RUN npm ci 

# Копируем исходный код
COPY . .

# Собираем проект
RUN npm run build


# === ЭТАП 2: Production ===
FROM node:16-alpine

WORKDIR /app

RUN npm install -g pm2

COPY --from=build /app/dist ./dist
COPY --from=build /app/package*.json ./
COPY ecosystem.config.js ./

RUN npm ci --omit=dev

EXPOSE 4000

CMD ["pm2-runtime", "ecosystem.config.js"]