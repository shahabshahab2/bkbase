FROM jenkins:2.7.4-alpine

USER root

# Skip initial setup.
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false" \
    # Customizable settings:
    # Default admin user and password and url 
    JENKINS_USER=admin \
    JENKINS_PASS=admin \
    # Default email.
    JENKINS_EMAIL=admin@localhost \
    # Change Jenkins port from 8080 to 9090 
    JENKINS_OPTS="--httpPort=9090"

# Get plugins list.
COPY plugins.txt /tmp/
# Add initial YADP configuration.
COPY configs/config.xml /usr/share/jenkins/default-config/
# Custom JENKINS groovy scripts to create JENKINS_USER and JENKINS_PASS
COPY groovy/*.groovy /usr/share/jenkins/ref/init.groovy.d/
# Install plugins from txt file.
RUN /usr/local/bin/install-plugins.sh $(cat /tmp/plugins.txt | tr '\n' ' ')
# prepare jenkins
COPY jenkins.sh /usr/local/bin/jenkins.sh 
RUN chmod +x /usr/local/bin/jenkins.sh 
#Create a static dummy job
COPY  --chown=jenkins:jenkins job/ /var/jenkins_home/
# Switch to jenkins users
USER jenkins 
# Exposed ports.
EXPOSE 50000 9090
