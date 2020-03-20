# CR ADMIN SYSTEME TP4 
### Jean-Baptiste BLAISE
### Lucie CHARRIER
### 20 mars 2020

## Exercice 1 : Gestion des utilisateurs et des groupes.
### 1. Commencez par créer deux groupes groupe1 et groupe2
`sudo addgroup groupe1`<br>
`sudo addgroup groupe2`<br>

### 2. Créez ensuite 4 utilisateurs u1,u2,u3,u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell.
La commande useradd ne permet pas de créer un dossier personnel du même nom dans Home, on utilise l'option -m pour faire cette action. On demande également que cette action soit faite avec shell par bash.<br>
`sudo useradd -m u1 --shell /bin/bash`<br>
`sudo useradd -m u2 --shell /bin/bash`<br>
`sudo useradd -m u3 --shell /bin/bash`<br>
`sudo useradd -m u4 --shell /bin/bash`<br>

### 3. Ajoutez les utilisateurs dans les groupes créés : u1, u2, u4 dans groupe1 et u2, u3, u4 dans groupe2.
On utilise la commande usermod pour modifier un compte utilisateur. L'option -a permet d'ajouter l'utilisateur aux groupes supplémentaires et ne s'utilise qu'avec -G. Cette seconde option permet de lister les groupes supplémentaires auquels l'utilisateur appartient.<br>
`sudo usermod -a -G groupe1 u1 `<br>
`sudo usermod -a -G groupe1,groupe2 u2`<br>
`sudo usermod -a -G groupe2 u3`<br>
`sudo usermod -a -G groupe1,groupe2 u4`<br>

### 4. Donnez deux moyens d’aﬀicher les membres de groupe2.
On affiche le contenu du fichier group, en ne selectionnant que groupe2 : <br>
`cat /etc/group | grep groupe2`<br>
Retourne : groupe2 : x : 1002 : u2,u3,u4<br>
On peut également installer le packet members qui, à l'appel, affiche les utilisateurs du groupe demandé :<br>
`members groupe2`<br>
Retourne : u2 u3 u4 <br>

### 5. Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4.
On souhaite donner la propriété à un groupe, on utilise la commande chown.<br>
`sudo chown :groupe1 /home/u1`<br>
`sudo chown :groupe1 /home/u2`<br>
`sudo chown :groupe2 /home/u3`<br>
`sudo chown :groupe2 /home/u4`<br>

### 6. Remplacez le groupe primaire des utilisateurs :groupe1 pour u1 et u2 et groupe2 pour u3 et u4
Afin de changer le groupe primaire d'un utilisateur, on utilise l'option -g.<br>
`sudo usermod -g groupe1 u1`<br>
`sudo usermod -g groupe1 u2`<br>
`sudo usermod -g groupe2 u3`<br>
`sudo usermod -g groupe2 u4`<br>

### 7. Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.
On peut utiliser deux commandes différentes : chown et chgrp qui font la même chose (on le vérifie avec la commande ll).<br>
`cd /home`<br>
<br>
`sudo mkdir groupe1`<br>
`sudo chown :groupe1 /home/groupe1`<br>
<br>
`sudo mkdir groupe2`<br>
`sudo chgrp groupe2 /home/groupe2`<br>

### 8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier?
On applique une modification des droits sur le fichier en fournissant une valeur numérique en octal :<br>
`sudo chmod 660 groupe1`<br>
`sudo chmod 660 groupe2`<br>
Ici on autorise la lecture et l'écriture mais pas l'exécution sur l'utilisateur et le groupe mais pas les autres.<br>

### 9. Pouvez-vous vous connecter en tant que u1 ? Pourquoi?
On utilise la commande switch user : su.<br>
`su u1`<br>
Retourne : Autentification failure.<br>
On ne peut pas se connecter en tant que u1 car aucun mot de passe n'ayant été déclaré, l'utilisateur est inactif.<br> 

### 10. Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.
`sudo passwd u1`<br>
On fixe le nouveau mot de passe sur 'psw'.<br>
Nous pouvons maintenant nous connecter avec u1. Nous sommes identifés dans le terminal comme u1@serveur.<br>

