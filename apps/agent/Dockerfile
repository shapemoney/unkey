FROM golang:1.22-alpine as builder



WORKDIR /go/src/github.com/unkeyed/unkey/apps/agent
COPY go.sum go.mod ./
RUN go mod download

COPY . .
ARG VERSION
RUN go build -o bin/unkey -ldflags "-X 'github.com/unkeyed/unkey/apps/agent/pkg/version.Version=${VERSION}'"  ./cmd/main.go

FROM golang:1.22-alpine
WORKDIR  /usr/local/bin
COPY --from=builder /go/src/github.com/unkeyed/unkey/apps/agent/bin/unkey .
COPY --from=builder /go/src/github.com/unkeyed/unkey/apps/agent/config.production.json .
COPY --from=builder /go/src/github.com/unkeyed/unkey/apps/agent/config.gossip.json .
COPY --from=builder /go/src/github.com/unkeyed/unkey/apps/agent/config.staging.json .
COPY --from=builder /go/src/github.com/unkeyed/unkey/apps/agent/config.local.json .
COPY --from=builder /go/src/github.com/unkeyed/unkey/apps/agent/config.fly.json .
COPY --from=builder /go/src/github.com/unkeyed/unkey/apps/agent/config.docker.json .

CMD [ "/usr/local/bin/unkey", "agent"]


