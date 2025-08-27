https://help.ovhcloud.com/csm/fr-vps-security-tips?id=kb_article_view&sysparm_article=KB0047708


# ❗❗❗⚠ Attention !
Probleme au lancement de fail2ban.
Si on utilise systemd pour gérer les logs alors failt2ban vas avoir un status failed au `sytemctl status fail2ban`
Pour resoudre se probleme il suffit de changer dans le fichier :

```bash
sudo nano /etc/fail2ban/jail.conf

# Acien contenu
[sshd]
port    = 16112 # <------ il ne faut pas oublier de modifier ça
logpath = %(sshd_log)s
backend = %(sshd_backend)s # <------- a modifier


[sshd]
port    = 16112
logpath = %(sshd_log)s
backend = systemd # <------ 
```

Avec ça le fail2ban sera où aller lire les logs pour faire son travail.

## Objectif

Fail2ban est un framework de prévention contre les intrusions dont le but est de bloquer les adresses IP depuis lesquelles des bots ou des attaquants tentent de pénétrer dans votre système.  
Ce paquet est recommandé, voire indispensable dans certains cas, pour protéger votre serveur des attaques de types _Brute Force_ ou _Denial of Service_

## Install

```bash
sudo apt install fail2ban
```

## Configure

Ovrire le fichier de text : 

```bash
sudo nano /etc/fail2ban/jail.local
```

Prenez soin de lire les informations en haut du fichier, notamment les commentaires sous `[DEFAULT]`.

Les paramètres `[DEFAULT]` sont globaux et s'appliqueront donc à tous les services définis pour être activés (`enabled`) dans ce fichier.

Il est important de savoir que les paramètres globaux ne seront pris en compte que s'il n'y a pas de valeurs différentes définies dans les sections services (`JAILS`) plus bas dans le fichier.

Prenons pour exemple ces lignes sous `[DEFAULT]` :

```console
bantime  = 10m
maxretry = 5
enabled = false
```

Cela signifie qu'une adresse IP à partir de laquelle un hôte tente de se connecter sera bloquée pendant dix minutes après la cinquième tentative d'ouverture de session infructueuse.  
De plus, tous les paramètres spécifiés par `[DEFAULT]` et dans les sections suivantes restent désactivés sauf si la ligne `enabled = true` est ajoutée pour un service (listée ci-dessous `# JAILS`).

À titre d’exemple d’utilisation, le fait d’avoir les lignes suivantes dans la section `[sshd]` activera des restrictions uniquement pour le service OpenSSH :

```console
[sshd]
enabled = true
port = ssh
filter = sshd
maxretry = 3
findtime = 5m
bantime  = 30m
```

Dans cet exemple, si une tentative de connexion SSH échoue trois fois en cinq minutes, la période d’interdiction des IP sera de 30 minutes.

Vous pouvez remplacer "ssh" par le numéro de port réel si vous l'avez modifié.

La meilleure approche consiste à activer Fail2ban uniquement pour les services qui sont réellement exécutés sur le serveur. Chaque paramètre personnalisé ajouté sous `# JAILS` sera alors prioritaire sur les valeurs par défaut.

Une fois vos modifications terminées, enregistrez le fichier et fermez l'éditeur.

Redémarrez le service pour vous assurer qu'il s'exécute avec les personnalisations appliquées :

1. Commande recommandée avec `systemctl` :

```bash
sudo systemctl restart fail2ban
```

2. Commande avec `service` (ancienne méthode, toujours compatible) :

```bash
sudo service fail2ban restart
```

Fail2ban dispose de nombreux paramètres et filtres de personnalisation ainsi que d’options prédéfinies, par exemple lorsque vous souhaitez ajouter une couche de protection à un serveur web Nginx.

Pour toute information complémentaire et pour des recommandations concernant Fail2ban, n'hésitez pas à consulter [la documentation officielle](https://www.fail2ban.org/wiki/index.php/Main_Page) de cet outil.