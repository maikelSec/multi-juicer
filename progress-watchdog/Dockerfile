FROM golang:1.13-alpine AS builder
RUN apk add git
WORKDIR /src
COPY go.mod go.sum ./
# cache deps before building and copying source so that we don't need to re-download as much
# and so that source changes don't invalidate our downloaded layer
RUN go mod download
COPY main.go main.go
ENV CGO_ENABLED 0
RUN go build
RUN chmod +x progress-watchdog

FROM alpine
RUN addgroup --system --gid 1001 app && adduser app --system --uid 1001 --ingroup app
COPY --from=builder --chown=app:app /src/progress-watchdog /home/app/progress-watchdog
USER 1001
CMD /home/app/progress-watchdog
