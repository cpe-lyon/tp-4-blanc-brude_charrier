# CR ADMIN SYSTEME TP4 
### Lucie CHARRIER
### 20 mars 2020

## Exercice 1 : Gestion des utilisateurs et des groupes
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

### 16. Modifiez le compte de u4 de sorte que :
### — il expire au 1er juin 2020,
### — il faut changer de mot de passe avant 90 jours,
### — il faut attendre 5 jours pour modifier un mot de passe,
### — l’utilisateur est averti 14 jours avant l’expiration de son mot de passe,
### — le compte sera bloqué 30 jours après expiration du mot de passe.

### 17. Quel est l’interpréteur de commandes (Shell) de l’utilisateur root?

### 18. à quoi correspond l’utilisateur _nobody_ ?

### 19. Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe?


## Exercice 2
### 1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier _fichier_ contenant quelques lignes de texte. Quels sont les droits sur test et fichier?

### 2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’aﬀicher entant que root. Conclusion?

### 3. Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echoHello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’unfichier s’il existe déjà. Que peut-on dire au sujet des droits?

### 4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

### 5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou aﬀichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test.

### 6. Créez dans test un fichie rnouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichie rnouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations ?

### 7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test.Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’enlister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?

### 8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd..”? Pouvez-vous donner une explication ?

### 9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suﬀisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.

### 10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

### 11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.

### 12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.

### 13. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :
### -chmod u=rx,g=wx,o=r fic-chmod uo+w,g-rx ficen sachant que les droits initiaux deficsontr--r-x---
### -chmod 653 ficen sachant que les droits initiaux deficsont711
### -chmod u+x,g=w,o-r ficen sachant que les droits initiaux deficsontr--r-x---

### 14. Aﬀichez les droits sur le programmepasswd. Que remarquez-vous ? En aﬀichant les droits du fichier/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?

### 15.

### 16.
