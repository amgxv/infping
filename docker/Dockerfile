# syntax=docker/dockerfile:1

FROM --platform=$BUILDPLATFORM golang:alpine AS build-env
ARG TARGETOS TARGETARCH
COPY . $GOPATH/src/github.com/majorcadevs/infping
WORKDIR $GOPATH/src/github.com/majorcadevs/infping
RUN apk add git --no-cache && \
    export GOOS=${TARGETOS} && \
    export GOARCH=${TARGETOS} && \
    go get -v && \
    go build -o /infping -v

# final stage
FROM alpine
COPY --from=build-env /infping /
RUN apk add --no-cache ca-certificates fping
ENTRYPOINT ["/infping"]
