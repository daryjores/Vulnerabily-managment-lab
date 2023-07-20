<h1>Gestion des vulnérabilités avec Nessus Essentials</h1>

<h2>Description</h2>
Ceci est une démonstration de la façon dont j'ai créé un environnement de machine virtuelle en utilisant VMWare sous Windows 10. J'ai réalisé ce projet pour acquérir de l'expérience avec Nessus Essentials et apprendre à rechercher des vulnérabilités et à y remédier. Ce projet présentera deux des principales étapes du cycle de vie de la gestion des vulnérabilités. J'utiliserai Nessus Essentials pour analyser des machines virtuelles locales hébergées sur VMWare Workstation afin d'effectuer des analyses avec accréditation pour découvrir des vulnérabilités, remédier à certaines d'entre elles, puis effectuer une nouvelle analyse pour vérifier la remédiation.
PS: J'ai répété le même processus avec Qualys Community edition après avoir participé à leur programme. Je vais lier quelques images ci-dessous

<br />

<h2>Services utilisés</h2>

- <b>Nessus Essentials</b> 
- <b>CMD</b>

<h2>Environnements utilisés </h2>

- <b>VMWare</b>
- <b>Windows 10</b> (21H2)

<h2>Liens</h2>

- <b>VMWare Workstation Pro:</b> https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html
- <b>Nessus Essentials:</b> https://www.tenable.com/products/nessus/nessus-essentials
- <b>Windows 10 ISO:</b> https://www.microsoft.com/en-us/software-download/windows10

<h2 align="center">Présentation du programme</h2>

<p align="center">
<b>La première chose que je vais faire est de télécharger Nessus Essentials, mon logiciel de gestion des vulnérabilités. Le téléchargement prend beaucoup de temps, ce qui me permet d'accomplir quelques tâches pendant le téléchargement, comme télécharger Windows 10 sur une machine virtuelle et le configurer (ce qui prend également beaucoup de temps).</b> <br/>
</p>

