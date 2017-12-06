---
title: Implementeren van Azure File-synchronisatie (preview) | Microsoft Docs
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
ms.openlocfilehash: 7b4de3e7b7e98ab76c02ea7c1cf069cee94706fc
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-file-sync-preview"></a>Azure File-synchronisatie (preview) implementeren
Gebruik Azure bestand Sync (preview) te centraliseren bestandsshares van uw organisatie in Azure-bestanden, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestand-server. Azure File-synchronisatie transformeert Windows Server in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB en NFS FTPS gebruiken. U kunt zoveel caches als u over de hele wereld nodig hebben.

Wij raden u lezen [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md) en [Planning voor de implementatie van een Azure-bestand Sync](storage-sync-files-planning.md) voordat u de stappen in dit artikel wordt beschreven.

## <a name="prerequisites"></a>Vereisten
* Een Azure Storage-account en een Azure-bestand delen in dezelfde regio die u wilt implementeren Azure File-synchronisatie. Zie voor meer informatie:
    - [Beschikbaarheid in regio's](storage-sync-files-planning.md#region-availability) voor Azure File-synchronisatie.
    - [Een opslagaccount maken](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor een stapsgewijze beschrijving van het maken van een opslagaccount.
    - [Een bestandsshare maken](storage-how-to-create-file-share.md) voor een stapsgewijze beschrijving van het maken van een bestandsshare.
* Ten minste één ondersteund exemplaar van Windows Server of Windows Server-cluster te synchroniseren met het synchroniseren van Azure-bestand. Zie voor meer informatie over ondersteunde versies van Windows Server [interoperabiliteit met Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability).

## <a name="deploy-the-storage-sync-service"></a>De synchronisatieservice opslag implementeren 
De Storage-Sync-Service is op het hoogste niveau Azure-resource voor Azure File-synchronisatie. Voor het implementeren van een opslag-Sync-Service, gaat u naar de [Azure-portal](https://portal.azure.com/), en zoek vervolgens naar Azure File-synchronisatie. Selecteer in de lijst met zoekresultaten **Azure bestand Sync (preview)**, en selecteer vervolgens **maken** openen de **implementeren opslag Sync** tabblad.

Voer de volgende gegevens in het deelvenster dat wordt geopend:

- **Naam**: een unieke naam (per abonnement) voor de synchronisatieservice opslag.
- **Abonnement**: het abonnement waarin u wilt maken van de Storage-Sync-Service. Afhankelijk van de strategie van de configuratie van uw organisatie wellicht u de toegang tot een of meer abonnementen. Een Azure-abonnement is de meest eenvoudige container voor facturering voor elke cloudservice (zoals Azure-bestanden).
- **Resourcegroep**: een resourcegroep is een logische groep van Azure-resources, zoals een opslagaccount of een opslag-Sync-Service. U kunt een nieuwe resourcegroep maken of een bestaande resourcegroep gebruiken voor Azure File-synchronisatie. (Aangeraden resourcegroepen gebruiken als containers voor het isoleren van resources voor uw organisatie, zoals HR-resources of resources voor een bepaald project groeperen logisch.)
- **Locatie**: de regio waarin u wilt implementeren van Azure File-synchronisatie. Alleen ondersteunde regio's zijn beschikbaar in deze lijst.

Wanneer u klaar bent, selecteert u **maken** voor het implementeren van de Storage-Sync-Service.

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Voorbereiden van Windows Server voor gebruik met Azure File-synchronisatie
Voor elke server die u gebruiken met het synchroniseren van Azure-bestand wilt, met inbegrip van knooppunten in een failovercluster, moet u de volgende stappen uitvoeren:

1. Schakel **verbeterde beveiliging van Internet Explorer**. Dit is alleen vereist voor de registratie van de oorspronkelijke server. U kunt deze weer inschakelen nadat de server is geregistreerd.
    1. Open Serverbeheer.
    2. Klik op **lokale Server**:  
        !['Lokale Server' aan de linkerkant van de Serverbeheer-UI](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. Op de **eigenschappen** subpane, selecteer de koppeling voor **verbeterde beveiliging van Internet Explorer**.  
        ![Het deelvenster 'IE verbeterde beveiliging' in de Serverbeheer-UI](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. In de **verbeterde beveiliging van Internet Explorer** dialoogvenster, **uit** voor **beheerders** en **gebruikers**:  
        ![De verbeterde beveiliging van Internet Explorer-pop-upvenster met 'Uit' geselecteerd](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. Zorg ervoor dat u ten minste uitgevoerd PowerShell 5.1.\* (PowerShell 5.1 is de standaardwaarde op Windows Server 2016). U kunt controleren of u PowerShell 5.1 worden uitgevoerd. \* door te kijken naar de waarde van de **PSVersion** eigenschap van de **$PSVersionTable** object:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    Als de waarde van de PSVersion minder dan 5.1 is. \*, worden als het geval is bij de meeste installaties van Windows Server 2012 R2, u kunt vervolgens gemakkelijk upgraden door te downloaden en installeren van [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). Het juiste pakket downloaden en installeren voor Windows Server 2012 R2 is **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

3. [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). U wordt aangeraden de nieuwste versie van de Azure PowerShell-modules.

## <a name="install-the-azure-file-sync-agent"></a>De Azure-bestand Sync-agent installeren
De Azure-bestand Sync-agent is een downloadbare pakket waarmee Windows-Server moet worden gesynchroniseerd met een Azure-bestandsshare. U kunt downloaden via de agent uit de [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Wanneer het downloaden is voltooid, dubbelklikt u op het MSI-pakket voor het starten van de installatie van de Azure-bestand Sync-agent.

> [!Important]  
> Als u gebruiken van Azure File-synchronisatie met een failovercluster wilt, moet u de Azure-bestand Sync-agent geïnstalleerd op elk knooppunt in het cluster.

Het installatiepakket voor de Azure-bestand Sync-agent moet relatief snel en zonder te veel extra prompts installeren. U wordt aangeraden dat u het volgende doen:
- Laat het standaardinstallatiepad (C:\Program Files\Azure\StorageSyncAgent), het oplossen van problemen en serveronderhoud vereenvoudigen.
- Schakel Microsoft Update om Azure bestand Sync up-to-date te houden. Alle updates optreden naar de Azure bestand Sync-agent, met inbegrip van functie-updates en hotfixes, van Microsoft Update. Het is raadzaam om de meest recente update installeren om te synchroniseren van Azure-bestand. Zie voor meer informatie [Azure bestand Sync updatebeleid](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Wanneer de installatie van de Azure-bestand Sync-agent is voltooid, wordt de gebruikersinterface van de registratie Server automatisch wordt geopend. Zie voor informatie over het registreren van deze server met het synchroniseren van Azure-bestand, de volgende sectie.

## <a name="register-windows-server-with-storage-sync-service"></a>WindowsServer registreren met Storage Sync-Service
Windows-Server registreren met een opslag-synchronisatieservice stelt een vertrouwensrelatie tussen uw server (of een cluster) en de opslag-synchronisatieservice. De gebruikersinterface van de registratie-Server wordt automatisch geopend na de installatie van de Azure-bestand Sync-agent. Als dit niet het geval is, kunt u deze handmatig openen vanuit de bestandslocatie: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe. Wanneer de gebruikersinterface van de registratie-Server wordt geopend, selecteert u **aanmelden** om te beginnen.

Nadat u zich aanmeldt, wordt u gevraagd om de volgende informatie:

![Een schermopname van de gebruikersinterface van de registratie van Server](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure-abonnement**: het abonnement dat de synchronisatieservice opslag bevat (Zie [implementeren van de synchronisatieservice opslag](#deploy-the-storage-sync-service)). 
- **Resourcegroep**: de resourcegroep met de Storage-Sync-Service.
- **Opslag-synchronisatieservice**: de naam van de Storage-Sync-Service die u wilt registreren.

Nadat u de juiste gegevens hebt geselecteerd, selecteert u **registreren** om de inschrijving van de server te voltooien. Als onderdeel van het registratieproces wordt u gevraagd voor een extra aanmelden.

## <a name="create-a-sync-group"></a>Maken van een groep voor synchronisatie
Een groep voor synchronisatie definieert de synchronisatie-topologie voor een set bestanden. Eindpunten in een groep voor synchronisatie zijn gesynchroniseerd met elkaar. Een groep voor synchronisatie moet ten minste één cloudeindpunt dat vertegenwoordigt een Azure-bestandsshare, en één servereindpunt, waarmee een pad op Windows Server bevatten. Maken van een groep voor synchronisatie in de [Azure-portal](https://portal.azure.com/), gaat u naar uw Storage-Sync-Service en selecteer vervolgens **+ groep voor synchronisatie**:

![Maak een nieuwe synchronisatie-groep in de Azure portal](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Voer de volgende informatie om u te maken van een groep voor synchronisatie met een cloudeindpunt in het deelvenster dat wordt geopend:

- **Synchronisatie-groepsnaam**: de naam van de groep voor synchronisatie moet worden gemaakt. Deze naam moet uniek zijn binnen de synchronisatieservice opslag, maar mag de naam die logisch is voor u.
- **Abonnement**: het abonnement waarbij u de Storage-Sync-Service in geïmplementeerd [implementeren van de synchronisatieservice opslag](#deploy-the-storage-sync-service).
- **Storage-account**: als u selecteert **storage-account selecteren**, een ander venster weergegeven waarin u het opslagaccount met de Azure-bestandsshare die u synchroniseren wilt met kunt selecteren.
- **Azure-bestandsshare**: de naam van de Azure-bestandsshare die u wilt synchroniseren.

Een servereindpunt toevoegen, gaat u naar de zojuist gemaakte sync-groep en selecteer vervolgens **servereindpunt toevoegen**.

![Een nieuw servereindpunt in het deelvenster van de groep sync toevoegen](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

In de **servereindpunt toevoegen** deelvenster, voer de volgende informatie om een servereindpunt te maken:

- **Geregistreerde server**: de naam van de server of cluster waar u wilt maken van de server het eindpunt.
- **Pad**: de Windows Server-pad moet worden gesynchroniseerd als onderdeel van de groep voor synchronisatie.
- **Cloud Tiering**: overschakelen naar de in- of uitschakelen, cloud tiering. Met cloud tiering worden zelden gebruikt of de bestanden zijn geopend kunt tiers worden verdeeld aan Azure-bestanden.
- **Vrije ruimte op volume**: de hoeveelheid vrije ruimte te reserveren op het volume waarop het servereindpunt zich bevindt. Bijvoorbeeld, als vrije ruimte op volume is ingesteld op 50% op een volume dat een eindpunt met één server is, is ongeveer de helft van de hoeveelheid gegevens gelaagd op Azure-bestanden. Ongeacht of cloud tiering is ingeschakeld, worden uw Azure-bestandsshare is altijd een volledige kopie van de gegevens in de groep voor synchronisatie.

Selecteer om de server het eindpunt toe **maken**. Uw bestanden worden nu gesynchroniseerd tussen uw Azure-bestandsshare en Windows Server. 

> [!Important]  
> U kunt wijzigingen aanbrengen in een cloudeindpunt of het servereindpunt in de groep voor synchronisatie en hebben de bestanden die zijn gesynchroniseerd met de andere eindpunten in de groep voor synchronisatie. Als u een wijziging van het eindpunt van de cloud (Azure-bestandsshare) rechtstreeks, moeten wijzigingen eerst worden gedetecteerd door een Azure-bestand Sync-taak voor het detecteren van wijzigen. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt slechts eenmaal per 24 uur. Zie voor meer informatie [Veelgestelde vragen over Azure-bestanden](storage-files-faq.md#afs-change-detection).

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>Een DFS-replicatie (DFS-R)-implementatie migreren naar Azure File-synchronisatie
Een DFS-R-implementatie migreren naar Azure File-synchronisatie:

1. Maak een synchronisatie-groep staat voor de DFS-R-topologie die u wilt vervangen.
2. Start op de server met de volledige set gegevens in uw DFS-R-topologie te migreren. Azure File-synchronisatie installeren op die server.
3. Registreren van die server en maak een servereindpunt voor de eerste server worden gemigreerd. Schakel geen cloud tiering.
4. Alle het synchroniseren van gegevens naar uw Azure-bestandsshare (cloudeindpunt) laten.
5. Installeren en registreren van de Azure-bestand Sync-agent op elk van de resterende DFS-R-servers.
6. Uitschakelen van DFS-R. 
7. Maak een servereindpunt op elk van de DFS-R-servers. Schakel geen cloud tiering.
8. Zorg ervoor dat synchronisatie is voltooid en testen van uw topologie desgewenst.
9. Buiten gebruik stellen van DFS-R.
10. Cloud lagen kan nu worden ingeschakeld op een willekeurig servereindpunt naar wens.

Zie voor meer informatie [Azure bestand Sync interop met Distributed File System (DFS)](storage-sync-files-planning.md#distributed-file-system-dfs).

## <a name="next-steps"></a>Volgende stappen
- [Toevoegen of verwijderen van een servereindpunt van het Azure-synchronisatie](storage-sync-files-server-endpoint.md)
- [Registreren of de registratie van een server met het synchroniseren van Azure-bestand](storage-sync-files-server-registration.md)
