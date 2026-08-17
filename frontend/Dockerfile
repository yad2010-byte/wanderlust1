# -----------Stage 1: Build Stage --------------------------

FROM node:24 AS frontend-builder

# Set the working directory to /app
WORKDIR /app

# Copy the package.json and package-lock.json for dependancy installation
COPY package.json package-lock.json /app

# Install dependancies 
RUN npm i

#Run tests

RUN npm run test -- --passWithNoTests

# Copy the rest of the application code 

COPY . .


# ----------------- Stage 2 : Final Stage ---------------------------

FROM node:24-slim 

# Ser the working directory to /app
WORKDIR /app

# COPY built assets and dependancies from frontend-builder stage
COPY --from=frontend-builder /app .

# Copy the .env.sample file to .env.local 

COPY .env.sample .env.local

# Expose the port 5173 for the Node.js application

EXPOSE 5173

# Defined the default command to run the application in development env

CMD ["npm", "run", "dev", "--", "--host"]
