FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["CodeCrate/Client/CodeCrate.Client.csproj", "CodeCrate/Client/"]
COPY ["CodeCrate/Shared/CodeCrate.Shared.csproj", "CodeCrate/Shared/"]
RUN dotnet restore "CodeCrate/Client/CodeCrate.Client.csproj"
COPY . .
WORKDIR "/src/CodeCrate/Client"
RUN dotnet build "CodeCrate.Client.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CodeCrate.Client.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CodeCrate.Client.dll"]
