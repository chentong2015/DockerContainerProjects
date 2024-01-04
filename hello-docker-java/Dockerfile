# Oracle image is freely available from Java-17
# FROM openjdk:17-oracle

# openjdk:17-jdk-slim creates lightweight image
FROM openjdk:17-jdk-slim

# 复制项目中生成的jar到容器中
ADD target/hello-docker-java-1.0.0-jar-with-dependencies.jar hello-docker-app.jar

# ENTRYPOINT所设置的命令是一定会被执行, 注意启动的相对位置
ENV JAVA_OPTS=""
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -jar /hello-docker-app.jar" ]

EXPOSE 8090