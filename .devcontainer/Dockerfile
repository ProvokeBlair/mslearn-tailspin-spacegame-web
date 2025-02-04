# Use the official .NET runtime image
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app

# Log statement: Print the contents of the artifact folder before copying
RUN echo "Checking if artifact exists..." && ls -lah /Drop/release/

# Copy the artifact from the pipeline
COPY Drop/release/ /app/

# Log statement: Validate the copied files
RUN echo "Contents of /app after copying:" && ls -lah /app

# Set the entry point
CMD ["dotnet", "/app/YourApplication.dll"]
