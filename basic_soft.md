# Базовый софт, который должен быть везде

    apt-get install -y htop atop iotop screen tmux mc git nano curl wget g++ build-essential gcc make cmake autoconf automake psmisc pciutils lm-sensors ethtool net-tools mtr-tiny expect

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
 * nvm ( `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash` )
   * source ~/.bashrc
   * nvm i 12
   * npm i -g iced-coffee-script
 * psmisc (killall)
 * pciutils (lspci)
 * lm-sensors
   * для centos пакет называется lm_sensors
 * ethtool
 * net-tools (ifconfig, netstat)
 * mtr-tiny
 * expect (https://superuser.com/questions/352697/preserve-colors-while-piping-to-tee)

# Настройки софта
## git

    curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
    echo "source ~/.git-completion.bash" >> ~/.bashrc

## npm

    npm completion >> ~/.bashrc

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
