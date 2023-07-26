FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src

COPY ["Directory.Build.props", "Directory.Build.targets", "./"]
COPY ["src/Directory.Build.props", "src/Directory.Build.targets", "src/"]
COPY ["src/RouterProxy/RouterProxy.csproj", "src/RouterProxy/"]

RUN dotnet restore "src/RouterProxy/RouterProxy.csproj"
COPY . .
RUN dotnet build "src/RouterProxy/RouterProxy.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "src/RouterProxy/RouterProxy.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
COPY --from=publish /app/publish .

ENTRYPOINT ["RouterProxy"]
