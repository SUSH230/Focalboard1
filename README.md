# Build stage
FROM node:16 AS build
WORKDIR /opt/focalboard
COPY . .
RUN cd webapp && npm install && npm run build

# Server stage
FROM golang:1.19 AS server
WORKDIR /opt/focalboard
COPY --from=build /opt/focalboard /opt/focalboard
RUN cd ./server && go build -o ./bin/focalboard-server

# Final image
FROM debian:bullseye-slim
WORKDIR /opt/focalboard
COPY --from=server /opt/focalboard /opt/focalboard
EXPOSE 8000
CMD ["./bin/focalboard-server", "--config", "./config.json"]
