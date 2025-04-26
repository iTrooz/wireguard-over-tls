FROM rust:alpine AS builder

RUN apk add --no-cache musl-dev

ARG INSTALL_CMD
ENV CARGO_INSTALL_ROOT=/install
RUN $INSTALL_CMD

FROM scratch
USER 1000
COPY --from=builder /install/bin/* /app.bin

ENTRYPOINT ["/app.bin"]
