---
title: IBM zD & T dev/test-omgeving installeren op Azure | Microsoft Docs
description: Ddeploy IBM Z ontwikkelings- en testomgeving (zD & T) op Azure Virtual Machine (VM)-infrastructuur als een dienst (IaaS).
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 5de5f2f48504a711d4b7cff614abdd4214d0ba09
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56739507"
---
# <a name="install-ibm-zdt-devtest-environment-on-azure"></a>IBM zD & T dev/test-omgeving op Azure installeren

Voor het maken van een dev/test-omgeving voor mainframe-workloads op IBM Z Systems, kunt u IBM Z Development en Test Environment (zD & T) implementeren op Azure Virtual Machine (VM)-infrastructuur als een dienst (IaaS).

Met zD & T, kunt u profiteren van de kosten te besparen van de x86 platform voor uw minder kritieke ontwikkel- en testomgevingen en push de updates back-ups maken in een productieomgeving Z-systeem. Zie voor meer informatie de [IBM ZD & T installatie-instructies](http://www-01.ibm.com/support/docview.wss?uid=swg24044565#INSTALL).

Ondersteuning voor de volgende versies van Azure en Azure Stack:

- zD & T persoonlijke editie
- zD & T parallelle Sysplex
- zD&T Enterprise Edition

Alle edities van zD & T uitvoeren op x86 Linux-systemen, niet Windows Server. Enterprise-editie wordt ondersteund op Red Hat Enterprise Linux (RHEL) of Ubuntu/Debian. RHEL- en virtuele Debian-machine-installatiekopieën zijn beschikbaar voor Azure.

In dit artikel leest u hoe het instellen van zD & T Enterprise Edition op Azure, zodat u de zD & T Enterprise Edition-webserver voor het maken en beheren van omgevingen gebruiken kunt. Installeren van zD & T alle omgevingen niet worden geïnstalleerd. U moet deze afzonderlijk als installatiepakketten maken. Bijvoorbeeld, toepassing ontwikkelaars beheerd distributies (ADCD) zijn installatiekopieën van volumes van testomgevingen. Ze zijn opgenomen in het zip-installatiekopieën in de mediadistributie van IBM beschikbaar. Zie hoe u [instellen van een omgeving ADCD op Azure](demo.md).

Zie voor meer informatie de [zD & T overzicht](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zdt.overview.gs.doc/topics/c_product_overview.html) in het midden van de kennis IBM.

In dit artikel leest u hoe het instellen van Z ontwikkelings- en testomgeving (zD & T) Enterprise Edition op Azure. Vervolgens kunt u de zD & T Enterprise Edition-webserver te maken en beheren van Z-omgevingen op Azure.

## <a name="prerequisites"></a>Vereisten

> [!NOTE]
> IBM kunt zD & T Enterprise Edition in dev/test-omgevingen alleen te worden geïnstalleerd:*niet* productie-omgevingen.

- Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

- U moet toegang tot de media, alleen beschikbaar voor IBM-klanten en partners is. Voor meer informatie contact op met uw vertegenwoordiger IBM of raadpleegt u de contactgegevens op de [zD & T](https://www.ibm.com/us-en/marketplace/z-systems-development-test-environment) website.

- Een [licentieserver](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.tools.user.guide.doc/topics/zdt_ee.html). Dit is vereist voor toegang tot de omgevingen. De manier waarop u dit hebt gemaakt, is afhankelijk van hoe u de software van IBM licentie:

     - **Op basis van hardware licentieserver** vereist een USB-apparaat dat de rationele Tokens die nodig zijn voor toegang tot alle onderdelen van de software bevat. U moet dit verkrijgen van IBM.

     - **Op basis van software licentieserver** , moet u voor het instellen van een centrale server voor het beheer van de licentieverlening sleutels. Deze methode heeft de voorkeur en moet u voor het instellen van de sleutels die u van IBM in de beheerserver ontvangt.

## <a name="create-the-base-image-and-connect"></a>De basisinstallatiekopie maken en verbinden

1. In Azure-portal [maken van een virtuele machine](/azure/virtual-machines/linux/quick-create-portal) met de gewenste configuratie van besturingssysteem. In dit artikel wordt ervan uitgegaan dat een VM B4ms (met 4 vcpu's en 16 GB geheugen) waarop Ubuntu 16.04 wordt uitgevoerd.

2. Nadat de virtuele machine is gemaakt, opent u poorten voor inkomend verkeer 22 voor SSH, 21 voor FTP en 9443 voor de webserver.

3. Ophalen van de SSH-referenties die worden weergegeven op de **overzicht** blade van de virtuele machine via de **Connect** knop. Selecteer de **SSH** tabblad en de SSH-aanmelding-opdracht naar het Klembord kopiëren.

4. Meld u aan bij een [Bash-shell](/azure/cloud-shell/quickstart) vanuit uw lokale PC en plak de opdracht. Dit is in het formulier **ssh\<gebruikers-id\>\@\<IP-adres\>**. Wanneer u hierom wordt gevraagd uw referenties op te geven, voert u zodat ze geen verbinding maken met de basismap van de.

## <a name="copy-the-installation-file-to-the-server"></a>Kopieer het installatiebestand naar de server

Het installatiebestand voor de webserver is **ZDT\_installeren\_EE\_V12.0.0.1.tgz**. Het is opgenomen in de media die wordt geleverd door IBM. U moet dit bestand uploaden naar uw Ubuntu-VM.

1. Voer de volgende opdracht om te controleren of dat alles is bijgewerkt in de zojuist gemaakte installatiekopie vanaf de opdrachtregel:

    ```
    sudo apt-get update
    ```

2. Maak de map voor de installatie:

    ```
    mkdir ZDT
    ```

3. Kopieer het bestand van uw lokale computer naar de virtuele machine:

    ```
    scp ZDT_Install_EE_V12.0.0.1.tgz  your_userid@<IP Address /ZDT>   =>
    ```
    
> [!NOTE]
> Deze opdracht wordt het installatiebestand naar de map ZDT in uw directory thuis, deze is afhankelijk van of de client Windows of Linux wordt uitgevoerd.

## <a name="install-the-enterprise-edition"></a>Installeren van de Enterprise-editie

1. Ga naar de map ZDT en decomprimeren van het ZDT\_installeren\_EE\_V12.0.0.1.tgz-bestand met de volgende opdrachten:

    ```
    cd ZDT
    chmod 755 ZDT\_Install\_EE\_V12.0.0.0.tgz
    ```

2. Voer het installatieprogramma:

    ```
    ./ZDT_Install_EE_V12.0.0.0.x86_64
    ```

3. Selecteer **1** om Enterprise-Server te installeren.

4. Druk op **Enter** en lees de licentievoorwaarden aandachtig door. Aan het einde van de licentie, voer **Ja** om door te gaan.

5. Wanneer u wordt gevraagd het wachtwoord voor de nieuwe gebruiker te wijzigen **ibmsys1**, gebruikt u de opdracht **sudo passwd ibmsys1** en voer het nieuwe wachtwoord.

6. Om te controleren of de installatie geslaagd is invoeren

    ```
    dpkg -l | grep zdtapp
    ```

7. Controleer of de uitvoer bevat de tekenreeks **zdtapp 12.0.0.0**, waarmee wordt aangegeven dat de brandstof pakket is geïnstalleerd

### <a name="starting-enterprise-edition"></a>Starting Enterprise Edition

Houd er rekening mee dat wanneer de server wordt gestart, deze wordt uitgevoerd onder de zD & T gebruikers-ID die is gemaakt tijdens de installatie.

1. Voor het starten van de webserver, gebruikt u de gebruikers-ID van de hoofdmap om uit te voeren van de volgende opdracht uit:

    ```
    sudo /opt/ibm/zDT/bin/startServer
    ```

2. Kopieer de URL-uitvoer door het script ziet eruit als:

    ```
    https://<your IP address or domain name>:9443/ZDTMC/login.htm
    ```

3. Plak de URL in een webbrowser te openen van het onderdeel management voor uw zD & T-installatie.

## <a name="next-steps"></a>Volgende stappen

[Instellen van een toepassing ontwikkelaars beheerd distributie (ADCD) in de IBM zD & T v1](./demo.md)
