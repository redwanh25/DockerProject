FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["DockerProject.csproj", "DockerProject/"]
#COPY ["DockerProject/DockerProject.csproj", "DockerProject/"]
RUN dotnet restore "./DockerProject/DockerProject.csproj"
COPY . DockerProject
#COPY . .
RUN dotnet build "./DockerProject/DockerProject.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./DockerProject/DockerProject.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DockerProject.dll"]


#
# base stage
# ------------------------------------------
#   /app (working directory)
#
# build & publish stage
# ------------------------------------------
#   /src (working directory)
#   │
#   ├── /DockerProject
#   │	│
#   │   ├── DockerProject.csproj
#   │   ├── restored dependencies...
#	│	└── other files...
#   │
#	├── /app/build
#   │	│
#	│	├── DockerProject.dll
#	│	└── other build artifacts...
#   │
#	└── /app/publish
#   	│
#		├── DockerProject.dll
#		└── other publish artifacts...
#
# final stage
# ------------------------------------------
#   /app (working directory)
#   │
#   ├── DockerProject.dll
#   └── other publish artifacts...
#