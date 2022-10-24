#_Exercice 1. Installation du serveur_ 
_Configuration de la machine virtuelle_     
1. Commencez par récupérer le fichier d’installation d’Ubuntu Server (ubuntu-xx.yy-live-server-amd64.iso)
(à CPE, il se trouve dans /softwares/sync/VMs/Admin-Système et placez-le dans /var/tmp)  
2. Dans VirtualBox, créez une nouvelle machine virtuelle avec les caractéristiques suivantes :  
- Nom : Ubuntu-Server_[numéro_binôme] où numéro_binôme = [ETI | IRC]-numéro (par exemple :  
Ubuntu-Server_ETI-12)
- Type : Linux  
- Version : Ubuntu (64 bits)  
-  Mémoire : 2048 Mo  
-  Disque dur : nouveau disque, de type VDI, d’une capacité de 10 Go, dynamiquement alloué  
3. Avant de démarrer la machine, ajoutez le support USB 2.0 (il y a parfois des problèmes avec l’USB 3.0
dans VirtualBox)  
4. La machine est configurée ! Démarrez-la : comme aucun système d’exploitation n’est encore installé,
une fenêtre apparaît et demande un ”disque de démarrage”. Choisissez le fichier iso copié à l’étape 1
et validez : le programme d’installation se lance.  
_Installation d’Ubuntu Server_  
Le processus d’installation est relativement simple grâce au nouveau programme d’installation, Subiquity.  
Il vous suffit de suivre les instructions ; la configuration attendue est la suivante :  
- Connexions réseau :
- conserver celle proposée par défaut
- pas de proxy
- Disque :
- utiliser le premier choix : ”utiliser le disque entier”
- Profil utilisateur :
- pour le nom de serveur : mettre simplement ”serveur”
- mot de passe : dans le cadre des TP, n’employez pas un mot de passe trop long, car vous serez
souvent amené à le retaper ; veillez aussi à ne pas l’oublier, car vous ne pourrez plus vous
connecter à votre VM !
- import d’une identité SSH : laisser non
- Modules supplémentaires : aucun
L’installation ne devrait prendre que quelques minutes !  
_Clonage de la VM_
Pour les TP sur les services réseau, nous aurons besoin d’une deuxième VM qui fera office de client et
communiquera avec notre serveur. Heureusement, VirtualBox propose un mécanisme de clonage de VM, ce
qui nous épargnera la configuration et l’installation de cette deuxième VM.  
1. si votre VM est lancée, tapez la commande sudo poweroff pour l’éteindre proprement En théorie, seul
l’administrateur a le pouvoir d’éteindre la machine ; l’utilisateur créé lors de l’installation dispose de
certains droits lui permettant de prendre temporairement le rôle d’administrateur : c’est à cela que
sert la commande sudo  
2. Dans VirtualBox, faites un clic droit sur la VM et choisissez Cloner  
3. Paramètres du clone :  
- Nom : ”Ubuntu-Client_[ETI|IRC]-[numéro_binôme]”
- Type : clone intégral
- Instantanés : état actuel
- Politique supplémentaire MAC : Générer de nouvelles adresses MAC pour toutes les interfaces
réseau  
Le clonage peut prendre quelques minutes.  
Exercice 2. Prise en main de l’interpréteur de commandes  
 A tout moment, vous pouvez sortir de la VM pour revenir dans le système hôte en appuyant sur la
touche CTRL (droite) du clavier.  
 Manuel  
