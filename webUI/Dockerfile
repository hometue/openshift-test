# syntax=docker/dockerfile:1

FROM node:19 as build
WORKDIR /src
COPY ./ ./
RUN npm install
RUN npm run build

FROM nginxinc/nginx-unprivileged:1.23
COPY --from=build /src/dist /usr/share/nginx/html
COPY ./default.conf /etc/nginx/conf.d/default.conf
CMD ["nginx", "-g", "daemon off;"]
EXPOSE 8080