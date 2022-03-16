FROM node:16-alpine
EXPOSE 5000
WORKDIR /usr/src/app
RUN npm install -g serve
COPY example-frontend .
RUN npm install
RUN npm run build
CMD ["serve","-s","-l","5000","build"]
