# Previous in Dockerfile --> worked with docker-compose.yml in VM`
# Stage 1: Build the React application
FROM node:14.15.0-alpine as build

RUN echo "Begin frontend-react build"

WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH
ENV PUBLIC_URL /

# Copy package.json and yarn.lock
COPY package.json ./
# COPY yarn.lock ./

# Install dependencies
RUN yarn install

# Updating browserslist
RUN echo "updating browserslist"
RUN npx browserslist@latest --update-db

# Copy the rest of your frontend-react directory into the image
COPY . ./

RUN echo "starting yard run build"

# Build the React application
RUN yarn run build

RUN echo "finished yard run build"

# Stage 2: Serve the app using Nginx

FROM nginx:stable

RUN echo "found nginx"

# Comment out to use default config
# Copy the NGINX configuration file
# COPY ./conf/nginx/nginx.conf /etc/nginx/nginx.conf

# Copy the built app from the first stage into Nginx's serving directory
COPY --from=build /app/build /usr/share/nginx/html

RUN echo "copied build"

# Expose port 80 for Nginx
EXPOSE 80

RUN echo "adding entrypoint"

# Start shell into container
CMD ["nginx", "-g", "daemon off;"]