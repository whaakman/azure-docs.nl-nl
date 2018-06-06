---
title: Beheren van de geregistreerde servers met het synchroniseren van Azure-bestand (preview) | Microsoft Docs
description: Informatie over het registreren en ongedaan maken van een Windows-Server met een Azure-bestand Sync Storage Sync-Service.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: wgries
ms.openlocfilehash: 7385e8b84668facf8bf44f569a611e7dcdba9a1e
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738289"
---
# <a name="manage-registered-servers-with-azure-file-sync-preview"></a>Geregistreerde servers met het synchroniseren van Azure-bestand (preview) beheren
Met Azure File Sync (preview) kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door het omzetten van de Windows-Servers in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Het volgende artikel ziet u het registreren en beheren van een server met een opslag-Sync-Service. Zie [het implementeren van Azure File-synchronisatie (preview)](storage-sync-files-deployment-guide.md) voor informatie over het implementeren van Azure bestand Sync end-to-end.

## <a name="registerunregister-a-server-with-storage-sync-service"></a>Een server met Storage-synchronisatieservice register/unregister
Een server met het synchroniseren van Azure-bestand te registreren, stelt u een vertrouwensrelatie tussen Windows Server en Azure. Deze relatie kan vervolgens worden gebruikt voor het maken *server eindpunten* op de server die staan voor specifieke mappen die moeten worden gesynchroniseerd met een Azure-bestandsshare (ook wel bekend als een *cloud eindpunt*). 

### <a name="prerequisites"></a>Vereisten
Voor het registreren van een server met een opslag-Sync-Service, moet u eerst de server met de benodigde vereisten voorbereiden:

