---
title: Het implementeren van Azure File-synchronisatie (preview) | Microsoft Docs
description: Informatie over het implementeren van Azure bestand synchronisatie van begin tot eind.
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: b31b6ae413f72c626e2601ba860aad44ddaa29cd
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-deploy-azure-file-sync-preview"></a>Het implementeren van Azure File-synchronisatie (preview)
Met Azure File Sync (preview) kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door de Windows-Servers om te zetten in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Het wordt aangeraden lezen [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md) en [Planning voor de implementatie van een Azure-bestand Sync](storage-sync-files-planning.md) voordat u de stappen in deze handleiding.

## <a name="prerequisites"></a>Vereisten
* Een Opslagaccount en een Azure-bestand delen in dezelfde regio die u wilt implementeren Azure File-synchronisatie. Zie voor meer informatie:
    - [Beschikbaarheid in regio's](storage-sync-files-planning.md#region-availability) van Azure bestand Sync
    - [Een Opslagaccount maken](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor stapsgewijze instructies voor het maken van een Opslagaccount en
    - [Een bestandsshare maken](storage-how-to-create-file-share.md) voor stapsgewijze instructies voor het maken van een bestandsshare.
* Ten minste één ondersteund Windows Server of Windows Server-cluster te synchroniseren met het synchroniseren van Azure-bestand. Zie [interoperabiliteit met Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability) voor meer informatie over ondersteunde versies van Windows Server.

## <a name="deploy-the-storage-sync-service"></a>De synchronisatieservice opslag implementeren 
De Storage-Sync-Service is de site op het hoogste Azure-resource voor Azure File-synchronisatie. Voor het implementeren van een opslag-Sync-Service, gaat u naar de [Azure-portal](https://portal.azure.com/), en zoek naar Azure File-synchronisatie. Als u 'Azure File-synchronisatie (preview)' in de zoekresultaten, selecteer 'Maken' moet worden weergegeven, open het ' implementeren opslag ' tabblad synchroniseren.

De resulterende blade vraagt om de volgende informatie:

- **Naam**: een unieke naam (per abonnement) voor de synchronisatieservice opslag.
- **Abonnement**: het abonnement in te maken van de Storage-Sync-Service. Afhankelijk van de strategie van de configuratie van uw organisatie wellicht u de toegang tot een of meer abonnementen. Een Azure-abonnement is de meest eenvoudige container voor facturering voor elke cloudservice (zoals Azure-bestanden).
- **Resourcegroep**: een resourcegroep is een logische groep van Azure-resources, zoals een Opslagaccount of een opslag-Sync-Service. U kunt een nieuwe resourcegroep maken of een bestaande resourcegroep gebruiken voor Azure File-synchronisatie (u wordt aangeraden de resourcegroepen gebruiken als containers die worden gebruikt voor het scheiden van resources voor uw organisatie, zoals HR-resources of resources voor een bepaald project groeperen logisch) .
- **Locatie**: de regio waarin u wilt implementeren Azure File-synchronisatie. Alleen ondersteunde regio's zijn beschikbaar in deze lijst.

Nadat de vorm 'Implementeren van opslag Sync' is voltooid, klikt u op 'Maken' voor het implementeren van de Storage-Sync-Service.

## <a name="prepare-windows-servers-for-use-with-azure-file-sync"></a>Windows-Servers voorbereiden voor gebruik met Azure File-synchronisatie
Voor ooit server die u wilt gebruiken voor de Azure bestand Sync, met inbegrip van knooppunten in een failovercluster, gebruiken de volgende stappen uitvoeren:

Voor elke server, inclusief serverknooppunten in een failovercluster dat u wilt gebruiken met het synchroniseren van Azure-bestand, de volgende stappen uit:

1. Verbeterde beveiliging van Internet Explorer uitschakelen. Dit is alleen voor serverregistratie bij de eerste vereist en kan worden ingeschakeld nadat de server is geregistreerd.
    1. Open Serverbeheer.
    2. Klik op **lokale Server**:  
        !['Lokale Server' aan de linkerkant van de Serverbeheer-UI](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. Selecteer de koppeling voor **verbeterde beveiliging van Internet Explorer** op het onderliggende deelvenster Eigenschappen:  
        ![De 'configuratie Verbeterde beveiliging IE' in de gebruikersinterface van Serverbeheer](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. Selecteer **uit** voor beheerders en gebruikers in het pop-upvenster met verbeterde beveiliging van Internet Explorer:  
        ![De verbeterde beveiliging van Internet Explorer-pop-upvenster met 'Uit' geselecteerd](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. Zorg ervoor dat u ten minste uitgevoerd PowerShell 5.1.\* (PowerShell 5.1 is de standaardwaarde op Windows Server 2016). U kunt controleren of dat u PowerShell 5.1 uitvoert. \* door te kijken naar de waarde van de eigenschap PSVersion van het object $PSVersionTable:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    - Als uw PSVersion minder dan 5.1 is. \*, worden als het geval is bij de meeste installaties van Windows Server 2012 R2, u kunt vervolgens gemakkelijk upgraden door te downloaden en installeren van [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). Het juiste pakket downloaden en installeren voor Windows Server 2012 R2 is **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

3. [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Altijd het beste de nieuwste versie van de Azure PowerShell-modules gebruiken.

## <a name="install-the-azure-file-sync-agent"></a>De Azure-bestand Sync-agent installeren
De Azure-bestand Sync-agent is een downloadbare pakket zodat een Windows Server kunnen worden gesynchroniseerd met een Azure-bestand delen. De agent kan worden gedownload vanaf de [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Zodra u hebt gedownload, dubbelklikt u op het MSI-pakket in de Azure-bestand Sync-agent-installatie te starten.

> [!Important]  
> Als u gebruiken van Azure File-synchronisatie met een failovercluster wilt, moet de Azure-bestand Sync-agent moet worden geïnstalleerd op elk knooppunt in het cluster.

Het installatiepakket voor de Azure-bestand Sync-agent moet installeren relatief snel zonder te veel extra prompts. We raden het volgende:
- Laat de eigenschap standaardinstallatiepad `C:\Program Files\Azure\StorageSyncAgent`) en het oplossen van problemen serveronderhoud vereenvoudigen.
- Inschakelen van Microsoft Update om Azure bestand Sync up-to-date te houden. Alle updates, met inbegrip van functie-updates en hotfixes met de Azure-bestand Sync-agent wordt uitgevoerd vanaf Microsoft Update. Aangeraden wordt altijd de meest recente update naar Azure bestand Sync geleid. Zie [Azure bestand Sync updatebeleid](storage-sync-files-planning.md#azure-file-sync-agent-update-policy) voor meer informatie.

Aan het einde van de installatie van de Azure-bestand Sync-agent, de gebruikersinterface van de registratie-Server wordt automatisch starten. Zie de volgende sectie voor informatie over het registreren van deze server met het synchroniseren van Azure-bestand.

## <a name="register-windows-server-with-storage-sync-service"></a>WindowsServer registreren met Storage Sync-Service
Uw Windows-Server registreren met een opslag-synchronisatieservice stelt een vertrouwensrelatie tussen uw server (of een cluster) en de opslag-synchronisatieservice. De gebruikersinterface van de registratie-Server moet automatisch starten na de installatie van de Azure-bestand Sync-agent, maar als dit niet het geval is, kunt u deze handmatig openen vanaf de locatie: `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`. Wanneer de gebruikersinterface van de registratie-Server geopend is, klikt u op **aanmelden** om te beginnen.

Na het aanmelden, wordt u gevraagd om de volgende informatie:

![Een schermopname van de gebruikersinterface van de registratie van Server](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure-abonnement**: het abonnement met de Storage-Sync-Service (Zie [implementeren van de synchronisatieservice opslag](#deploy-the-storage-sync-service) hierboven). 
- **Resourcegroep**: de resourcegroep met de Storage-Sync-Service.
- **Opslag-synchronisatieservice**: de naam van de Storage-Sync-Service die u wilt registreren.

Nadat u de juiste informatie in de keuzelijsten neerzetten hebt geselecteerd, klikt u op **registreren** om de inschrijving van de server te voltooien. Als onderdeel van het registratieproces wordt u gevraagd voor een extra aanmelden.

## <a name="create-a-sync-group"></a>Maken van een groep voor synchronisatie
Een groep voor synchronisatie definieert de synchronisatie-topologie voor een set bestanden. Eindpunten in een groep voor synchronisatie worden, synchroon met elkaar bewaard. Een groep voor synchronisatie moet ten minste één Cloudeindpunt dat vertegenwoordigt een Azure-bestandsshare, en één servereindpunt waarmee een pad op een Windows-Server bevatten. Voor het maken van een groep voor synchronisatie, gaat u naar uw Storage-Sync-Service in de [Azure-portal](https://portal.azure.com/), en klik op **+ groep voor synchronisatie**:

![Nieuwe groep voor synchronisatie in de Azure portal maken](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Het resulterende deelvenster vraagt om de volgende informatie voor het maken van een groep voor synchronisatie met een Cloud-eindpunt:

- **Groepsnaam synchroniseren**: de naam van de groep voor synchronisatie moet worden gemaakt. Deze naam moet uniek zijn binnen de synchronisatieservice opslag, maar mag de naam die logisch is voor u.
- **Abonnement**: het abonnement dat u de Storage-Sync-Service in geïmplementeerd [implementeren van de synchronisatieservice opslag](#deploy-the-storage-sync-service) hierboven.
- **Storage-account**: als u 'Storage-account selecteren', pop opent een extra deelvenster zodat u het opslagaccount met de Azure-bestandsshare die u wilt synchroniseren selecteren.
- **Azure-bestandsshare**: de naam van de Azure-bestandsshare die u wilt synchroniseren.

Als u wilt een eindpunt van de Server toevoegen, gaat u naar de zojuist gemaakte groep voor synchronisatie en klik op 'servereindpunt toevoegen'.

![Een nieuwe Server-eindpunt toevoegen in het deelvenster van de groep voor synchronisatie](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

Het deelvenster resulterende 'Add servereindpunt' moet de volgende informatie om een eindpunt van de Server te maken:

- **Server geregistreerd**: de naam van de server of het cluster waarop moet worden gemaakt van de Server het eindpunt.
- **Pad**: het pad op de Windows Server kunnen worden gesynchroniseerd als onderdeel van de groep voor synchronisatie.
- **Cloud Tiering**: een switch of cloud tiering uit te schakelen zodat zelden gebruikt of toegang tot bestanden op Azure-bestanden in tiers worden verdeeld.
- **Vrije ruimte op volume**: de hoeveelheid vrije ruimte te reserveren op het volume waarop het eindpunt van de Server zich bevindt. Bijvoorbeeld, als de vrije ruimte op Volume is ingesteld op 50% op een volume met een eindpunt met één Server, ongeveer de helft van de hoeveelheid gegevens wordt tiers worden verdeeld aan Azure-bestanden. Let op: ongeacht of cloud tiering is ingeschakeld, wordt uw Azure-bestandsshare is altijd een volledige kopie van de gegevens in de groep voor synchronisatie.

Klik op 'Maken' om toe te voegen van de Server het eindpunt. Uw bestanden wordt nu gesynchroniseerd blijven in uw Azure-bestandsshare en uw Windows-Server. 

> [!Important]  
> U kunt wijzigingen aanbrengen in een Cloud of een eindpunt van de Server in de groep voor synchronisatie en uw bestanden gesynchroniseerd hebben met de andere eindpunten in de groep voor synchronisatie. Als u een wijziging van het eindpunt van de Cloud (Azure File share) rechtstreeks, houd er rekening mee dat wijzigingen moeten eerst worden gedetecteerd door een Azure-bestand synchronisatie wijzigen detectie taak, dat alleen initatiated voor een Cloudeindpunt om de 24 uur. Zie de [Veelgestelde vragen over Azure-bestanden](storage-files-faq.md#afs-change-detection) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
- [Een Azure-bestand Sync servereindpunt toevoegen of verwijderen](storage-sync-files-server-endpoint.md)
- [Een server met het synchroniseren van Azure bestand registreren/opgeheven](storage-sync-files-server-registration.md)