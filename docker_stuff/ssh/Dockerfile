FROM ubuntu:latest

RUN apt-get update && apt-get install -y openssh-server vim

COPY sshd_config /etc/ssh/sshd_config

RUN mkdir /var/run/sshd

RUN echo 'root:root' | chpasswd


# run user-creation and home-directory management script
RUN mkdir -p /home
COPY initial_home_contents /home
COPY create_users.sh create_users.sh
RUN chmod +x create_users.sh && /create_users.sh > out.txt
RUN rm create_users.sh

# PE to groot
RUN chmod 1777 /home/common_playground
RUN chown Groot:Groot /home/common_playground/groot_is_learning_playground.py
RUN chmod 765 /home/common_playground/groot_is_learning_playground.py


# PE Groot -> Drax

# cronjob for Drax -> Rocket privilege escalation
RUN apt-get install -y cron
RUN chmod 775 /home/Rocket/get_the_juice.sh
RUN chown Rocket:Rocket /home/Rocket/get_the_juice.sh
ADD crontab /etc/cron.d/hello-cron
RUN chmod 0644 /etc/cron.d/hello-cron
RUN crontab /etc/cron.d/hello-cron
RUN touch /var/log/cron.log
CMD cron && tail -f /var/log/cron.log
RUN echo "1 * * * * su - Gamora -c \"/home/Rocket/get_the_juice.sh\"" | crontab -
RUN service cron start


EXPOSE 22

# set entrypoint
CMD ["/usr/sbin/sshd", "-D"]
