FROM alpine:latest

ARG USERNAME=test
ARG PW=1234

# Install necessary packages
RUN apk update && apk add --no-cache \
    wget \
    git \
    vim \
    nano \
    nginx \
    openssh \
    sudo \
    bash \
    shadow  # For usermod command

# Create a directory for SSH daemon to run
RUN mkdir /var/run/sshd

# optional 둘중하나 선택 또는 모두
# root 암호 설정 
RUN echo "root:${PW}" | chpasswd

# 새로운 사용자 생성
RUN adduser -D -s /bin/bash ${USERNAME}
RUN echo "${USERNAME}:${PW}" | chpasswd
RUN adduser ${USERNAME} wheel

# ssh 서버 설정
## sed 커맨드를 이용한 문자열 변경.
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config


# SSH 호스트 키 생성
RUN ssh-keygen -A

# SSH PORT
EXPOSE 22

# Start SSH service
CMD ["/usr/sbin/sshd", "-D"]
# CMD /usr/sbin/sshd && tail -f /dev/null