---
title: Het installatieprogramma SMT-server voor SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Klik hier voor meer informatie over het SMT-server instellen voor SAP HANA op Azure (grote instantie).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f387c1afe88f2bba476309b2e2e01942d2b7ae5b
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982622"
---
# <a name="setting-up-smt-server-for-suse-linux"></a>SMT-server instellen voor SUSE Linux
SAP HANA grote instanties hebt geen directe verbinding met Internet. Het is daarom niet een eenvoudig proces aan zoals eenheden registreren bij de OS-provider te downloaden en toepassen van patches. Als SUSE Linux, een oplossing worden kan voor het instellen van een SMT-server in een Azure-VM. Terwijl de virtuele Azure-machine moet worden gehost in een Azure-VNet is verbonden met de HANA grote instantie. Met dergelijke een SMT-server, kan de eenheid HANA grote instantie registreren en downloaden van patches. 

SUSE biedt een grotere handleiding op hun [abonnement beheerprogramma voor SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

Als voorwaarde voor de installatie van een SMT-server die voldoet aan vereisten voor de taak voor HANA grote instantie moet:

- Een Azure-VNet die is verbonden met het circuit HANA grote instantie ER.
- Een SUSE-account dat is gekoppeld aan een organisatie. Terwijl de organisatie een geldige SUSE-abonnement hebt moet.

## <a name="installation-of-smt-server-on-azure-vm"></a>Installatie van SMT-server op Azure VM

In deze stap installeert u het SMT-server in een Azure-VM. De eerste meting is te melden bij de [SUSE klant Center](https://scc.suse.com/).

Als u bent aangemeld, geeft gaat u naar de organisatie--> referenties van de organisatie. In deze sectie vindt u de referenties die nodig zijn voor het instellen van de SMT-server.

De derde stap is het installeren van een SUSE Linux-VM in de Azure-VNet. Voor het implementeren van de virtuele machine, neemt u een installatiekopie van de galerie SLES 12 SP2 van Azure (Selecteer SUSE BYOS afbeelding). Een DNS-naam niet definiëren in het implementatieproces en gebruik geen statische IP-adressen zoals te zien is in deze schermafbeelding

![VM-implementatie voor SMT-server](./media/hana-installation/image3_vm_deployment.png)

De geïmplementeerde virtuele machine een kleinere virtuele machine is en het interne IP-adres in het Azure-VNet van 10.34.1.4 kreeg. De naam van de virtuele machine is smtserver. Na de installatie, is de verbinding met de HANA grote instantie eenheid/eenheden gecontroleerd. Afhankelijk van hoe u naamomzetting georganiseerd mogelijk moet u de resolutie van de eenheden HANA grote instantie in etc/hosts van de Azure-VM configureren. Een extra schijf toevoegen aan de virtuele machine die u moet worden gebruikt voor het opslaan van de patches. De opstartschijf zelf kan worden te klein. In het geval is aangetoond dat is de schijf gekoppeld aan /srv/www/htdocs zoals wordt weergegeven in de volgende schermafbeelding. Er moet een schijf van 100 GB voldoende.

![VM-implementatie voor SMT-server](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Aanmelden bij de HANA grote instantie eenheid/eenheden, te onderhouden/etc/hosts en controleer of u de Azure-VM die moet worden uitgevoerd, het SMT-server via het netwerk kunt bereiken.

Nadat u deze controle is voltooid, moet u zich aanmeldt bij de Azure-VM die het SMT-server moet worden uitgevoerd. Als u putty gebruikt om aan te melden bij de virtuele machine, moet u deze reeks opdrachten uitvoeren in de bash-venster:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Na deze opdrachten wordt uitgevoerd, opnieuw opstarten van uw bash voor het activeren van de instellingen. Start vervolgens YAST.

Verbind uw VM (smtserver) met de SUSE-site.

```
smtserver:~ # SUSEConnect -r <registration code> -e s<email address> --url https://scc.suse.com
Registered SLES_SAP 12.2 x86_64
To server: https://scc.suse.com
Using E-Mail: email address
Successfully registered system.
```

Zodra de virtuele machine is verbonden met de SUSE-site, moet u het smt-pakketten installeren. Gebruik de volgende putty opdracht om het smt-pakketten te installeren.

```
smtserver:~ # zypper in smt
Refreshing service 'SUSE_Linux_Enterprise_Server_for_SAP_Applications_12_SP2_x86_64'.
Loading repository data...
Reading installed packages...
Resolving package dependencies...
```


U kunt ook YAST hulpprogramma gebruiken om het smt-pakketten te installeren. In YAST, gaat u naar het onderhoud van Software en zoek naar smt. Selecteer smt, die automatisch wordt overgeschakeld naar yast2 smt zoals hieronder wordt weergegeven

![SMT in yast](./media/hana-installation/image5_smt_in_yast.PNG)


De selectie voor installatie op de smtserver accepteren. Wanneer geïnstalleerd, gaat u naar de configuratie van de SMT-server en voer de referenties van de organisatie van de SUSE klant Center die u eerder hebt opgehaald. De hostnaam van uw virtuele Azure-machine ook opgeven als de URL van de SMT-Server. In deze demonstratie is https://smtserver zoals weergegeven in de volgende afbeeldingen.

![Configuratie van SMT-server](./media/hana-installation/image6_configuration_of_smtserver1.png)

Als volgende stap moet u testen of de verbinding met het midden van de klant SUSE werkt. Zoals u in de volgende afbeeldingen, in het geval demonstratie ziet, dit werkt.

![Test verbinding maken met SUSE klant Center](./media/hana-installation/image7_test_connect.png)

Eenmaal de SMT setup wordt gestart, moet u een databasewachtwoord op te geven. Omdat dit een nieuwe installatie, moet u dit wachtwoord definiëren, zoals wordt weergegeven in de volgende afbeeldingen.

![Wachtwoord voor de database definiëren](./media/hana-installation/image8_define_db_passwd.PNG)

De volgende interactie hebt is wanneer een certificaat wordt gemaakt. Ga in het dialoogvenster zoals hierna ziet en de stap te gaan.

![Certificaat voor SMT-server maken](./media/hana-installation/image9_certificate_creation.PNG)

Er is mogelijk enkele minuten in de stap van "Controle uitvoeren synchronisatie" besteed aan het einde van de configuratie. Nadat de installatie en configuratie van de server SMT, vindt u de opslagplaats van de directory onder het koppelpunt punt /srv/www/htdocs/plus enkele submappen onder de opslagplaats. 

Start opnieuw op de server SMT en gerelateerde services met de volgende opdrachten.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

## <a name="download-of-packages-onto-smt-server"></a>Het downloaden van pakketten naar SMT-server

Nadat alle services opnieuw gestart zijn, selecteert u de juiste pakketten in SMT-beheer via het Yast. De selectie van het pakket is afhankelijk van de installatiekopie van het besturingssysteem van de server HANA grote instantie en niet op de SLES-versie of de versie van de virtuele machine waarop het SMT-server wordt uitgevoerd. Hieronder ziet u een voorbeeld van het scherm selecteren.

![Pakketten selecteren](./media/hana-installation/image10_select_packages.PNG)

Zodra u klaar met de selectie van het pakket bent, moet u de eerste kopie van de select-pakketten naar de SMT-server die u instelt starten. Dit exemplaar is geactiveerd in de shell met behulp van de opdracht smt-mirror zoals hieronder wordt weergegeven


![Pakketten naar SMT-server downloaden](./media/hana-installation/image11_download_packages.PNG)

Zoals u hierboven ziet, moeten de pakketten gekopieerd naar de mappen die zijn gemaakt op basis van het koppelpunt punt /srv/www/htdocs. Dit proces kan even duren. Afhankelijk van hoeveel pakketten die u selecteert, het kan duren een uur of langer.
Als dit proces is voltooid, moet u verplaatsen naar het SMT-clientinstallatie. 

## <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>Instellen van de client SMT op HANA grote instantie eenheden

De client (s) zijn in dit geval de eenheden HANA grote instantie. Het script clientSetup4SMT.sh de installatie van de SMT gekopieerd naar de Azure-VM. Kopie die via script zijn aan de eenheid HANA grote instantie dat u wilt verbinding maken met uw SMT-server. Start het script met de optie -h en geef deze als parameter de naam van uw SMT-server. In dit voorbeeld smtserver.

![SMT-client configureren](./media/hana-installation/image12_configure_client.PNG)

Er is mogelijk een scenario waarbij de belasting van het certificaat van de server door de client is voltooid, maar de registratie is mislukt, zoals hieronder weergegeven.

![Registratie van de client is mislukt](./media/hana-installation/image13_registration_failed.PNG)

Als de registratie is mislukt, Lees dit [SUSE ondersteuning biedt voor document](https://www.suse.com/de-de/support/kb/doc/?id=7006024) en bevat de stappen uit te voeren.

> [!IMPORTANT] 
> Als de servernaam van de moet u de naam van de virtuele machine, in dit geval smtserver, zonder de volledig gekwalificeerde domeinnaam opgeven. Alleen de virtuele machine de naam werkt. 

Nadat deze stappen zijn uitgevoerd, moet u de volgende opdracht uitvoeren op de eenheid HANA grote instantie

```
SUSEConnect –cleanup
```

> [!Note] 
> In onze tests hebben we altijd moeten over een paar minuten na deze stap. De clientSetup4SMT.sh direct na de corrigerende maatregelen die worden beschreven in het artikel SUSE is beëindigd met berichten die het certificaat niet gebruikt nog worden. Doorgaans 5-10 minuten wachten en het uitvoeren van clientSetup4SMT.sh is in configuratie van een geslaagde client beëindigd.

Als u in het probleem dat u nodig hebt om op te lossen op basis van de stappen van het SUSE-artikel hebt uitgevoerd, moet u opnieuw clientSetup4SMT.sh op de eenheid HANA grote instantie. Nu het moet worden voltooid, zoals hieronder weergegeven.

![Registratie is voltooid](./media/hana-installation/image14_finish_client_config.PNG)

Met deze stap maakt u het SMT-client van de eenheid HANA grote instantie verbinding maken op basis van de SMT-server die u hebt geïnstalleerd in de Azure-VM geconfigureerd. U kunt nu 'zypper van' of 'zypper in' patches voor het besturingssysteem installeren op HANA grote instanties of installeren van extra pakketten nemen. Het is duidelijk dat alleen u patches die u hebt gedownload krijgt voordat u op de SMT-server.

**Volgende stappen**
- Raadpleeg [HANA-installatie op HLI](hana-example-installation.md).











