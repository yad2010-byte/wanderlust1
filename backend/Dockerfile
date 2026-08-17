#Stage 1
FROM node:24 AS backend-builder

#setup the working directory
WORKDIR /app

#code
COPY . .

#packages install
RUN npm i

#tests
RUN npm run test -- --passWithNoTests

#Stage 2
FROM node:24-slim

#setup the working dir
WORKDIR /app

#copy the above stage as compressed 
COPY --from=backend-builder /app .
COPY .env.sample .env

EXPOSE 5000

CMD ["npm", "start"]
