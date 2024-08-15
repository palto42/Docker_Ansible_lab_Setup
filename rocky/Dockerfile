FROM rockylinux:9
LABEL AUTHOR "Karthick"

# Set the proxy as a build argument (default empty)
ARG HTTP_PROXY
ARG HTTPS_PROXY
ARG NO_PROXY

# Install packages
RUN dnf upgrade --refresh -y \
    && dnf install epel-release -y\
    && dnf install -y --allowerasing vim curl wget openssh-server jq net-tools iputils python3 python3-pip

# Create ANSUSER, .ssh directory and set ownsership,permission
RUN adduser -s /bin/sh -G wheel ansuser\
    && echo "PermitRootLogin yes" >> /etc/ssh/sshd_config \
    && echo "ansuser ALL=(ALL) NOPASSWD: ALL " >> /etc/sudoers \
    && mkdir -p /var/run/sshd

# Copy entrypoint script
COPY script/entrypoint.sh /usr/local/bin
RUN chmod u+x /usr/local/bin/entrypoint.sh

# Set root user and ansuser password
RUN echo "root:password123" | chpasswd
RUN echo "ansuser:password123" | chpasswd

# SSHD will listen at port 22
EXPOSE 22

# Start SSHD service
ENTRYPOINT ["entrypoint.sh"]
CMD ["/usr/sbin/sshd","-D","-e"]
