# Build stage
FROM node:16 as build-stage

WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Ensure vue-cli-service is executable
RUN chmod +x /app/node_modules/.bin/vue-cli-service

# Ensure all shell scripts are executable
RUN find /app -type f -name "*.sh" -exec chmod +x {} \;

# Set permissions (if needed on Linux)
RUN chown -R node:node /app

# Build the application
RUN npm run build

# Production stage
FROM nginx:latest

# Copy the built files from the build stage
COPY --from=build-stage /app/dist/ /usr/share/nginx/html

# Copy the Nginx configuration file
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
