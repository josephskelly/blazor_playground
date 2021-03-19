#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["blazor_playground/Server/blazor_playground.Server.csproj", "blazor_playground/Server/"]
COPY ["blazor_playground/Client/blazor_playground.Client.csproj", "blazor_playground/Client/"]
COPY ["blazor_playground/Shared/blazor_playground.Shared.csproj", "blazor_playground/Shared/"]
RUN dotnet restore "blazor_playground/Server/blazor_playground.Server.csproj"
COPY . .
WORKDIR "/src/blazor_playground/Server"
RUN dotnet build "blazor_playground.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "blazor_playground.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "blazor_playground.Server.dll"]
