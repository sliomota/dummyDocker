FROM node:lts AS builder

WORKDIR /app

COPY ./package.json ./

RUN npm i

#-----------------------------------------------------------------------------

FROM node:lts-alpine AS serve

WORKDIR /app

COPY --from=builder /app/node_modules ./node_modules

COPY ./server.js .

EXPOSE 3000

CMD [ "node","server.js"  ]