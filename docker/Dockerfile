ARG BASE_IMAGE_VERSION="v1.4.0"

FROM darthsim/imgproxy-base:${BASE_IMAGE_VERSION}
LABEL maintainer="Sergey Alexandrovich <darthsim@gmail.com>"

COPY . .
RUN go build -v -o /usr/local/bin/imgproxy

# ==================================================================================================
# Final image

FROM debian:bullseye-slim
LABEL maintainer="Sergey Alexandrovich <darthsim@gmail.com>"

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
  bash \
  ca-certificates \
  libsm6 \
  liblzma5 \
  libzstd1 \
  fontconfig \
  && rm -rf /var/lib/apt/lists/*

COPY --from=0 /usr/local/bin/imgproxy /usr/local/bin/
COPY --from=0 /usr/local/lib /usr/local/lib

COPY NOTICE /usr/local/share/doc/imgproxy/

ENV VIPS_WARNING=0
ENV MALLOC_ARENA_MAX=2
ENV LD_LIBRARY_PATH /usr/local/lib
ENV IMGPROXY_ALLOWED_SOURCES=https://s3-newsifier.ams3.digitaloceanspaces.com/,https://s3-newsifier.ams3.cdn.digitaloceanspaces.com/
ENV IMGPROXY_MAX_SRC_RESOLUTION=200
ENV IMGPROXY_SKIP_PROCESSING_FORMATS=gif
ENV IMGPROXY_TTL=31536000
CMD ["imgproxy"]

EXPOSE 8080
