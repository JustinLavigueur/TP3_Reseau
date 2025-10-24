# TP3 – Réseau
**Cours : Réseau**  
**Travail pratique 3 : Installation et configuration de vsftpd**

---

## 1. Installation de vsftpd
Mettre à jour le système et installer vsftpd : 
`sudo apt update`
`sudo apt install vsftpd -y`


sudo mkdir -p /srv/ftp/root_anon

echo "Ha ha, blague simple !" | sudo tee /srv/ftp/root_anon/UneBlague.txt
echo "J’aime les pâtes." | sudo tee /srv/ftp/root_anon/MonMetFavoris.txt
Ha ha, blague simple !
J’aime les pâtes.

sudo mkdir -p /srv/ftp/root_anon/MyMemes
sudo touch /srv/ftp/root_anon/MyMemes/MonMeme.png
sudo touch /srv/ftp/root_anon/MyMemes/audioHumoristique.mp3

sudo chmod 555 /srv/ftp/root_anon

sudo nano /etc/vsftpd.conf

sudo systemctl restart vsftpd

sudo systemctl enable vsftpd

sudo apt-get install firewalld
 sudo systemctl enable firewalld
 sudo systemctl start firewalld

 sudo firewall-cmd --zone=public --add-port=20/tcp --permanent
 sudo firewall-cmd --zone=public --add-port=21/tcp --permanent
 sudo firewall-cmd --reload
 sudo firewall-cmd --list-all

 Petite plus : Tree 
 sudo apt install tree

 ubuntu@instance-20251023-0727:~$ tree /srv/ftp/root_anon
/srv/ftp/root_anon
├── MonMetFavoris.txt
├── MyMemes
│   ├── MonMeme.png
│   └── audioHumoristique.mp3
└── UneBlague.txt
