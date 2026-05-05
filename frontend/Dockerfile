FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3002
ENV PORT=3002
ENV HOST=0.0.0.0
ENV CI=false
CMD ["npm", "start"]
