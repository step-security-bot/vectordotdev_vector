FROM docker.io/alpine:3.23@sha256:865b95f46d98cf867a156fe4a135ad3fe50d2056aa3f25ed31662dff6da4eb62 AS builder

WORKDIR /vector

COPY vector-*-unknown-linux-musl*.tar.gz ./
RUN tar -xvf vector-0*-"$(cat /etc/apk/arch)"-unknown-linux-musl*.tar.gz --strip-components=2

RUN mkdir -p /var/lib/vector

# distroless doesn't use static tags
# hadolint ignore=DL3007
FROM gcr.io/distroless/static:latest@sha256:cd64bec9cec257044ce3a8dd3620cf83b387920100332f2b041f19c4d2febf93

# https://github.com/opencontainers/image-spec/blob/main/annotations.md
LABEL org.opencontainers.image.url="https://vector.dev"
LABEL org.opencontainers.image.source="https://github.com/vectordotdev/vector"
LABEL org.opencontainers.image.documentation="https://vector.dev/docs"

COPY --from=builder /vector/bin/* /usr/local/bin/
COPY --from=builder /vector/config/vector.yaml /etc/vector/vector.yaml
COPY --from=builder /var/lib/vector /var/lib/vector

# Smoke test
RUN ["vector", "--version"]

ENTRYPOINT ["/usr/local/bin/vector"]
