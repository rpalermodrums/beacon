FROM node:18-bullseye-slim AS builder

WORKDIR /app

COPY package.json yarn.lock .yarnrc.yml .pnp* ./
COPY .yarn ./.yarn

RUN yarn set version berry
RUN yarn --immutable

COPY src ./src
COPY vite.config.ts index.html tsconfig.json .env* ./

RUN yarn build

FROM nginx:stable-alpine

COPY --from=builder /app/dist /usr/share/nginx/html/
COPY nginx/default.conf /etc/nginx/conf.d/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
