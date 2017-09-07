FROM ruby:2.4.1-stretch

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN echo "deb http://ftp.debian.org/debian jessie-backports main" | tee "/etc/apt/sources.list.d/jesse-backports.list"
RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		ca-certificates	\
		jq \
		numactl \
		libssl1.0.0 \
		awscli \
	&& rm -rf /var/lib/apt/lists/*

ENV GPG_KEYS \
# pub   4096R/AAB2461C 2014-02-25 [expires: 2016-02-25]
#       Key fingerprint = DFFA 3DCF 326E 302C 4787  673A 01C4 E7FA AAB2 461C
# uid                  MongoDB 2.6 Release Signing Key <packaging@mongodb.com>
	DFFA3DCF326E302C4787673A01C4E7FAAAB2461C \
# pub   4096R/EA312927 2015-10-09 [expires: 2017-10-08]
#       Key fingerprint = 42F3 E95A 2C4F 0827 9C49  60AD D68F A50F EA31 2927
# uid                  MongoDB 3.2 Release Signing Key <packaging@mongodb.com>
	42F3E95A2C4F08279C4960ADD68FA50FEA312927
# https://docs.mongodb.com/manual/tutorial/verify-mongodb-packages/#download-then-import-the-key-file
RUN set -ex; \
	export GNUPGHOME="$(mktemp -d)"; \
	for key in $GPG_KEYS; do \
		gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
	done; \
	gpg --export $GPG_KEYS > /etc/apt/trusted.gpg.d/mongodb.gpg; \
	apt-key list

# Allow build-time overrides (eg. to build image with MongoDB Enterprise version)
# Options for MONGO_PACKAGE: mongodb-org OR mongodb-enterprise
# Options for MONGO_REPO: repo.mongodb.org OR repo.mongodb.com
# Example: docker build --build-arg MONGO_PACKAGE=mongodb-enterprise --build-arg MONGO_REPO=repo.mongodb.com .
ARG MONGO_PACKAGE=mongodb-org
ARG MONGO_REPO=repo.mongodb.org
ENV MONGO_PACKAGE=${MONGO_PACKAGE} MONGO_REPO=${MONGO_REPO}

ENV MONGO_MAJOR 3.2
ENV MONGO_VERSION 3.2.14

RUN echo "deb http://$MONGO_REPO/apt/debian jessie/${MONGO_PACKAGE%-unstable}/$MONGO_MAJOR main" | tee "/etc/apt/sources.list.d/${MONGO_PACKAGE%-unstable}.list"

RUN set -x \
	&& apt-get update \
	&& apt-get install -y \
		${MONGO_PACKAGE}-shell=$MONGO_VERSION \
		${MONGO_PACKAGE}-tools=$MONGO_VERSION \
	&& rm -rf /var/lib/apt/lists/* \