### 11. Quels sont l’uid et le gid de u1 ?
La commande id permet d'obtenir les identifiants d'utilisateur(uid) et de groupe(gid).<br>
`id u1`<br>
Retourne : uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1).

### 12. Quel utilisateur a pour uid 1003 ?
On consulte le fichier passwd pour déterminer l'utilisateur 1003.<br>
`cat /etc/passwd`<br>
On voit que l'utilisateur avec l'uit 1003 est u3.<br>

### 13. Quel est l’id du groupe groupe1 ?
En lisant le contenu de ce même fichier, on trouve que le gid du groupe1 est 1001.<br>

### 14. Quel groupe a pour guid 1002 ? (Rien n’empêche d’avoir un groupe dont le nom serait 1002...)
De même, c'est le groupe2 qui a un gid de 1002.<br>

### 15. Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.
La commande gpasswd permet d'administrer /etc/group et /etc/gshadow. L'option -d noous permet de supprimer un utilisateur du groupe choisi.<br>
`sudo gpasswd -d u3 groupe2`<br>
Retourne : u1 is not in the sudoers file.<br>
Nous ne sommes pas autorisés à supprimer l'utilisateur u3 du groupe2 car u1@serveur, l'utilisateur à partir duquel nous sommes identifiés, ne fait pas parti des superutilisateurs et ne peut donc pas se permettre d'en supprimer un autre.<br>

### 16. Modifiez le compte de u4 de sorte que :
Afin de modifier le compte de u4, on utilisera la page du manuel de usermod et de passwd puis on jouera sur les différentes options.<br>
### — il expire au 1er juin 2020,
`sudo usermod -e 2020-06-01 u4`<br>
### — il faut changer de mot de passe avant 90 jours,
`sudo passwd -x 90 u4`<br>
### — il faut attendre 5 jours pour modifier un mot de passe,
`sudo passwd -n 5 u4`<br>
### — l’utilisateur est averti 14 jours avant l’expiration de son mot de passe,
`sudo passwd -w 14 u4`<br>
### — le compte sera bloqué 30 jours après expiration du mot de passe.
`sudo usermod -f 30 u4`<br>

### 17. Quel est l’interpréteur de commandes (Shell) de l’utilisateur root?
On accède au dossier /etc/passwd et on cherche l'utilisateur root puis on lit ses informations.<br>
`cat /etc/passwd | grep 'root'`<br>
On voit que l'interpréteur Shell de root est bash.<br>

### 18. à quoi correspond l’utilisateur _nobody_ ?
C'est le nom conventionnel d'un compte d'utilisateur à qui aucun fichier n'appartient, qui n'est dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles que tous les "autres utilisateurs" ont. <br>
src : https://fr.wikipedia.org/wiki/Nobody_%28informatique%29<br>

### 19. Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe?
On va voir sur la page `man sudo` et on lit que par défaut le mot de passe est conservé pour 15 minutes. On lit également que `sudo -k` force le superutilisateur à oublier son mot de passe.


## Exercice 2 : Gestion des permissions.
### 1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier _fichier_ contenant quelques lignes de texte. Quels sont les droits sur test et fichier?
On execute `ll | grep 'test'` et `ll | grep 'fichier'`<br>
test : drwxr-xr-x c'est-à-dire lire/écrire/exécuter pour l'utilisateur, lire/exécuter pour le groupe et autres.<br>
fichier : -rw-r--r-- c'est-à-dire lire/écrire pour l'utilisateur et lire uniquement pour le groupe et autres.<br>

### 2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’aﬀicher en tant que root. Conclusion ?
Comme dans l'exercice précédent, on utilise la commande chmod et on applique 000 pour indiquer que ni l'utilisateur, le groupe ou autres ne pourront lire, écricre ou exécuter.<br>
`sudo chmod 000 fichier`<br>
En appelant la commande `ll`, on voit bien que les droits sur fichier sont ---------. On essaie d'ouvrir et de modifier le fichier fichier, les droits sont refusés.<br>
Cependant en se plaçant en tant que root, on arrive à ouvrir fichier, le lire et le modifier, ce qui signifie que tous les droits sont ouverts pour root.<br>

### 3. Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echoHello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?
`sudo chmod 300 fichier`, on redonne les droits d'écriture et d'exécution uniquement à l'utilisateur.<br>
On exécute `echo "echoHello" > fichier`. Bash nous refuse les droits d'écriture sur fichier. <br>
**A FINIR**

