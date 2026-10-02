# dotfiles

My personal [oh-my-posh](https://ohmyposh.dev) prompt configs, kept here so I can set up a new machine in a few minutes.

Same look on both systems: dark purple pills, OS label, user, folder, git branch, Python env, execution time and date.

## Structure

```
dotfiles/
├── wsl/
│   └── tiramisu.json      # Ubuntu / WSL (bash)
└── powershell/
    └── tiramisu.json      # Windows (PowerShell)
```

The two files are separate because they differ slightly.

## Requirements

- [oh-my-posh](https://ohmyposh.dev/docs/installation)
- A Nerd Font selected in the terminal profile (needed for the icons). Install one with `oh-my-posh font install`.
- Git

## Setup on Ubuntu / WSL

```bash
# 1. Install oh-my-posh
sudo apt update && sudo apt install -y curl unzip
curl -s https://ohmyposh.dev/install.sh | bash -s

# 2. Get the configs
git clone https://github.com/neptunedev77/dotfiles.git ~/dotfiles

# 3. Install the theme
mkdir -p ~/.config/ohmyposh
cp ~/dotfiles/wsl/tiramisu.json ~/.config/ohmyposh/

# 4. Enable it in bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(oh-my-posh init bash --config ~/.config/ohmyposh/tiramisu.json)"' >> ~/.bashrc
echo 'shopt -s checkwinsize' >> ~/.bashrc

# 5. Reload
exec bash
```

## Setup on Windows (PowerShell)

```powershell
# 1. Install oh-my-posh
winget install JanDeDobbeleer.OhMyPosh

# 2. Get the configs
git clone https://github.com/neptunedev77/dotfiles.git $HOME\dotfiles

# 3. Install the theme (the profile expects this exact name)
Copy-Item $HOME\dotfiles\powershell\tiramisu.json $HOME\jandedobbeleer-custom.json

# 4. Open the profile
notepad $PROFILE
```

Add this line to the profile, save, and open a new terminal:

```powershell
oh-my-posh --config "$HOME\jandedobbeleer-custom.json" init pwsh | Invoke-Expression
```

If `notepad $PROFILE` says the file doesn't exist, create it first with `New-Item -Path $PROFILE -ItemType File -Force`.

## Updating the configs

After changing a theme on a machine, copy it back into the repo and push.

Ubuntu / WSL:

```bash
cp ~/.config/ohmyposh/tiramisu.json ~/dotfiles/wsl/
cd ~/dotfiles && git add . && git commit -m "update wsl theme" && git push
```

PowerShell (from Ubuntu, if using WSL):

```bash
cp "$(wslpath 'C:\Users\anton\jandedobbeleer-custom.json')" ~/dotfiles/powershell/tiramisu.json
cd ~/dotfiles && git add . && git commit -m "update powershell theme" && git push
```

## Troubleshooting

- **Boxes or question marks instead of icons:** the terminal is not using a Nerd Font.
- **`oh-my-posh: command not found` on Ubuntu:** add `~/.local/bin` to `PATH` (step 4 above) and reload.
- **Prompt did not change:** open a new terminal, or run `exec bash` (Ubuntu) / `. $PROFILE` (PowerShell).
- **Right side or long lines wrap after zooming:** press Enter once so the prompt redraws with the new width.
