# configservice
maveni sse ka v�imalik kui vaja java parameetrid, profile ja sh 

mvn spring-boot:run -Drun.jvmArguments="-Xmx512m" -Drun.profiles=dev

v�i ka pom-i

		<plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <jvmArguments>-Xmx64m</jvmArguments>
            </configuration>
        </plugin>
        