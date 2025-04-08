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

FROM public.ecr.aws/amazonlinux/amazonlinux:latest

# Update installed packages and install Apache
RUN yum update -y && \
 yum install -y httpd

# Write hello world message
RUN echo 'Hello World!' > /var/www/html/index.html

# Configure Apache
RUN echo 'mkdir -p /var/run/httpd' >> /root/run_apache.sh && \
 echo 'mkdir -p /var/lock/httpd' >> /root/run_apache.sh && \
 echo '/usr/sbin/httpd -D FOREGROUND' >> /root/run_apache.sh && \
 chmod 755 /root/run_apache.sh

EXPOSE 80

CMD /root/run_apache.sh
