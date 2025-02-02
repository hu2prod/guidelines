# Базовый софт, который должен быть везде

    export DEBIAN_FRONTEND=noninteractive
    apt-get update
    apt-get install -y htop atop iotop screen tmux mc git nano curl wget g++ build-essential gcc make cmake autoconf automake psmisc pciutils lm-sensors ethtool net-tools mtr-tiny expect moreutils autossh pkgconf jq

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
   * nvm i 18
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
 * pkgconf
 * jq

# Настройки софта
## git

    curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
    echo "source ~/.git-completion.bash" >> ~/.bashrc

## npm

    npm completion >> ~/.bashrc
    source ~/.bashrc

## .bashrc aliases etc
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
    *   soft    nofile   1000000
    *   hard    nofile   1000000


## locale

    export LANG="en_US.utf8"
    export LANGUAGE="en_US.utf8"
    export LC_ALL="en_US.utf8"
    update-locale LANG="en_US.utf8" LANGUAGE="en_US.utf8" LC_ALL="en_US.utf8"
    localectl set-locale LANG=en_US.utf8


## git clone unique
nano ~/.screenrc
```
gcu() {
    # Check if URL is provided
    if [ -z "$1" ]; then
        echo "A GitHub repository URL is required"
        return 1
    fi

    # Extract the contributor and repo name from the URL
    local repo="$1"
    local contributor=$(echo $repo | cut -d'/' -f4)
    local repo_name=$(echo $repo | cut -d'/' -f5)

    # Form the new directory name
    local dir_name="${contributor}_${repo_name}"

    # Clone the repository into the new directory
    git clone --recursive $repo $dir_name
}

git_canon() {
    # Check if directory is provided
    if [ -z "$1" ]; then
        echo "A directory name is required."
        return 1
    fi

    local dir="$1"

    # Check if directory exists
    if [ ! -d "$dir" ]; then
        echo "The specified directory does not exist."
        return 1
    fi

    # Save the current working directory
    local original_dir=$(pwd)

    # Navigate to the repository directory
    cd "$dir"

    # Check if it's a git repository
    if [ ! -d ".git" ]; then
        echo "The specified directory is not a git repository."
        # Return to the original directory before exiting
        cd "$original_dir"
        return 1
    fi

    # Get the remote repository URL
    local repo_url=$(git config --get remote.origin.url)

    # Check if remote URL is obtained
    if [ -z "$repo_url" ]; then
        echo "No remote URL found for this repository."
        # Return to the original directory before exiting
        cd "$original_dir"
        return 1
    fi

    # Extract the contributor and repo name from the URL
    local contributor=$(echo $repo_url | sed -n 's#.*/\([^/]*\)/\([^/]*\)\(.git\)*#\1#p')
    local repo_name=$(echo $repo_url | sed -n 's#.*/\([^/]*\)/\([^/]*\)\(.git\)*#\2#p')

    # Form the new directory name and path
    local new_dir_name="${contributor}_${repo_name}"
    local parent_dir=$(dirname "$(pwd)")

    # Check if you are already in a directory with the desired name
    if [ "$(basename "$(pwd)")" = "$new_dir_name" ]; then
        echo "The directory is already named with the canonical name."
        # Return to the original directory before exiting
        cd "$original_dir"
        return 0
    fi

    # Move to the parent directory and rename the repository directory
    cd "$parent_dir"
    mv "$dir" "$new_dir_name"

    echo "The repository has been renamed to $new_dir_name"

    # Return to the original directory
    cd "$original_dir"
}

git_canon_all() {
    # Iterate over all directories in the current path
    for dir in ./*; do
        # Check if it's a directory
        if [ -d "$dir" ]; then
            # Check if the directory is a git repository
            if [ -d "$dir/.git" ]; then
                # Apply the git_canon function
                git_canon "$dir"
            else
                echo "Skipping $dir: Not a git repository."
            fi
        fi
    done
}
```

# Composed script (user)

    cp ~/.bashrc ~/.bashrc.bak
    
    export DEBIAN_FRONTEND=noninteractive
    sudo apt-get update
    sudo apt-get install -y htop atop iotop screen tmux mc git nano curl wget g++ build-essential gcc make cmake autoconf automake psmisc pciutils lm-sensors ethtool net-tools mtr-tiny expect moreutils autossh pkgconf jq
    
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

