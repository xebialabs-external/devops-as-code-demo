FROM devops-as-code-demo_xl-release:latest

USER root
RUN apt-get update && apt-get -y install wget && rm -rf /var/lib/apt/lists/*

USER 10001
RUN cd /opt/xebialabs/xl-release-server/default-plugins/__local__ && wget https://github.com/xebialabs-community/xlr-selenium-plugin/releases/download/v1.1.0-beta/xlr-selenium-plugin-1.1.0-beta.jar
RUN cd /opt/xebialabs/xl-release-server/default-plugins/__local__ && wget https://github.com/xebialabs-community/xlr-jenkins2-plugin/releases/download/v1.2.1/xlr-jenkins2-plugin-1.2.1.jar

