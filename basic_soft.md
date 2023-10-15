# Базовый софт, который должен быть везде

    export DEBIAN_FRONTEND=noninteractive
    apt-get update
    apt-get install -y htop atop iotop screen tmux mc git nano curl wget g++ build-essential gcc make cmake autoconf automake psmisc pciutils lm-sensors ethtool net-tools mtr-tiny expect moreutils autossh

 * htop
 * atop
 * iotop
 * screen
 * tmux
 * mc
 * git
 * nano
 * curl
 * wget
 * ubuntu specific packages
   * g++ build-essential *(кажется только эти специфичны для ubuntu)
   * gcc make cmake autoconf automake
 * nvm `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`
   * source ~/.bashrc
   * nvm i 16
   * npm i -g iced-coffee-script pnpm
 * psmisc (killall)
 * pciutils (lspci)
 * lm-sensors
   * для centos пакет называется lm_sensors
 * ethtool
 * net-tools (ifconfig, netstat)
 * mtr-tiny
 * expect (https://superuser.com/questions/352697/preserve-colors-while-piping-to-tee)
 * moreutils (sponge)
 * autossh

# Настройки софта
## git

    curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
    echo "source ~/.git-completion.bash" >> ~/.bashrc

## npm

    npm completion >> ~/.bashrc
    source ~/.bashrc

# .bashrc aliases etc
nano ~/.bashrc

    function mkcd() {
      mkdir $1
      cd $1
    }

## screen
nano ~/.screenrc

    vbell on
    vbell_msg ''
    termcapinfo *  vb=:

## sysctl limits

nano /etc/sysctl.conf
    
    fs.inotify.max_user_watches=524288
    fs.file-max=100000000

Применить без перезагрузки `sysctl -p`

## security limits

nano /etc/security/limits.conf

    *   soft    nproc   1000000
    *   hard    nproc   1000000


## locale

    export LANG="en_US.utf8"
    export LANGUAGE="en_US.utf8"
    export LC_ALL="en_US.utf8"
    update-locale LANG="en_US.utf8" LANGUAGE="en_US.utf8" LC_ALL="en_US.utf8"
    localectl set-locale LANG=en_US.utf8

# Composed script (user)

    cp ~/.bashrc ~/.bashrc.bak
    
    export DEBIAN_FRONTEND=noninteractive
    sudo apt-get update
    sudo apt-get install -y htop atop iotop screen tmux mc git nano curl wget g++ build-essential gcc make cmake autoconf automake psmisc pciutils lm-sensors ethtool net-tools mtr-tiny expect moreutils autossh
    
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
    source ~/.bashrc
    nvm i 16
    npm i -g iced-coffee-script pnpm
    
    curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
    echo "source ~/.git-completion.bash" >> ~/.bashrc
    
    npm completion >> ~/.bashrc
    source ~/.bashrc
    
    echo 'function mkcd() { ' >> ~/.bashrc && \
    echo '  mkdir -p "$1" &&' >> ~/.bashrc && \
    echo '  cd "$1"' >> ~/.bashrc && \
    echo '}' >> ~/.bashrc
    source ~/.bashrc
    
    echo 'vbell on' >> ~/.screenrc && \
    echo 'vbell_msg ""' >> ~/.screenrc && \
    echo 'termcapinfo * vb=:' >> ~/.screenrc
    
    echo 'fs.inotify.max_user_watches=524288' | sudo tee -a /etc/sysctl.conf && \
    echo 'fs.file-max=100000000' | sudo tee -a /etc/sysctl.conf
    sudo sysctl -p
    
    echo '*   soft    nproc   1000000' | sudo tee -a /etc/security/limits.conf && \
    echo '*   hard    nproc   1000000' | sudo tee -a /etc/security/limits.conf

