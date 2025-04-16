FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# 1. 필수 패키지 설치 + PostgreSQL client 추가
RUN apt-get update && \
    apt-get install -y \
    build-essential \
    gcc \
    make \
    curl \
    git \
    openjdk-17-jdk \
    postgresql-client \
    python3.11 \
    python3.11-dev \
    python3.11-venv \
    python3-pip && \
    apt-get clean

# 2. 작업 디렉토리 이동
WORKDIR /root

# 3. requirements.txt, .env, test_codes 복사
COPY requirements.txt ./
COPY ../../env ./
COPY test_codes/ ./test_codes/
COPY ingest_codes/ ./ingest_codes/

# 4. JAVA_HOME 설정 + 가상환경 생성 및 의존성 설치
RUN echo 'export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which javac))))' >> /etc/profile && \
    echo 'export PATH=$JAVA_HOME/bin:$PATH' >> /etc/profile && \
    python3.11 -m venv venv && \
    JAVA_HOME=$(dirname $(dirname $(readlink -f $(which javac)))) && \
    PATH=$JAVA_HOME/bin:$PATH && \
    export JAVA_HOME && export PATH && \
    venv/bin/pip install --upgrade pip && \
    venv/bin/pip install -r requirements.txt

# 5. bash 진입
CMD ["/bin/bash"]
