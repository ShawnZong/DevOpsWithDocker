# Original Dockerfile
## frontend, size: 1.21GB
```dockerfile
FROM node:16
EXPOSE 5000
WORKDIR /usr/src/app
COPY example-frontend .
RUN npm install && \
    npm install -g serve && \
    REACT_APP_BACKEND_URL=http://localhost:8080 npm run build
CMD ["serve","-s","-l","5000","build"]
```
## backend, size: 1.12GB
```dockerfile
FROM golang:1.18
WORKDIR /usr/src/app
COPY ./example-backend .
RUN go build
ENV PORT=8080
ENV REQUEST_ORIGIN=http://localhost
EXPOSE 8080
CMD ["./server"]
```

# Optimized Dockerfile
## frontend, size: 418MB
```dockerfile
FROM node:16-alpine
EXPOSE 5000
WORKDIR /usr/src/app
COPY example-frontend .
RUN npm install && \
    npm install -g serve && \
    REACT_APP_BACKEND_URL=http://localhost:8080 npm run build
CMD ["serve","-s","-l","5000","build"]
```
## backend, size: 484MB
```dockerfile
FROM golang:1-alpine
WORKDIR /usr/src/app
COPY ./example-backend .
RUN go build
ENV PORT=8080
ENV REQUEST_ORIGIN=http://localhost
EXPOSE 8080
CMD ["./server"]
```