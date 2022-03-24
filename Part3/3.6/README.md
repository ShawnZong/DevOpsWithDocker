# Original Dockerfile
## frontend, size:
```dockerfile
FROM node:16
EXPOSE 5000
WORKDIR /usr/src/app
COPY example-frontend .
RUN npm install && \
    npm install -g serve && \
    REACT_APP_BACKEND_URL=http://localhost:8080 npm run build && \
    chmod a+x /usr/src/app/build && \
    adduser -D appuser && \
    rm -rf /var/lib/apt/lists/*
USER appuser
CMD ["serve","-s","-l","5000","build"]
```
## backend, size: 1.07GB
```dockerfile
FROM golang:1.18
WORKDIR /usr/src/app
COPY ./example-backend .
RUN go build && \
    chmod a+x /usr/src/app/server && \
    useradd -m appuser &&\
    rm -rf /var/lib/apt/lists/*
ENV PORT=8080
ENV REQUEST_ORIGIN=http://localhost
EXPOSE 8080
USER appuser
CMD ["./server"]
```

# Optimized Dockerfile
## frontend, size:
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
    rm -rf /var/lib/apt/lists/*
USER appuser
CMD ["serve","-s","-l","5000","build"]
```

##backend, size:
```dockerfile
FROM golang:1-alpine
WORKDIR /usr/src/app
COPY ./example-backend .
RUN go build && \
    chmod a+x /usr/src/app/server && \
    useradd -m appuser &&\
    rm -rf /var/lib/apt/lists/*
ENV PORT=8080
ENV REQUEST_ORIGIN=http://localhost
EXPOSE 8080
USER appuser
CMD ["./server"]
```