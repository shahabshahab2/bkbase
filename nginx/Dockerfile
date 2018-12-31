FROM nginx:1.15-alpine
# Copy nginx rules to docker. These rules include the Listeneing port of Nginx and paths while error happens
COPY default.conf /etc/nginx/conf.d/default.conf
# A Simple Hello World Nginx page
COPY index.html /usr/share/nginx/html

EXPOSE 8080

# Since Docker is only watching the PID of the original command, the container then halts. Therefore \
# use Nginx  the daemon off directive to run in the foregroun
CMD ["nginx","-g","daemon off;" ]