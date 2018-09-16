FROM debian:stretch
MAINTAINER Benedikt Bartscher (benedikt@bartscher.me)
EXPOSE 22

RUN apt-get update
RUN apt-get install -y git gnupg2 apt-transport-https alsa-utils dbus supervisor dirmngr pulseaudio mpg123 python3 python3-pip openssh-server
RUN echo "deb https://debian.snips.ai/stretch stable main" > /etc/apt/sources.list.d/snips.list
RUN apt-key adv --recv-keys F727C778CCB0A455
RUN apt-get update
RUN apt-get install -y snips-platform-voice snips-asr-injection snips-watch

RUN pip3 install toml paho-mqtt boto3 awscli
RUN aws configure set default.region eu-central-1
RUN aws configure set region eu-central-1 --profile testing
RUN aws configure set profile.testing2.region eu-central-1
RUN aws configure set preview.cloudsearch true
RUN git clone https://github.com/Thyraz/snips-tts-polly.git /opt/snips-tts-polly
RUN cp /opt/snips-tts-polly/snips-tts-polly.py /usr/bin

RUN mkdir -p /var/run/sshd && sed -i "s/UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config  && touch /root/.Xauthority
RUN /usr/sbin/sshd -t
ADD credentials /root/.aws/credentials
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord"]

