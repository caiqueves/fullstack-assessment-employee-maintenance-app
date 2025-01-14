# Usado durante a execução no VS no modo rápido (Padrão para a configuração de Depuração)
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Usado para compilar o projeto de serviço
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copiar o arquivo .csproj (ajustando para o caminho correto se necessário)
COPY ["backend/EmployeeMaintenance/EmployeeMaintenance.csproj", "EmployeeMaintenance/"]

# Restaurar dependências
RUN dotnet restore "EmployeeMaintenance/EmployeeMaintenance.csproj"

# Copiar o restante do código fonte para o contêiner
COPY ./backend/EmployeeMaintenance/ ./

# Compilar o projeto
WORKDIR "/src/EmployeeMaintenance"
RUN dotnet build "EmployeeMaintenance.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Usado para publicar o projeto
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "EmployeeMaintenance/EmployeeMaintenance.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Usado para produção ou execução no modo normal (padrão)
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Definir o ponto de entrada da aplicação
ENTRYPOINT ["dotnet", "EmployeeMaintenance.dll"]
