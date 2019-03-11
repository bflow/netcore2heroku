FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /app

# copy everything else and build app
COPY . .
WORKDIR /app/netcore2heroku
RUN dotnet publish -c Release -o out

FROM microsoft/dotnet:2.2-aspnetcore-runtime AS runtime
WORKDIR /app
COPY --from=build /app/netcore2heroku/out ./

RUN echo "ASPNETCORE_URLS=http://0.0.0.0:\$PORT DOTNET_RUNNING_IN_CONTAINER=true" > /app/setup_heroku_env.sh && chmod +x /app/setup_heroku_env.sh

CMD /bin/bash -c "source /app/setup_heroku_env.sh && dotnet netcore2heroku.dll"
