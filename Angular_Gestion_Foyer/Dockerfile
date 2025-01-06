# Use Node.js as the base image
FROM node:20

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

# Expose port 4200 for the Angular app
EXPOSE 4200

# Run the Angular app
CMD ["ng", "serve", "--host", "0.0.0.0", "--port", "4200", "--disable-host-check"]
