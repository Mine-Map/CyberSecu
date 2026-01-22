**Outils utilisés :**



\- VM Windows 11 64 bits infectée

\- WinPMEM (https://github.com/Velocidex/WinPmem) OU DumpIt (https://www.magnetforensics.com/fr/resources/magnet-dumpit-pour-windows/)

\- Volatility 3



**Documents joints :**



\- pslist.txt

\- psscan.txt

\- netscan.txt



PS : Le dump n'est pas joint car il pèse trop lourd (> 4Go)



**Acquisition de la RAM :**



* Avec WinPMEM :



 	- Commandes :



   	cd C:\\

   	.\\winpmem\_mini\_x64\_rc2.exe C:\\ram\_dump.raw



 	- Résultats :



   	Une fois les commandes exécutées, un fichier ram\_dump.raw sera créé. Ce fichier sera exploitable avec Volatility.



* Avec DumpIt :



 	- Protocole :



 	Lancer l'exécutable DumpIt.exe 



 	- Résultats :



   	Une fois l'exécutable exécuté, un fichier .dmp (contenant le nom du PC comme CYBERSECURITE-20260122-082535) sera créé. Ce fichier sera exploitable avec Volatility.



**Analyses Mémoire :**



Ces analyses ont été effectuées avec le dump RAM récupéré avec DumpIt.



* Processus actifs :



 	- Commandes :



 	vol -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.pslist

 	vol -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.pslist > pslist.txt

 		OU

 	C:\\Users\\\[User]\\AppData\\Local\\Python\\pythoncore-3.14-64\\Scripts\\vol.exe -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.pslist

 	C:\\Users\\\[User]\\AppData\\Local\\Python\\pythoncore-3.14-64\\Scripts\\vol.exe -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.pslist > pslist.txt



 	- Résultats :



&nbsp;	L'investigation des processus en cours à l'aide du plugin windows.pslist a aidé à détecter le processus Res.exe, qui est associé à l'un des exécutables que le logiciel malveillant a implantés sur la machine.

Ce processus fonctionne dans un environnement utilisateur et est lié à des processus de console (conhost.exe, WindowsTerminal.exe), ce qui correspond au comportement constaté lors de l'exécution du trojan (fenêtre terminal qui enregistre les frappes clavier).

Aucune autre procédure suspecte, portant un nom atypique ou lancée depuis une localisation inhabituelle, n'a été détectée.



* Processus terminés/cachés :



 	- Commandes :

 

 	vol -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.psscan

 	vol -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.psscan > psscan.txt

 		OU

 	C:\\Users\\\[User]\\AppData\\Local\\Python\\pythoncore-3.14-64\\Scripts\\vol.exe -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.psscan

 	C:\\Users\\\[User]\\AppData\\Local\\Python\\pythoncore-3.14-64\\Scripts\\vol.exe -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.psscan > psscan.txt



 	- Résultats :



 	L'examen effectué avec l'outil windows.psscan révèle la présence du processus Res.exe dans la mémoire, sans signaler de tentative de camouflage ou de furtivité.

On n'a observé aucun processus caché, orphelin ou incohérent, ce qui indique que le malware fonctionne comme un processus utilisateur standard, sans dispositif de dissimulation sophistiqué en mémoire vive.



* Connexions réseau :



 	- Commandes :



 	vol -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.netscan

 	vol -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.netscan > netscan.txt

 		OU

 	C:\\Users\\\[User]\\AppData\\Local\\Python\\pythoncore-3.14-64\\Scripts\\vol.exe -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.netscan

 	C:\\Users\\\[User]\\AppData\\Local\\Python\\pythoncore-3.14-64\\Scripts\\vol.exe -f C:\\DumpIt\\CYBERSECURITE-20260122-082535.dmp windows.netscan > netscan.txt



 	- Résultats :



 	L'étude des liaisons réseau grâce au plugin windows.netscan, au moment de l'acquisition mémoire, n'a détecté aucune connexion réseau active.

Bien que le logiciel malveillant intègre des bibliothèques réseau, notamment par l'intermédiaire du framework Qt, aucune communication continue ou tentative d'exfiltration n'a été détectée en mémoire lors de la sauvegarde.



**Conclusion :**



L'examen de la mémoire vive a révélé la présence d'un logiciel malveillant identifié comme étant le processus utilisateur Res.exe, en cours d'exécution au moment de la collecte et lié à une exécution en mode console, ce qui est compatible avec un comportement typique de keylogger.

Néanmoins, aucune preuve de tentative de dissimulation, de persistance en mémoire vive ou de communication réseau active n'a été trouvée.

Ces facteurs suggèrent que le logiciel malveillant est un trojan basique (cheval de Troie), qui ne fonctionne que lorsqu'il est en cours d'exécution et qui s'appuie essentiellement sur des artefacts stockés sur le disque dur. Cela souligne l'importance du moment choisi lors d'une acquisition mémoire forensic.

