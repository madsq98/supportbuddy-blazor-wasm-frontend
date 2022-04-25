FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /SupportBuddyWebSPA
COPY supportbuddy-blazor-wasm-frontend.csproj .
RUN dotnet restore supportbuddy-blazor-wasm-frontend.csproj
COPY . .
RUN dotnet build supportbuddy-blazor-wasm-frontend.csproj -c Release -o /app/build

FROM build AS publish
RUN dotnet publish supportbuddy-blazor-wasm-frontend.csproj -c Release -o /app/publish

FROM nginx:alpine AS final
WORKDIR /usr/share/nginx/html
COPY --from=publish /app/publish/wwwroot .
COPY nginx.conf /etc/nginx/nginx.conf