---
title: Een upgrade uitvoeren naar een opslagaccount voor algemeen gebruik v2 - Azure Storage | Microsoft Docs
description: Upgrade uitvoeren naar storage-accounts voor algemeen gebruik v2.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/26/2019
ms.author: tamram
ms.openlocfilehash: a1ee95ad847d20159c79af8f080cc7878e114759
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58444530"
---
# <a name="upgrade-to-a-general-purpose-v2-storage-account"></a>Een upgrade uitvoert naar een opslagaccount voor algemeen gebruik v2

Opslagaccounts voor algemeen gebruik v2 ondersteuning voor de nieuwste opslagfuncties van Azure en alle functionaliteit van voor algemeen gebruik v1 en Blob storage-accounts. Voor algemeen gebruik v2-accounts worden aanbevolen voor de meeste scenario's voor opslag. Voor algemeen gebruik v2-accounts bieden de laagste per GB capaciteit prijzen voor Azure Storage, evenals de bedrijfstak van concurrerende transactieprijzen in.

Een upgrade naar een opslagaccount voor algemeen gebruik v2 in uw voor algemeen gebruik v1- of Blob storage-accounts is eenvoudig. U kunt upgraden met behulp van de Azure portal, PowerShell of Azure CLI.

> [!IMPORTANT]
> Upgraden van een voor algemeen gebruik v1- of Blob storage-opslagaccount voor algemeen gebruik v2 is permanent en kan niet ongedaan worden gemaakt.

## <a name="upgrade-using-the-azure-portal"></a>Upgraden met Azure portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar uw opslagaccount.
3. In de **instellingen** sectie, klikt u op **configuratie**.
4. Klik onder **Soort account** op **Upgrade**.
5. Typ bij **Upgrade bevestigen** de naam van uw account.
6. Klik op **Upgrade** aan de onderkant van de blade.

    ![Soort Account upgraden](../blobs/media/storage-blob-account-upgrade/upgrade-to-gpv2-account.png)

## <a name="upgrade-with-powershell"></a>Een upgrade uitvoeren met PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Als u wilt een account voor algemeen gebruik v1 upgraden naar een algemeen gebruik v2-account met behulp van PowerShell, moet u eerst PowerShell voor het gebruik van de meest recente versie van bijwerken de **Az.Storage** module. Zie [Install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van PowerShell.

Daarna roept u de volgende opdracht uit om het account, vervangen door de naam van de resourcegroep en storage-account te werken:

```powershell
Set-AzStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

## <a name="upgrade-with-azure-cli"></a>Een upgrade uitvoeren met Azure CLI

Als u wilt een account voor algemeen gebruik v1 upgraden naar een algemeen gebruik v2-account met behulp van Azure CLI, moet u eerst de meest recente versie van Azure CLI installeren. Zie [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) voor meer informatie over het installeren van de CLI.

Daarna roept u de volgende opdracht uit om het account, vervangen door de naam van de resourcegroep en storage-account te werken:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
```

## <a name="specify-an-access-tier-for-blob-data"></a>Geef een toegangslaag voor blob-gegevens

Voor algemeen gebruik v2-accounts ondersteunen alle Azure-opslagservices en gegevensobjecten, maar toegangslagen zijn alleen beschikbaar voor blok-blobs in Blob-opslag. Wanneer u een naar een opslagaccount voor algemeen gebruik v2 upgrade, kunt u een toegangslaag opgeven voor uw blob-gegevens.

Laag kunnen u de meest rendabele opslag op basis van uw Verwachte gebruikspatronen kiezen. Blok-blobs kunnen worden opgeslagen in een laag warm, koud of archief. Zie voor meer informatie over toegangslagen [Azure Blob-opslag: Hot, Cool, en Archiefopslaglaag](../blobs/storage-blob-storage-tiers.md).

