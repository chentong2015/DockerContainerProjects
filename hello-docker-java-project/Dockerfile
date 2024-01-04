FROM eclipse-temurin:17-jdk-jammy as base

WORKDIR /app
COPY .mvn/ .mvn
COPY mvnw pom.xml ./

# 自动构建项目，解析项目构建所需的依赖
RUN ./mvnw dependency:resolve
COPY src ./src

# Activate the MySQL Spring profile defined in the application
# CMD ["./mvnw", "spring-boot:run", "-Dspring-boot.run.profiles=mysql"]

# Multi-stage 构建容器运行时的不同阶段，划分开发，测试和部署
FROM base as test
CMD ["./mvnw", "test"]

# 在开发阶段提供jvm远程调试
FROM base as development
CMD ["./mvnw", "spring-boot:run", "-Dspring-boot.run.profiles=mysql", "-Dspring-boot.run.jvmArguments='-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:8000'"]

FROM base as build
RUN ./mvnw package

FROM eclipse-temurin:17-jre-jammy as production
COPY --from=build /app/target/spring-petclinic-*.jar /spring-petclinic.jar
CMD ["java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/spring-petclinic.jar"]
EXPOSE 8080