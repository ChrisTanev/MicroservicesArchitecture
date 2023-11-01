FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80

ENV ASPNETCORE_URLS=http://+:80

FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["src/MicroservicesArchitecture.Client/MicroservicesArchitecture.Client.csproj", "src/MicroservicesArchitecture.Client/"]
RUN dotnet restore "src/MicroservicesArchitecture.Client/MicroservicesArchitecture.Client.csproj"
COPY . .
WORKDIR "/src/src/MicroservicesArchitecture.Client"
RUN dotnet build "MicroservicesArchitecture.Client.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "MicroservicesArchitecture.Client.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MicroservicesArchitecture.Client.dll"]
