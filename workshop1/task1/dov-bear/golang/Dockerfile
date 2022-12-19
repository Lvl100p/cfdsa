FROM arm64v8/golang:1.19

# Create directory /app and cd into it
WORKDIR /app

# pre-copy/cache go.mod for pre-downloading dependencies and only redownloading them in subsequent builds if they change
COPY go.mod go.sum ./
RUN go mod download && go mod verify

COPY . .
RUN go build -v -o /usr/local/bin/app ./...

ENV PORT=5000
ENV INSTANCE_NAME=hello
ENV INSTANCE_HASH=world

# Healthcheck
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
    CMD curl -s http://localhost:${PORT}/healthz || exit 1

# Port that application binds to
EXPOSE ${PORT}

# Start app
CMD ["app"]

# For multi-container build: see below
# FROM golang:1.19-alpine3.17 AS builder

# WORKDIR /app

# ADD go.mod .
# ADD go.sum .
# ADD main.go .

# RUN go build -o main main.go

# # Runtime container
# FROM alpine:3.17

# WORKDIR /app

# COPY --from=builder /app/main main
# COPY public public
# COPY templates templates

# ENV PORT=3000 INSTANCE_NAME="dov bear golang"

# HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
#     CMD curl http://localhost:${PORT}/healthz || exit 1

# EXPOSE ${PORT}

# ENTRYPOINT [ "/app/main" ]