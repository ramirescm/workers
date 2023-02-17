FROM mcr.microsoft.com/dotnet/runtime:7.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["workers/workers.csproj", "workers/"]
RUN dotnet restore "workers/workers.csproj"
COPY . .
WORKDIR "/src/workers"
RUN dotnet build "workers.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "workers.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "workers.dll"]
