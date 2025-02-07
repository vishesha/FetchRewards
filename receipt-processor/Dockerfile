# Use the official Golang image to create a build artifact.
FROM golang:1.19-alpine as builder

# Set the working directory inside the container.
WORKDIR /app

# Copy the Go module files.
COPY go.mod .
COPY go.sum .

# Download the dependencies.
RUN go mod download

# Copy the source code into the container.
COPY . .

# Build the application.
RUN CGO_ENABLED=0 GOOS=linux go build -o receipt-processor .

# Use a minimal alpine image for the final stage.
FROM alpine:latest

# Set the working directory.
WORKDIR /root/

# Copy the binary from the builder stage.
COPY --from=builder /app/receipt-processor .

# Expose the application on port 8080.
EXPOSE 8080

# Run the application.
CMD ["./receipt-processor"]