Standaard een nieuw opslagaccount wordt gemaakt in de hot Storage-toegangslaag en een opslagaccount voor algemeen gebruik v1 is bijgewerkt naar de hot Storage-toegangslaag. Als u welke toegangslaag moet worden gebruikt voor uw na de gegevensupgrade verkennen weet, kunt u uw scenario. Er zijn twee scenario's voor normale gebruiker voor het migreren naar een algemeen gebruik v2-account:

* U beschikt over een bestaand opslagaccount voor algemeen gebruik v1 en wilt een upgrade naar een opslagaccount voor algemeen gebruik v2, met de juiste storage-toegangslaag voor blob-gegevens te evalueren.
* U hebt besloten om een algemeen gebruik v2-opslagaccount gebruiken of al hebt en wilt evaluearen of u de Hot of Cool storage-toegangslaag voor blob-gegevens gebruiken moet.

In beide gevallen is de eerste prioriteit schat de kosten voor opslag, toegang tot en werken met uw gegevens die zijn opgeslagen in een opslagaccount voor algemeen gebruik v2 en vergelijkt die met uw huidige kosten.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Een v1-storage-account upgraden naar een algemeen gebruik v2-account is gratis. Wijzigen van de storage-toegangslaag kan echter resulteren in wijzigingen op uw factuur. 

Alle opslagaccounts maken gebruik van een prijsmodel voor het opslaan van blobs op basis van laag van elke blob. Als u een opslagaccount gebruikt, zijn de volgende factureringsvoorwaarden van toepassing:

* **Kosten voor opslag**: Naast de hoeveelheid gegevens die zijn opgeslagen, wordt de kosten voor opslag van gegevens is afhankelijk van de storage-toegangslaag. De kosten per GB nemen af als de laag minder dynamisch ('cooler') wordt.

* **Kosten van gegevenstoegang**: Kosten voor gegevenstoegang vergroten als de laag cooler. U betaalt voor de gegevens in de cool en archive storage-toegangslaag, een post voor toegang tot gegevens per GB voor leesbewerkingen.

* **Transactiekosten**: Er is een kosten per transactie voor alle lagen die wordt verhoogd als de laag cooler.

* **Kosten voor gegevensoverdracht geo-replicatie**: Deze kosten is alleen van toepassing op accounts waarvoor geo-replicatie is geconfigureerd, inclusief GRS en RA-GRS. Kosten voor gegevensoverdracht met geo-replicatie worden in rekening gebracht per GB.

* **Kosten voor uitgaande gegevensoverdracht**: Uitgaande gegevensoverdracht (gegevens die buiten een Azure-regio worden overgedragen) worden gefactureerd voor bandbreedtegebruik per GB per, consistent met opslagaccounts voor algemeen gebruik.

* **Wijzigen van de storage-toegangslaag**: De account storage-toegangslaag wijzigt van cool naar ' hot ' leidt tot kosten voor het lezen van alle bestaande gegevens in de storage-account. Echter de toegangslaag van hot wijzigen naar ' Cool ' leidt tot kosten voor het schrijven van alle gegevens in de koude laag (alleen GPv2-accounts).

