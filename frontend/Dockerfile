# --- Stage 1: Build React App ---
    FROM node:20-alpine AS build
    WORKDIR /app
    
    # build sırasında kullanılacak bir argüman tanımla
    ARG VITE_API_URL
    
    # bu argümanı bir environment variable olarak ayarla ki build script'i okuyabilsin
    ENV VITE_API_URL=$VITE_API_URL
    
    COPY package.json package-lock.json ./
    RUN npm ci
    COPY . .
    # build script'i artık VITE_API_URL'i kullanacak
    RUN npm run build
    
    # --- Stage 2: Serve with Nginx ---
    FROM nginx:1.27-alpine
    COPY --from=build /app/dist /usr/share/nginx/html
    COPY nginx.conf /etc/nginx/conf.d/default.conf
    EXPOSE 80
    CMD ["nginx", "-g", "daemon off;"]