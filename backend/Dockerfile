FROM golang:1.17-alpine AS builder

WORKDIR /app

ENV DOCKER_CONTENT_TRUST=1

COPY . .

RUN apk add git &&\ 
    adduser -D appuser &&\
    go mod download &&\
    go build -o api ./cmd/api

FROM alpine:3.21

WORKDIR /app

ENV DOCKER_CONTENT_TRUST=1

COPY --from=builder /app/api .

RUN addgroup -g 1001 -S appgroup &&\
    adduser -u 1001 -S appuser -G appgroup &&\
    chmod +x /app/api && apk add curl

USER appuser

EXPOSE 8081

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8081/health || exit 1

CMD ["/app/api"]