FROM aspnet-quic:7.0 AS base

WORKDIR /app
EXPOSE 80
EXPOSE 443
EXPOSE 443/udp

FROM sdk-cdn:7.0 AS build
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

ENTRYPOINT ["/app/RouterProxy", "--urls http://*:80;https://*:443"]
