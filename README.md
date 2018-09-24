# configservice
maveni sse ka võimalik kui vaja java parameetrid, profile ja sh 

mvn spring-boot:run -Drun.jvmArguments="-Xmx512m" -Drun.profiles=dev

või ka pom-i

		<plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <jvmArguments>-Xmx64m</jvmArguments>
            </configuration>
        </plugin>
        

dockeri optionid, Dockerfaili
https://medium.com/@matt_rasband/dockerizing-a-spring-boot-application-6ec9b9b41faf
#!/usr/bin/env bash



# You should make these values be dynamic based on your env, but I made

# them static to make the usage a little more obvious:

APPLICATION_TOTAL_MEMORY=512M  # how much memory is available, see /sys/fs/cgroup/memory/* if in Docker

APPLICATION_SIZE_ON_DISK_IN_MB=120  # how big on disk, e.g. `du -h target/*.jar`



# Calculations based on "rules of thumb" from

# https://github.com/dsyer/spring-boot-memory-blog/blob/master/cf.md

loaded_classes=$(($APPLICATION_SIZE_ON_DISK_IN_MB * 400))
stack_threads=$((15 + $APPLICATION_SIZE_ON_DISK_IN_MB * 6 / 10))

docker_opts=$(java-buildback-memory-calculator \
  -loadedClasses $loaded_classes \
  -poolType metaspace \
  -stackThreads $stack_threads \
  -totMemory ${APPLICATION_TOTAL_MEMORY})

java -jar ${application} ${docker_opts}



./java-buildpack-memory-calculator \
    -loadedClasses (400 * appJarInMB) \
    -poolType metaspace \  # hope you are on java8
    -stackThreads (15 + appJarInMB * 6 / 10) \
    -totMemory 512M
    
$ java-buildpack-memory-calculator -loadedClasses 400 -poolType metaspace -stackThreads 300 -totMemory 1024M
-XX:CompressedClassSpaceSize=8085K -XX:MaxDirectMemorySize=10M -XX:MaxMetaspaceSize=15937K -Xss1M -Xmx461352K -XX:ReservedCodeCacheSize=240M


Update 2017/12/04: Jochen Mader (@codepitbull on twitter) suggests some added JVM flags, available in 8u131 in this tweet:
-XX:+UnlockExperimentalVMOptions
-XX:+UseCGroupMemoryLimitForHeap
-XX:MaxRAMFraction=1




https://github.com/dsyer/spring-boot-memory-blog/blob/master/cf.md
$ java-buildpack-memory-calculator-linux -memorySizes='metaspace:64m..' -memoryWeights=heap:75,metaspace:10,native:10,stack:5 -memoryInitials=heap:100%,metaspace:100% -totMemory=128m
-Xmx54613K -XX:MaxMetaspaceSize=64M -Xss568K -Xms54613K -XX:MetaspaceSize=64M


https://dzone.com/articles/how-to-decrease-jvm-memory-consumption-in-docker-u
jetty:9-alpine
docker run -m 600m
openjdk:8-jre-alpine



https://dzone.com/articles/spring-boot-memory-performance


https://www.marccostello.com/memory-analysis-of-a-spring-boot-application-in-docker-lessons-learnt/
CMD ["java", "-jar", "build/libs/{app name}.jar"]
ENTRYPOINT exec java $JAVA_OPTS -jar build/libs/{app name}.jar
