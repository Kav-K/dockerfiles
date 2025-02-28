FROM golang:1.19.4-alpine3.17@sha256:a9b24b67dc83b3383d22a14941c2b2b2ca6a103d805cac6820fd1355943beaf1 AS build

WORKDIR /go/src/github.com/openrdap/rdap/

ARG BUILD_VERSION
ARG ARCHIVE_URL=https://github.com/openrdap/rdap/archive/
ARG GIT_URL=https://github.com/jauderho/rdap.git

ENV CGO_ENABLED 0

RUN test -n "${BUILD_VERSION}" \
	&& apk update \
	&& apk add --no-cache git ca-certificates \ 
	&& git clone --depth 1 ${GIT_URL} /go/src/github.com/openrdap/rdap \
	&& cd /go/src/github.com/openrdap/rdap \
	&& go get -u all \
	&& go build -o rdap -v -trimpath -ldflags="-s -w" ./cmd/rdap

WORKDIR /config


# ----------------------------------------------------------------------------


FROM scratch
#FROM alpine:3.17.0@sha256:8914eb54f968791faf6a8638949e480fef81e697984fba772b3976835194c6d4

LABEL org.opencontainers.image.authors="Jauder Ho <jauderho@users.noreply.github.com>"
LABEL org.opencontainers.image.url="https://github.com/jauderho/dockerfiles"
LABEL org.opencontainers.image.documentation="https://github.com/jauderho/dockerfiles"
LABEL org.opencontainers.image.source="https://github.com/jauderho/dockerfiles"
LABEL org.opencontainers.image.title="jauderho/rdap"
LABEL org.opencontainers.image.description="OpenRDAP is an command line RDAP client implementation in Go"

COPY --from=build /etc/ssl/certs /etc/ssl/certs

COPY --from=build /go/src/github.com/openrdap/rdap/rdap /usr/local/bin/rdap
COPY --from=build /config /config

ENTRYPOINT ["/usr/local/bin/rdap"]

