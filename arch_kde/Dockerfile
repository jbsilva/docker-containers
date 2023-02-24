FROM lscr.io/linuxserver/baseimage-rdesktop-web:arch
MAINTAINER "Julio Batista Silva <docker@juliobs.com>"

RUN set -ex; \
#
  echo "**** Fixing /etc permission ****" && \
  chmod 755 /etc && \
#
  echo "**** SynologyPhotos group ****" && \
  groupadd SynologyPhotos --gid 105733 && \
  usermod -a -G SynologyPhotos abc && \
#
  echo "**** Pacman config ****" && \
  sed -i -e '/#\[testing\]/i [kde-unstable]\nInclude = /etc/pacman.d/mirrorlist\n' /etc/pacman.conf && \
  sed -i -e '/# Misc options/a ILoveCandy' /etc/pacman.conf && \
  sed -i -e 's/^# Color/Color/' /etc/pacman.conf && \
#
  echo "**** Reflector ****" && \
  pacman -Sy --needed --noconfirm \
    reflector && \
  reflector \
    --country Germany,France \
    --verbose \
    --latest 100 \
    --protocol http,https \
    --sort rate \
    --save /etc/pacman.d/mirrorlist && \
#
  echo "**** Updating packages ****" && \
  pacman-db-upgrade && \
  pacman -Syyu --noconfirm && \
#
  echo "**** Installing packages ****" && \
  pacman -Sy --needed --noconfirm \
    ack \
    base-devel \
    binutils \
    byobu \
    darktable \
    digikam \
    discover \
    dolphin \
    exiv2 \
    firefox \
    gdb \
    git \
    go \
    hugin \
    hugo \
    imagemagick \
    jq \
    kate \
    kitty \
    konsole \
    make \
    mariadb \
    neovim \
    noto-fonts \
    p7zip \
    perl \
    perl-image-exiftool \
    plasma-desktop \
    pyenv \
    python \
    python-pip \
    rawtherapee \
    renameutils \
    sudo \
    ttf-hack-nerd \
    wget \
    zsh && \
#
  ln -s \
    /usr/bin/vendor_perl/exiftool \
    /usr/bin/exiftool && \
#
  echo "**** Poetry ****" && \
  mkdir -p /opt/poetry && \
  curl -sSL https://install.python-poetry.org | POETRY_HOME=/opt/poetry python3 - && \
#
  echo "**** Installing Yay ****" && \
  mkdir -p /config/build && \
  chown -R abc:abc /config && \
  cd /config/build && \
  sudo -u abc git clone --depth 1 https://aur.archlinux.org/yay.git && \
  cd yay && \
  sudo -u abc makepkg -si --noconfirm --rmdeps && \
  rm -rf /config/build && \
#
  echo "**** Installing from AUR ****" && \
  sudo -u abc yay -S --noconfirm --noprogressbar --needed \
    ttf-meslo-nerd-font-powerlevel10k && \
#
  echo "**** Cleanup ****" && \
  pacman -Scc --noconfirm && \
  rm -rf \
    /tmp/* \
    /var/cache/pacman/pkg/* \
    /var/lib/pacman/sync/* 

# add local files
COPY /root /

# ports and volumes
EXPOSE 3000

VOLUME /config
