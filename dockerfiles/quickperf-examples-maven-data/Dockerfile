FROM quay.io/eclipse/che-java11-maven:nightly as builder
ENV MAVEN_OPTS='-XX:MaxRAMPercentage=50 -XX:+UseParallelGC -XX:MinHeapFreeRatio=10 -XX:MaxHeapFreeRatio=20 -XX:GCTimeRatio=4 -XX:AdaptiveSizePolicyWeight=90 -Dsun.zip.disableMemoryMapping=true -Xms20m -Djava.security.egd=file:/dev/./urandom -Duser.home=/home/user' \
    MAVEN_CONFIG='' \
    JAVA_OPTS='-XX:MaxRAMPercentage=50 -XX:+UseParallelGC -XX:MinHeapFreeRatio=10 -XX:MaxHeapFreeRatio=20 -XX:GCTimeRatio=4 -XX:AdaptiveSizePolicyWeight=90 -Dsun.zip.disableMemoryMapping=true -Xms20m -Djava.security.egd=file:/dev/./urandom' \
    JAVA_TOOL_OPTIONS='-XX:MaxRAMPercentage=50 -XX:+UseParallelGC -XX:MinHeapFreeRatio=10 -XX:MaxHeapFreeRatio=20 -XX:GCTimeRatio=4 -XX:AdaptiveSizePolicyWeight=90 -Dsun.zip.disableMemoryMapping=true -Xms20m -Djava.security.egd=file:/dev/./urandom'    
    
COPY . /quickperf-examples/
WORKDIR /quickperf-examples
RUN env && \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f jvm-junit4/pom.xml -Dtest=org.quickperf.jvm.JvmAnnotationsJunit4Test || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f jvm-junit5/pom.xml -Dtest=org.quickperf.jvm.JvmAnnotationsJunit5Test || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f jvm-testng/pom.xml -Dtest=org.quickperf.jvm.JvmAnnotationsTestNGTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f hibernate-junit4/pom.xml -Dtest=org.quickperf.sql.HibernateJUnit4Test || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f hibernate-junit5/pom.xml -Dtest=org.quickperf.sql.HibernateJUnit5Test || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f hibernate-testng/pom.xml -Dtest=org.quickperf.sql.HibernateTestNGTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f springboot-junit4/pom.xml -Dtest=football.controller.PlayerControllerTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f springboot-junit4/pom.xml -Dtest=football.service.PlayerServiceTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f springboot-junit4/pom.xml -Dtest=football.repository.PlayerRepositoryTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f springboot-junit5/pom.xml -Dtest=football.controller.PlayerControllerTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f springboot-junit5/pom.xml -Dtest=football.service.PlayerServiceTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f springboot-junit5/pom.xml -Dtest=football.repository.PlayerRepositoryTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f micronaut-data-jdbc/pom.xml -Dtest=org.quickperf.micronaut.micronauttest.service.PlayerServiceTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f micronaut-data-jdbc/pom.xml -Dtest=org.quickperf.micronaut.micronauttest.controller.PlayerControllerTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f quarkus/pom.xml -Dtest=org.quickperf.quarkus.quarkustest.service.PlayerServiceTest || \
    mvn clean install -Dmaven.test.skip=false -Dmaven.test.failure.ignore -f quarkus/pom.xml -Dtest=org.quickperf.quarkus.quarkustest.controller.PlayerControllerTest || \
    true;
FROM registry.access.redhat.com/ubi8/ubi-minimal
COPY --from=builder /home/user/.m2/repository /work/m2repo
COPY dockerfiles/quickperf-examples-maven-data/data-copy.sh /data-copy.sh
RUN  chmod 775 /data-copy.sh
CMD ["/data-copy.sh"]


