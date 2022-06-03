FROM nodered/node-red:2.2.0

COPY node-red.json ./package.json
RUN npm install --unsafe-perm --no-update-notifier --no-fund --only=production
