#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Services/ReservationAPI/Reservation.API/Reservation.API.csproj", "Services/ReservationAPI/Reservation.API/"]
COPY ["Services/ReservationAPI/Reservation.API.Models/Reservation.API.Models.csproj", "Services/ReservationAPI/Reservation.API.Models/"]
COPY ["Services/ReservationAPI/Reservation.API.Infrastructure/Reservation.API.Infrastructure.csproj", "Services/ReservationAPI/Reservation.API.Infrastructure/"]
COPY ["Reservation.API.Services/Reservation.API.Services.csproj", "Reservation.API.Services/"]
RUN dotnet restore "Services/ReservationAPI/Reservation.API/Reservation.API.csproj"
COPY . .
WORKDIR "/src/Services/ReservationAPI/Reservation.API"
RUN dotnet build "Reservation.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Reservation.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Reservation.API.dll"]