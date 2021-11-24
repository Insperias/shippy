FROM golang:alpine as builder

RUN apk update && apk upgrade && \
    apk add --no-cache git

RUN mkdir /app
WORKDIR /app

ENV GO111MODULE=on

COPY . .

RUN go mod download
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -installsuffix cgo -o shippy-service-consignment

# Run container
FROM alpine:latest

RUN apk add --no-cache ca-certificates

RUN mkdir /app
WORKDIR /app
COPY --from=builder /app/shippy-service-consignment .

CMD ["./shippy-service-consignment"]