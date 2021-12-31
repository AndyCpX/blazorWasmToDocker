#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM nginx:alpine AS  base
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["BlazorWasmToDocker/BlazorWasmToDocker.csproj", "BlazorWasmToDocker/"]
RUN dotnet restore "BlazorWasmToDocker/BlazorWasmToDocker.csproj"
COPY . .
WORKDIR "/src/BlazorWasmToDocker"
RUN dotnet build "BlazorWasmToDocker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorWasmToDocker.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /usr/share/nginx/html
COPY --from=publish /app/publish/wwwroot .
COPY nginx.conf /etc/nginx/nginx.conf