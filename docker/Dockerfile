# syntax=docker/dockerfile:1

FROM --platform=$BUILDPLATFORM golang:alpine AS build-env
ARG TARGETOS TARGETARCH
COPY . $GOPATH/src/github.com/majorcadevs/infping
WORKDIR $GOPATH/src/github.com/majorcadevs/infping
RUN apk add git --no-cache && \
    export GOOS=${TARGETOS} && \
     case ${TARGETARCH} in \
        amd64) export GOARCH=amd64 ;; \
        arm/v7) export GOARCH=arm; export GOARM=7 ;; \
        arm/v6) export GOARCH=arm; export GOARM=6 ;; \
        arm64) export GOARCH=arm64 ;; \
        *) echo Maybe unsupported arch ${TARGETARCH}; export GOARCH=${TARGETARCH} ;; \
    esac && \
    go get -v && \
    go build -o /infping -v

# final stage
FROM alpine
COPY --from=build-env /infping /
RUN apk add --no-cache ca-certificates fping
ENTRYPOINT ["/infping"]
