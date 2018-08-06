
FROM golang:1.8

WORKDIR /app
# Set an env var that matches your github repo name, replace arashsari/dockergo here with your repo name
ENV SRC_DIR=/go/src/github.com/arashsari/dockergo/
# # Add the source code:
ADD . $SRC_DIR
# Build it:

RUN cd $SRC_DIR; go build -o myapp; cp myapp /app/
ENTRYPOINT ["./myapp"]