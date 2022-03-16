FROM golang:1.16
WORKDIR /usr/src/app
ENV PORT=8080
EXPOSE 8080
COPY ./example-backend .
RUN go build
CMD ["./server"]
