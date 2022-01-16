ARG ALPINE_VERSION=3.15
FROM alpine:${ALPINE_VERSION}

RUN apk add --no-cache tshark

ENTRYPOINT [ "tshark" ]
CMD [ "--help" ]
