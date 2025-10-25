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

On met ce contenu suivant dans le fichier de configuration ftp (On va devoir le changer pour les utilisateurs locaux plus tard)

### 🧩 Configuration FTP Anonyme
**Auteur : Justin**

Ce fichier décrit la configuration du serveur **vsftpd** pour permettre un accès **anonyme en lecture seule**.

#### ⚙️ Tableau de configuration

| Directive | Description |
|------------|--------------|
| `listen=YES` | Active le service FTP sur IPv4. |
| `anonymous_enable=YES` | Autorise la connexion anonyme (sans nom d’utilisateur ni mot de passe). |
| `local_enable=NO` | Désactive l’accès FTP des utilisateurs locaux du système (ex. ubuntu, writer, reader). |
| `write_enable=NO` | Interdit toute action d’écriture, de suppression ou de renommage. |
| `anon_root=/srv/ftp/root_anon` | Définit le dossier racine pour l’utilisateur anonyme. |
| `anon_upload_enable=NO` | Empêche l’anonyme de téléverser (uploader) des fichiers. |
| `anon_mkdir_write_enable=NO` | Empêche la création de nouveaux dossiers. |
| `anon_other_write_enable=NO` | Bloque toute autre action nécessitant l’écriture. |
| `dirmessage_enable=YES` | Active les messages automatiques dans les répertoires. |
| `use_localtime=YES` | Utilise l’heure locale du serveur pour les journaux et transferts. |
| `xferlog_enable=YES` | Active la journalisation (logs) des connexions et transferts FTP. |
| `ftpd_banner=Bienvenue sur le serveur FTP anonyme de Justin !` | Message affiché lors de la connexion FTP. |
| `listen_ipv6=NO` | Désactive l’écoute sur IPv6 (utilise seulement IPv4). |

✅ **Résumé :**  
Cette configuration permet à quiconque de se connecter en FTP en mode anonyme (`ftp -A <adresse_IP>`),  
de **lire uniquement** les fichiers présents dans `/srv/ftp/root_anon`,  
sans pouvoir les modifier, supprimer ni en créer de nouveaux.


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
 Dans mon cas, j'ai installé tree pour visualiser l'arborescence du dossier root_anon:
 - `sudo apt install tree`

On vérifie la structure : 
- `tree /srv/ftp/root_anon`

Résultat attendu :
/srv/ftp/root_anon
├── MonMetFavoris.txt
├── MyMemes/
│   ├── MonMeme.png
│   └── audioHumoristique.mp3
└── UneBlague.txt


---
