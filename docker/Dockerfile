FROM node:11

WORKDIR /data/themesrc
ENV HOME=/data

RUN mkdir -p /data/themesrc/node_modules

COPY package*.json /data/themesrc/
COPY .eslintrc /data/themesrc/
COPY .npmrc /data/themesrc/
COPY .*.yml /data/themesrc/
COPY gulpfile.js /data/themesrc/gulpfile.js

RUN npm install -g grunt-cli \
    && npm install -g gulp

CMD ["bash"]
