FROM openjdk:11-jdk-slim
COPY docker/spring-boot-thin-wrapper-1.0.15.RELEASE.jar /thin/wrapper.jar
COPY pom.xml /thin/pom.xml
WORKDIR /thin
RUN jar cvf pom.jar pom.xml
RUN java -jar wrapper.jar \
      --thin.archive=/thin/pom.jar \
      --thin.dryrun=true \
      --thin.debug=true

FROM openjdk:11-jre-slim
COPY --from=0 /root/.m2 /root/.m2
COPY target/demo-thin-docker-0.0.1-SNAPSHOT.jar /app.jar
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
