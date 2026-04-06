# 1. Choose the base image (The OS)
# We use 'alpine' because it is a super lightweight version of Linux
FROM node:20-alpine

# 2. Set the working directory inside the container
WORKDIR /app

# 3. Copy only the package files first
# We do this before copying the rest of the code to take advantage of Docker caching
COPY package*.json ./

# 4. Install the dependencies inside the container
RUN npm install

# 5. Copy the rest of your backend code into the container
COPY . .

# 6. Expose the port your app runs on
EXPOSE 4000

# 7. The command to start your server
CMD ["node", "server.js"]