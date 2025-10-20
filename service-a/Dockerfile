FROM golang:1.23.5 AS builder
WORKDIR /app
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o service-a cmd/api/main.go

FROM scratch
COPY --from=builder /app/service-a .
CMD ["./service-a"]