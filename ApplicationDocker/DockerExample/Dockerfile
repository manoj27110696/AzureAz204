#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

# Setting an image as the base image from the mcr registry
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
# Setting the WorkDir in the container
WORKDIR /app
# Exposing the necessary ports
EXPOSE 80
EXPOSE 443

# Setting an image as the build image from the mcr registry
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
# Setting the WorkDirectory 
WORKDIR /src
# Copying the CSProj in the work directory
COPY ["DotnetWebApiExample.csproj", "."]
# Restoring the project dependencies 
RUN dotnet restore "./DotnetWebApiExample.csproj"
# Copy everything else
COPY . .
# Setting a work directory named source
WORKDIR "/src/."
# Create build the entire project with Release flag and publish into /app/build
RUN dotnet build "DotnetWebApiExample.csproj" -c Release -o /app/build

# setting the build image as publish
FROM build AS publish
# publish everything with a release flag under the publish folder
RUN dotnet publish "DotnetWebApiExample.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Finally use base as the final image
FROM base AS final
# Set the Work directory as app
WORKDIR /app
# Copy everything from publish to app/publish folder.
COPY --from=publish /app/publish .
# Set the entrypoint as indicated.
ENTRYPOINT ["dotnet", "DotnetWebApiExample.dll"]