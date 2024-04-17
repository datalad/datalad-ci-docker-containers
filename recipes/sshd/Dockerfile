FROM debian:stable-slim

# install the OpenSSH with the least fuzz
RUN apt-get update -qq -y --allow-releaseinfo-change
# we also pull in git and 7z, mainly to aid use of this image for server-side
# archive/store operations, like RIA would do
RUN apt-get install -q --no-install-recommends -y openssh-server git p7zip-full
RUN apt-get clean

# setup 'sshuser' account, with the provided SSH key authorized for
# login, and a 'sshuserpwd' default password
RUN useradd -m -d /home/sshuser -s /bin/bash -u 1000 sshuser
RUN echo 'sshuser:sshuserpwd' | chpasswd
COPY id_rsa.pub /home/sshuser/.ssh/authorized_keys
RUN chown -R sshuser:sshuser /home/sshuser/.ssh
RUN chmod 600 /home/sshuser/.ssh/authorized_keys

# configure SSHD for startup
RUN service ssh start
EXPOSE 22/tcp

# run the SSHD process in the foreground
CMD ["/usr/sbin/sshd","-D"]
