# Linux Package Manager仓库

Sublime Text 3在Windows和OS X上包含一个自动升级机制，可以快速进行升级。大多数主要发行版都提供了软件包和软件包存储库，而不是违背Linux生态系统的要求。

开发者频道中列出的构建仅适用于许可用户。在购买前评估Sublime Text的用户需要使用Stable的频道。

*   [apt](linux_repositories#apt)\-*Ubuntu, Debian*
*   [pacman](linux_repositories#pacman)\-*Arch*
*   [yum](linux_repositories#yum)\-*CentOS*
*   [dnf](linux_repositories#dnf)\-*Fedora*
*   [zypper](linux_repositories#zypper)\-*openSUSE*

## apt

安装GPG密钥：

~~~
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

~~~

确保apt已设置为使用https源：

~~~
sudo apt-get install apt-transport-https

~~~

选择要使用的频道：

Stable

~~~
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

~~~

Dev

~~~
echo "deb https://download.sublimetext.com/ apt/dev/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

~~~

更新apt源并安装Sublime Text

~~~
sudo apt-get update
sudo apt-get install sublime-text

~~~

## Debian

安装GPG密钥：

~~~
curl -O https://download.sublimetext.com/sublimehq-pub.gpg && sudo pacman-key --add sublimehq-pub.gpg && sudo pacman-key --lsign-key 8A8F901A && rm sublimehq-pub.gpg

~~~

选择要使用的频道：

Stable

~~~
echo -e "\n[sublime-text]\nServer = https://download.sublimetext.com/arch/stable/x86_64" | sudo tee -a /etc/pacman.conf

~~~

Dev

~~~
echo -e "\n[sublime-text]\nServer = https://download.sublimetext.com/arch/dev/x86_64" | sudo tee -a /etc/pacman.conf

~~~

更新pacman并安装Sublime Text

~~~
sudo pacman -Syu sublime-text

~~~

## yum

安装GPG密钥：

~~~
sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg

~~~

选择要使用的频道：

Stable

~~~
sudo yum-config-manager --add-repo https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo

~~~

Dev

~~~
sudo yum-config-manager --add-repo https://download.sublimetext.com/rpm/dev/x86_64/sublime-text.repo

~~~

更新yum并安装Sublime Text

~~~
sudo yum install sublime-text

~~~

## DNF

安装GPG密钥：

~~~
sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg

~~~

选择要使用的频道：

Stable

~~~
sudo dnf config-manager --add-repo https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo

~~~

Dev

~~~
sudo dnf config-manager --add-repo https://download.sublimetext.com/rpm/dev/x86_64/sublime-text.repo

~~~

更新dnf并安装Sublime Text

~~~
sudo dnf install sublime-text

~~~

## zypper

安装GPG密钥：

~~~
sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg

~~~

选择要使用的频道：

Stable

~~~
sudo zypper addrepo -g -f https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo

~~~

Dev

~~~
sudo zypper addrepo -g -f https://download.sublimetext.com/rpm/dev/x86_64/sublime-text.repo

~~~

更新zypper并安装Sublime Text

~~~
sudo zypper install sublime-text
~~~