> [!NOTE]
> Zie de pagina [Prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor meer informatie over het prijsmodel voor opslagaccounts. Zie de pagina [Prijsinformatie voor bandbreedte](https://azure.microsoft.com/pricing/details/data-transfers/) voor meer informatie over de kosten voor uitgaande gegevensoverdracht.

### <a name="estimate-costs-for-your-current-usage-patterns"></a>Schat de kosten voor uw huidige verbruikspatroon

Voor een schatting van de kosten van het opslaan en toegang tot blobgegevens in een opslagaccount voor algemeen gebruik v2 in een bepaalde laag, uw bestaande gebruikspatroon evalueren of een schatting maken van het verwachte gebruikspatroon. Doorgaans zijn de volgende gegevens hiervoor van belang:

* Uw Blob storage verbruik, in gigabytes, met inbegrip van:
    - Hoeveel gegevens worden opgeslagen in het opslagaccount?
    - Hoe verandert het gegevensvolume op maandbasis; worden oude gegevens voortdurend vervangen door nieuwe gegevens?
* Het primaire toegangspatroon voor uw Blob storage-gegevens, met inbegrip van:
    - Hoeveel gegevens worden gelezen en geschreven naar het storage-account?
    - Het aantal leesbewerkingen ten opzichte van schrijf-bewerkingen plaatsvinden op de gegevens in de storage-account?

Om te beslissen over de beste toegangslaag voor uw behoeften, kan het handig zijn om te bepalen de capaciteit van de blob-gegevens en hoe die gegevens wordt gebruikt. Dit kan beste worden gedaan door te kijken de bewaking metrische gegevens voor uw account.

### <a name="monitoring-existing-storage-accounts"></a>Bewaking van bestaande opslagaccounts

Voor het bewaken van uw bestaande opslagaccounts en het verzamelen van deze gegevens, kunt u gebruikmaken van Azure Opslaganalyse, dat logboekregistratie uitvoert en metrische gegevens biedt voor een opslagaccount. Opslaganalyse kan metrische gegevens opslaan die samengevoegde transactiestatistieken en capaciteitsgegevens bevat over serviceaanvragen voor GPv1- en GPv2-opslagaccounts en Blob Storage-accounts. Deze gegevens worden opgeslagen in bekende tabellen in hetzelfde opslagaccount.

Zie [About Storage Analytics Metrics](https://msdn.microsoft.com/library/azure/hh343258.aspx) (Metrische gegevens in opslaganalyse) en [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx) (Tabelschema van metrische gegevens in opslaganalyse) voor meer informatie

> [!NOTE]
> Blob Storage-accounts geven het service-eindpunt van de tabel alleen weer voor het opslaan en openen van de metrische gegevens voor dat account.

Voor het controleren van het opslagverbruik voor Blob Storage moet u de metrische gegevens over capaciteit inschakelen.
Als dit is ingeschakeld, worden de capaciteitsgegevens van een Blob Storage-serviceaccount dagelijks geregistreerd en vastgelegd als een tabelvermelding die naar de *$MetricsCapacityBlob*-tabel binnen hetzelfde opslagaccount wordt geschreven.

Voor het controleren van gegevenstoegangspatronen voor Blob Storage, moet u de metrische gegevens die per uur worden verzameld voor de transactie inschakelen vanaf de API. Als deze methode is ingeschakeld, worden er elk uur per-API-transacties verzameld en geregistreerd als een tabelvermelding die is naar de *$MetricsHourPrimaryTransactionsBlob*-tabel binnen hetzelfde opslagaccount wordt geschreven. De *$MetricsHourSecondaryTransactionsBlob*-tabel registreert de transacties naar het secundaire eindpunt bij gebruik van RA-GRS-opslagaccounts.

> [!NOTE]
> Hebt u een algemeen opslagaccount waarin u pagina-blobs en virtuele-machineschijven of wachtrijen, bestanden of tabellen, naast blok hebt opgeslagen en toevoegblobgegevens, is dit schattingsproces niet van toepassing. De capaciteitsgegevens maken geen onderscheid tussen blok-blobs en andere typen en geven geen capaciteitsgegevens voor andere gegevenstypen. Als u deze typen gebruikt, is een alternatieve methode om te kijken naar de hoeveelheden op uw meest recente factuur.

Als u een goede schatting wilt maken van uw gegevensverbruik en toegangspatroon, raden we u aan voor de metrische gegevens een retentieperiode te kiezen die een goede afspiegeling is van uw normale gebruik en dat als uitgangspunt te nemen. Een optie is de metrische gegevens zeven dagen te bewaren en de gegevens elke week te verzamelen en aan het einde van de maand te analyseren. Een andere optie is de metrische gegevens van de afgelopen 30 dagen te bewaren en deze gegevens aan het einde van deze periode van 30 dagen te verzamelen en te analyseren.

Zie voor meer informatie over het inschakelen, verzamelen en weergeven van metrische gegevens, [Storage analytics metrics](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Het opslaan, openen en downloaden van analytische gegevens wordt op dezelfde manier in rekening gebracht als normale gebruikersgegevens.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Metrische gegevens over het gebruik inzetten voor het schatten van de kosten

#### <a name="capacity-costs"></a>Kosten voor capaciteit

De meest recente vermelding in de metrische gegevenstabel voor capaciteit *$MetricsCapacityBlob* met de rijsleutel *'data'* toont de opslagcapaciteit die wordt gebruikt door gebruikersgegevens. De meest recente vermelding in de metrische gegevenstabel voor capaciteit *$MetricsCapacityBlob* met de rijsleutel *'analytics'* toont de opslagcapaciteit die wordt gebruikt door de analyselogboeken.

Deze totale capaciteit die wordt verbruikt door zowel gebruikersgegevens en analyselogboeken (indien ingeschakeld) kunnen vervolgens worden gebruikt om de opslagkosten te schatten voor gegevens in het opslagaccount. Deze manier kan ook worden gebruikt voor het schatten van de opslagkosten in GPv1-opslagaccounts.

#### <a name="transaction-costs"></a>Transactiekosten

Het totaal van *'TotalBillableRequests'*, in alle items voor een API in de metrische gegevenstabel voor transactie, geeft het totale aantal transacties voor die bepaalde API weer. *Bijvoorbeeld*: het totale aantal *GetBlob*-transacties in een bepaalde periode kan worden berekend door het totaal te nemen van het totale aantal factureerbare aanvragen voor alle items met de rijsleutel *user;GetBlob*.

Voor het schatten van de transactiekosten van Blob Storage-accounts moet u de transacties in drie groepen opdelen omdat ze verschillend zijn geprijsd.

* Schrijftransacties zoals *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* en *'CopyBlob'*.
* Verwijdertransacties zoals *'DeleteBlob'* en *'DeleteContainer'*.
* Alle andere transacties.

Voor het schatten van de transactiekosten voor GPv1-opslagaccounts moet u alle transacties verzamelen ongeacht de bewerking/API.

#### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Gegevenstoegang en overdrachtskosten voor geo-replicatiegegevens

Opslaganalyse biedt geen informatie over de hoeveelheid gegevens die zijn gelezen en geschreven van en naar een opslagaccount, maar deze hoeveelheid kan min of meer worden geschat door te kijken naar de metrische gegevenstabel voor transacties. Het totaal van *'TotalIngress'* in alle items voor een API in de metrische gegevenstabel voor transacties, geeft de totale hoeveelheid inkomende gegevens in bytes voor die bepaalde API weer. Op dezelfde manier geeft het totaal van *'TotalEgress'* de totale hoeveelheid uitgaande gegevens in bytes weer.

Voor het schatten van de kosten voor het openen van gegevens in Blob Storage-accounts moet u de transacties in twee groepen opdelen.

* De hoeveelheid gegevens die is opgehaald van het opslagaccount, kan worden geschat door te kijken naar het totaal van *'TotalEgress'* voor met name de *'GetBlob'*- en *'CopyBlob'*-bewerkingen.

* De hoeveelheid gegevens die wordt geschreven naar het opslagaccount kan worden geschat door te kijken naar het totaal van *'TotalIngress'* voor met name de *'PutBlob'*-, *'PutBlock'*-, *'CopyBlob'*- en *'AppendBlock'*-bewerkingen.

De overdrachtskosten van geo-replicatiegegevens voor Blob Storage-accounts kan ook worden berekend met behulp van de schatting voor de hoeveelheid gegevens die wordt geschreven bij gebruik van een GRS- of RA-GRS-opslagaccount.

> [!NOTE]
> Voor een uitgebreider voorbeeld over het berekenen van de kosten voor het gebruik van de toegangslaag hot of cool storage, bekijk de veelgestelde vraag *'Wat zijn warme en koude toegangslagen en hoe bepaal ik welke computer te gebruiken?'* op de [pagina met prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="next-steps"></a>Volgende stappen

- [Een opslagaccount maken](storage-quickstart-create-account.md)
- [Azure storage-accounts beheren](storage-account-manage.md)
