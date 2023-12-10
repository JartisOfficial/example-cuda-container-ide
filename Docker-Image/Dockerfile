FROM anibali/pytorch:1.13.0-cuda11.8

USER root
# Set up time zone.
ENV TZ=CET
RUN sudo ln -snf /usr/share/zoneinfo/$TZ /etc/localtime

ARG my_user="py" 
ARG my_password="py"
WORKDIR "/"


ENV DEBIAN_FRONTEND="noninteractive" TZ="Europe/Berlin"

RUN apt-get update && apt-get install -y git openssh-server tmux minicom sudo vim bmon htop mc supervisor

# user
RUN useradd --create-home --shell /bin/bash "$my_user"
RUN echo "$my_user:$my_password" | chpasswd
RUN usermod -aG dialout "$my_user"
RUN usermod -aG sudo "$my_user"

## ssh
## genkey @entrypoint
COPY sshd_config /etc/ssh/sshd_config
RUN mkdir /run/sshd

# ssh:2222
EXPOSE 2222


# install requirements
# Create the environment with virtualenv --system-site-packages
RUN pip install -r requirements.txt


COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY entrypoint.sh /

CMD ["/entrypoint.sh"]
