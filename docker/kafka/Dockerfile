FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# 1. 필수 시스템 도구 및 Java 설치
RUN apt-get update && \
    apt-get install -y \
        openjdk-8-jdk \
        curl \
        wget \
        net-tools \
        iputils-ping \
        dnsutils \
        vim \
        lsof \
        netcat \
        unzip \
        tar \
        git && \
    apt-get clean

# 2. Kafka 다운로드 및 설치
RUN wget https://dlcdn.apache.org/kafka/3.9.0/kafka_2.12-3.9.0.tgz && \
    tar -xzf kafka_2.12-3.9.0.tgz && \
    mv kafka_2.12-3.9.0 /usr/local/kafka && \
    rm kafka_2.12-3.9.0.tgz

# 3. Kafka 설정 파일 내 zookeeper 주소 수정 (localhost → zookeeper)
RUN sed -i 's|localhost:2181|zookeeper:2181|' /usr/local/kafka/config/server.properties

# 4. Kafka 환경 변수 등록
ENV KAFKA_HOME=/usr/local/kafka
ENV PATH="$KAFKA_HOME/bin:$PATH"

# 5. 작업 디렉토리 설정
WORKDIR /usr/local/kafka

# 6. Kafka 서버용 포트 개방
EXPOSE 9092

# 7. Kafka 브로커 실행
CMD ["kafka-server-start.sh", "/usr/local/kafka/config/server.properties"]