1. A l’aide du manuel, identifiez le rôle de la commande which  
man which ⇒ Localise une commande  
2. Quand on consulte une page du manuel, comment peut-on rechercher un terme (par exemple, chercher
le terme option dans la page de manuel de which ?  
/option  
3. Comment quitte-t-on le manuel ?  
Touche Q  
4. Chaque section du manuel a une première page, qui présente le contenu de la section. Afficher la
première page de la section 6 ; de quoi parle cette section ?  
man 6 intro  
 Navigation dans l’arborescence des fichiers  
1. allez dans le dossier /var/log  
cd /var/log  
2. remontez dans le dossier parent (/var) en utilisant un chemin relatif  
cd /var  
3. retournez dans le dossier personnel  
cd  
4. revenez au dossier précédent (/var) sans utiliser de chemin  
cd -  
5. essayez d’accéder au dossier /root ; que se passe-t-il ?  
Impossible : accès refusé  
6. essayez la commande sudo cd /root ; que se passe-t-il ? Expliquez  
Ca ne marche pas car la commande sudo ne s’applique pas aux primitives du Shell, comme
cd (voir type cd)  
7. à partir de votre dossier personnel, créez l’arborescence suivante :  
Utiliser mkdir et touch  
8. revenez dans votre dossier personnel ; à l’aide de la commande rm, essayez de supprimer Fichier1, puis
Dossier1 ; que se passe-t-il ?  
rm refuse de supprimer Dossier1 car c’est un dossier  
9. quelle commande permet de supprimer un dossier ?  
rmdir ou rm -r  
10. que se passe-t-il quand on applique cette commande à Dossier2 ?  
Proteste car Dossier2 n’est pas vide  
11. comment supprimer en une seule commande Dossier2 et son contenu ?  
rm -rf Dossier2  
 Commandes importantes  
1. Quelle commande permet d’afficher l’heure ? A quoi sert la commande time ?  
date +%T ; time permet de chronométrer un processus  
2. Dans votre dossier personnel, tapez successivement les commandes ls puis la ; que peut-on en déduire
sur les fichiers commençant par un point ?  
Ce sont des fichiers cachés  
3. Où se situe le programme ls ?  
which ls ⇒ dans /bin  
4. Essayez la commande ll. Existe-t-il une entrée de manuel pour cette commande ? Utilisez les commandes alias ou alias pour en savoir plus sur la nature de cette commande.  
C’est un alias (raccourci) pour ls -alF  
5. Quelle commande permet d’afficher les fichiers contenus dans le dossier /bin ?  
ls /bin  
6. Que fait la commande ls .. ?  
Affiche le contenu du dossier parent  
7. Quelle commande donne le chemin complet du dossier courant ?  
pwd  
8. Que fait la commande echo 'bip' > plop exécutée 2 fois ?  
Effacer le fichier plop et écrit bip dedans ; bip n’apparaît donc qu’une seule fois dans le
fichier  
9. Que fait la commande echo 'bip' >> plop exécutée 2 fois ?  
Ajoute deux lignes bip à la fin du fichier plop (et le crée s’il n’existe pas)  
10. Interprétez le comportement de la commande sleep 10 | echo 'toto' ?  
Un pipe n’attend pas la fin de la première commande pour commencer à traiter la
deuxième.  
11. A quoi sert la commande file ? Essayez la sur des fichiers de types différents.  
Affiche le type de fichier  
12. Créez un fichier original qui contient la chaîne Hello Toto ! ; créer ensuite un lien lien_phy vers
ce fichier avec la commande ln original lien_phy. Modifiez à présent le contenu de original et
affichez le contenu de lien_phy : qu’observe-t-on ? Supprimez le fichier original ; quelle conséquence
cela a-t-il sur lien_phy ?  
La modification affecte les deux fichiers (ils sont liés) ; après suppression de original,
lien_phy permet encore d’accéder au fichier car c’est un lien physique  
13. Créez à présent un lien symbolique lien_sym sur lien_phy avec la commande ln -s lien_phy lien_sym.
Modifiez le contenu de lien_phy ; quelle conséquence pour lien_sym ? Et inversement ? Supprimez le
fichier lien_phy ; quelle conséquence cela a-t-il sur lien_sym ?  
La modification affecte les deux fichiers (ils sont liés) ; après suppression de lien_phy,
lien_sym ne permet plus d’accéder au fichier car c’est un lien symbolique  
14. Affichez à l’écran le fichier /var/log/syslog. Quels raccourcis clavier permettent d’interrompre et
reprendre le défilement à l’écran ?  
cat /var/log/syslog ; CTRL + S ; CTRL + Q  
15. Affichez les 5 premières lignes du fichier /var/log/syslog, puis les 15 dernières, puis seulement les
lignes 10 à 20.  
head -5 /var/log/syslog ; tail -15 /var/log/syslog ; head -20 /var/log/syslog | tail -11  
16. Que fait la commande dmesg | less ?  
Affiche page par page les messages émis par le noyau  
17. Affichez à l’écran le fichier /etc/passwd ; que contient-il ? Quelle commande permet d’afficher la page
de manuel de ce fichier ?  
cat /etc/passwd ; contient les mots de passe et autres informations sur les utilisateurs ;  
man 5 passwd (Attention, man passwd affiche l’aide sur la commande système passwd ;   la
documentation sur les fichiers de configuration se trouve dans le section 5 du manuel
18. Affichez seulement la première colonne triée par ordre alphabétique inverse  
cut -d: -f1 /etc/passwd | sort -r  
19. Quelle commande nous donne le nombre d’utilisateurs ayant un compte sur cette machine (pas seulement les utilisateurs connectés) ?  
wc -l /etc/passwd (compte le nombre de lignes du fichier /etc/passwd et donc le nombre
d’utilisateurs  
20. Combien de pages de manuel comportent le mot-clé conversion dans leur description ?  
man -k conversion | wc -l  
21. A l’aide de la commande find, recherchez tous les fichiers se nommant passwd présents sur la machine  
find / -name passwd  
22. Modifiez la commande précédente pour que la liste des fichiers trouvés soit enregistrée dans le fichier
~/list_passwd_files.txt et que les erreurs soient redirigées vers le fichier spécial /dev/null  
find / -name passwd 1> ~/list_passwd_files.txt 2> /dev/null  
23. Dans votre dossier personnel, utilisez la commande grep pour chercher où est défini l’alias ll vu
précédemment  
grep -r "alias ll" .  
24. Utilisez la commande locate pour trouver le fichier history.log.  
locate history.log  
25. Créer un fichier dans votre dossier personnel puis utilisez locate pour le trouver. Apparaît-il ? Pourquoi ?  
Le fichier n’apparaît pas car il n’a pas encore été indexé. On peut forcer l’indexation avec
la commande updatedb  
Travailler avec plusieurs shells  
On peut utiliser jusqu’à 6 shells en parallèle. Ils portent les noms ttyX (où X va de 1 à 6) et sont
accessibles via Alt + FX
Exercice 3. Découverte de l’éditeur de texte nano  
Nano est un éditeur de texte rudimentaire, où toutes les commandes évidemment se font au clavier.  
 Les raccourcis clavier les plus courants sont affichés en bas de l’écran, mais sous une forme peut-être
inhabituelle :  
• ^G signifie Ctrl + G  
• M-U signifie Alt + U  
 Quelques raccourcis utiles :  
F1 ou Ctrl + G Affichage de l’aide  
- Ctrl + X Quitter nano / Fermer une fenêtre / Exécuter une commande
- Ctrl + R Ouvrir un fichier
- Ctrl + O Enregistrer sous
- Ctrl + S Enregistrer
- Ctrl + K Couper
- Ctrl + U Coller
- Ctrl + W Rechercher
- Ctrl + \ Remplacer
- Ctrl + C Afficher des informations sur la position du curseur (numéro de ligne, de colonne)
- Alt + U Annuler
- Alt + E Refaire
1. Copiez le fichier /var/log/syslog dans votre dossier personnel sous le nom log.txt, puis ouvrez-le avec
nano  
2. Remplacez toutes les occurrences du mot kernel par le mot noyau  
3. Déplacer les 10 premières lignes à la fin du fichier  
4. Annulez cette action  
5. Enregistrez le fichier avant de quitter nano  
_Exercice 4. Personnalisation du shell_
Le shell par défaut est plutôt austère, mais il existe de nombreux moyens de le personnaliser, en modifiant
le fichier ~/.bashrc.  
1. Commencez par créer une copie de ce fichier, que vous appellerez .bashrc_bak  
2. Editez le fichier .bashrc avec nano et décommentez la ligne force_color_prompt=yes pour activer
la couleur. Enregistrez le fichier et quittez nano.  
3.  Le fichier .bashrc est lu au démarrage du shell ; pour le recharger, il faudrait donc se déconnecter
puis se reconnecter ; mais il existe un autre moyen : la commande source .bashrc. Testez-la, l’invite
de commande devrait immédiatement passer en couleurs.  
4.  Les couleurs par défauts (surtout celle du dossier courant) ne sont pas très visibles. Dans .bashrc,
cherchez les lignes commençant par PS1= ; elles indiquent la mise en forme de l’invite de commande
(selon que l’on est en couleurs ou non).  
Sur cette ligne, on peut distinguer un certain nombre de raccourcis :  
 -\u Nom de l’utilisateur
 -\h Nom de la machine (hôte)
 -\d Date
 -\t Heure avec les secondes
 -\A Heure sans les secondes
 -\w Chemin complet du dossier courant
Remarquez la séquence particulière : \[\033[1;32m]\u@\h\[\033[00m\] C’est cette instruction qui
indique d’affiche le nom de l’utilisateur et de la machine en vert clair. Plus précisément :  
- un code couleur se place entre \[\033[ et \]
- on peut remplacer \033 par \e (ce n’est pas forcément toujours plus lisible…)
- un code couleur se termine toujours par la lettre m
- le code couleur 00 efface toute mise en forme
- on peut combiner différents attributs (couleur, souligné, clignotant, etc.) en les séparant par des
points virgules  
 La page suivante vous donne les différents codes couleurs possibles : https://misc.flogisoft.com/  
bash/tip_colors_and_formatting
Modifiez l’invite de commande pour qu’elle s’affiche sous la forme suivante :  
[heure] - user@host:chemin_courant$  
où l’heure est affichée en violet et entre crochets, et le chemin du dossier courant en cyan  
_Exercice 5. Pour les plus rapides_  
Faites le tutoriel sur AWK disponible ici : https://www.tutorialspoint.com/awk/index.htm
