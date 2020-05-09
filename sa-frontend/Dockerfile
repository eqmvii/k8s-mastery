# Install node and build the react webapp
FROM node:latest as react-app-builder
COPY . .
RUN yarn install
RUN yarn build

# Multi-stage build; now start an nginx image
FROM nginx

# Copy the build output from the first image into the nginx image
COPY --from=react-app-builder build /usr/share/nginx/html
