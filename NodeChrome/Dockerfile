# !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
# NOTE: DO *NOT* EDIT THIS FILE.  IT IS GENERATED.
# PLEASE UPDATE Dockerfile.txt INSTEAD OF THIS FILE
# !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
FROM selenium/node-base:3.0.1-dysprosium
MAINTAINER Selenium <selenium-developers@googlegroups.com>

USER root

#============================================
# Google Chrome
#============================================
# can specify versions by CHROME_VERSION;
#  e.g. google-chrome-stable=53.0.2785.101-1
#       google-chrome-beta=53.0.2785.92-1
#       google-chrome-unstable=54.0.2840.14-1
#       latest (equivalent to google-chrome-stable)
#       google-chrome-beta  (pull latest beta)
#============================================

#ARG CHROME_VERSION="google-chrome-stable"
#ARG CHROME_VERSION="58.0.3029.110-1"
#ARG CHROME_VERSION="chromium-browser"
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
  && echo "deb http://archive.ubuntu.com/ubuntu xenial main universe\n" > /etc/apt/sources.list \
  && echo "deb http://archive.ubuntu.com/ubuntu xenial-updates main universe\n" >> /etc/apt/sources.list \
  && apt-get update -qqy \
  && sudo sed -i -re 's/([a-z]{2}\.)?us.archive.ubuntu.com|security.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list \
  && sudo apt-get update -qqy \
  && sudo apt-get dist-upgrade -qqy \
  && sudo apt-get install -y gconf-service libasound2 libatk1.0-0 libatomic1 libcairo2 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk2.0-0 \
  && sudo apt-get install -y libxss1 xdg-utils \
  && wget http://security.ubuntu.com/ubuntu/pool/universe/c/chromium-browser/chromium-codecs-ffmpeg_57.0.2987.98-0ubuntu1.1348_amd64.deb \
  && sudo dpkg -i chromium-codecs-ffmpeg_57.0.2987.98-0ubuntu1.1348_amd64.deb \
  && wget http://security.ubuntu.com/ubuntu/pool/universe/c/chromium-browser/chromium-codecs-ffmpeg-extra_57.0.2987.98-0ubuntu1.1348_amd64.deb \
  && sudo dpkg -i chromium-codecs-ffmpeg-extra_57.0.2987.98-0ubuntu1.1348_amd64.deb \
  && wget http://security.ubuntu.com/ubuntu/pool/universe/c/chromium-browser/chromium-browser_57.0.2987.98-0ubuntu1.1348_amd64.deb \
  && sudo dpkg -i chromium-browser_57.0.2987.98-0ubuntu1.1348_amd64.deb
    
#==================
# Chrome webdriver
#==================
#2.25
ARG CHROME_DRIVER_VERSION=2.29
RUN wget --no-verbose -O /tmp/chromedriver_linux64.zip https://chromedriver.storage.googleapis.com/$CHROME_DRIVER_VERSION/chromedriver_linux64.zip \
  && rm -rf /opt/selenium/chromedriver \
  && unzip /tmp/chromedriver_linux64.zip -d /opt/selenium \
  && rm /tmp/chromedriver_linux64.zip \
  && mv /opt/selenium/chromedriver /opt/selenium/chromedriver-$CHROME_DRIVER_VERSION \
  && chmod 755 /opt/selenium/chromedriver-$CHROME_DRIVER_VERSION \
  && ln -fs /opt/selenium/chromedriver-$CHROME_DRIVER_VERSION /usr/bin/chromedriver

#========================
# Selenium Configuration
#========================
ENV NODE_MAX_INSTANCES 1
ENV NODE_MAX_SESSION 1
ENV NODE_REGISTER_CYCLE 5000
ENV NODE_PORT 5555
COPY generate_config /opt/selenium/generate_config
RUN chmod +x /opt/selenium/generate_config \
  && touch /opt/selenium/config.json \
  && chmod 666 /opt/selenium/config.json

#=================================
# Chrome Launch Script Modication
#=================================
COPY chrome_launcher.sh /opt/google/chrome/google-chrome
RUN chmod +x /opt/google/chrome/google-chrome

RUN chown -R seluser:seluser /opt/selenium

USER seluser
# Following line fixes
# https://github.com/SeleniumHQ/docker-selenium/issues/87
ENV DBUS_SESSION_BUS_ADDRESS=/dev/null
