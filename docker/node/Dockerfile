# Use Node.js 22 LTS Alpine as the base image
FROM node:22-alpine
RUN apk add --no-cache ca-certificates

# Set the working directory inside the container
WORKDIR /var/www/app

# Copy package.json and package-lock.json for efficient dependency caching
COPY package*.json ./

COPY . .

RUN npm ci && npm run build

# Expose the application port (default: 3000)
EXPOSE 3000

CMD [ "node", ".output/server/index.mjs" ]
