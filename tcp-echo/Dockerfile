# First stage: build the executable.
FROM golang:1.11 as builder
WORKDIR /go/src/github.com/rekzi/tcp-echo/
COPY . .
# CGO_ENABLED=0 to build a statically-linked executable
# GOFLAGS=-mod=vendor to force `go build` to look into the `/vendor` folder.
ENV CGO_ENABLED=0 GOFLAGS=-mod=vendor
RUN go build -installsuffix 'static' -o tcp-echo main.go

# Final stage: the running container.
FROM alpine:3.9 as final
RUN apk add --update --no-cache ca-certificates
WORKDIR /bin/
# Import the compiled executable from the first stage.
COPY --from=builder /go/src/github.com/rekzi/tcp-echo/tcp-echo .
EXPOSE 9000
ENTRYPOINT [ "/bin/tcp-echo" ]
