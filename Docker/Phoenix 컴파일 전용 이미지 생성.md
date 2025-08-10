### 1. Dockerfile 파일 생성
```
# Dockerfile

# RedHat 8 최소이미지
FROM registry.access.redhat.com/ubi8/ubi-minimal AS builder

# 필수 패키지 설치
RUN microdnf install -y git make gcc gcc-c++ glibc-langpack-en tar ncurses openssl unzip && microdnf clean all

# Erlang Solutions 공식 저장소 추가 및 Erlang/OTP 27 설치
RUN curl -fsSL https://binaries2.erlang-solutions.com/centos/esl-erlang-27/esl-erlang_27.3.4_1~centos~8_x86_64.rpm -o erlang.rpm && \
    rpm -ivh ./erlang.rpm && \
    rm ./erlang.rpm

# Elixir 1.18.4 (OTP 27) 설치
RUN curl -fsSL https://repo.hex.pm/builds/elixir/v1.18.4-otp-27.zip -o elixir.zip && \
    unzip elixir.zip -d /usr/local/elixir && \
    rm elixir.zip && \
    ln -s /usr/local/elixir/bin/* /usr/local/bin/

# 환경 변수 
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV MIX_ENV=prod

# 작업 디렉토리 설정
WORKDIR /app

# tailwind 컴파일 필요한 파일 tailwind-linux-x64,linux-x64-0.17.11 파일 다운로드
RUN curl -fsSL https://github.com/tailwindlabs/tailwindcss/releases/download/v3.4.3/tailwindcss-linux-x64 -o tailwind-linux-x64 && \ 
    curl -fsSL https://registry.npmjs.org/@esbuild/linux-x64/-/linux-x64-0.17.11.tgz -o linux-x64-0.17.11.tgz && \
    tar -xzf /app/linux-x64-0.17.11.tgz && \ 
    mv /app/package/bin/esbuild /app/esbuild-linux-x64 && \ 
    rm -f *.tgz && \ 
    rm -Rf package && \ 
    chmod 755 *-x64 

## PC버전 .mix, .hex 파일 복사 
#COPY .mix /root/.mix
#COPY .hex /root/.hex

# 테스트 프로그램 복사 /test/_build에  tailwind-linux-x64,linux-x64-0.17.11 파일 넣고 mix.deps, mix phx.server 테스트 할것
#COPY test /app/test

# 이미지 계속 기동되게 설정
CMD ["sleep", "infinity"]
```

### 2. Image 생성
```
# Dockerfile 존재하는 곳에서
# docker build -t 이름:버전

docker build . -t erlang:27.3.4_1-ubi8
```

### 3. 생성된 이미지에 설치된 내용 확인 하기
1) 컨테이너 종료 되면 삭제 되는 실행
```
# erlang:27.3.4.1 이미지 최초 실행이고 실행이 종료되면 container 도 삭제 됨
docker run -it --rm erlang:27.3.4_1-ubi8 /bin/bash
```

2) 컨테이너를 삭제 하지 않고 실행
```
docker container run -d erlang:27.3.4_1-ubi8
172f301cf556e9e92e224139fadc107307821cab6cd9767b5877a2c6e5447762

# 컨테이너의 shell에 접근
docker container exec -it 172 /bin/bash
```

### 4. 오프라인 환경에 사용할 이미지 파일 생성
```
# save -o 파일명 명:태그
docker save -o erlang-27.3.4_1-ubi8.tar erlang:27.3.4_1-ubi8
```