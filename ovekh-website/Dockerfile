FROM node:14.16-alpine3.13 AS typescript_builder

WORKDIR /home/node/app

COPY tsconfig.json ./
COPY ovekh.ts ./

RUN npm install typescript@4.4.2 -g
RUN tsc

FROM golang:1.15 as go_builder

WORKDIR /go/src/app

COPY main.go ./
COPY go.mod ./

# Install the dependencies for the project
RUN go mod download

RUN CGO_ENABLED=0 GOOS=linux go build -o ovekh-website .

FROM alpine:3.13

COPY --from=typescript_builder /home/node/app/ovekh.js ./
COPY --from=typescript_builder /home/node/app/ovekh.js.map ./
COPY --from=typescript_builder /home/node/app/ovekh.ts ./

COPY --from=go_builder /go/src/app/ovekh-website ./

COPY translations/ ./translations

COPY styles.css ./

EXPOSE 8080

CMD ["./ovekh-website"]
