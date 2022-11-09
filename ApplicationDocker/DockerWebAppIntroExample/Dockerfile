#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

# Create a base container with the aspnet runtime
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
# Create a folder in the base container
WORKDIR /app
# Expose ports for http and https
EXPOSE 80
EXPOSE 443

# Create the build container 
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
# Create a work directory 
WORKDIR /src
# Copy the csproj and restore the depedencies
COPY ["DockerIntroWebApi.csproj", "."]
RUN dotnet restore "./DockerIntroWebApi.csproj"
#Copy everything to the src folder in the container
COPY . .
# Set the working directory to src
WORKDIR "/src/."
# Run the dotnet build command with Release flag and place in the app/build folder
RUN dotnet build "DockerIntroWebApi.csproj" -c Release -o /app/build

# Creating an intermediate container using the build container built earlier
FROM build AS publish
# Run the publish command in the intermedite container 
RUN dotnet publish "DockerIntroWebApi.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Create an intermediate container with the base container
FROM base AS final
# set the working directory to app
WORKDIR /app
# Copy everything from the publish folder in the intermediate container to the publish folder
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DockerIntroWebApi.dll"]