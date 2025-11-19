FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app

# ##Use Go build cache for dependencies
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build

# Copy
COPY go.mod go.sum ./

# Download
RUN go mod download

# Copy the rest of the source code
COPY . .

RUN go mod download

RUN go build -o /go/bin/product-catalog ./

FROM alpine AS release

WORKDIR /usr/src/app

COPY ./products ./products
COPY --from=builder /go/bin/product-catalog ./

ENV PRODUCT_CATALOG_PORT 8088
EXPOSE ${PRODUCT_CATALOG_PORT}
ENTRYPOINT ["./product-catalog"]

