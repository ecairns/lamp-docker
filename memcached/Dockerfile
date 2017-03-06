FROM ubuntu:latest

MAINTAINER Ethan Cairns <ecairns@oklahoman.com>

# Update the default application repository sources list
RUN apt-get update

RUN apt-get install -y memcached

EXPOSE 11211

# Set the user to run Memcached daemon
USER daemon

CMD ["-u", "root", "-m", "1024"]

ENTRYPOINT memcached
