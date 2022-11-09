#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

# Getting a base image
FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base
# Creating a directory in the base image container
WORKDIR /app

# Creating an intermediate container for (dotnet sdk) building the dotnet project
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
# Creating a src directory in the intermediate container
WORKDIR /src
# Copy the csproj file into the intermediate container for the local directory
COPY ["DockerConsoleApp.csproj", "."]
# Run the dotnet restore command to restore the depedencies
RUN dotnet restore "./DockerConsoleApp.csproj"

# Copy the rest of the files 
COPY . .
# Create a work directory named src in the intermediate container
WORKDIR "/src/."

# Building the dotnet application with the Release configuration
RUN dotnet build "DockerConsoleApp.csproj" -c Release -o /app/build

# Creating another intermediate container as publish
FROM build AS publish
# publishing the Dotnet application in release configuration  into app/publish folder
RUN dotnet publish "DockerConsoleApp.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Creating another intermediate container
FROM base AS final
# Create a work directory in the intermediate container
WORKDIR /app
# Copy from intermediate publish container to the final container
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DockerConsoleApp.dll"]