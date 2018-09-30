# spring-boot-thin-sandbox

* demo-fat: Normal SpringBoot app.
* demo-thin: Thin app.
* demo-thin-docker: Thin app with docker.

## demo-fat

This is just a simple spring boot app with web.

```
❯ cd demo-fat
❯ ./mvnw clean package
❯ ls -ahl target/demo-fat-0.0.1-SNAPSHOT.jar
-rw-rw-r-- 1 bufferings bufferings 16M Sep 30 22:16 target/demo-fat-0.0.1-SNAPSHOT.jar
```

The size is 16MB.

```
❯ java -jar target/demo-fat-0.0.1-SNAPSHOT.jar
...
2018-09-30 22:29:04.617  INFO 32639 --- [           main] com.example.demo.DemoApplication         : Started DemoApplication in 3.852 seconds (JVM running for 4.464)
...
```

## demo-thin

This is also a simple spring boot app, but with Spring Boot Thin Launcher.

https://github.com/dsyer/spring-boot-thin-launcher

```
❯ cd demo-thin
❯ ./mvnw clean package
❯ ls -ahl target/demo-thin-0.0.1-SNAPSHOT.jar
-rw-rw-r-- 1 bufferings bufferings 11K Sep 30 22:24 target/demo-thin-0.0.1-SNAPSHOT.jar
```

It's only 11KB.

It downloads a launcher and libraries on startup, but actually it uses .m2 directory so it doesn't take so long.

```
❯ java -jar target/demo-thin-0.0.1-SNAPSHOT.jar
...
2018-09-30 22:30:51.689  INFO 1303 --- [           main] com.example.demo.DemoApplication         : Started DemoApplication in 3.012 seconds (JVM running for 4.987)
...
```

## demo-thin-docker

This is a thin app with Docker.

```
❯ cd demo-thin-docker
❯ ./mvnw clean package
❯ docker build -t demo-thin-docker .
```

This takes time to download all the dependencies.

```
❯ docker run -p 8080:8080 demo-thin-docker --thin.debug=true --thin.offline=true 
...
2018-09-30 13:42:32.978  INFO 1 --- [           main] com.example.demo.DemoApplication         : Started DemoApplication in 3.437 seconds (JVM running for 5.804)
...
```

## The good parts with docker

As long as we don't change the dependencies, the layer is reused. So the build finishes in no time.
