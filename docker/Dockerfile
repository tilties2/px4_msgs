FROM ros:humble as base

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install --no-install-recommends -y \
    -y wget apt-utils git openssh-server nano bc 
    
RUN apt-get install -y --reinstall ca-certificates

# zsh
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.2/zsh-in-docker.sh)" -- \
    -t agnoster \
    -p git \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-syntax-highlighting 


# color bash
RUN sed -i 's/#force_color_prompt=yes/force_color_prompt=yes/g' ~/.bashrc

# fzf
WORKDIR /root/third_parties
RUN git clone https://github.com/junegunn/fzf.git
WORKDIR /root/third_parties/fzf
RUN /bin/zsh -c "./install"
WORKDIR /root

# --------------------------------------------
FROM base as ros2
WORKDIR /root/ros2_ws/src
WORKDIR /root/ros2_ws
RUN echo "source /opt/ros/$ROS_DISTRO/setup.zsh" >> ~/.zshrc
RUN /bin/zsh -c "source /opt/ros/$ROS_DISTRO/setup.zsh && colcon build"
RUN echo "source /root/ros2_ws/install/setup.zsh" >> ~/.zshrc
