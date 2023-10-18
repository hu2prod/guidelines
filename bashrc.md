Extra recipes for bashrc (experimental)

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
```

```
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
```

```
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
