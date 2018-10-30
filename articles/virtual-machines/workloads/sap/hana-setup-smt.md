---
title: Over het instellen van SMT-server voor SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Over het instellen van SMT-server voor SAP HANA op Azure (grote instanties).
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
ms.openlocfilehash: d9fe644b7cc7d1a13cb9ed2f7016f25b3e346dfb
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233171"
---
# <a name="set-up-smt-server-for-suse-linux"></a>SMT-server instellen voor SUSE Linux
Grote instanties van SAP HANA hebt geen directe verbinding met internet. Het is niet een eenvoudig proces zoals eenheden registreren bij de provider van het besturingssysteem, en voor het downloaden en toepassen van updates. Er is een oplossing voor het SUSE Linux voor het instellen van een SMT-server in een virtuele machine van Azure. Host voor de virtuele machine in een Azure-netwerk, die is verbonden met de HANA grote instantie. Met deze een SMT-server, kan de eenheid HANA grote instantie registreren en updates te downloaden. 

Zie voor meer documentatie over SUSE hun [abonnement beheerprogramma voor SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

Vereisten voor het installeren van een SMT-server die voldoet aan vereisten voor de taak voor HANA grote instanties zijn:

- Een Azure-netwerk dat is verbonden met het HANA grote instantie ExpressRoute-circuit.
- Een SUSE-account dat is gekoppeld aan een organisatie. De organisatie moet een geldig SUSE-abonnement hebben.

## <a name="install-smt-server-on-an-azure-virtual-machine"></a>SMT-server installeren op een virtuele machine van Azure

Eerst aanmelden bij de [SUSE klant Center](https://scc.suse.com/).

Ga naar **organisatie** > **organisatie referenties**. In deze sectie vindt u de referenties die nodig zijn voor het instellen van de SMT-server.

Vervolgens installeert u een SUSE Linux-VM in Azure virtual network. Voor het implementeren van de virtuele machine, neemt u een installatiekopie van de galerie SLES 12 SP2 van Azure (Selecteer SUSE BYOS afbeelding). Een DNS-naam niet definiëren in het implementatieproces en statische IP-adressen niet worden gebruikt.

![Schermafbeelding van de implementatie van virtuele machines voor SMT-server](./media/hana-installation/image3_vm_deployment.png)

De geïmplementeerde virtuele machine kleiner is en is het interne IP-adres in de Azure-netwerk van 10.34.1.4. De naam van de virtuele machine is *smtserver*. Na de installatie, wordt de verbinding met de HANA grote instantie eenheid of eenheden gecontroleerd. Afhankelijk van hoe u naamomzetting georganiseerd, moet u mogelijk de resolutie van de eenheden HANA grote instantie configureren in etc/hosts van virtuele machine van Azure. 

Een schijf toevoegen aan de virtuele machine. Gebruikmaken van deze schijf voor het opslaan van de updates en de opstartschijf zelf kan worden te klein. Hier worden de schijf is gekoppeld aan /srv/www/htdocs, zoals wordt weergegeven in de volgende schermafbeelding. Er moet een schijf van 100 GB voldoende.

![Schermafbeelding van de implementatie van virtuele machines voor SMT-server](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Aanmelden bij de HANA grote instantie eenheid / eenheden, te onderhouden/etc/hosts en controleer of u de Azure virtuele machine die moet worden uitgevoerd, het SMT-server via het netwerk kunt bereiken.

Na deze controle, moet u zich aanmelden met de Azure-machine waarop het SMT-server moet worden uitgevoerd. Als u putty gebruikt om aan te melden bij de virtuele machine, voert u deze reeks opdrachten uit in de bash-venster:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Start opnieuw op uw bash voor het activeren van de instellingen. Start vervolgens YAST.

Verbind uw VM (smtserver) met de SUSE-site.

```
smtserver:~ # SUSEConnect -r <registration code> -e s<email address> --url https://scc.suse.com
Registered SLES_SAP 12.2 x86_64
To server: https://scc.suse.com
Using E-Mail: email address
Successfully registered system.
```

Nadat de virtuele machine is verbonden met de SUSE-site, moet u het smt-pakketten installeren. Gebruik de volgende putty opdracht om het smt-pakketten te installeren.

```
smtserver:~ # zypper in smt
Refreshing service 'SUSE_Linux_Enterprise_Server_for_SAP_Applications_12_SP2_x86_64'.
Loading repository data...
Reading installed packages...
Resolving package dependencies...
```


U kunt het hulpprogramma YAST ook gebruiken om het smt-pakketten te installeren. In YAST, gaat u naar **softwareonderhoud**, en zoek naar smt. Selecteer **smt**, die automatisch aan yast2 smt verandert.

![Schermopname van het SMT in YAST](./media/hana-installation/image5_smt_in_yast.PNG)


De selectie voor installatie op de smtserver accepteren. Nadat de installatie is voltooid, gaat u naar de configuratie van de SMT-server. Voer de referenties van de organisatie van de SUSE klant Center die u eerder hebt opgehaald. De hostnaam van uw virtuele Azure-machine ook opgeven als de URL van de SMT-Server. In deze demonstratie heeft https://smtserver.

![Schermafbeelding van SMT-serverconfiguratie](./media/hana-installation/image6_configuration_of_smtserver1.png)

Test nu of de verbinding met het midden van de klant SUSE werkt. Zoals u in de volgende schermafbeelding, in dit geval demonstratie ziet, dit werkt.

![Schermopname van het testen van verbinding met SUSE klant Center](./media/hana-installation/image7_test_connect.png)

Nadat u het SMT-setup wordt gestart, bieden u een wachtwoord voor de database. Omdat het is een nieuwe installatie, kunt u dit wachtwoord moet definiëren, zoals wordt weergegeven in de volgende schermafbeelding.

![Schermopname van het wachtwoord voor de database te definiëren](./media/hana-installation/image8_define_db_passwd.PNG)

De volgende stap is het maken van een certificaat.

![Schermopname van het maken van een certificaat voor SMT-server](./media/hana-installation/image9_certificate_creation.PNG)

Aan het einde van de configuratie duurt het een paar minuten om de synchronisatie uit te voeren. Nadat de installatie en configuratie van de server SMT, vindt u de opslagplaats van de directory onder het koppelpunt punt /srv/www/htdocs /. Er zijn ook enkele submappen onder de opslagplaats. 

Start opnieuw op de server SMT en gerelateerde services met de volgende opdrachten.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

## <a name="download-packages-onto-smt-server"></a>Downloaden van pakketten naar SMT-server

Nadat alle services opnieuw gestart zijn, selecteert u de juiste pakketten in SMT Management met behulp van YAST. De selectie van het pakket is afhankelijk van de installatiekopie van de server HANA grote instantie. De selectie van het pakket afhankelijk niet van de SLES-versie of de versie van de virtuele machine waarop het SMT-server wordt uitgevoerd. De volgende Schermafbeelding toont een voorbeeld van het scherm selecteren.

![Schermafbeelding van het selecteren van pakketten](./media/hana-installation/image10_select_packages.PNG)

Start vervolgens de eerste kopie van de select-pakketten naar de SMT-server die u instelt. Deze kopie wordt geactiveerd in de shell met behulp van de opdracht smt-mirror.

![Schermafbeelding van het downloaden van pakketten naar SMT-server](./media/hana-installation/image11_download_packages.PNG)

De pakketten moeten in de mappen die zijn gemaakt op basis van het koppelpunt punt /srv/www/htdocs gekopieerd. Dit proces kan duren een uur of langer, afhankelijk van hoeveel pakketten die u selecteert. Als dit proces is voltooid, verplaatsen naar het SMT-clientinstallatie. 

## <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>Instellen van de client SMT op HANA grote instantie eenheden

De client of de clients zijn in dit geval de eenheden HANA grote instantie. Het script clientSetup4SMT.sh de installatie van de SMT gekopieerd naar de virtuele machine van Azure. Kopie die via script zijn aan de eenheid HANA grote instantie dat u wilt verbinding maken met uw SMT-server. Start het script met de optie -h en geef de naam van uw server SMT als parameter. In dit voorbeeld wordt de naam is *smtserver*.

![Schermafbeelding van de client SMT configureren](./media/hana-installation/image12_configure_client.PNG)

Het is mogelijk dat de belasting van het certificaat van de server door de client is geslaagd, maar de registratie is mislukt, zoals wordt weergegeven in de volgende schermafbeelding.

![Schermafbeelding van de client-registratie is mislukt](./media/hana-installation/image13_registration_failed.PNG)

Als de registratie is mislukt, raadpleegt u [SUSE ondersteuning biedt voor document](https://www.suse.com/de-de/support/kb/doc/?id=7006024), en voer de stappen bevat.

> [!IMPORTANT] 
> Voor de naam van de server, geeft u de naam van de virtuele machine (in dit geval *smtserver*), zonder de volledig gekwalificeerde domeinnaam. 

Voer de volgende opdracht op de eenheid HANA grote instantie na het uitvoeren van deze stappen:

```
SUSEConnect –cleanup
```

> [!Note] 
> Wacht een paar minuten na deze stap. Als u onmiddellijk clientSetup4SMT.sh uitvoert, krijgt u mogelijk een fout.

Als er een probleem die u nodig hebt om op te lossen op basis van de stappen van het artikel SUSE, opnieuw opstarten clientSetup4SMT.sh op de eenheid HANA grote instantie. Nu het moet worden voltooid.

![Schermafbeelding van de client registratie geslaagd](./media/hana-installation/image14_finish_client_config.PNG)

U hebt geconfigureerd de SMT-client van de eenheid HANA grote instantie verbinding maken met het SMT-server die u hebt geïnstalleerd in de virtuele machine van Azure. Nu kunnen nemen 'zypper van' of 'zypper in' voor het installeren van besturingssysteemupdates op HANA grote instanties of extra pakketten installeren. U kunt alleen updates die u hebt gedownload voordat u op de server SMT ophalen.

## <a name="next-steps"></a>Volgende stappen
- [HANA-installatie op HLI](hana-example-installation.md).











