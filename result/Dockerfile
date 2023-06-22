FROM node:18-slim

# add curl for healthcheck
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl \
    tini \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# have nodemon available for local dev use (file watching)
RUN npm install -g nodemon

COPY package*.json ./

ENV NEW_RELIC_NO_CONFIG_FILE=true
ENV NEW_RELIC_DISTRIBUTED_TRACING_ENABLED=true \
NEW_RELIC_LOG=stdout  
ENV NEW_RELIC_APP_NAME="voting-results" 
ENV NEW_RELIC_LICENSE_KEY="<YOUR-NR-INGEST-LICENSE-KEY>" 
  



RUN npm ci \
 && npm cache clean --force \
 && mv /app/node_modules /node_modules

COPY . .

ENV PORT 80
EXPOSE 80

ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["node", "server.js"]
