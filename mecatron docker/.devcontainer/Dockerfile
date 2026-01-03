FROM osrf/ros:humble-desktop-full

# Arguments
ARG USERNAME=ubuntu
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG ROS_DISTRO=humble
ENV ROS_DISTRO=$ROS_DISTRO

# Create non-root user
RUN groupadd --gid $USER_GID $USERNAME && \
    useradd -m -s /bin/zsh --uid $USER_UID --gid $USER_GID $USERNAME && \
    apt-get update && apt-get install -y sudo && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Add to video group
RUN usermod --append --groups video $USERNAME

# Install ROS tools and other dependencies (no need for zsh/starship)
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y \
        git \
        vim \
        python3-pip \
        python3-colcon-common-extensions \
        zsh \
        ros-$ROS_DISTRO-mavros \
        ros-$ROS_DISTRO-mavros-extras \
        ros-$ROS_DISTRO-foxglove-bridge \
        ros-$ROS_DISTRO-geographic-msgs \
        ros-$ROS_DISTRO-generate-parameter-library \
        ros-$ROS_DISTRO-tf2-geometry-msgs && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y libvulkan1 mesa-vulkan-drivers mesa-utils vulkan-tools 
RUN mkdir -p /home/ubuntu/.config/unity3d && chown -R ubuntu:ubuntu /home/ubuntu/.config/unity3d

RUN curl -sS https://starship.rs/install.sh | sh -s -- -y

# Switch to non-root user
USER $USERNAME
WORKDIR /home/$USERNAME

# Source ROS setup in zsh for the user
RUN echo "source /opt/ros/$ROS_DISTRO/setup.zsh" >> ~/.zshrc

# Optional: set zsh as default shell for the user
SHELL ["/usr/bin/zsh", "-c"]
