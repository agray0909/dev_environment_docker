FROM ubuntu:14.04
MAINTAINER Gray "gray_chen@pegatroncorp.com"

RUN echo '127.0.0.1 localhost.localdomain localhost' >> /etc/hosts
RUN useradd -d /home/gray -m -s /bin/bash gray
RUN echo gray:gray | chpasswd
RUN echo 'gray ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers.d/gray
RUN chmod 0440 /etc/sudoers.d/gray

RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
RUN apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net --recv-key 0xB01FA116
RUN  echo "deb http://archive.ubuntu.com/ubuntu trusty main universe\n" > /etc/apt/sources.list \
  && echo "deb http://archive.ubuntu.com/ubuntu trusty-updates main universe\n" >> /etc/apt/sources.list

RUN apt-get update -qqy
ENV LANGUAGE zh_TW.UTF-8
ENV LANG zh_TW.UTF-8
ENV LC_ALL=zh_TW.UTF-8
RUN /usr/share/locales/install-language-pack zh_CN \
      && locale-gen zh_TW.UTF-8 \
  && dpkg-reconfigure --frontend noninteractive locales \
  && apt-get -qqy --no-install-recommends install language-pack-zh-hans language-pack-zh-hant

RUN apt-get -qqy --no-install-recommends install \
    fonts-ipafont-gothic \
    xfonts-100dpi \
    xfonts-75dpi \
    xfonts-cyrillic \
    xfonts-scalable


RUN apt-get install -qqy locales
RUN echo "Asia/Taipei" > /etc/timezone \
  && dpkg-reconfigure --frontend noninteractive tzdata
RUN locale-gen zh_TW.UTF-8; DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales

RUN apt-get install -qqy --no-install-recommends software-properties-common



RUN add-apt-repository ppa:nilarimogard/webupd8
RUN sed 's/main$/main universe/' -i /etc/apt/sources.list && \
    add-apt-repository ppa:webupd8team/java -y
RUN add-apt-repository ppa:gnome-terminator
RUN apt-get update -qqy
RUN apt-get install -qqy --no-install-recommends \
            curl python2.7 python2.7-dev git sshpass \
            make g++ diffstat texinfo gawk chrpath wget \
            vim python-catkin-pkg cmake python-empy tmux \
            man python-pip pkg-config autoconf libtool \
            libglib2.0-dev tree android-tools-adb usbutils zsh
RUN update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1

RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
    apt-get install -qqy --no-install-recommends oracle-java8-installer libxext-dev libxrender-dev libxtst-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/*
RUN apt-get update -qqy --no-install-recommends && apt-get install -qqy --no-install-recommends libgtk2.0-0 libcanberra-gtk-module terminator

RUN apt-get autoclean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/*

RUN wget -c http://eclipse.c3sl.ufpr.br/technology/epp/downloads/release/neon/1.RC3/eclipse-cpp-neon-1-RC3-linux-gtk-x86_64.tar.gz -O /tmp/eclipse.tar.gz && \
    echo 'Installing eclipse' && \
    tar -xf /tmp/eclipse.tar.gz -C /opt && \
    rm /tmp/eclipse.tar.gz

ADD run /usr/local/bin/eclipse
RUN chmod +x /usr/local/bin/eclipse

RUN curl http://commondatastorage.googleapis.com/git-repo-downloads/repo >/usr/local/bin/repo
RUN chmod a+x /usr/local/bin/repo

RUN sudo ln -sf /bin/bash /bin/sh

# Install Zsh
USER gray
WORKDIR /home/gray
ENV HOME /home/gray
RUN git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh \
      && cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc


RUN mkdir -p ~/project/
ADD dircolors.ansi-light /home/gray/.dircolors

CMD ["bash"]
