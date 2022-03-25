project github link: https://github.com/ShawnZong/unicafe
# Original Dockerfile, image size: 386MB
```dockerfile
FROM node:16-alpine
COPY package* ./
RUN npm install
RUN npm install -g serve
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["serve","-s","-l","3000","build"]
```
# Optimized Dockerfile, image size: 680KB
```dockerfile
FROM node:16-alpine as build-stage
WORKDIR /usr/src/app
COPY . .
RUN npm install && npm run build

FROM lipanski/docker-static-website:latest
COPY --from=build-stage /usr/src/app/build /home/static/build
EXPOSE 3000
CMD ["/thttpd", "-D", "-h", "0.0.0.0", "-p", "3000", "-d", "/home/static/build", "-u", "static", "-l", "-", "-M", "60"]
```