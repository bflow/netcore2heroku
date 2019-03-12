FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src
COPY ["netcore2heroku/netcore2heroku.csproj", "netcore2heroku/"]
RUN dotnet restore "netcore2heroku/netcore2heroku.csproj"
COPY . .
WORKDIR "/src/netcore2heroku"
RUN dotnet build "netcore2heroku.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "netcore2heroku.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
RUN echo "ASPNETCORE_URLS=http://0.0.0.0:\$PORT DOTNET_RUNNING_IN_CONTAINER=true" > /app/setup_heroku_env.sh && chmod +x /app/setup_heroku_env.sh

CMD /bin/bash -c "source /app/setup_heroku_env.sh && dotnet netcore2heroku.dll"

#restart Docker after booting Windows