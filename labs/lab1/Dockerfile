# Pull the latest tomcat image from Docker Hub
FROM tomcat

# Name of the New Docker Image Creator
MAINTAINER tkinsoon@pivotal.io

# Copy the WAR file into the Tomcat Webapps folder
ADD docker101.war /usr/local/tomcat/webapps/ROOT.war

# Run the Tomcat
CMD ["catalina.sh", "run"]
