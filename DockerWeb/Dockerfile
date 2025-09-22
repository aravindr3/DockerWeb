# Base image for runtime
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 8080

# Build stage
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["DockerWeb/DockerWeb.csproj", "DockerWeb/"]
RUN dotnet restore "./DockerWeb/DockerWeb.csproj"
COPY . .
WORKDIR "/src/DockerWeb"
RUN dotnet build "./DockerWeb.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish stage
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./DockerWeb.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final image for Production
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
# Set production environment explicitly
ENV DOTNET_ENVIRONMENT=Production
ENTRYPOINT ["dotnet", "DockerWeb.dll"]
