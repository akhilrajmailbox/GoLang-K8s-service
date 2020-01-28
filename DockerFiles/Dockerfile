FROM golang:1.12.0 AS gobuild
MAINTAINER Akhil Raj <akhilrajmailbox@gmail.com>
RUN go get -u github.com/globalsign/mgo
WORKDIR /go/src/github.com/globalsign/mgo
RUN go build
COPY service.go /opt/data/
WORKDIR /opt/data/
RUN go clean -cache \
    && go build service.go

# This results in a single layer image
FROM ubuntu:16.04
MAINTAINER Akhil Raj <akhilrajmailbox@gmail.com>
ENV MONGO_URL mongo
COPY --from=gobuild /opt/data/service /opt/service
ADD entrypoint.sh /opt/entrypoint.sh
RUN chmod a+x /opt/service /opt/entrypoint.sh
ENTRYPOINT ["/opt/entrypoint.sh"]