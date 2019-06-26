docker network create ms_arch

cd service-discovery
mvn clean package
docker build --no-cache -t "br.com.rstephano/service-discovery:0.1.0" .
docker run -d --name service-discovery -p 8761:8761 --net=ms_arch br.com.rstephano/service-discovery:0.1.0

cd ..

cd awesome-phrases
mvn clean package
docker build --no-cache -t "br.com.rstephano/awesome-phrases:0.1.0" .
docker run -d --name awesome-phrases-1 --net=ms_arch -e EUREKA_URI="http://service-discovery:8761/eureka" br.com.rstephano/awesome-phrases:0.1.0
docker run -d --name awesome-phrases-2 --net=ms_arch -e EUREKA_URI="http://service-discovery:8761/eureka" br.com.rstephano/awesome-phrases:0.1.0