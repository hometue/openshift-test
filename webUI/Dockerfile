# syntax=docker/dockerfile:1

FROM node:19
WORKDIR /src
COPY ./ ./
RUN npm install
CMD npm run dev --host
EXPOSE 3000