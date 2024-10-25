# См. статью по ссылке https://aka.ms/customizecontainer, чтобы узнать как настроить контейнер отладки и как Visual Studio использует этот Dockerfile для создания образов для ускорения отладки.

# В зависимости от операционной системы хост-компьютеров, которые будут выполнять сборку контейнеров или запускать их, может потребоваться изменить образ, указанный в инструкции FROM.
# Дополнительные сведения см. на странице https://aka.ms/containercompat

# Используем базовый образ с ASP.NET 6.0
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base

# Устанавливаем рабочую директорию внутри контейнера
WORKDIR /app

# Используем SDK
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

# Копируем файлы в контейнер
COPY . /src

# Устанавливаем рабочую директорию внутри контейнера
WORKDIR /src

# Устанавливаем зависимости приложения и параметры компиляции
RUN ls
RUN dotnet restore
RUN dotnet build "./Calculator.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "./Calculator.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish ./

# Определяем команду запуска контейнера
ENTRYPOINT ["dotnet", "Calculator.dll"]
