FROM tomcat:8.5-alpine
MAINTAINER ardy.dedase@gmail.com

COPY tynan.war /usr/local/tomcat/webapps/
COPY tomcat-users.xml /usr/local/tomcat/conf/
COPY context.xml /usr/local/tomcat/webapps/manager/META-INF/

EXPOSE "8080"

CMD ["catalina.sh", "run"]