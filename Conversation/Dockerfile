# FROM mcr.microsoft.com/dotnet/aspnet:8.0-nanoserver-1809 AS base
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5000

ENV ASPNETCORE_URLS=http://+:5000

# FROM mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-1809 AS build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["Conversation/Services.Conversation.csproj", "Conversation/"]
RUN dotnet restore "Conversation\Services.Conversation.csproj"
COPY . .
WORKDIR "/src/Conversation"
RUN dotnet build "Services.Conversation.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "Services.Conversation.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Services.Conversation.dll"]
 