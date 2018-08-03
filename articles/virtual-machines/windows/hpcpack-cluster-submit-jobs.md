---
title: Verzenden van taken in een HPC Pack-cluster in Azure | Microsoft Docs
description: Meer informatie over het instellen van een on-premises computer voor het verzenden van taken naar een HPC Pack-cluster in Azure
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 05/14/2018
ms.author: danlep
ms.openlocfilehash: f2cf26bc9f980729e74c4a4e0b4e3f4b311fd754
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421131"
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>HPC-taken vanaf een on-premises computer verzenden naar een HPC Pack-cluster die in Azure is geïmplementeerd
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Configureren van een on-premises clientcomputer voor het verzenden van taken naar een [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) -cluster in Azure. Dit artikel leest u hoe het instellen van een lokale computer met clienthulpprogramma's taak verzenden via HTTPS met het cluster in Azure. Op deze manier kunnen kunnen meerdere gebruikers van het cluster verzenden van taken aan een cloud-gebaseerde HPC Pack-cluster, maar zonder directe verbinding met het hoofdknooppunt VM of toegang tot een Azure-abonnement.

![Een taak verzenden naar een cluster in Azure][jobsubmit]

## <a name="prerequisites"></a>Vereisten
* **Hoofdknooppunt van HPC Pack geïmplementeerd in een Azure VM** -het is raadzaam dat u gebruik van geautomatiseerde hulpprogramma's zoals een [Azure quickstart-sjabloon](https://azure.microsoft.com/documentation/templates/) implementatie van het hoofdknooppunt en het cluster. U moet de DNS-naam van het hoofdknooppunt en de referenties van de Clusterbeheerder van een om de stappen in dit artikel te voltooien.
* **Clientcomputer** -moet u een Windows- of Windows Server-clientcomputer die hulpprogramma's van HPC Pack-client kan worden uitgevoerd (Zie [systeemvereisten](https://technet.microsoft.com/library/dn535781.aspx)). Als u wilt dat alleen de HPC Pack-webportal of de REST-API gebruiken om te verzenden van taken, kunt u een clientcomputer van uw keuze.
* **HPC Pack-installatiemedia** - voor het installeren van het installatiepakket voor de gratis hulpprogramma's client HPC Pack voor de nieuwste versie van HPC Pack beschikbaar via is de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56360). Zorg ervoor dat u dezelfde versie van HPC Pack die is geïnstalleerd op het hoofdknooppunt VM downloaden.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Stap 1: Installeren en configureren van de webonderdelen op het hoofdknooppunt
Zorg ervoor dat de webonderdelen van HPC Pack zijn geconfigureerd op het hoofdknooppunt van HPC Pack zodat een REST-interface voor het verzenden van taken naar het cluster via HTTPS. Als ze niet al zijn geïnstalleerd, moet u eerst de webonderdelen installeren door het uitvoeren van het installatiebestand HpcWebComponents.msi. Configureer vervolgens de onderdelen van de HPC PowerShell-script uit te voeren **Set HPCWebComponents.ps1**.

