# `zsh` / `oh-my-zsh`

## Notes on installing `oh-my-zsh` on GCE instances

### Preliminary steps

Allow changing the shell without requiring user password.
This is necessary since the user password on a GCE is unknown (afaik).

```(shell)
sudo sed -ri -- 's/auth(\s*?)required(\s*?)pam_shells.so/auth\1sufficient\2pam_shells.so/g' /etc/pam.d/chsh
```

### Download and install zsh and oh-my-zsh

```(shell)
sudo apt-get install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

You should now be automatically brought to `zsh`.

### Adding missing environment variables

```(shell)
echo '
## CUDA
export CUDA_HOME=/usr/local/cuda
export PATH=$PATH:$CUDA_HOME/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_HOME/lib64

## added by Anaconda3 4.3.1 installer
export PATH="/opt/anaconda/anaconda3/bin:$PATH"' >> ~/.zshrc
```

### Set theme to frisk + install zsh-syntax-highlighting

```(shell)
sed -Ei -- 's/ZSH_THEME="[a-z]+"/ZSH_THEME="frisk"/g' ~/.zshrc
mkdir ~/src
cd ~/src
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
source ~/.zshrc
```
