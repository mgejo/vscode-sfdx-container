FROM ubuntu AS tarrer
RUN apt-get update && apt-get install -y \
    wget \
    xz-utils
RUN wget https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.10%2B9/OpenJDK11U-jdk_x64_linux_hotspot_11.0.10_9.tar.gz
RUN mkdir java
RUN tar xvf OpenJDK11U-jdk_x64_linux_hotspot_11.0.10_9.tar.gz -C java



FROM ubuntu
COPY --from=tarrer /java /usr/lib/jvm
COPY --from=tarrer /var/lib/apt/lists/ /var/lib/apt/lists/
ENV JAVA_HOME="/usr/lib/jvm/jdk-11.0.10+9"
ENV PATH "$PATH:$JAVA_HOME/bin"
RUN DEBIAN_FRONTEND="noninteractive" apt-get install -y \
    git --no-install-recommends \
    npm --no-install-recommends
RUN ln -s /usr/bin/node /usr/local/bin/node
RUN apt-get autoremove --assume-yes \
    && apt-get clean --assume-yes \
    && rm -rf /var/lib/apt/lists/*
RUN npm install -g npm
RUN npm install -g sfdx-cli
