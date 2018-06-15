---
title: Verzenden van taken naar een Pack HPC-cluster in Azure | Microsoft Docs
description: Meer informatie over het instellen van een on-premises computer voor het verzenden van taken naar een cluster HPC Pack in Azure
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
ms.openlocfilehash: 025ff3dea365ab75af55f107da1fb7331861eb06
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34166366"
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>HPC-taken vanaf een on-premises computer verzenden naar een HPC Pack-cluster die in Azure is geïmplementeerd
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Verzenden van taken naar een client op de lokale computer configureren voor een [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure. In dit artikel leest u hoe een lokale computer met clienthulpprogramma's instellen voor het verzenden van de taak via HTTPS aan het cluster in Azure. Op deze manier kunnen meerdere cluster gebruikers verzenden van taken naar een cluster met cloud-gebaseerde HPC Pack, maar zonder rechtstreeks verbinding maken met het hoofdknooppunt VM of toegang tot een Azure-abonnement.

![Verzenden van een taak aan een cluster in Azure][jobsubmit]

## <a name="prerequisites"></a>Vereisten
* **HPC Pack hoofdknooppunt geïmplementeerd in een Azure VM** -het is raadzaam dat u hulpprogramma's, zoals een [snelstartsjabloon met de Azure](https://azure.microsoft.com/documentation/templates/) het hoofdknooppunt en cluster implementeren. U moet de DNS-naam van het hoofdknooppunt en de referenties van de Clusterbeheerder van een om de stappen in dit artikel te voltooien.
* **Clientcomputer** -moet u een Windows- of Windows Server-clientcomputer die HPC Pack client-hulpprogramma's kan worden uitgevoerd (Zie [systeemvereisten](https://technet.microsoft.com/library/dn535781.aspx)). Als u alleen de HPC Pack web-portal of REST-API gebruiken om taken te verzenden wilt, kunt u elke clientcomputer van uw keuze.
* **HPC Pack installatiemedia** - installeren van het installatiepakket voor de gratis hulpprogramma's client HPC Pack voor de nieuwste versie van HPC Pack beschikbaar via is de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56360). Zorg ervoor dat u dezelfde versie van HPC Pack die is geïnstalleerd op het hoofdknooppunt VM downloaden.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Stap 1: Installeren en configureren van de webonderdelen op het hoofdknooppunt
Zorg ervoor dat de webonderdelen HPC Pack zijn geconfigureerd op het hoofdknooppunt HPC Pack zodat een REST-interface om taken te verzenden naar het cluster via HTTPS. Als ze niet zijn geïnstalleerd, moet u eerst de webonderdelen installeren door het uitvoeren van het installatiebestand HpcWebComponents.msi. Configureer vervolgens de onderdelen in de HPC PowerShell-script uit te voeren **Set HPCWebComponents.ps1**.

Zie voor gedetailleerde procedures [installeren van de webonderdelen van Microsoft HPC Pack](http://technet.microsoft.com/library/hh314627.aspx).

> [!TIP]
> Bepaalde Azure-snelstartsjablonen voor clusters HPC Pack installeren en configureren van de webonderdelen automatisch.
> 
> 

**De web-onderdelen installeren**

1. Verbinding maken met het hoofdknooppunt VM met de referenties van de Clusterbeheerder van een.
2. Uitvoeren vanuit de installatiemap van HPC Pack HpcWebComponents.msi op het hoofdknooppunt.
3. Volg de stappen in de wizard voor het installeren van de webonderdelen

**De webonderdelen configureren**

1. Start op het hoofdknooppunt HPC PowerShell als beheerder.
2. Als u de map naar de locatie van het configuratiescript, typ de volgende opdracht:
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. Als u wilt configureren, de REST-interface en start de HPC-webservice, typ de volgende opdracht:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. Wanneer u wordt gevraagd om een certificaat te selecteren, kiest u het certificaat dat overeenkomt met de openbare DNS-naam van het hoofdknooppunt. Bijvoorbeeld, als u het hoofdknooppunt VM die gebruikmaakt van het klassieke implementatiemodel implementeert, de certificaatnaam ziet eruit als CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Als u het implementatiemodel van Resource Manager gebruikt, wordt de certificaatnaam lijkt op CN =&lt;*HeadNodeDnsName*&gt;.&lt; *regio*&gt;. cloudapp.azure.com.
   
   > [!NOTE]
   > U selecteert dit certificaat later bij het verzenden van taken met het hoofdknooppunt van een on-premises computer. Geen selecteren of een certificaat configureren dat overeenkomt met de naam van het hoofdknooppunt in Active Directory-domein (bijvoorbeeld CN =*MyHPCHeadNode.HpcAzure.local*).
   > 
   > 
5. Typ de volgende opdracht voor het configureren van de web-portal voor het indienen van de taak:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Nadat het script is voltooid, stoppen en de HPC Job Scheduler-Service opnieuw starten door de volgende opdrachten te typen:
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Stap 2: De hulpprogramma's voor HPC Pack client installeren op een on-premises computer
Als u de hulpprogramma's voor HPC Pack client installeren op uw computer wilt, de HPC Pack setup-bestanden (volledige installatie) downloaden van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56360). Wanneer u de installatie begint, selecteer de installatieoptie voor de **HPC Pack client utilities**.

Als u de hulpprogramma's voor HPC Pack wilt verzenden van taken naar het hoofdknooppunt VM, moet u ook een certificaat exporteren van het hoofdknooppunt en installeer deze op de clientcomputer. Het certificaat moet zich in. CER-indeling.

**Het certificaat te exporteren vanaf het hoofdknooppunt van**

1. Op het hoofdknooppunt toevoegen de module Certificaten aan een Microsoft Management Console voor het lokale computeraccount. Zie voor stappen om toe te voegen de module [de module Certificaten toevoegen aan een MMC](https://technet.microsoft.com/library/cc754431.aspx).
2. Vouw in de consolestructuur **certificaten-lokale Computer** > **persoonlijke**, en klik vervolgens op **certificaten**.
3. Zoek het certificaat dat u hebt geconfigureerd voor de webonderdelen HPC Pack in [stap 1: installeren en configureren van de webonderdelen op het hoofdknooppunt](#step-1:-install-and-configure-the-web-components-on-the-head-node) (bijvoorbeeld CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).
4. Met de rechtermuisknop op het certificaat en klik op **alle taken** > **exporteren**.
5. Klik in de Wizard Certificaat exporteren op **volgende**, en zorg ervoor dat **Nee, de persoonlijke sleutel niet exporteren** is geselecteerd.
6. Voer de resterende stappen van de wizard voor het exporteren van het certificaat in DER encoded binary X.509 (. De indeling van de CER).

**Het certificaat op de clientcomputer importeren**

1. Het certificaat dat u hebt geëxporteerd vanaf het hoofdknooppunt van naar een map op de clientcomputer kopiëren.
2. Voer op de clientcomputer certmgr.msc.
3. Vouw in de certificaatbeheerder **certificaten-huidige gebruiker** > **Trusted Root Certification Authorities**, met de rechtermuisknop op **certificaten**, en vervolgens Klik op **alle taken** > **importeren**.
4. Klik in de Wizard Certificaat importeren op **volgende** en volg de stappen voor het importeren van het certificaat dat u hebt geëxporteerd uit het hoofdknooppunt in het archief Vertrouwde basiscertificeringsinstanties.

> [!TIP]
> U ziet mogelijk een beveiligingswaarschuwing, omdat de certificeringsinstantie op het hoofdknooppunt wordt niet herkend door de clientcomputer. Voor testdoeleinden kunt u deze waarschuwing negeren en het certificaat importeren voltooien.
> 
> 

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Stap 3: Voer testtaken op het cluster
Om te controleren of uw configuratie, probeer taken uitgevoerd op het cluster in Azure vanaf de lokale computer. U kunt bijvoorbeeld HPC Pack GUI-hulpprogramma's of de opdrachtregelopdrachten gebruiken om taken voor het cluster te verzenden. U kunt ook een web gebaseerde portal gebruiken om taken te verzenden.

**Taak verzending opdrachten uitvoeren op de clientcomputer**

1. Start een opdrachtprompt op een clientcomputer waarop de hulpprogramma's voor HPC Pack client zijn geïnstalleerd.
2. Typ een voorbeeld van een opdracht. Bijvoorbeeld, als alle taken op het cluster wilt weergeven, typt u een opdracht die vergelijkbaar is met een van de volgende, afhankelijk van de volledige DNS-naam van het hoofdknooppunt:
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    of
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > Gebruik de volledige DNS-naam van het hoofdknooppunt, niet het IP-adres in de scheduler-URL. Als u het IP-adres opgeeft, lijkt een fout op 'het servercertificaat moet ofwel een geldige vertrouwensketen hebben of in het vertrouwde basisarchief worden geplaatst.'
   > 
   > 
3. Wanneer u wordt gevraagd, typt u de gebruikersnaam (in de vorm &lt;DomainName&gt;\\&lt;gebruikersnaam&gt;) en het wachtwoord van de beheerder van de HPC-cluster of een ander cluster gebruiker die u hebt geconfigureerd. U kunt de referenties voor meer taakbewerkingen lokaal opgeslagen.
   
    Een lijst met taken wordt weergegeven.

**Gebruik van HPC Job Manager op de clientcomputer**

1. Als u niet eerder domeinreferenties voor een cluster gebruiker opslaat bij het indienen van een taak, kunt u de referenties in Aanmeldingsgegevensbeheer toevoegen.
   
    a. Start Referentiebeheer in het Configuratiescherm op de clientcomputer.
   
    b. Klik op **Windows-referenties** > **toevoegen van een algemene referentie**.
   
    c. Geef het internetadres (bijvoorbeeld https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler of https://&lt;HeadNodeDnsName&gt;.&lt; regio&gt;.cloudapp.azure.com/HpcScheduler), en de gebruikersnaam (&lt;DomainName&gt;\\&lt;gebruikersnaam&gt;) en het wachtwoord van de Clusterbeheerder van de of een ander cluster-gebruiker die u hebt geconfigureerd.
2. Start HPC Job Manager op de clientcomputer.
3. In de **hoofdknooppunt selecteren** dialoogvenster, typ de URL naar het hoofdknooppunt in Azure (bijvoorbeeld https://&lt;HeadNodeDnsName&gt;. cloudapp.net of https://&lt;HeadNodeDnsName&gt;.&lt; regio&gt;. cloudapp.azure.com).
   
    HPC Job Manager wordt geopend en ziet u een lijst met taken op het hoofdknooppunt.

**De webportal uitgevoerd op het hoofdknooppunt gebruiken**

1. Start een webbrowser op de clientcomputer en voer een van de volgende adressen, afhankelijk van de volledige DNS-naam van het hoofdknooppunt:
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    of
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Typ in het beveiligingsdialoogvenster de domeinreferenties van HPC-Clusterbeheer. (U kunt ook andere cluster gebruikers toevoegen in verschillende rollen. Zie [het beheren van gebruikers van de Cluster](https://technet.microsoft.com/library/ff919335.aspx).)
   
    De web-portal wordt geopend met de taak lijstweergave.
3. Als u een voorbeeld-taak die als resultaat de tekenreeks 'Hallo wereld' uit het cluster geeft, klikt u op **nieuwe taak** in de linkernavigatiebalk.
4. Op de **nieuwe taak** pagina onder **van verzending van pagina's**, klikt u op **HelloWorld**. De taak verzendpagina wordt weergegeven.
5. Klik op **indienen**. Als u wordt gevraagd, geeft u de domeinreferenties van HPC-Clusterbeheer. De taak wordt verzonden en de taak-ID wordt weergegeven op de **mijn taken** pagina.
6. U kunt de resultaten van de taak die u hebt ingediend, klikt u op de taak-ID en klik vervolgens op **taken weergeven** om uitvoer van de opdracht weer te geven (onder **uitvoer**).

## <a name="next-steps"></a>Volgende stappen
* U kunt ook verzenden van taken naar de Azure-cluster met de [HPC Pack REST-API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).
* Als u verzenden van cluster taken van een Linux-client wilt, Zie het voorbeeld Python in de [HPC Pack 2012 R2 SDK en voorbeeldcode](https://www.microsoft.com/download/details.aspx?id=41633).

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
