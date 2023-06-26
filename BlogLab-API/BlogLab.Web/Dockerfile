#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["BlogLab.Web/BlogLab.Web.csproj", "BlogLab.Web/"]
COPY ["BlogLab.Identity/BlogLab.Identity.csproj", "BlogLab.Identity/"]
COPY ["BlogLab.Models/BlogLab.Models.csproj", "BlogLab.Models/"]
COPY ["BlogLab.Repository/BlogLab.Repository.csproj", "BlogLab.Repository/"]
COPY ["BlogLab.Services/BlogLab.Services.csproj", "BlogLab.Services/"]
RUN dotnet restore "BlogLab.Web/BlogLab.Web.csproj"
COPY . .
WORKDIR "/src/BlogLab.Web"
RUN dotnet build "BlogLab.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlogLab.Web.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlogLab.Web.dll"]