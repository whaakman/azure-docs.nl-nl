---
title: Geregistreerde servers met Azure File Sync beheren | Microsoft Docs
description: Informatie over het registreren en de registratie van een Windows-Server met een Azure File Sync-Opslagsynchronisatieservice.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: cec6da78ae47b509e2bb5f8ba0007208545062e7
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478063"
---
# <a name="manage-registered-servers-with-azure-file-sync"></a>Geregistreerde servers met Azure File Sync beheren
Met Azure File Sync kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door het transformeren van uw Windows-Servers in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Het volgende artikel ziet u hoe u te registreren en beheren van een server met een Opslagsynchronisatieservice. Zie [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md) voor informatie over het implementeren van Azure File Sync end-to-end.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="registerunregister-a-server-with-storage-sync-service"></a>Een server met de Opslagsynchronisatieservice registreren/registratie
Een server registreren met Azure File Sync brengt een vertrouwensrelatie tussen Windows Server en Azure. Deze relatie kan vervolgens worden gebruikt om te maken *servereindpunten* op de server, die staan voor specifieke mappen die moeten worden gesynchroniseerd met een Azure-bestandsshare (ook wel bekend als een *cloud-eindpunt*). 

### <a name="prerequisites"></a>Vereisten
Voor het registreren van een server met een Opslagsynchronisatieservice, moet u eerst de server met de vereiste items voor voorbereiden:

