#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["HelloWorldAspNetCore.csproj", "."]
RUN dotnet restore "./HelloWorldAspNetCore.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "HelloWorldAspNetCore.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "HelloWorldAspNetCore.csproj" -c Release -o /app/publish

FROM base AS final

ENV ASPNETCORE_URLS https://*:8080

RUN apt update && \
    apt install nginx
ADD nginx-website.conf /etc/nginx/sites-enabled
RUN service nginx restart

WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "HelloWorldAspNetCore.dll"]
