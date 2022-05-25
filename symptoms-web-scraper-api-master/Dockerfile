FROM node:lts-alpine AS server-build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
CMD ["node","dist/main"]
EXPOSE 3000