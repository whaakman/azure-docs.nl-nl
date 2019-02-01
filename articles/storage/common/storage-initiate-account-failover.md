---
title: Start een storage-account failover (preview) - Azure Storage
description: Informatie over het starten van de failover van een account in het geval dat het primaire eindpunt voor uw opslagaccount niet meer beschikbaar is. De failover werkt de secundaire regio om te worden van de primaire regio voor uw opslagaccount.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 01/30/2019
ms.author: tamram
ms.component: common
ms.openlocfilehash: 1290a990515cd68f46d137c276e8bb36834ff482
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513489"
---
# <a name="initiate-a-storage-account-failover-preview"></a>Initieer een failover van storage-account (preview)

Als het primaire eindpunt voor uw geografisch redundante opslagaccount beschikbaar is voor een bepaalde reden, kunt u de failover van een account (preview) kunt starten. De failover van een account werkt het secundaire eindpunt als u wilt worden van het primaire eindpunt voor uw opslagaccount. Nadat de failover voltooid is, kunnen clients begint te schrijven naar de nieuwe primaire regio. Geforceerde failover kunt u hoge beschikbaarheid voor uw toepassingen te onderhouden.

In dit artikel laat zien hoe de failover van een account voor uw opslagaccount met behulp van de Azure portal, PowerShell of Azure CLI worden gestart. Zie voor meer informatie over het account failover, [Disaster recovery en account-failover (preview) in Azure Storage](storage-disaster-recovery-guidance.md).

> [!WARNING]
> De failover van een account leidt doorgaans er gegevens verloren gaan. Om de gevolgen van de failover van een account te begrijpen en om voor te bereiden voor verlies van gegevens, Bekijk [informatie over het failoverproces account](storage-disaster-recovery-guidance.md#understand-the-account-failover-process).

## <a name="prerequisites"></a>Vereisten

Controleer voordat u de failover van een account op uw storage-account uitvoeren kunt, dat u de volgende stappen hebt uitgevoerd:

- Registreren voor de evaluatieversie van de failover-account. Zie voor meer informatie over het registreren, [over de Preview-versie](storage-disaster-recovery-guidance.md#about-the-preview).
- Zorg ervoor dat uw storage-account is geconfigureerd voor het gebruik van geografisch redundante opslag (GRS) of geo-redundante opslag met leestoegang (RA-GRS). Zie voor meer informatie over de geografisch redundante opslag, [geografisch redundante opslag (GRS): Regio-overschrijdend-replicatie voor Azure Storage](storage-redundancy-grs.md). 

## <a name="important-implications-of-account-failover"></a>Belangrijke gevolgen van het account failover

Wanneer u de failover van een account voor uw opslagaccount hebt gestart, wordt de DNS-records voor het secundaire eindpunt worden bijgewerkt zodat het secundaire eindpunt het primaire eindpunt wordt. Zorg ervoor dat u de potentiële impact op uw storage-account begrijpt voordat u een failover starten.

Voor een schatting van de omvang van gegevensverlies waarschijnlijk voordat u een failover starten, Controleer de **tijd van laatste synchronisatie** met behulp van de eigenschap de `Get-AzureRmStorageAccount` PowerShell-cmdlet en bevatten de `-IncludeGeoReplicationStats` parameter. Controleer vervolgens de `GeoReplicationStats` eigenschap voor uw account. 

Het opslagaccounttype wordt na de failover automatisch geconverteerd naar lokaal redundante opslag (LRS) in de nieuwe primaire regio. U kunt de geografisch redundante opslag (GRS) of geografisch redundante opslag met leestoegang (RA-GRS) voor het account opnieuw inschakelen. Houd er rekening mee dat converteren van LRS naar GRS of RA-GRS worden er gelden aanvullende kosten in rekening gebracht. Zie voor meer informatie, [Bandwidth Pricing Details](https://azure.microsoft.com/pricing/details/bandwidth/). 

Nadat u opnieuw GRS voor uw opslagaccount hebt ingeschakeld, begint Microsoft de gegevens in uw account te repliceren naar de nieuwe secundaire regio. Tijd van de replicatie is afhankelijk van de hoeveelheid gegevens die worden gerepliceerd.  

## <a name="azure-portal"></a>Azure Portal

Volg deze stappen voor het starten van een failover van de account van de Azure-portal:

1. Ga naar uw opslagaccount.
2. Onder **instellingen**, selecteer **Geo-replicatie**. De volgende afbeelding toont de geo-replicatie en failover-status van een storage-account.

    ![Schermopname van geo-replicatie en failover-status](media/storage-initiate-account-failover/portal-failover-prepare.png)

3. Controleren of uw storage-account is geconfigureerd voor geografisch redundante opslag (GRS) of geo-redundante opslag met leestoegang (RA-GRS). Als dit niet het geval is, selecteert u **configuratie** onder **instellingen** om bij te werken van uw account kan geografisch redundant. 
4. De **tijd van laatste synchronisatie** eigenschap geeft aan hoe ver de secundaire achter van de primaire. **Laatste synchronisatietijd** biedt een schatting van de omvang van gegevensverlies die u ondervindt nadat de failover is voltooid.
5. Selecteer **voorbereiden voor failover (preview)**. 
6. Bekijk het bevestigingsvenster. Wanneer u klaar bent, voert u **Ja** om te bevestigen en start de failover.

    ![Schermopname die laat zien bevestigingsvenster voor een account-failover](media/storage-initiate-account-failover/portal-failover-confirm.png)

## <a name="powershell"></a>PowerShell

Als u PowerShell wilt Start de failover van een account, moet u eerst de 6.0.1 installeren preview-module. Volg deze stappen voor het installeren van de module:

1. Verwijder eerdere installaties van Azure PowerShell:

    - Verwijderen van eerdere installaties van Azure PowerShell uit met behulp van Windows de **Apps en functies** bij **instellingen**.
    - Verwijder alle **Azure*** modules van `%Program Files%\WindowsPowerShell\Modules`.
    
1. Zorg ervoor dat u de nieuwste versie van PowerShellGet geïnstalleerd hebt. Open een Windows PowerShell-venster en voer de volgende opdracht voor het installeren van de meest recente versie:
 
    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```
1. Sluit en Open de PowerShell-venster na de installatie van PowerShellGet. 

1. Installeer de nieuwste versie van Azure PowerShell:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Een module van Azure Storage Preview-versie die ondersteuning biedt voor Azure AD installeren:
   
    ```powershell
    Install-Module -Name AzureRM.Storage -AllowPrerelease
    ```
1. Sluit en Open de PowerShell-venster.
 

Voor het starten van een failover van de account van PowerShell, voert u de volgende opdracht uit:

```powershell
Invoke-AzureRmStorageAccountFailover -ResourceGroupName <resource-group-name> -Name <account-name> 
```

## <a name="azure-cli"></a>Azure-CLI

Als u Azure CLI wilt Start de failover van een account, voert u de volgende opdrachten uit:

```cli
az storage account show \ --name accountName \ --expand geoReplicationStats
az storage account failover \ --name accountName
```

## <a name="next-steps"></a>Volgende stappen

- [Disaster recovery en account-failover (preview) in Azure Storage](storage-disaster-recovery-guidance.md)
- [Maximaal beschikbare toepassingen met RA-GRS ontwerpen](storage-designing-ha-apps-with-ragrs.md)
- [Zelfstudie: Een maximaal beschikbare toepassing met Blob-opslag maken](../blobs/storage-create-geo-redundant-storage.md) 
