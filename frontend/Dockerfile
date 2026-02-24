# Stage 1: Build Angular
FROM node:18 as build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build -- --configuration production

# Stage 2: Serve with Nginx
FROM nginx:alpine

# Remove default nginx files
RUN rm -rf /usr/share/nginx/html/*

# IMPORTANT: Copy correct Angular build folder
COPY --from=build /app/dist/angular-15-crud/ /usr/share/nginx/html/

EXPOSE 80
