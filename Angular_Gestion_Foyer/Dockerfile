# Stage 1: Build the Angular application
FROM node:20 AS build

# Set the working directory
WORKDIR /app

# Install Angular CLI globally
RUN npm install -g @angular/cli@15

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy the entire project
COPY . .

# Build the Angular project with production configuration
RUN ng build --configuration production

# Stage 2: Serve the Angular application with a lightweight image
FROM nginx:alpine

# Copy the Angular build output to the Nginx HTML directory
COPY --from=build /app/dist/<your-angular-project-name> /usr/share/nginx/html

# Expose port 80 for the Nginx server
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
