FROM node:24-alpine AS build

WORKDIR /app

COPY package.json package-lock.json* ./

RUN npm ci --only=production && npm cache clean --force 

COPY . .

RUN npm run build 

#----production ____

FROM node:24-alpine AS production

RUN addgroup -S appgroup && adduser -S appuser -G appgroup 

RUN apk --no--cache add dumb-init

WORKDIR /app

COPY --from=build /app/node_modules ./node_modules

COPY src/ ./src/ 

USER appuser

EXPOSE 5000

CMD ["node", "src/index.js"]