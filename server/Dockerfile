FROM golang:1.14-alpine as builder

WORKDIR /go/src/app

# Copy module sources and download
COPY go.mod go.sum /go/src/app/
RUN go mod download

# Add binary sources and compile
COPY . /go/src/app
RUN CGO_ENABLED=0 go build -a -installsuffix cgo -ldflags '-w -s -extldflags "-static"' -o /go/bin/app

FROM gcr.io/distroless/static:latest
USER nonroot
COPY --from=builder --chown=nonroot /go/bin/app /app

ENTRYPOINT ["/app"]
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 CMD [ "/app", "-health" ]
