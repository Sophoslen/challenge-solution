#stage1: build react application
FROM node:15.14.0 as build-stage

#set the working directory
WORKDIR /app

#copy the package.json and package-lock.json
COPY package*.json ./

#install dependencies
RUN npm install

#copy the rest of the application
COPY . .

#build the application
RUN npm run build && npm prune --production

#stage2: serve the react application using Node.js
FROM node:15.14.0-alpine

#create a non-root user to run the application
RUN addgroup -g 1001 appuser && adduser -S -u 1001 -G appuser appuser

#set the work directory
WORKDIR /app

#copy the react app from the previous stage
COPY --from=build-stage --chown=appuser:appuser /app/build /app/

#switch non-root user
USER appuser

#create a directory for the npm packages in the hone directory of the user
RUN mkdir -p /home/appuser/.npm-global
ENV PATH=/home/appuser/.npm-global/bin:$PATH
ENV NPM_CONFIG_PREFIX=/home/appuser/.npm-global

#install http server to serve the react app
RUN npm install -g http-server

#expose the port where the app will be served
EXPOSE 8080

#set the NODE_ENV to prod
ENV NODE_ENV=production

#start the http server
CMD [ "http-server", "-p", "8080" ]



