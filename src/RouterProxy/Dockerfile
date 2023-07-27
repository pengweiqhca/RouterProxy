FROM aspnet-quic:7.0 AS base

ENV Kestrel:Endpoints:Http:Url=https://*:80
ENV Kestrel:Endpoints:Https:Url=https://*:443

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
RUN dotnet build "src/RouterProxy/RouterProxy.csproj" -c Release -o /app/build --no-restore

FROM build AS publish
RUN dotnet publish "src/RouterProxy/RouterProxy.csproj" -c Release -o /app/publish

FROM base AS final
COPY --from=publish /app/publish .

ENTRYPOINT ["/app/RouterProxy"]
