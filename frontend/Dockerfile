# ==========================================
# Stage 1: The Kitchen (Build the React App)
# ==========================================
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# ==========================================
# Stage 2: The Food Truck (Serve with Nginx)
# ==========================================
FROM nginx:alpine

# ADDED THIS LINE: Copy our custom Nginx routing configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]