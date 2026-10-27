FROM --platform=$BUILDPLATFORM dhi.io/golang:1.26-debian13-dev AS build

WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .

ARG TARGETOS
ARG TARGETARCH
RUN CGO_ENABLED=0 GOOS=$TARGETOS GOARCH=$TARGETARCH \
    go build -trimpath -ldflags="-s -w" -o /out/votes .

FROM scratch

COPY --from=build /out/votes /votes

USER 65532:65532
EXPOSE 8081
ENTRYPOINT ["/votes"]
