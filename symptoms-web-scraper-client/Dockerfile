FROM node:lts-alpine AS clinet-build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
CMD ["npx","serve","dist","-l","8080"]
EXPOSE 8080