ARG ROOT_IMAGE

FROM ${ROOT_IMAGE}

ENV USER="jenkins"
ENV APP_DIR="/data/env"

USER root

# Create jenkins user and make it root
RUN groupadd -g 497 jenkins \
 && useradd jenkins -u 497 -g jenkins -s /bin/bash -d /home/jenkins

# Set timezone to Paris
ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Create appdir
RUN mkdir -p ${APP_DIR} \
 && mkdir -p ${APP_DIR}/fs/license \
 && chown -R ${USER} ${APP_DIR}

WORKDIR ${APP_DIR}

COPY --chown=${USER} mandatory/ *.jar ./

USER ${USER}

# Install Primary and Secondary
RUN  java -jar primary.jar -wizardconfig init/wizardconfig-mxPrimary.xml && \
     rm primary.jar && \
     java -jar secondary.jar  -wizardconfig init/wizardconfig-mxSecondary.xml && \
     rm secondary.jar && \
     cd fs/license && jar -xvf ../../license.jar

# Copy the remaining files
COPY mandatory/. ${APP_DIR}

CMD ["bash"]