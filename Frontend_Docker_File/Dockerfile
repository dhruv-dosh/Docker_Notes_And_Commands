# --------------------------------------------------------------------------
# STAGE 1: Build the React Application
# --------------------------------------------------------------------------
FROM node:20-alpine AS build

# Set the working directory inside the container
WORKDIR /app

# Copy dependency definition files
COPY package*.json .

# Install dependencies
RUN npm install

# Copy the rest of the frontend source code
COPY . .

# Run the build process (creates optimized static files, often in /app/dist or /app/build)
# NOTE: Adjust 'npm run build' if your package.json uses a different command (e.g., 'npm run prod')
RUN npm run build

# --------------------------------------------------------------------------
# STAGE 2: Serve the Application using Nginx
# --------------------------------------------------------------------------
FROM nginx:alpine

# Expose the frontend port (5173, as specified in your project)
EXPOSE 5173

# Remove the default Nginx index.html file
RUN rm -rf /usr/share/nginx/html/*

# Copy the compiled static assets from the 'build' stage into the Nginx serving directory
# NOTE: If your build output directory is 'build', change /app/dist to /app/build
COPY --from=build /app/dist /usr/share/nginx/html

# Copy a custom Nginx configuration to handle routing (especially important for React Router)
# You MUST have a custom nginx.conf file in your frontend folder for proper routing
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Command to start Nginx is included in the base image
# CMD ["nginx", "-g", "daemon off;"]