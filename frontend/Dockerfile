<<<<<<< HEAD
# ----------------------------
# Stage 1: Build Angular App
# ----------------------------
FROM node:18-alpine AS build
=======
# Stage 1: Build Angular
FROM node:16 as build
>>>>>>> 1f7e8a15efcc9e4f0779520dc389319ffb3e4382

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy full project
COPY . .

# Build Angular for production
RUN npm run build -- --configuration production


# ----------------------------
# Stage 2: Serve using Nginx
# ----------------------------
FROM nginx:alpine

# Remove default nginx config
RUN rm /etc/nginx/conf.d/default.conf

# Copy our custom nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy Angular build output
COPY --from=build /app/dist/angular-15-crud/ /usr/share/nginx/html/

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]