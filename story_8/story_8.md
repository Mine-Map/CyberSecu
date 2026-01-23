**Scénario d'attaque (théorique) :**



Étape 1 : 



On exécute le malware, ce qui déclenche le keylogger.



Étape 2 :



Les fichiers du trojan sont copiés dans C:\\WindSyst\\ contenant :



* Res.exe
* Env.exe
* des bibliothèques Qt5 (Dt5Core.dll, Qt5Gui.dll, Qt5Network.dll et Qt5Widgets.dll)
* des dépendances C++ (libgcc\_s\_dw2-1.dll, libstdc++-6.dll et libwinpthread-1.dll)
* log.txt



Étape 3 :



Une fois les fichiers copiés, Res.exe est exécuté. Côté mémoire (RAM), Res.exe apparaît comme un processus actif. Côté interface, le terminal ouvert par Res.exe est réduit dans la barre des tâches.



Étape 4 :



Le terminal récupère nos saisies au clavier, le fichier log.txt conserve nos saisies et se met à jour à chaque fois que la touche "entrée" est saisie. Les données sont stockées sur le disque et non dans la RAM.



Étape 5 :



À l'exécution du malware, une communication réseau ponctuelle est effectuée vers l'adresse 23.58.193.189 sur le port 443 (HTTPS). Cette communication permet l'envoie de données et la réception de commandes.



Étape 6 :

Après redémarrage du système, le malware se relance automatiquement.



**Correspondances RAM et disque :**

Dans la mémoire vive, le fichier Res.exe est repéré comme étant un processus actif et ce fichier se trouve dans le disque et n'est jamais supprimé. Les consoles ouvertes par le trojan pour lancer le keylogger sont visibles dans la RAM et les saisies sont stockées en brut dans le disque dans log.txt.

L'analyse des connexions réseau via le dump de la RAM n'a pas permis de détecter de liaison avec un quelconque réseau, mais une communication TCP a été repérée dans l'artéfact réseau du disque.



**Persistance :**



D'après nos analyses, le processus Res.exe est relancé automatiquement, relançant ainsi le keylogger. Le malware persiste donc sur le système, même après un reboot, bien que le mécanisme exact n'ait pas été identifié.

