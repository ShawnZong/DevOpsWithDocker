# Original Dockerfiles
## frontend, size: 418MB
``` dockerfile
FROM node:16-alpine
EXPOSE 5000
WORKDIR /usr/src/app
RUN npm install -g serve
COPY example-frontend .
RUN npm install
RUN REACT_APP_BACKEND_URL=http://localhost:8080 npm run build
RUN chmod a+x /usr/src/app/build
RUN adduser -D appuser
USER appuser
CMD ["serve","-s","-l","5000","build"]

```
## backend, size: 1.08GB
``` dockerfile
FROM golang:1.16
WORKDIR /usr/src/app
COPY ./example-backend .
RUN go build
RUN chmod a+x /usr/src/app/server
ENV PORT=8080
ENV REQUEST_ORIGIN=http://localhost
EXPOSE 8080
RUN useradd -m appuser
USER appuser
CMD ["./server"]

```
# Optimized dockerfiles
## frontend, size: 418MB
```dockerfile
FROM node:16-alpine
EXPOSE 5000
WORKDIR /usr/src/app
COPY example-frontend .
RUN npm install && \
    npm install -g serve && \
    REACT_APP_BACKEND_URL=http://localhost:8080 npm run build && \
    chmod a+x /usr/src/app/build && \
    adduser -D appuser && \
    rm -rf /var/lib/apt/lists/* && \
    find . \! -name 'build' -delete
USER appuser
CMD ["serve","-s","-l","5000","build"]
```
## backend, size: 1.07GB
```dockerfile
FROM golang:1.16
WORKDIR /usr/src/app
COPY ./example-backend .
RUN go build && \
    chmod a+x /usr/src/app/server && \
    useradd -m appuser &&\
    rm -rf /var/lib/apt/lists/* && \
    find . \! -name 'server' -delete
ENV PORT=8080
ENV REQUEST_ORIGIN=http://localhost
EXPOSE 8080
USER appuser
CMD ["./server"]
```