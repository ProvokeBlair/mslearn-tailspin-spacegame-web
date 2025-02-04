# Base Image (Ensure it matches your runtime)
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app

# Install Node.js (if needed for frontend tasks)
ARG NODE_VERSION="16"
RUN if [ "${NODE_VERSION}" != "none" ]; then \
      apt-get update && apt-get install -y curl && \
      curl -fsSL https://deb.nodesource.com/setup_16.x | bash - && \
      apt-get install -y nodejs && \
      npm install --global gulp-cli; \
    fi

# Copy the artifact from the pipeline into the container
# The pipeline should place the built application into the "artifact" folder
COPY artifact /app

# Set permissions (if needed)
RUN chmod -R 755 /app

# Expose the necessary port (adjust based on your application)
EXPOSE 80

# Define the entry point to run the application
CMD ["dotnet", "/app/YourApplication.dll"]