* De server moet een ondersteunde versie van Windows Server worden uitgevoerd. Zie voor meer informatie, [Azure File Sync-systeemvereisten en interoperabiliteit](storage-sync-files-planning.md#azure-file-sync-system-requirements-and-interoperability).
* Zorg ervoor dat een Opslagsynchronisatieservice is geïmplementeerd. Zie voor meer informatie over het implementeren van een Opslagsynchronisatieservice [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).
* Zorg ervoor dat de server is verbonden met internet en dat Azure toegankelijk is.
* De Internet Explorer Verbeterde beveiliging uitschakelt voor beheerders met de Server Manager-UI.
    
    ![Serverbeheer-UI met de Internet Explorer Verbeterde beveiliging gemarkeerd](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Zorg ervoor dat de Azure PowerShell-module is geïnstalleerd op uw server. Als uw server lid van een failovercluster is, moet elk knooppunt in het cluster de Az-module. Meer informatie over het installeren van de Az-module kunnen worden gevonden op de [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-Az-ps).

    > [!Note]  
    > Het is raadzaam om een server registreren/registratie ongedaan te maken met behulp van de nieuwste versie van de Az-PowerShell-module. Als het pakket Az eerder is geïnstalleerd op deze server (en de PowerShell-versie op deze server is 5.* of hoger), kunt u de `Update-Module` cmdlet dit pakket wilt bijwerken. 
* Als u een proxy-server in uw omgeving gebruiken, kunt u de proxy-instellingen configureren op de server voor de sync-agent gebruiken.
    1. Proxy IP-adres en poort nummer van uw bepalen
    2. Bewerk deze twee bestanden:
        * C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config
        * C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config
    3. De regels in afbeelding 1 (onder in deze sectie) toevoegen onder /System.ServiceModel in de bovenstaande twee bestanden wijzigen 127.0.0.1:8888 naar het juiste IP-adres (127.0.0.1 vervangen) en het juiste poortnummer (vervangen 8888):
    4. Stel de WinHTTP-proxy-instellingen via de opdrachtregel:
        * De proxy weergeven: netsh winhttp proxy weergeven
        * Stel de proxy: netsh winhttp proxy 127.0.0.1:8888 instellen
        * Opnieuw instellen van de proxy: netsh winhttp proxy opnieuw instellen
        * Als dit setup nadat de agent is geïnstalleerd, start u onze sync-agent: net stop filesyncsvc
    
```XML
    Figure 1:
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
    </system.net>
```    

### <a name="register-a-server-with-storage-sync-service"></a>Een server registreren bij de Opslagsynchronisatieservice
Voordat u een server kan worden gebruikt als een *servereindpunt* in een Azure File Sync *synchronisatiegroep*, moet deze worden geregistreerd met een *Opslagsynchronisatieservice*. Een server kan alleen worden geregistreerd met een Opslagsynchronisatieservice tegelijk.

#### <a name="install-the-azure-file-sync-agent"></a>Azure File Sync-agent installeren
1. [Download de Azure File Sync-agent](https://go.microsoft.com/fwlink/?linkid=858257).
2. Start het installatieprogramma van de Azure File Sync-agent.
    
    ![Het eerste gedeelte van het installatieprogramma van de Azure File Sync-agent](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Zorg ervoor dat updates op de Azure File Sync-agent met behulp van Microsoft Update inschakelen. Het is belangrijk omdat kritieke beveiligingsproblemen en functieverbeteringen aan het serverpakket worden verzonden via Microsoft Update.

    ![Controleer of dat Microsoft Update is ingeschakeld in het deelvenster Microsoft Update van het installatieprogramma van de Azure File Sync-agent](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Als de server is niet eerder hebt geregistreerd, wordt de gebruikersinterface van de serverregistratie onmiddellijk na voltooiing van de installatie weergegeven.

> [!Important]  
> Als de server lid van een Failover-Cluster is, moet de Azure File Sync-agent worden geïnstalleerd op elk knooppunt in het cluster.

#### <a name="register-the-server-using-the-server-registration-ui"></a>De server met behulp van de serverregistratie gebruikersinterface registreren
> [!Important]  
> Cloud Solution Provider (CSP)-abonnementen kunnen de gebruikersinterface van de serverregistratie niet gebruiken. In plaats daarvan PowerShell gebruiken (onder in deze sectie).

1. Als de gebruikersinterface van de serverregistratie niet onmiddellijk na voltooiing van de installatie van de Azure File Sync-agent is gestart, het kan handmatig worden gestart door het uitvoeren van `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`.
2. Klik op *aanmelden* voor toegang tot uw Azure-abonnement. 

    ![Dialoogvenster van de serverregistratie UI te openen](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Kies het juiste abonnement, resourcegroep en Opslagsynchronisatieservice in het dialoogvenster.

    ![Opslaggegevens Sync-Service](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. Preview-versie, is een meer aanmelding vereist om het proces te voltooien. 

    ![Meld u aan in het dialoogvenster](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Als de server lid van een failovercluster is, moet elke server om uit te voeren van de registratie van de Server. Wanneer u de geregistreerde servers in de Azure Portal weergeven, Azure File Sync wordt automatisch elk knooppunt wordt herkend als een lid van hetzelfde failovercluster en op de juiste wijze samen gegroepeerd.

#### <a name="register-the-server-with-powershell"></a>De server registreren met PowerShell
U kunt ook serverregistratie via PowerShell uitvoeren. Dit is de enige ondersteunde manier van serverregistratie voor abonnementen van de Cloud Solution Provider (CSP):

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzureRmStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

### <a name="unregister-the-server-with-storage-sync-service"></a>Registratie van de server met de Opslagsynchronisatieservice
Er zijn verschillende stappen die nodig zijn voor de registratie van een server met een Opslagsynchronisatieservice. We gaan kijken hoe goed registratie van een server.

> [!Warning]  
> Probeer niet aan het oplossen van problemen met synchronisatie, cloud-opslaglagen of een andere aspect van Azure File Sync door registratie en een server registreren of te verwijderen en opnieuw maken van de servereindpunten, tenzij expliciet naar een Microsoft-technicus. Registratie van een server- en servereindpunten verwijderen is een destructieve bewerking en gelaagde bestanden op de volumes met servereindpunten wordt niet opnieuw worden 'verbonden' op de locaties van de Azure-bestandsshare nadat de geregistreerde server en de servereindpunten zijn opnieuw worden gemaakt, wat ervoor zal synchroon fouten. Let ook op, gelaagde bestanden die buiten een naamruimte van de eindpunt bestaat mogelijk permanent verloren. Gelaagde bestanden kunnen bestaan in server-eindpunten zelfs niet als cloud-opslaglagen nooit is ingeschakeld.

#### <a name="optional-recall-all-tiered-data"></a>(Optioneel) Alle gelaagde gegevens intrekken
Als u bestanden die momenteel zijn beschikbaar na het verwijderen van Azure File Sync (dat wil zeggen dit is een productie, niet een test, omgeving) worden geschakeld, intrekken dat alle bestanden op elk volume met servereindpunten. Uitschakelen van cloud-opslaglagen voor alle servereindpunten en voer vervolgens de volgende PowerShell-cmdlet:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <a-volume-with-server-endpoints-on-it>
```

> [!Warning]  
> Als het lokale volume die als host fungeert voor het servereindpunt niet voldoende vrije ruimte voor het intrekken van alle gelaagde gegevens, de `Invoke-StorageSyncFileRecall` -cmdlet mislukt.  

#### <a name="remove-the-server-from-all-sync-groups"></a>De server uit alle synchronisatiegroepen verwijderen
Voordat u de server op de Opslagsynchronisatieservice verwijderen van de registratie moeten alle servereindpunten op die server worden verwijderd. Dit kan worden gedaan via de Azure-portal:

1. Navigeer naar de Opslagsynchronisatieservice waar de server is geregistreerd.
2. Verwijder alle servereindpunten voor deze server in elk synchronisatiegroep in de Opslagsynchronisatieservice. Dit kan worden bereikt met de rechtermuisknop op het eindpunt van de relevante server in het deelvenster van de groep synchroniseren.

    ![Een servereindpunt verwijderen uit een groep voor synchronisatie](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Dit kan ook worden bereikt met een eenvoudige PowerShell-script:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Connect-AzAccount
Login-AzStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzureRmStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

#### <a name="unregister-the-server"></a>Registratie van de server
Nu dat alle gegevens is ingetrokken en de server is verwijderd uit alle synchronisatiegroepen, is de server kan worden verwijderd. 

1. In de Azure-portal, gaat u naar de *servers geregistreerd* sectie van de Opslagsynchronisatieservice.
2. Met de rechtermuisknop op de server die u wilt de registratie ongedaan maken en klikt u op 'Registratie van Server'.

    ![De registratie van de server opheffen](media/storage-sync-files-server-registration/unregister-server-1.png)

## <a name="ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter"></a>Ervoor zorgen dat Azure File Sync is een goede buur in uw datacenter 
Omdat Azure File Sync zelden de enige service die wordt uitgevoerd in uw datacenter worden, kunt u de netwerk- en -gebruik van Azure File Sync beperken.

> [!Important]  
> Limieten te laag instelt is van invloed op de prestaties van Azure File Sync-synchronisatie en intrekken.

### <a name="set-azure-file-sync-network-limits"></a>Limieten voor Azure File Sync-netwerk instellen
U kunt het netwerkgebruik van Azure File Sync beperken met behulp van de `StorageSyncNetworkLimit` cmdlets. 

U kunt bijvoorbeeld een nieuwe vertragingslimiet om ervoor te zorgen dat Azure File Sync niet meer dan 10 Mbps tussen 9:00 tot 17: 00 uur (17:00 uur) tijdens de werkweek gebruikt maken: 

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
New-StorageSyncNetworkLimit -Day Monday, Tuesday, Wednesday, Thursday, Friday -StartHour 9 -EndHour 17 -LimitKbps 10000
```

U kunt uw limiet zien met behulp van de volgende cmdlet:

```PowerShell
Get-StorageSyncNetworkLimit # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

Als u wilt verwijderen van netwerk-limieten, gebruikt u `Remove-StorageSyncNetworkLimit`. De volgende opdracht verwijdert bijvoorbeeld alle limieten van netwerk:

```PowerShell
Get-StorageSyncNetworkLimit | ForEach-Object { Remove-StorageSyncNetworkLimit -Id $_.Id } # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

### <a name="use-windows-server-storage-qos"></a>QoS voor opslag in Windows Server gebruiken 
Wanneer Azure File Sync wordt gehost in een virtuele machine wordt uitgevoerd op een Windows Server virtualization host, kunt u QoS voor opslag (quality of service voor opslag) om te regelen opslag i/o-gebruik. Het beleid voor QoS voor opslag kan worden ingesteld als een maximum (of een limiet, zoals hoe StorageSyncNetwork limiet hierboven wordt afgedwongen) of als een minimum (of een reservering). Minimaal in plaats van een maximum in te stellen, kunt Azure File Sync om uit te breiden naar beschikbare opslag bandbreedte gebruiken als andere werkbelastingen worden niet gebruikt. Zie voor meer informatie, [opslag Quality of Service](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview).

## <a name="see-also"></a>Zie ook
- [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
- [Azure Files Sync implementeren](storage-sync-files-deployment-guide.md)
- [Monitor voor Azure File Sync](storage-sync-files-monitoring.md)
- [Azure Files Sync oplossen](storage-sync-files-troubleshoot.md)
