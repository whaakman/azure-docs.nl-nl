---
title: Registreren/verwijderen van een server met het synchroniseren van Azure-bestand (preview) | Microsoft Docs
description: Informatie over het registreren en ongedaan maken van een Windows-Server met een Azure-bestand Sync Storage Sync-Service.
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
ms.openlocfilehash: 13a75d5cafd94435346660614721399f2d77919b
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2017
---
# <a name="registerunregister-a-server-with-azure-file-sync-preview"></a>Een server met het synchroniseren van Azure-bestand (preview) register/unregister
Met Azure File Sync (preview) kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door de Windows-Servers om te zetten in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Het volgende artikel ziet u hoe registreren en ongedaan maken van een server met een opslag-Sync-Service. Dit kan nodig zijn als een server uit bedrijf wordt genomen, of als u een nieuw eindpunt van de Server in een groep voor synchronisatie. Zie [het implementeren van Azure File-synchronisatie (preview)](storage-sync-files-deployment-guide.md) voor informatie over het implementeren van Azure bestand Sync end-to-end.

## <a name="prerequisites"></a>Vereisten
Voor het registreren van een Windows-Server met een opslag-Sync-Service, moet u eerst een Windows-Server met de benodigde vereisten voorbereiden:

* Zorg ervoor dat een opslag-Sync-Service is geïmplementeerd. Zie voor meer informatie over het implementeren van een opslag-synchronisatieservice [het implementeren van Azure File-synchronisatie (preview)](storage-sync-files-deployment-guide.md).
* Zorg ervoor dat de server is verbonden met internet en of Azure toegankelijk is.
* De Internet Explorer Verbeterde beveiliging uitschakelt voor beheerders met de Serverbeheer-UI.
    
    ![Serverbeheer UI met de Internet Explorer Verbeterde beveiliging gemarkeerd](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Zorg ervoor dat de AzureRM PowerShell-module is geïnstalleerd op uw server. Als uw server lid is van een failovercluster, moet elk knooppunt in het cluster de AzureRM-module. Meer informatie over het installeren van de module AzureRM vindt u op de [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

    > [!Note]  
    > U kunt het beste de nieuwste versie van de AzureRM PowerShell-module registreren/verwijderen van een server gebruikt. Als het pakket AzureRM eerder op deze server is geïnstalleerd (en de PowerShell-versie op deze server is 5.* of hoger), kunt u de `Update-Module` cmdlet bij te werken van dit pakket. 

## <a name="register-a-server-with-storage-sync-service"></a>Een server met Storage Sync-Service registreren
Voordat u een Windows-Server kan worden gebruikt als een *servereindpunt* in een Azure-bestand synchronisatie *groep voor synchronisatie*, moet zijn geregistreerd bij een *Sync opslagservice*. Een server kan alleen worden geregistreerd met een *Sync opslagservice* tegelijk.

### <a name="install-the-azure-file-sync-agent"></a>De Azure-bestand Sync-agent installeren
1. [De Azure-bestand Sync-agent downloaden](https://go.microsoft.com/fwlink/?linkid=858257).
2. Start het installatieprogramma van de Azure-bestand Sync-agent.
    
    ![Het eerste gedeelte van het installatieprogramma van de Azure-bestand Sync-agent](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Zorg ervoor dat de Azure-bestand Sync-agent met behulp van Microsoft Update-updates inschakelen. Het is belangrijk omdat kritieke beveiligingsproblemen en functieverbeteringen aan het serverpakket worden verzonden via Microsoft Update.

    ![Zorg ervoor dat Microsoft Update is ingeschakeld in het deelvenster Microsoft Update van het installatieprogramma van de Azure-bestand Sync-agent](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Als de server heeft niet eerder is geregistreerd, wordt de registratie van de server UI onmiddellijk na voltooiing van de installatie weergegeven.

> [!Important]  
> Als de server lid is van een failovercluster, moet de Azure-bestand Sync-agent worden geïnstalleerd op elk knooppunt in het cluster.

### <a name="register-the-server-using-the-server-registration-ui"></a>De server met behulp van de registratie van UI-server registreren

> [!Important]  
> Abonnementen voor cloud Solution Provider kunnen de registratie van UI-server niet gebruiken. Gebruik in plaats daarvan PowerShell (onder deze sectie).

1. Als de registratie van UI-server niet gestart is onmiddellijk na voltooiing van de installatie van de Azure-bestand Sync-agent, het kan handmatig worden gestart door het uitvoeren van `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`.
2. Klik op *aanmelden* voor toegang tot uw Azure-abonnement. 

    ![Dialoogvenster van de gebruikersinterface van de registratie Server openen](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Kies de juiste abonnement, resourcegroep en Sync-Service voor opslag in het dialoogvenster.

    ![Storage-Sync-Service gegevens](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. In voorbeeld van is een meer aanmelden vereist om het proces te voltooien. 

    ![Dialoogvenster Aanmelden](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Als de server lid is van een failovercluster, wordt elke server moet worden uitgevoerd van de registratie van de Server. Wanneer u de geregistreerde Servers in de Azure Portal weergeeft, Azure File-synchronisatie wordt automatisch elk knooppunt wordt herkend als een lid van hetzelfde failovercluster en op de juiste wijze samen gegroepeerd.

### <a name="register-the-server-with-powershell"></a>De server geregistreerd met PowerShell
U kunt ook de registratie van de server via PowerShell uitvoeren. Dit is de enige ondersteunde manier van registratie van de server voor Cloud Solution Provider-abonnementen:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzureRmStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

## <a name="unregister-the-server-with-storage-sync-service"></a>Hef de registratie van de server met Storage Sync-Service
Er zijn verschillende stappen die nodig zijn voor de registratie van een server met een opslag-Sync-Service. Eens kijken hoe goed Hef de registratie van een server.

### <a name="optional-recall-all-tiered-data"></a>(Optioneel) Alle gelaagde gegevens intrekken
Wanneer ingeschakeld voor een eindpunt van de Server, cloud lagen wordt *laag* bestanden naar uw Azure-bestandsshares. Hierdoor kunnen lokale bestandsshares om te fungeren als een cache in plaats van een volledige kopie van de gegevensset om efficiënt gebruik van de ruimte op de bestandsserver te maken. Echter, als een eindpunt van de Server wordt verwijderd met gelaagde bestanden nog steeds lokaal op de server, die bestanden worden ontoegankelijk. Als u nog steeds toegang tot het bestand gewenst is, moet u alle bestanden van gelaagde van Azure Files daarom voordat u doorgaat met de uitschrijving intrekken. 

Dit kunt doen met de PowerShell-cmdlet zoals hieronder wordt weergegeven:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Warning]  
> Als het lokale volume waarop de Server niet voldoende vrije ruimte heeft voor het intrekken van de gelaagde gegevens, de `Invoke-StorageSyncFileRecall` -cmdlet mislukt.  

### <a name="remove-the-server-from-all-sync-groups"></a>De server verwijderen uit alle groepen voor synchronisatie
Voordat u de registratie van de server op de Storage-Sync-Service, moeten alle Server-eindpunten voor die server worden verwijderd. Dit kan worden gedaan via de Portal:

1. Navigeer naar de opslag-synchronisatieservice waar uw server is geregistreerd.
2. Verwijder alle Server-eindpunten voor deze server in elke groep voor synchronisatie in de Storage-Sync-Service. Dit kunt doen met de rechtermuisknop op de relevante Server-eindpunt in het deelvenster van de groep voor synchronisatie.

    ![Een servereindpunt verwijderen uit een groep voor synchronisatie](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Dit kan ook worden bewerkstelligd met een eenvoudig PowerShell-script:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Login-AzureRmAccount
Login-AzureRmStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzureRmStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

### <a name="unregister-the-server"></a>Hef de registratie van de server
Nu dat alle gegevens is ingetrokken en de server is verwijderd uit alle synchronisatiegroepen, is de server kan worden verwijderd. 

1. Navigeer in de Azure portal, de sectie 'Servers geregistreerd' van de Storage-Sync-Service.
2. Klik met de rechtermuisknop op de server die u wilt registratie en klik op 'Registratie van de Server'.

    ![Hef de registratie](media/storage-sync-files-server-registration/unregister-server-1.png)