![Downloading Nessus](https://user-images.githubusercontent.com/108043108/177885108-9e637032-e965-4ecb-8166-b6931dc2dcff.JPG)

<br />
<br />
<p align="center">
<b>Pour la machine virtuelle sur laquelle je vais gérer les vulnérabilités, je dois configurer l'adaptateur réseau pour qu'il soit bridgé afin qu'il puisse être sur le même réseau que mon réseau natif. Je fais cela parce que Nessus doit se connecter au serveur sécurisé (SSL) dans la machine virtuelle et que c'est plus facile si elle utilise mon réseau local.</b> <br/>
</p>

![Configure_Network_Adapters](https://user-images.githubusercontent.com/108043108/177885541-29039f1c-6217-43b4-92ca-86a60d092d3b.JPG)

<br />
<br />
<p align="center">
<b>Nessus at this point is still downloading and my Virtual Machine successfully downloads Windows 10, so I open up CMD to figure out it's IP address which I will need to be able to run vulnerability scans on Nessus. I named the VM Admin. The screenshot shows that the IP address is 192.168.50.185</b> <br/>
</p>

![VM_IP](https://user-images.githubusercontent.com/108043108/177885744-426519f2-bd27-406c-a6eb-f71ef5a6bdcb.JPG)

<br />
<br />
<p align="center">
<b>J'essaie d'envoyer un ping à la VM à partir de mon ordinateur d'origine pour voir si je peux communiquer avec elle. La raison pour laquelle je fais cela est que si je ne peux pas établir de ping avec la VM, Nessus ne pourra pas le faire non plus et ne pourra pas exécuter ses analyses. Comme nous pouvons le voir, les pings sont interrompus, ce qui signifie que mon ordinateur natif ne peut pas établir de connexion avec la VM pour le moment.</b> <br/>
</p>

![trying_to_ping_VM_Fails](https://user-images.githubusercontent.com/108043108/177886078-cec7b98b-60da-47a6-8e70-838a649a456f.JPG)

<br />
<br />
<p align="center">
<b>La raison pour laquelle mon PC ne peut pas établir de connexion est que la VM a un pare-feu actif et qu'il bloque toutes les tentatives de connexion (ce qui est une bonne chose, mais pas pour les besoins de ce laboratoire) ; je dois donc désactiver les pare-feu. C'est quelque chose que je ne ferais JAMAIS dans un environnement de production, car cela pourrait être catastrophique, mais il ne s'agit que d'une VM de pacotille, donc pas d'inquiétude.</b> <br/>
</p>

![windows_firewall_disable](https://user-images.githubusercontent.com/108043108/177886413-df1ba042-8e42-4ff6-a004-0057ac793cd1.JPG)

<br />
<br />
<p align="center">
<b>Je dois désactiver les trois pare-feu qui sont encerclés en haut en rouge, bleu et vert. Le cercle jaune indique que les pare-feu sont actuellement activés. Je dois les désactiver pour pouvoir communiquer correctement avec la VM..</b> <br/>
</p>

![Turning_off_Firewalls](https://user-images.githubusercontent.com/108043108/177886437-e6b4addf-99e3-455f-b46f-c3941a433109.JPG)

<br />
<br />
<p align="center">
<b>Maintenant que le pare-feu de la VM est désactivé, j'essaie à nouveau d'effectuer un ping depuis mon PC d'origine et, cette fois, j'y parviens.</b> <br/>
</p>

![Ping_works](https://user-images.githubusercontent.com/108043108/177887412-f8078e7b-13a3-4480-b000-5ae84108cfab.JPG)

<br />
<br />
<p align="center">
<b>À ce moment-là, Nessus Essentials se télécharge avec succès. La première chose à faire est de créer une nouvelle analyse, puis de sélectionner Basic Network Scan.</b> <br/>
</p>

![Create_New_Scan](https://user-images.githubusercontent.com/108043108/177887661-793476e8-7e68-4c21-8983-8f955d7cff2e.JPG)

![basic_network_scan](https://user-images.githubusercontent.com/108043108/177887672-2d955508-edf1-4735-b78a-2836a81d2c9f.JPG)


<br />
<br />
<p align="center">
<b>The newly created scan asks me to name the scan and select a target to scan. I configure it to scan the VM's IP address which is 192.168.50.185</b> <br/>
</p>

![Scan_VM_IP](https://user-images.githubusercontent.com/108043108/177887914-87a2da10-be51-481c-a672-ec1104e3df7a.JPG)

<br />
<br />
<p align="center">
<b>I launch the newly created scan and it immediately goes to work scanning for any known vulnerabilities. When the grey checkmark appears, the scan is complete.</b> <br/>
</p>

![Launch_newly_created_scan](https://user-images.githubusercontent.com/108043108/177888041-95c53001-b8d2-4355-9311-3ee2637dff94.JPG)

![Scan_Running](https://user-images.githubusercontent.com/108043108/177888049-661dddfc-ebe0-42cf-ad87-14bc5af53fe5.gif)

![Scan_Completed](https://user-images.githubusercontent.com/108043108/177888068-25d4a86a-c150-4e77-a993-9df4178c5ba1.JPG)

<br />
<br />
<p align="center">
<b>Lets look at the results! It is showing 33 results, 32 of which are info and 1 low. If this was an actual production environment these most likely would be left alone. The Info results are probably because some things don't have proper credentials and are not essentially vulnerabilities</b> <br/>
</p>

![Results_Of_Scan](https://user-images.githubusercontent.com/108043108/177888196-3141c52f-df79-4c58-9fee-5daa68d78c5b.gif)

<br />
<br />
<p align="center">
<b>L'examen de l'un des résultats INFO montre que le processus d'authentification de l'état des informations d'identification de la cible a été déclenché parce que nous n'avons pas fourni d'informations d'identification pour cette analyse..</b> <br/>
</p>

![No_credentials_Given](https://user-images.githubusercontent.com/108043108/177888648-5679c8c1-e8ce-47e3-a424-067375964054.JPG)

<br />
<br />
<p align="center">
<b>La prochaine chose à faire est de configurer la VM pour qu'elle puisse accepter des analyses authentifiées et fournir des informations d'identification à Nessus. Je vais ensuite scanner à nouveau la VM et comparer les résultats. Je vais dans services.MSC pour lancer ce processus et j'active Remote Registry. Cela permettra à Nessus de se connecter au registre de la VM et d'analyser correctement les vulnérabilités telles que les connexions non sécurisées ou les suites de chiffrement obsolètes. Je suis les étapes de Nessus et ce qu'ils recommandent pour effectuer des scans crédités. Il y a peut-être une meilleure façon de procéder.</b> <br/>
</p>

![Services_MSC](https://user-images.githubusercontent.com/108043108/177888931-9dbd1224-5155-4129-ba09-4f495976a0e2.JPG)

![Enabling_Remote_Registry](https://user-images.githubusercontent.com/108043108/177889042-0a885e18-bf64-4390-8f43-454bdaf79004.gif)

<br />
<br />
<p align="center">
<b>De là, je vais dans le Contrôle de compte d'utilisateur et je le désactive. Je dois faire cela parce que cette VM n'est pas sur un domaine, donc je dois en quelque sorte faire du piratage pour qu'elle fonctionne correctement. Je ne ferais jamais cela dans une organisation réelle ou dans un environnement de production.</b> <br/>
</p>

![user_account_Settings](https://user-images.githubusercontent.com/108043108/177889466-907890fc-e67e-490b-8a17-b335263d0359.JPG)

![User_Account_settings_configuration](https://user-images.githubusercontent.com/108043108/177889473-ba988135-ce62-4717-bf04-1e426ec8c4b6.gif)

<br />
<br />
<p align="center">
<b>Je vais ensuite ouvrir le registre et ajouter une clé censée permettre à Nessus de se connecter en désactivant le contrôle des comptes d'utilisateurs.</b> <br/>
</p>

![Registry_editor](https://user-images.githubusercontent.com/108043108/177889663-24088363-2291-4af0-8ea9-6f6486fc82ad.JPG)

<br />
<br />
<p align="center">
<b>Je navigue maintenant dans le Registre jusqu'au fichier que Nessus nous indique (surligné en jaune dans la barre de recherche) et je dois ajouter une valeur DWORD et la nommer LocalAccountTokenFilterPolicy et lui donner une valeur de 1. </b> <br/>
</p>

![Creating_a_new_DWORD](https://user-images.githubusercontent.com/108043108/177889894-f94747fa-fa05-401f-82d4-1c9ec9faa57f.JPG)

![DWORD_name](https://user-images.githubusercontent.com/108043108/177889904-aa294428-0864-4018-8cb5-b61da5e65478.JPG)

![Edit_DWORD](https://user-images.githubusercontent.com/108043108/177889915-70e9814c-5167-460c-be85-10abf056fd8f.JPG)

<br />
<br />
<p align="center">
<b>Après cela, je dois redémarrer la machine virtuelle pour que les modifications soient prises en compte..</b> <br/>
</p>

![Restart_The_VM](https://user-images.githubusercontent.com/108043108/177889955-2511a00a-9b59-4b4d-a9be-c2abda9869a6.JPG)

<br />
<br />
<p align="center">
<b>Le registre étant configuré, il est maintenant temps de retourner dans Nessus et de configurer le scan que j'ai créé. Je dois ajouter les Credentials au scan pour qu'il puisse fonctionner correctement. Les informations d'identification dont je parle sont le nom d'utilisateur et le mot de passe de la VM. Cela permettra à Nessus d'utiliser ces informations d'identification aux endroits où elles sont requises dans le registre de la VM.</b> <br/>
</p>

![Configure_Nessus](https://user-images.githubusercontent.com/108043108/177890138-e6420231-4ce8-4a2d-99fb-e1e992da6ebb.JPG)

![Adding_Credentials](https://user-images.githubusercontent.com/108043108/177890151-27496a20-72b8-4763-8c82-368a06a15d3f.gif)

<br />
<br />
<p align="center">
<b>Une fois que l'analyse est correctement configurée avec les bonnes informations d'identification, je l'exécute à nouveau.</b> <br/>
</p>

![Run_The_Scan_Again](https://user-images.githubusercontent.com/108043108/177890377-b412d84a-d8e2-40da-9995-dddbd3c4d388.JPG)

<br />
<br />
<p align="center">
<b>Ce nouveau scan nous a fourni beaucoup plus de vulnérabilités que le premier car il est capable de scanner plus profondément dans la VM grâce à ses informations d'identification. L'image du haut est la nouvelle analyse avec identifiants et l'image du bas est celle de la première analyse sans identifiants. La plupart des vulnérabilités trouvées sont probablement dues au fait que la version de Windows 10 utilisée par cette VM n'est pas à jour.</b> <br/>
</p>

![new_scan_properly_credentialed](https://user-images.githubusercontent.com/108043108/177890582-7f4d0eec-3b5f-4a5f-b708-47b3ccfb5d83.JPG)

![Old_scan_not_credentialed](https://user-images.githubusercontent.com/108043108/177890589-e4e882ab-6733-4930-ad4c-1115a2c620b3.JPG)

<br />
<br />
<p align="center">
<b>Je veux voir à quel point ce scanner Nessus est puissant. Je vais donc télécharger une très vieille version de Firefox qui a probablement de nombreuses vulnérabilités et voir si Nessus peut les découvrir (je suis sûr qu'il le fera).</b><br/>
</p>

![downloading_an_old_version_of_firefox](https://user-images.githubusercontent.com/108043108/177890787-5cd80be3-99a1-40a4-bddc-feb06ea9cda7.JPG)

<br />
<br />
<p align="center">
<b>Après le téléchargement d'une version obsolète de Firefox, je lance une nouvelle analyse. Nous pouvons voir de nombreuses nouvelles alertes et vulnérabilités rien qu'avec Firefox ! 68 Critiques !</b> <br/>
</p>

![Old_Firefox_Scan](https://user-images.githubusercontent.com/108043108/177890872-ed890db9-a15d-4de9-b3b3-6723dd161f22.JPG)

<br />
<br />
<p align="center">
<b>Une comparaison entre les analyses pour montrer la progression des alertes et des vulnérabilités.</b>  <br/>
</p>

![Vulnerability_Comparison](https://user-images.githubusercontent.com/108043108/177890918-c2fc7078-34b9-4120-a430-f096169ae177.jpg)

<br />
<br />
<p align="center">
<b>Montrer à quoi ressemblent certaines des alertes et des vulnérabilités. Nous pouvons voir que la plupart des alertes critiques proviennent de Firefox. Nous pouvons remédier à certaines de ces vulnérabilités en mettant à jour Firefox, ce qui permettra probablement de remédier à la plupart d'entre elles, ou en supprimant tout simplement Firefox.</b>  <br/>
</p>

![Showing_Vulnerabilities](https://user-images.githubusercontent.com/108043108/177891162-cf86e335-0539-4f9f-abb2-7150b482e689.gif)

<br />
<br />
<p align="center">
<b>Pour commencer à remédier aux vulnérabilités, j'ai choisi de supprimer Firefox. Cela résoudra instantanément un grand nombre de ces problèmes.</b>  <br/>
</p>

![Fixing_Vulnerabilities_Uninstall_Firefox](https://user-images.githubusercontent.com/108043108/177891363-17bdc0ea-c872-434f-95a6-4bf6e4694ddf.JPG)

<br />
<br />
<p align="center">
<b>Pour remédier aux vulnérabilités de Windows, j'ai choisi de mettre à jour Windows. Cette version étant ancienne, il faut quelques redémarrages pour la mettre à jour.</b>  <br/>
</p>

![Remediating_Vulnerabilities_Updating_WIndows_1](https://user-images.githubusercontent.com/108043108/177891464-75d2b603-cadf-4923-bc1f-61409b11169d.gif)

![Remediating_Vulnerabilities_Updating_Windows](https://user-images.githubusercontent.com/108043108/177891436-a7b26e21-8376-4650-aaee-aaf4bc2e451e.JPG)

<br />
<br />
<p align="center">
<b>Après quelques redémarrages, Windows est enfin à jour. Je lance une dernière analyse Nessus pour vérifier si les mesures que j'ai prises pour remédier à certaines alertes ont fonctionné.</b>  <br/>
</p>

![VM_Up_To_date](https://user-images.githubusercontent.com/108043108/177891501-767d6764-8e4a-4bd0-85cc-6b70e48c62aa.JPG)

<br />
<br />
<p align="center">
<b>Voici une comparaison finale entre les quatre scanners que j'ai réalisés dans le cadre de ce laboratoire. La dernière image est le scan après remédiation. On peut y voir qu'une grande partie des vulnérabilités qui étaient alertées ont disparu ! Il reste encore une vulnérabilité 1 critique, mais je la remédierai une autre fois !</b>  <br/>
</p>

![After_Vulnerability_Remediation](https://user-images.githubusercontent.com/108043108/177891719-3066c9bb-38dd-4b10-b8bb-6f9dd8a45a09.JPG)

<br />
<br />


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

