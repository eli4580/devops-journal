# ===== AIDE-MEMOIRE LINUX - Cours 1                           

# --- SE REPERER ---
whoami                               # mon compte
hostname                             # nom de la machine
cat /etc/os-release | head -3        # système
uptime                               # depuis quand ça tourne
id                                   # uid, gid, groupes
pwd                                  # dossier actuel
which ls                             # où est le programme

# --- SE DEPLACER ---
ls                                   # lister
ls -la                               # tout lister, en détail (fichiers cachés)
ls -ld /tmp                          # le dossier lui-même
cd dossier                           # entrer dans un dossier
cd ~                                 # aller dans mon home
cd ..                                # remonter d'un niveau
cd -                                 # dossier précédent

# --- FICHIERS ET DOSSIERS ---
mkdir -p mboa/notes mboa/scripts     # créer des dossiers (avec parents)
touch idees.txt                      # créer un fichier vide
echo "texte" > journal.txt           # écrire (ÉCRASE le fichier)
echo "texte" >> journal.txt          # ajouter à la fin
cp source destination                # copier
cp -r dossier dossier2               # copier un dossier
mv fichier ../scripts/               # déplacer / renommer
rm fichier                           # supprimer (pas de corbeille)
rm -r dossier                        # supprimer un dossier

# --- LIRE ---
cat fichier                          # afficher tout
less fichier                         # paginer (espace, /mot, n, q)
head -n 3 fichier                    # 3 premières lignes
tail -n 20 fichier                   # 20 dernières lignes
tail -f fichier                      # suivre en direct (Ctrl+C pour arrêter)
wc -l fichier                        # compter les lignes
history | tail -5                    # 5 dernières commandes

# --- CHERCHER ---
grep -i error /var/log/mboamarket/app.log           # chercher (sans casse)
grep -c ERROR /var/log/mboamarket/app.log           # compter les lignes trouvées
grep "Permission denied" /var/log/mboamarket/app.log
grep -o "commande-[0-9]*" app.log | sort -u         # extraire + trier sans doublons
find /srv/mboamarket -name "*.py"                   # chercher des fichiers
ls /usr/bin | wc -l                                 # enchaîner avec le pipe |

# --- DISQUE ---
df -h /                              # place libre
du -sh /srv/mboamarket               # taille d'un dossier

# --- COPIER ORDI <-> VM (scp, depuis ton ordinateur) ---
scp bienvenue.txt awa@203.0.113.21:~/mboa/notes/    # envoyer
scp awa@203.0.113.21:/var/log/mboamarket/app.log .  # récupérer
scp -r awa@203.0.113.21:~/mboa ./mboa-sauvegarde    # un dossier entier
scp -P 2222 fichier awa@IP:dossier/                 # autre port (P majuscule)

# --- UTILISATEURS ET GROUPES ---
grep mboa /etc/passwd                # voir un compte
getent group sudo                    # membres d'un groupe
sudo su - mboa                       # devenir un autre utilisateur
sudo groupadd equipe                 # créer un groupe
sudo useradd -m -s /bin/bash -G equipe amina        # créer un compte
sudo usermod -aG equipe koffi        # AJOUTER un groupe (toujours -a !)
sudo usermod -L -e 1 paul            # couper un compte (offboarding)
sudo chage -l paul                   # dates du compte
sudo userdel -r paul                 # supprimer un compte

# --- DROITS ---
ls -l fichier                        # voir les droits
stat -c '%a %A' fichier              # droits en chiffres et lettres
sudo chmod 640 fichier               # droits en chiffres
sudo chmod o+r fichier               # ajouter la lecture aux autres
sudo chmod u=rw,g=r,o= fichier       # fixer les 3 paquets
sudo chown root:equipe fichier       # propriétaire:groupe
sudo chmod 2770 /srv/demo/partage    # dossier d'équipe (setgid)
umask                                # droits par défaut
# r=4 w=2 x=1 | 755 dossiers | 644 fichiers | 640 secrets | 600 clé SSH | 777 JAMAIS

# --- TESTER LES DROITS ---
sudo -u jules cat fichier            # tester en tant qu'un autre compte
sudo -u amina cat fichier && echo "OK"
echo "texte" | sudo tee fichier > /dev/null   # écrire en root

# --- SUDO ---
sudo -l -U amina                     # ce qu'amina peut faire avec sudo
sudo journalctl _COMM=sudo -n 5 --no-pager      # journal de sudo
sudo visudo -f /etc/sudoers.d/ma-regle          # écrire une règle sudo
sudo visudo -c                       # vérifier la config sudo
# règle : koffi ALL=(root) NOPASSWD: /usr/local/bin/mboa-deploy

# --- VERIFICATION DU TP ---
sudo labo-verifier

# --- REFLEXES ---
# Tab = compléter | ↑ / Ctrl+R = historique | Ctrl+C = arrêter
man ls                               # documentation (q pour sortir)
ls --help
