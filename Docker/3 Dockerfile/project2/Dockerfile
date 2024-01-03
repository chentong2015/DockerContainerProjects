FROM docker-all.nexus.com/coretech-infrastructure/maven-3.6.3-oracle-open-jdk-11.0.2-rhel-6.4-takari:latest
ARG UID=1000
ARG GID=1000

# Set timezone to Paris
ENV TZ=Europe/Paris

# Make jenkins user root
COPY etc/sudoers /etc/sudoers

# Set timezone to Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
# Install java
&& yum install -y \
   sysstat-9.0.4-20.el6 \
# Needed for fetchFromFs

# Used to install thirdparties needed for amber (java, python, openssl, maven...)
   tar \
# Needed to analyze current SCM data
   git19-git-1.9.4-4.el6.1 \
# Needed for elasticsearch
   which-2.19-6.el6 \
# Needed for messaging integration tests
   openssl-1.0.1e-57.el6.x86_64 \
   krb5-devel-1.10.3-10.el6.x86_64 \
&& yum clean all \
# Symlink explicit path to Java
&& mkdir -p /usr/local/java \
&& ln -s /usr/java/jdk-11.0.2 /usr/local/java/jdk-11.0.2 \

# Install bats
RUN mkdir /tmp/bats \
&& wget http://nexus.fr.com/nexus3/repository/npm-all/bats/-/bats-1.1.0.tgz -P /tmp/bats \
&& tar xvzf /tmp/bats/bats-1.1.0.tgz -C /tmp/bats/
ENV PATH="/tmp/bats/package/bin:${PATH}"

CMD ["bash"]