### 4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.
`./fichier` nous retourne Permission denied.<br>
`sudo ./fichier` nous retourne ADiplodocus: not found.<br>
**A FINIR**

### 5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou aﬀichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test.
`cd /home`<br>
`sudo chmod 355 test`<br>
`cd test`<br>
`ls`<br>
On voit que test contient le fichier fichier.<br>
`./fichier`<br>
Nous ne sommes pas autorisés à lire le contenu de fichier.<br> On en déduit que refuser le droit de lecture d'un répertoire, implique également le refus de lecture de son contenu, même s'il autorise lui-même la lecture.<br>

### 6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations ?
`sudo touch nouveau`<br>
`sudo mkdir sstest`<br>
On retire maintenant à nouveau et test les droits en écriture (on ne modifie que pour l'utilisateur, on conserve les paramètres par défaut pour groupe et autres).<br>
`sudo chmod 444 nouveau`<br>
`cd ..`<br>
`sudo chmod 155 test`<br>
Comme attendu, on ne peut pas modifier le fichier nouveau. On rétabli ses droits en écriture au répertoire test.<br>
`sudo chmod 355 test`<br>
On constate qu'il n'est toujours pas possible d'écrire dans le fichier nouveau.<br>
`rm nouveau`<br>
Nous ne sommes pas autorisés à supprimer ce fichier. Nous concluons que le droit d'écriture d'un répertoire ne garanti pas le droit d'écriture sur le contenu du répertoire.<br>

### 7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
`cd`<br>
`sudo chmod 255 /home/test`<br>
On tente : `rm /home/test/nouveau`, permission denied est retourné.<br>
On tente : `/home/test/./nouveau`, permission denied est retourné (on a autorisé son exécution au préalable).<br>
On tente : `ls /home/test`, le contenu est bien listé.<br>
On en conclue que retirer le droit d'exécution d'un répertoire retire le droit d'exécution des fichiers contenu mais n'empêche pas d'accéder aux répertoires.<br>

### 8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd..”? Pouvez-vous donner une explication ?
`sudo chmod 355 /home/test`<br>
`cd /home/test`<br>
`sudo chmod 255 /home/test`<br>
On ne peut ni créer un fichier, ni en supprimer un, ni en modifier un. On peut cependant accéder à un repertoire enfant et le lister mais pas créer de fichier.<br>
On en conclue que les droits que l'on possède sur le répertoire courant n'influent que sur ce répertoire et les fichiers, uniquement, qu'il contient.<br>
On peut utiliser la commande cd .. car elle n'est pas liée au répertoire test.<br>

### 9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suﬀisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
`sudo chmod 355 test`<br>
`sudo chmod 340 /test/fichier` donne les droit à groupe en lecture uniquement.<br>

### 10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
`cd`<br>
`umask 077`<br>, seul l'utilisateur a le droit d'écriture, de lecture et d'exécution.<br>
On vérifie que les autres membres du groupe n'ont aucun droit sur mon dossier.<br>

### 11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
`umask 022`<br>
On verifie que tous le monde peut accéder et lire mais seul moi ai le droit d'écrire.<br>

### 12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
`umask 037`<br>
On vérifie que les membres du groupe peuvent seulement lire, et que les autres n'ont aucun droits.<br>

### 13. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :
### - chmod u=rx,g=wx,o=r fic,
`chmod 534 fic`<br>
### - chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---,
`chmod 602 fic`
### - chmod 653 fic en sachant que les droits initiaux de fic sont 711,
`chmod u-x,g+r,o+w fic`
### - chmod u+x, g=w, o-r fic en sachant que les droits initiaux de fic sont r--r-x---.
`chmod 520 fic`
### 14. Aﬀichez les droits sur le programme passwd. Que remarquez-vous ? En aﬀichant les droits du fichier/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?
`ll etc/passwd`<br>
Les droits sont les suivants : -rw-r--r--. L'utilisateur actuel peut lire le fichier et le modifier : il peut changer son mot de passe mais les autres utilisateurs du groupe et autres ne peuvent que lire, ce qui peut être problématique.
