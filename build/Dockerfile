FROM quay.io/app-sre/boilerplate:image-v2.3.2 AS builder
COPY go.mod /
COPY go.sum /
RUN go mod download
COPY . /osd-metrics-exporter
WORKDIR /osd-metrics-exporter
RUN make go-build

FROM registry.access.redhat.com/ubi8/ubi-minimal:latest
ENV OPERATOR=/usr/local/bin/osd-metrics-exporter \
    USER_UID=1001 \
    USER_NAME=osd-metrics-exporter
# install operator binary
COPY --from=builder /osd-metrics-exporter/build/_output/bin/osd-metrics-exporter ${OPERATOR}
COPY build/bin /usr/local/bin
RUN  /usr/local/bin/user_setup
ENTRYPOINT ["/usr/local/bin/entrypoint"]
USER ${USER_UID}
