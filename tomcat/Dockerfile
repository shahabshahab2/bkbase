FROM tomcat:8.0-alpine
WORKDIR /usr/local/tomcat/
# Import sample project to tomcat 
RUN rm -rf webapps/*
COPY ./sample.war ./webapps
# Provide a help on homepage to redirect to /sample
COPY ./ROOT ./webapps/ROOT
# Change tomcat port from 8080 to 8090
RUN value=`cat conf/server.xml` && echo "${value//8080/8090}" >| conf/server.xml
# expose port
EXPOSE 8090
CMD ["catalina.sh", "run"]