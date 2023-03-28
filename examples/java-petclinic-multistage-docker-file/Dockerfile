### Building ###
ARG VERSION=latest

# Fetching the latest ubuntu image 
FROM ubuntu:$VERSION AS build

# Declaring args
ARG VERSION
ARG MAVEN_VERSION="3.9.1"

MAINTAINER kumar5483reddy@gmail.com

# Installing dependencies
RUN apt-get update -y && apt-get install openjdk-11-jdk -y && apt-get install wget -y &&  apt-get install unzip -y && apt-get install git -y
RUN java -version

# Setting up the work directory
WORKDIR /app

# Download maven from internet
RUN wget  https://dlcdn.apache.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz -O maven.tar.gz

# Extract the tar file and Check the maven version
RUN tar -zxvf maven.tar.gz
RUN apache-maven-$MAVEN_VERSION/bin/mvn --version

# Copying all the files in our project
COPY . .

# Building the war package with MySQL database
RUN apache-maven-$MAVEN_VERSION/bin/mvn clean install -P MySQL

### Release ###
FROM tomcat:9

# Copy the war file from above stage to this stage tomcat webapps folder
COPY --from=build /app/target/petclinic.war /usr/local/tomcat/webapps/

# Expose the Port Number
EXPOSE 8080