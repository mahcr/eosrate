FROM keymetrics/pm2:10-alpine

ARG SERVICE

# Install Global Depedencies
RUN apk add --no-cache yarn
RUN apk add --no-cache git
RUN apk add --no-cache bash
RUN apk add --no-cache vim
RUN apk add --no-cache postgresql-client
RUN yarn global add pm2

# Install pm2 modules
RUN pm2 install typescript
RUN pm2 install ts-node

# Create app directory
WORKDIR /opt/application

# Install application dependencies
COPY package.json ./
COPY yarn.lock ./
RUN yarn --ignore-optional --frozen-lockfile --network-timeout 1000000 --network-concurrency 1

EXPOSE 3030

# Copy app source -- required for CI/CD, overwritten by docker compose mounting
COPY . .

## Add the wait script to the image
ADD ./utils/wait /wait
RUN chmod +x /wait

## Launch the wait tool and then use pm2-dev to start the app with live reload
CMD /wait && ./start.sh
