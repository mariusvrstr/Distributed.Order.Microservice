#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Distributed.Order.Distribution/Distributed.Order.Distribution.csproj", "Distributed.Order.Distribution/"]
RUN dotnet restore "Distributed.Order.Distribution/Distributed.Order.Distribution.csproj"
COPY . .
WORKDIR "/src/Distributed.Order.Distribution"
RUN dotnet build "Distributed.Order.Distribution.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Distributed.Order.Distribution.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Distributed.Order.Distribution.dll"]