FROM node:18-alpine

# Create app directory
WORKDIR /usr/src/app

# Install dependencies
COPY package*.json ./
RUN npm install --production

# Copy the backend source code
COPY . .

# Backend listens on port 3000
ENV PORT=3000
EXPOSE 3000

# Start the app
# If your file is 'server.js' keep it. If not, tell me
CMD ["node", "server.js"]

