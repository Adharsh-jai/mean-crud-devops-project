# ----------------------------
# Stage 1: Build Angular App
# ----------------------------
FROM node:18-alpine AS build

WORKDIR /app

# Copy only package files first (better layer caching)
COPY package*.json ./

# Install dependencies (cleaner for CI)
RUN npm ci

# Copy remaining project files
COPY . .

# Build Angular production build
RUN npm run build -- --configuration production


# ----------------------------
# Stage 2: Serve using Nginx
# ----------------------------
FROM nginx:alpine

# Remove default Nginx config
RUN rm /etc/nginx/conf.d/default.conf

# Copy custom nginx config
COPY frontend/nginx.conf /etc/nginx/conf.d/default.conf

# Copy Angular build output
# ⚠ Ensure folder name matches your angular.json outputPath
COPY --from=build /app/dist/angular-15-crud/ /usr/share/nginx/html/

# Expose container port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]