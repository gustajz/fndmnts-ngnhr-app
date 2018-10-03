FROM fabric8/java-centos-openjdk8-jre:1.5

ENV AB_OFF=true \
    TZ=America/Sao_Paulo \
    SPRING_PROFILES_ACTIVE="docker" \
    JAVA_MAX_MEM_RATIO="38" \
    JAVA_OPTIONS="-Duser.language=pt-BR"

ADD @project.artifactId@-@project.version@.jar $JAVA_APP_DIR/
