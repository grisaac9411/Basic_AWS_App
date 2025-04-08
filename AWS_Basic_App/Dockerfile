# Use the official .NET SDK image to build the app
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

# Set the working directory in the container
WORKDIR /src

# Copy the .csproj file and restore any dependencies (via 'dotnet restore')
COPY ["grisaac/my-dotnet-app.csproj", "./"]
RUN dotnet restore "grisaac/my-dotnet-app.csproj"

# Copy the rest of the application code
COPY . .

# Publish the app to a directory (e.g., /app)
RUN dotnet publish "grisaac/my-dotnet-app.csproj" -c Release -o /app

# Use the official .NET runtime image to run the app
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS runtime

# Set the working directory in the container
WORKDIR /app

# Copy the published app from the build stage
COPY --from=build /app .

# Expose port 80 to the outside world
EXPOSE 80

# Set the entry point for the container
ENTRYPOINT ["dotnet", "AWS_Basic_App.dll"]
