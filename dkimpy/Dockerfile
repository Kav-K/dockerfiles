FROM alpine:3.17.0@sha256:8914eb54f968791faf6a8638949e480fef81e697984fba772b3976835194c6d4
#FROM python:3.11.0-slim-bullseye@sha256:f8cc89f5e47347703ec0c2b755464d7db2fa16f255ab860c4b24ba6ef2402020
#FROM python:3.9-alpine

ARG BUILD_VERSION

#ENV PIP_DISABLE_PIP_VERSION_CHECK=1
ENV PIP_NO_CACHE_DIR=1

# requirements.txt now generated from Pipfile
# pipenv lock -r > requirements.txt
COPY requirements.txt . 

RUN \
	apk update && \
	apk upgrade -a && \
	build_pkgs="cargo build-base libffi-dev openssl-dev python3-dev" && \
  runtime_pkgs="ca-certificates openssl python3 py3-pip" && \
  apk --no-cache add --virtual build-dependencies ${build_pkgs} && \
  apk --no-cache add ${runtime_pkgs} && \
	python3 -m pip install --no-cache-dir --upgrade pip && \
	pip3 install --no-cache-dir --upgrade wheel && \
	pip3 install --no-cache-dir -r requirements.txt && \
	apk del build-dependencies && \
  rm -rf /var/cache/apk/* && \
  rm -rf /root/.cache/pip && \
  find /usr/lib/ -name '__pycache__' -print0 | xargs -0 -n1 rm -rf && \
  find /usr/lib/ -name '*.pyc' -print0 | xargs -0 -n1 rm -rf && \
  rm -rf /root/.cargo

#	apt-get purge -y python3-dev && \
#RUN pip install sslyze==${BUILD_VERSION}
#RUN pip3 install --no-cache-dir -r requirements.txt

LABEL org.opencontainers.image.authors="Jauder Ho <jauderho@users.noreply.github.com>"
LABEL org.opencontainers.image.url="https://github.com/jauderho/dockerfiles"
LABEL org.opencontainers.image.documentation="https://github.com/jauderho/dockerfiles"
LABEL org.opencontainers.image.source="https://github.com/jauderho/dockerfiles"
LABEL org.opencontainers.image.title="jauderho/dkimpy"
LABEL org.opencontainers.image.description="dkimpy is a library that implements DKIM (DomainKeys Identified Mail) email signing and verification"

CMD ["dkimverify"]
#CMD ["dknewkey"]
#CMD ["dkimsign"]
