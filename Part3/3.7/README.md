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
# Optimized Dockerfile, image size: 120MB
```dockerfile
FROM node:16-alpine as build-stage
WORKDIR /usr/src/app
COPY . .
RUN npm install && npm run build

FROM node:16-alpine
WORKDIR /usr/src/app
COPY --from=build-stage /usr/src/app/build /usr/src/app/build
RUN npm install -g serve && adduser -D appuser
USER appuser
EXPOSE 3000
CMD ["serve","-s","-l","3000","build"]
```