# Installation

The Aviator command line tool (invoked as `av`) can be used to streamline and automate common tasks within your Git and GitHub workflows. Currently, the tool is used primarily to manage Stacked PRs<mark style="color:blue;">.</mark>

## First-time setup

### 1. Download the command line tool

The `av` tool can be installed in a few ways, depending on your operating system and package manager preferences.

#### Homebrew (macOS)

First, if not already done, [<mark style="color:blue;">install Homebrew</mark>](https://brew.sh/)<mark style="color:blue;">.</mark>

Then, install `av` using the Homebrew tap.

```
brew install aviator-co/tap/av
```

#### Debian / Ubuntu

Download the `.deb` file from [the releases page](https://github.com/aviator-co/av/releases).

```
apt install ./av_$VERSION_linux_$ARCH.deb
```

#### RPM-based systems

Download the `.rpm` file from [the releases page](https://github.com/aviator-co/av/releases).

```
rpm -i ./av_$VERSION_linux_$ARCH.rpm
```

#### Arch Linux (AUR)

Published as [av-cli-bin](https://aur.archlinux.org/packages/av-cli-bin) in AUR.

```
yay av-cli
```

#### Download executable (advanced, all operating systems)

Download the latest `av` executable from the [<mark style="color:blue;">GitHub releases page on the</mark> <mark style="color:blue;">`av`</mark> <mark style="color:blue;">repository</mark>](https://github.com/aviator-co/av/releases). Extract the archive and add the executable to your `PATH`.

### 2. Connect av to GitHub

av CLI interacts with GitHub to create and update PRs. To authenticate with GitHub, it can automatically use [GitHub CLI credential](https://cli.github.com/) if it's available. Alternatively, you can create a Personal Access Token to authenticate with it. See [create-a-user-access-token.md](how-to-guides/create-a-user-access-token.md "mention").

### 3. Initialize your repository

Finally, in the repo where you want to use `av`, make sure to initialize the repository. From within your git repository, run the following command:

```
av init
```

## Upgrade <a href="#upgrade" id="upgrade"></a>

#### Homebrew

Run the following command to upgrade `av` if it was installed with Homebrew.

```
brew upgrade av
```

#### Download executable

Follow the [<mark style="color:blue;">installation instructions above</mark>](installation.md#download-executable-advanced-all-operating-systems) and overwrite the previous `av` binary with the newly downloaded binary.
