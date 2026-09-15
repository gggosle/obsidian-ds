
22  sudo userdel -r creator
   23  sudo userdel -r destroyer
   24  sudo useradd -m /creator -g universe creator //is this the primary group? 
   25  sudo useradd -md /creator -g universe creator
   26  sudo useradd -md /home/des -g universe destroyer
   27  passwd creator
   28  sudo passwd creator
   29  sudo passwd destroyer

sudo visudo /etc/sudoers

chattr +a /append_only
chattr -a /append_only

chattr +i /immutable

sudo -l -- what the user can do 