#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["hello-k8s-cs/hello-k8s-cs.csproj", "hello-k8s-cs/"]
RUN dotnet restore "hello-k8s-cs/hello-k8s-cs.csproj"
COPY . .
WORKDIR "/src/hello-k8s-cs"
RUN dotnet build "hello-k8s-cs.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "hello-k8s-cs.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "hello-k8s-cs.dll"]