Zie voor gedetailleerde procedures [de webonderdelen van Microsoft HPC Pack installeren](http://technet.microsoft.com/library/hh314627.aspx).

> [!TIP]
> Bepaalde Azure-quickstart-sjablonen voor clusters met HPC Pack installeren en configureren van de webonderdelen automatisch.
> 
> 

**Voor het installeren van de webonderdelen**

1. Verbinding maken met het hoofdknooppunt VM met behulp van de referenties van de Clusterbeheerder van een.
1. Vanuit de installatiemap van HPC Pack HpcWebComponents.msi worden uitgevoerd op het hoofdknooppunt.
1. Volg de stappen in de wizard voor het installeren van de webonderdelen

**Het configureren van de webonderdelen**

1. Start op het hoofdknooppunt HPC PowerShell als beheerder.
1. Als u de map naar de locatie van het configuratiescript, typ de volgende opdracht:
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
1. Voor het configureren van de REST-interface en de HPC-webservice starten, typt u de volgende opdracht:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
1. Wanneer u hierom wordt gevraagd om een certificaat te selecteren, kiest u het certificaat dat overeenkomt met de openbare DNS-naam van het hoofdknooppunt. Bijvoorbeeld, als u het hoofdknooppunt VM met het klassieke implementatiemodel implementeert, naam van het certificaat ziet eruit als CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Als u de Resource Manager-implementatiemodel, naam van het certificaat ziet eruit als CN =&lt;*HeadNodeDnsName*&gt;.&lt; *regio*&gt;. cloudapp.azure.com.
   
   > [!NOTE]
   > U selecteert dit certificaat later bij het verzenden van taken met het hoofdknooppunt van een on-premises computer. Geen selecteren of een certificaat dat overeenkomt met de naam van de computer van het hoofdknooppunt in Active Directory-domein configureren (bijvoorbeeld: CN =*MyHPCHeadNode.HpcAzure.local*).
   > 
   > 
1. Voor het configureren van de web-portal voor het verzenden van taken, typ de volgende opdracht:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
1. Nadat het script is voltooid, stoppen en opnieuw opstarten van de HPC Job Scheduler-Service door de volgende opdrachten te typen:
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Stap 2: Installeer de hulpprogramma's voor HPC Pack-client op een on-premises computer
Als u de hulpprogramma's voor HPC Pack-client installeren op uw computer wilt, het downloaden van de setup-bestanden van HPC Pack (volledige installatie) van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56360). Wanneer u de installatie begint, kiest u de optie voor de **client hulpprogramma's van HPC Pack**.

Als u de clienthulpprogramma's van HPC Pack wilt verzenden van taken naar het hoofdknooppunt VM, moet u ook een certificaat exporteren van het hoofdknooppunt en installeer deze op de clientcomputer. Het certificaat moet zich in. CER-indeling.

**Het certificaat exporteren van het hoofdknooppunt**

1. De module Certificaten aan een Microsoft Management Console voor het account lokale Computer toevoegen op het hoofdknooppunt. Zie voor stappen om toe te voegen de module [de module Certificaten toevoegen aan een MMC](https://technet.microsoft.com/library/cc754431.aspx).
1. Vouw in de consolestructuur **certificaten-lokale Computer** > **persoonlijke**, en klik vervolgens op **certificaten**.
1. Zoek het certificaat dat u hebt geconfigureerd voor de webonderdelen van HPC Pack in [stap 1: installeren en configureren van de webonderdelen op het hoofdknooppunt](#step-1-install-and-configure-the-web-components-on-the-head-node) (bijvoorbeeld: CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).
1. Met de rechtermuisknop op het certificaat en klik op **alle taken** > **exporteren**.
1. Klik in de Wizard Certificaat exporteren op **volgende**, en zorg ervoor dat **Nee, de persoonlijke sleutel niet exporteren** is geselecteerd.
1. Volg de resterende stappen van de wizard voor het exporteren van het certificaat in DER encoded binary X.509 (. CER)-indeling.

**Voor het importeren van het certificaat op de clientcomputer**

1. Kopieer het certificaat dat u hebt geëxporteerd uit het hoofdknooppunt naar een map op de clientcomputer.
1. Voer op de clientcomputer certmgr.msc.
1. Vouw in de certificaatbeheerder **certificaten-huidige gebruiker** > **Trusted Root Certification Authorities**, met de rechtermuisknop op **certificaten**, en vervolgens Klik op **alle taken** > **importeren**.
1. Klik in de Wizard Certificaat importeren op **volgende** en volg de stappen voor het importeren van het certificaat dat u hebt geëxporteerd uit het hoofdknooppunt aan het archief Vertrouwde basiscertificeringsinstanties.

> [!TIP]
> U ziet mogelijk een beveiligingswaarschuwing, omdat de certificeringsinstantie op het hoofdknooppunt is niet herkend door de clientcomputer. Voor testdoeleinden kunt u deze waarschuwing negeren en het certificaat importeren voltooien.
> 
> 

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Stap 3: Voer test-taken op het cluster
Probeer om te controleren of uw configuratie, uit te voeren taken op het cluster in Azure vanaf de lokale computer. Bijvoorbeeld, kunt u HPC Pack GUI-hulpprogramma's of opdrachtregelopdrachten taken indienen bij het cluster. U kunt ook een webportal gebruiken voor het verzenden van taken.

**Taak indienen opdrachten uitvoeren op de clientcomputer**

1. Op een clientcomputer waarop de hulpprogramma's voor HPC Pack-client zijn geïnstalleerd, start u een opdrachtprompt.
1. Typ een voorbeeldopdracht in. Bijvoorbeeld, als alle taken op het cluster, typt u een opdracht die vergelijkbaar is met een van de volgende, afhankelijk van de volledige DNS-naam van het hoofdknooppunt:
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    of
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > Gebruik de volledige DNS-naam van het hoofdknooppunt, niet het IP-adres, in de scheduler-URL. Als u het IP-adres opgeeft, een fout wordt weergegeven die vergelijkbaar is met "het servercertificaat moet te hebben een geldige vertrouwensketen of in het vertrouwde basisarchief worden geplaatst."
   > 
   > 
1. Wanneer u wordt gevraagd, typt u de gebruikersnaam (in het formulier &lt;DomainName&gt;\\&lt;gebruikersnaam&gt;) en het wachtwoord van de beheerder van de HPC-cluster of een ander clustergebruiker die u hebt geconfigureerd. U kunt de referenties voor meer taakbewerkingen lokaal opgeslagen.
   
    Er wordt een lijst weergegeven van taken.

**Gebruik van HPC Job Manager op de clientcomputer**

1. Als u niet eerder domeinreferenties voor een clustergebruiker opslaat bij het indienen van een taak, kunt u de referenties in Aanmeldingsgegevensbeheer toevoegen.
   
    a. Start Referentiebeheer in het Configuratiescherm op de clientcomputer.
   
    b. Klik op **Windows-referenties** > **een algemene referentie toevoegen**.
   
    c. Geef het adres (bijvoorbeeld: https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler of https://&lt;HeadNodeDnsName&gt;.&lt; regio&gt;.cloudapp.azure.com/HpcScheduler), en de naam van de gebruiker (&lt;DomainName&gt;\\&lt;gebruikersnaam&gt;) en het wachtwoord van de Clusterbeheerder van de of een andere clustergebruiker die u hebt geconfigureerd.
1. Start HPC Job Manager op de clientcomputer.
1. In de **hoofdknooppunt Selecteer** dialoogvenster vak, typ de URL naar het hoofdknooppunt in Azure (bijvoorbeeld: https://&lt;HeadNodeDnsName&gt;. cloudapp.net of https://&lt;HeadNodeDnsName&gt;.&lt; regio&gt;. cloudapp.azure.com).
   
    HPC Job Manager wordt geopend en ziet u een lijst met taken op het hoofdknooppunt.

**Het gebruik van de webportal die worden uitgevoerd op het hoofdknooppunt**

1. Start een webbrowser op de clientcomputer en voer een van de volgende adressen, afhankelijk van de volledige DNS-naam van het hoofdknooppunt:
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    of
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
1. Typ in het beveiligingsdialoogvenster de domeinreferenties van de beheerder van de HPC-cluster. (U kunt ook andere cluster gebruikers toevoegen in verschillende rollen. Zie [beheren van gebruikers van de Cluster](https://technet.microsoft.com/library/ff919335.aspx).)
   
    De web-portal de takenlijstweergave wordt geopend.
1. Als u een voorbeeldtaak die de tekenreeks "Hallo wereld" uit het cluster retourneert wilt, klikt u op **nieuwe taak** in de navigatiebalk links.
1. Op de **nieuwe taak** pagina onder **van verzending van pagina's**, klikt u op **HelloWorld**. De pagina van de taak verzenden wordt weergegeven.
1. Klik op **indienen**. Als u hierom wordt gevraagd, bieden de domeinreferenties van de beheerder van de HPC-cluster. De taak is verzonden, en de taak-ID wordt weergegeven op de **mijn taken** pagina.
1. Als u wilt weergeven van de resultaten van de taak die u wilde verzenden, klikt u op de taak-ID en klik vervolgens op **taken** om de opdrachtuitvoer weer te geven (onder **uitvoer**).

## <a name="next-steps"></a>Volgende stappen
* U kunt ook verzenden van taken naar de Azure-cluster met de [HPC Pack REST-API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).
* Als u verzenden van taken van de cluster van een client voor Linux wilt, raadpleegt u het Python-voorbeeld in de [HPC Pack 2012 R2 SDK en voorbeeldcode](https://www.microsoft.com/download/details.aspx?id=41633).

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