* De server moet een ondersteunde versie van Windows Server worden uitgevoerd. Zie voor meer informatie [ondersteunde versies van Windows Server](storage-sync-files-planning.md#supported-versions-of-windows-server).
* Zorg ervoor dat een opslag-Sync-Service is geïmplementeerd. Zie voor meer informatie over het implementeren van een opslag-synchronisatieservice [het implementeren van Azure File-synchronisatie (preview)](storage-sync-files-deployment-guide.md).
* Zorg ervoor dat de server is verbonden met internet en of Azure toegankelijk is.
* De Internet Explorer Verbeterde beveiliging uitschakelt voor beheerders met de Serverbeheer-UI.
    
    ![Serverbeheer UI met de Internet Explorer Verbeterde beveiliging gemarkeerd](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Zorg ervoor dat de AzureRM PowerShell-module is geïnstalleerd op uw server. Als uw server lid is van een failovercluster, moet elk knooppunt in het cluster de AzureRM-module. Meer informatie over het installeren van de module AzureRM vindt u op de [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

    > [!Note]  
    > U kunt het beste de nieuwste versie van de AzureRM PowerShell-module registreren/verwijderen van een server gebruikt. Als het pakket AzureRM eerder op deze server is geïnstalleerd (en de PowerShell-versie op deze server is 5.* of hoger), kunt u de `Update-Module` cmdlet bij te werken van dit pakket. 
* Als u een proxy-server in uw omgeving gebruiken, kunt u de proxy-instellingen configureren op uw server voor de synchronisatie-agent te kunnen gebruiken.
    1. Bepalen van uw proxy-IP-adres en poort-nummer
    2. Bewerk deze twee bestanden:
        * C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config
        * C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config
    3. De regels in afbeelding 1 (onder deze sectie) toevoegen onder /System.ServiceModel in de bovenstaande twee bestanden wijzigen 127.0.0.1:8888 naar het juiste IP-adres (vervangen 127.0.0.1) en het juiste poortnummer (vervangen 8888):
    4. Stel de WinHTTP-proxy-instellingen via de opdrachtregel:
        * De proxy weergeven: netsh winhttp proxy weergeven
        * De proxy instellen: netsh winhttp proxy 127.0.0.1:8888 instellen
        * Opnieuw instellen van de proxy: netsh winhttp proxy opnieuw instellen
        * Als dit installatieprogramma nadat de agent is geïnstalleerd, start opnieuw op onze sync-agent: net stop filesyncsvc
    
```XML
    Figure 1:
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
    </system.net>
```    

### <a name="register-a-server-with-storage-sync-service"></a>Een server met Storage Sync-Service registreren
Voordat u een server kan worden gebruikt als een *servereindpunt* in een Azure-bestand Sync *groep voor synchronisatie*, moet zijn geregistreerd bij een *Sync opslagservice*. Een server kan slechts met één opslag-Sync-Service worden geregistreerd.

#### <a name="install-the-azure-file-sync-agent"></a>De Azure-bestand Sync-agent installeren
1. [De Azure-bestand Sync-agent downloaden](https://go.microsoft.com/fwlink/?linkid=858257).
2. Start het installatieprogramma van de Azure-bestand Sync-agent.
    
    ![Het eerste gedeelte van het installatieprogramma van de Azure-bestand Sync-agent](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Zorg ervoor dat de Azure-bestand Sync-agent met behulp van Microsoft Update-updates inschakelen. Het is belangrijk omdat kritieke beveiligingsproblemen en functieverbeteringen aan het serverpakket worden verzonden via Microsoft Update.

    ![Zorg ervoor dat Microsoft Update is ingeschakeld in het deelvenster Microsoft Update van het installatieprogramma van de Azure-bestand Sync-agent](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Als de server heeft niet eerder is geregistreerd, wordt de registratie van de server UI onmiddellijk na voltooiing van de installatie weergegeven.

> [!Important]  
> Als de server lid is van een failovercluster, moet de Azure-bestand Sync-agent worden geïnstalleerd op elk knooppunt in het cluster.

#### <a name="register-the-server-using-the-server-registration-ui"></a>De server met behulp van de registratie van UI-server registreren
> [!Important]  
> Abonnementen voor cloud Solution Provider (CSP) kunnen de registratie van UI-server niet gebruiken. Gebruik in plaats daarvan PowerShell (onder deze sectie).

1. Als de registratie van UI-server niet gestart is onmiddellijk na voltooiing van de installatie van de Azure-bestand Sync-agent, het kan handmatig worden gestart door het uitvoeren van `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`.
2. Klik op *aanmelden* voor toegang tot uw Azure-abonnement. 

    ![Dialoogvenster van de registratie van UI-server openen](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Kies de juiste abonnement, resourcegroep en Sync-Service voor opslag in het dialoogvenster.

    ![Storage-Sync-Service gegevens](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. In voorbeeld van is een meer aanmelden vereist om het proces te voltooien. 

    ![Dialoogvenster Aanmelden](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Als de server lid is van een failovercluster, wordt elke server moet worden uitgevoerd van de registratie van de Server. Wanneer u de geregistreerde servers in de Azure Portal weergeeft, Azure File-synchronisatie wordt automatisch elk knooppunt wordt herkend als een lid van hetzelfde failovercluster en op de juiste wijze samen gegroepeerd.

#### <a name="register-the-server-with-powershell"></a>De server geregistreerd met PowerShell
U kunt ook de registratie van de server via PowerShell uitvoeren. Dit is de enige ondersteunde manier van registratie van de server voor abonnementen van de Cloud Solution Provider (CSP):

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzureRmStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

### <a name="unregister-the-server-with-storage-sync-service"></a>Hef de registratie van de server met Storage Sync-Service
Er zijn verschillende stappen die nodig zijn voor de registratie van een server met een opslag-Sync-Service. Eens kijken hoe goed Hef de registratie van een server.

> [!Warning]  
> Probeer niet het oplossen van problemen met synchronisatie, cloud tiering of een andere aspect van Azure bestand Sync door de registratie en een server te registreren of te verwijderen en opnieuw maken van de server-eindpunten tenzij expliciet naar een engineer van Microsoft. De registratie van een server en het verwijderen van server-eindpunten is een destructieve bewerking en gelaagde bestanden op de volumes met de eindpunten van de server wordt niet opnieuw worden 'verbonden' naar de locatie op de Azure-bestandsshare nadat de geregistreerde server- en eindpunten van de server zijn opnieuw gemaakt, wordt die synchroon fouten oplevert. Ook merk gelaagde bestanden die aanwezig zijn buiten de naamruimte van een server eindpunt mogelijk permanent verloren. Gelaagde bestaan mogelijk binnen server eindpunten, zelfs als cloud tiering nooit is ingeschakeld.

#### <a name="optional-recall-all-tiered-data"></a>(Optioneel) Alle gelaagde gegevens intrekken
Als u bestanden die momenteel worden geschakeld dat wilt zijn beschikbaar nadat de synchronisatie van Azure-bestand (dat wil zeggen dit is een productie-, niet een test, omgeving) verwijderd, intrekken u alle bestanden op elke server-eindpunten met volume. Cloud tiering voor alle server-eindpunten uitschakelen en voer vervolgens de volgende PowerShell-cmdlet:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <a-volume-with-server-endpoints-on-it>
```

> [!Warning]  
> Als het lokale volume die als host fungeert voor het servereindpunt niet voldoende vrije ruimte heeft voor het intrekken van de gelaagde gegevens, de `Invoke-StorageSyncFileRecall` -cmdlet mislukt.  

#### <a name="remove-the-server-from-all-sync-groups"></a>De server verwijderen uit alle synchronisatiegroepen
Voordat u de registratie van de server op de Storage-Sync-Service, moeten alle eindpunten van de server op die server worden verwijderd. Dit kan worden gedaan via de Azure-portal:

1. Navigeer naar de opslag-synchronisatieservice waar uw server is geregistreerd.
2. Verwijder alle server-eindpunten voor deze server in elke groep voor synchronisatie in de Storage-Sync-Service. Dit kunt doen met de rechtermuisknop op de relevante server-eindpunt in het deelvenster van de groep synchronisatie.

    ![Een servereindpunt verwijderen uit een groep voor synchronisatie](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Dit kan ook worden bewerkstelligd met een eenvoudig PowerShell-script:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Connect-AzureRmAccount
Login-AzureRmStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzureRmStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

#### <a name="unregister-the-server"></a>Hef de registratie van de server
Nu dat alle gegevens is ingetrokken en de server is verwijderd uit alle synchronisatiegroepen, is de server kan worden verwijderd. 

1. Navigeer in de Azure-portal naar de *servers geregistreerd* sectie van de Storage-Sync-Service.
2. Klik met de rechtermuisknop op de server die u wilt registratie en klik op 'Registratie van de Server'.

    ![De registratie van de server opheffen](media/storage-sync-files-server-registration/unregister-server-1.png)

## <a name="ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter"></a>Synchronisatie van zorgt Azure-bestand is een goede neighbor in uw datacenter 
Omdat Azure bestand Sync zelden de enige service in uw datacenter wordt uitgevoerd, kunt u het gebruik van het netwerk en opslag van Azure bestand Sync beperken.

> [!Important]  
> Als grenzen te laag instelt invloed op de prestaties van Azure bestand Sync synchronisatie en intrekken.

### <a name="set-azure-file-sync-network-limits"></a>Limieten voor het netwerk van Azure File-synchronisatie instellen
Kunt u het netwerkgebruik van Azure bestand Sync beperken met behulp van de `StorageSyncNetworkLimit` cmdlets. 

U kunt bijvoorbeeld een nieuwe limiet voor vertraging om ervoor te zorgen dat Azure bestand synchronisatie maakt geen gebruik van meer dan 10 Mbps tussen 9: 00 uur en 17: 00 uur (17:00 uur) tijdens de werkweek maken: 

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
New-StorageSyncNetworkLimit -Day Monday, Tuesday, Wednesday, Thursday, Friday -StartHour 9 -EndHour 17 -LimitKbps 10000
```

U kunt uw limiet zien met behulp van de volgende cmdlet:

```PowerShell
Get-StorageSyncNetworkLimit # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

U kunt limieten netwerk verwijderen `Remove-StorageSyncNetworkLimit`. De volgende opdracht worden bijvoorbeeld alle netwerk limieten verwijderd:

```PowerShell
Get-StorageSyncNetworkLimit | ForEach-Object { Remove-StorageSyncNetworkLimit -Id $_.Id } # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

### <a name="use-windows-server-storage-qos"></a>Windows Server opslag QoS gebruiken 
Wanneer Azure File-synchronisatie wordt gehost op een virtuele machine uitgevoerd op een Windows Server virtualization host, kunt u QoS-opslag (quality of service-opslag) regelen van opslag-i/o-verbruik. Het opslag-QoS-beleid kan worden ingesteld als een maximum (of de limiet, zoals hoe StorageSyncNetwork limiet wordt afgedwongen hierboven) of als een minimale (of de reservering). Minimaal in plaats van een maximum instellen, kunt Azure File-synchronisatie met burst naar beschikbare opslag bandbreedte gebruiken als andere werkbelastingen worden niet gebruikt. Zie voor meer informatie [Storage Quality of Service](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview).

## <a name="see-also"></a>Zie ook
- [Planning voor de implementatie van een Azure-bestand Sync (preview)](storage-sync-files-planning.md)
- [Azure File-synchronisatie (preview) implementeren](storage-sync-files-deployment-guide.md) 
- [Problemen met Azure File-synchronisatie (preview)](storage-sync-files-troubleshoot.md)
