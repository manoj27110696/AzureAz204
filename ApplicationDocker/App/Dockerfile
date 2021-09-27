#Set the runtime info for the Docker Container
FROM mcr.microsoft.com/dotnet/runtime:6.0 

#Command to Copy the App files to a folder called App on the Container
COPY bin/Release/net6.0/publish/ App/

#Set the Working Directory to the App Folder in the Container
WORKDIR /App

#Set the Entry point (this is specific to .Net and varies based on the language)
ENTRYPOINT ["dotnet", "NetCore.Manoj.dll"]

#Disable the pipeline diagnostics
ENV DOTNET_EnableDiagnostics=0
