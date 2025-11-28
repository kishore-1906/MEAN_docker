# ---------- Build Stage ----------
FROM node:18-alpine AS build

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy source
COPY . .

# Build Angular app
RUN npm run build -- --configuration=production

# ---------- Run Stage (Nginx) ----------
FROM nginx:alpine

COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html

# Custom nginx config (we will create next)
COPY nginx/default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

