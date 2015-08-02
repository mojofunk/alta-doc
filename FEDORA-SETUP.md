# Fedora 21 Devel setup notes

copy .ssh folder to ~/

sudo yum install vim

git clone git@github.com:mojofunk/dotfiles.git

cd dotfiles && ./install.sh

sudo yum groupinstall 'C Development Tools and Libraries'

sudo yum-builddep ardour3

for alta

sudo yum install portaudio-devel

sudo yum install portmidi-devel

sudo yum install clang
