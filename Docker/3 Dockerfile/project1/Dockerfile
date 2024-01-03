# 公司级使用的DB镜像
ARG ROOT_IMAGE=docker-all.nexus.com/coretech/mssql-mx-runtime-rhel-8:71
FROM ${ROOT_IMAGE}

# 配置环境变量
ENV USER="jenkins"
ENV APP_DIR="/data/env"
ARG MVR_JAR

USER root

# Create jenkins user and make it root
RUN groupadd -g 497 jenkins \
 && useradd jenkins -u 497 -g jenkins -s /bin/bash -d /home/jenkins

# Set timezone to Paris
ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Create appdir
RUN mkdir -p ${APP_DIR} \
 && mkdir ${APP_DIR}/init \
 && chown -R ${USER} ${APP_DIR}

WORKDIR ${APP_DIR}

ADD "$MVR_JAR" mvr.jar
RUN chown ${USER} mvr.jar

USER ${USER}

# Copy wizard and install MVR
COPY mandatory/init/wizardconfig-mvr.xml init/

RUN javaHome=$(dirname $(dirname $(realpath $(command -v java)))) \
 && echo javaHome=$javaHome \
 && sed -i -E "s|(name=\"javaHome\" value=\")[/a-zA-Z0-9.\\-]+(\")|\1$javaHome\2|g" init/wizardconfig-mvr.xml

RUN msSqlServerHome=$(realpath $(ls -d /opt/microsoft/*/etc/ | grep msodbc)) \
 && echo msSqlServerHome=$msSqlServerHome \
 && sed -i -E "s|(name=\"msSqlServerHome\" value=\")[/a-zA-Z0-9.\\-]+(\")|\1$msSqlServerHome\2|g" init/wizardconfig-mvr.xml

RUN msSqlJdbcHome=$(realpath $(ls -d /opt/microsoft/sqljdbc/*/ | tail -1)) \
 && echo msSqlServerHome=$msSqlJdbcHome \
 && sed -i -E "s|(name=\"msSqlServerHome\" value=\")[/a-zA-Z0-9.\\-]+(\")|\1$msSqlJdbcHome\2|g" init/wizardconfig-mvr.xml

RUN java -jar mvr.jar -wizardconfig init/wizardconfig-mvr.xml \
 && rm mvr.jar mvr.sh _mvr.sh

# Copy the remaining files
COPY mandatory/. ${APP_DIR}

CMD ["bash"]