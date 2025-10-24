# TP3 – Réseau
**Cours : Réseau**  
**Travail pratique 3 : Installation et configuration de vsftpd**

---

## 1. Installation de vsftpd
On met à jour le système et on installe vsftpd : 
- `sudo apt update`
- `sudo apt install vsftpd -y`

On crée un dossier pour l’FTP anonyme :  
- `sudo mkdir -p /srv/ftp/root_anon`

## 2. Ajout de fichiers dans le FTP anonyme
On crée des fichiers pour tester : 
- `echo "Ha ha, blague simple !" | sudo tee /srv/ftp/root_anon/UneBlague.txt`
- `echo "J’aime les pâtes." | sudo tee /srv/ftp/root_anon/MonMetFavoris.txt`

On crée par la suite un sous-dossier pour d’autres fichiers selon les dires du prof :  
- `sudo mkdir -p /srv/ftp/root_anon/MyMemes`
- `sudo touch /srv/ftp/root_anon/MyMemes/MonMeme.png`
- `sudo touch /srv/ftp/root_anon/MyMemes/audioHumoristique.mp3`
  
On applique les permissions de l'anonyme (lecture seulement) :  
- `sudo chmod 555 /srv/ftp/root_anon`

## 3. Configuration de vsftpd
On édite le fichier de configuration pour qu'un utilisateur anonyme utilise FTP :
- `sudo nano /etc/vsftpd.conf`

On redémarre et active le service vsftpd :  
- `sudo systemctl restart vsftpd`
- `sudo systemctl enable vsftpd`

## 4. Configuration du firewall
On installe firewalld et on le démarre :  
- `sudo apt-get install firewalld`
- `sudo systemctl enable firewalld`
- `sudo systemctl start firewalld`

On ouvre les ports FTP et recharge le firewall :
- `sudo firewall-cmd --zone=public --add-port=20/tcp --permanent`
- `sudo firewall-cmd --zone=public --add-port=21/tcp --permanent`
- `sudo firewall-cmd --reload`
- `sudo firewall-cmd --list-all`

## 5. Vérification de la structure du dossier FTP
 Dans mon cas, j'ai installé tree pour visualiser l'arborescence du dossier root_anon - `sudo apt install tree`

On vérifie la structure : 
-`tree /srv/ftp/root_anon`

Résultat attendu :
/srv/ftp/root_anon
├── MonMetFavoris.txt
├── MyMemes
│   ├── MonMeme.png
│   └── audioHumoristique.mp3
└── UneBlague.txt


---
