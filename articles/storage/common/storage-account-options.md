---
title: Opties voor Azure Storage-account | Microsoft Docs
description: Informatie over opties voor het gebruik van Azure Storage.
services: storage
author: hux
manager: jwillis
ms.service: storage
ms.workload: storage
ms.topic: get-started-article
ms.date: 05/02/2018
ms.author: hux
ms.openlocfilehash: 69da15b98e6c519a3a8352cc7ca7212286cb4e52
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="azure-storage-account-options"></a>Opties voor Azure Storage-account

## <a name="overview"></a>Overzicht
Azure Storage biedt drie verschillende accountopties, met verschillende prijzen en functies. Neem deze verschillen in overweging voordat u een opslagaccount maakt om de optie te kiezen die het best bij uw toepassingen past. De drie verschillende opslagaccounts zijn:

* Accounts voor **algemeen gebruik v2 (GPv2)** 
* Accounts voor **algemeen gebruik v1 (GPv1)**
* **Blob Storage**-accounts

Elk accounttype wordt in de volgende sectie in meer detail beschreven:

## <a name="storage-account-options"></a>Opslagaccountopties

### <a name="general-purpose-v2"></a>Algemeen gebruik v2

Accounts voor algemeen gebruik v2 (GPv2) zijn opslagaccounts die de nieuwste functies voor blobs, bestanden, wachtrijen en tabellen ondersteunen. GPv2-accounts ondersteunen alle API’s en functies die in GPv1- en Blob Storage-accounts worden ondersteund. Ze ondersteunen ook dezelfde functies voor duurzaamheid, beschikbaarheid, schaalbaarheid en prestatieverhogende functies in deze accounttypen. Prijsmodellen voor GPv2-accounts bieden de laagste prijzen per gigabyte en concurrerende transactieprijzen in de branche.

U kunt uw GPv1-account upgraden naar een GPv2-account via Azure Portal, PowerShell of Azure CLI. 

Voor blok-blobs in een GPv2-opslagaccount kunt u kiezen tussen dynamische- en statische-opslaglagen op accountniveau, of dynamische-, statische- en archiefopslaglagen op blob-niveau op basis van toegangspatronen. Sla gegevens die vaak, onregelmatig en zelden worden geopend op in respectievelijk de dynamische-, statische- en archiefopslaglaag om de kosten te optimaliseren. 

GPv2-opslag-accounts geven het kenmerk **Toegangslaag** op accountniveau weer. De standaardopslagaccountlaag wordt aangeduid als **Dynamisch** of **Statisch**. De standaardopslagaccountlaag wordt toegepast op blobs waarvoor op blob-niveau geen expliciete laag is ingesteld. Als er een wijziging optreedt in het gebruikspatroon van de gegevens, kunt u op elk gewenst moment schakelen tussen deze opslaglagen. De **archive-laag** kan alleen worden toegepast op blob-niveau.

> [!NOTE]
> Aan het wijzigen van de opslaglaag kunnen extra kosten zijn verbonden. Zie [Prijzen en facturering](#pricing-and-billing) voor meer informatie.
>
> U wordt voor de meeste scenario’s aangeraden gebruik te maken van opslagaccounts voor algemeen gebruik v2 in plaats van Blob Storage-accounts.

### <a name="upgrade-a-storage-account-to-gpv2"></a>Een upgrade van een opslagaccount naar GPv2 uitvoeren

Gebruikers kunnen op elk gewenst moment een GPv1-account upgraden naar een GPv2-account via PowerShell of Azure CLI. Deze wijziging kan niet ongedaan worden gemaakt en er zijn geen andere wijzigingen toegestaan.

#### <a name="upgrade-with-powershell"></a>Een upgrade uitvoeren met PowerShell

Als u een upgrade wilt uitvoeren van een GPv1-account naar een GPv2-account via PowerShell, werkt u PowerShell eerst bij zodat van de meest recente versie van de **AzureRm.Storage**-module gebruik wordt gemaakt. Zie [Install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van PowerShell. Roep vervolgens de volgende opdracht aan om een upgrade van het account uit te voeren, waarmee u de namen van de resourcegroep en het opslagaccount vervangt:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

#### <a name="upgrade-with-azure-cli"></a>Een upgrade uitvoeren met Azure CLI

Als u een upgrade wilt uitvoeren van een GPv1-account naar een GPv2-account via Azure CLI, installeert u eerst de meest recente versie van Azure CLI. Zie [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) voor meer informatie over het installeren van de CLI. Roep vervolgens de volgende opdracht aan om een upgrade van het account uit te voeren, waarmee u de namen van de resourcegroep en het opslagaccount vervangt:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
```` 

### <a name="general-purpose-v1"></a>Algemeen gebruik v1

Accounts voor algemeen gebruik v1 (GPv1) hebben toegang tot alle Azure Storage-services, maar hebben mogelijk niet de nieuwste functies of de laagste prijzen per gigabyte. Statische en archiefopslag worden bijvoorbeeld niet ondersteund in GPv1. Prijzen zijn lager voor GPv1-transacties, zodat workloads met een hoog verloop of hoge leessnelheden van dit accounttype kunnen profiteren.

Opslagaccounts voor algemeen gebruik v1 (GPv1) zijn de oudste opslagaccounttypen en het enige type dat gebruikt kan worden in het klassieke implementatiemodel. 

### <a name="blob-storage-accounts"></a>Blob Storage-accounts

Blob Storage-accounts ondersteunen dezelfde blok-blob-functies als GPv2, maar zijn beperkt tot het alleen ondersteunen van blok-blobs. Prijzen zijn vergelijkbaar met die voor accounts voor algemeen gebruik v2. Klanten moeten de prijsverschillen tussen Blob Storage-accounts en GPv2 goed bekijken en overwegen om te upgraden naar GPv2. Deze upgrade kan niet ongedaan worden gemaakt.

> [!NOTE]
> Blob Storage-accounts ondersteunen alleen blok-blobs en toevoeg-blobs. Pagina-blobs worden niet ondersteund.

## <a name="recommendations"></a>Aanbevelingen

Zie [Over Azure Storage-accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie over opslagaccounts.

Voor toepassingen waarvoor alleen de opslag van blok- of toevoeg-blobs is vereist, wordt aangeraden gebruik te maken van GPv2-opslagaccounts. Zo profiteert u maximaal van het gedifferentieerde prijsmodel voor gelaagde opslag. In bepaalde scenario’s kunt u echter het gebruik van GPv1 overwegen, bijvoorbeeld:

* U moet nog steeds gebruikmaken van het klassieke implementatiemodel. Blob Storage-accounts zijn alleen beschikbaar via het Azure Resource Manager-implementatiemodel.

* U gebruikt omvangrijke transacties of geo-replicatie bandbreedte, die beiden meer kosten in GPv2-opslagaccounts en Blob Storage-accounts dan in GPv1, en u hebt niet genoeg opslagruimte om te kunnen profiteren van de lagere kosten van GB-opslag.

* U gebruikt een versie van de [REST API voor Storage Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) die ouder is dan 2014-02-14 of een clientbibliotheek met een lagere versie dan 4.x en u uw toepassing niet kunt upgraden.

> [!NOTE]
> Blob Storage-accounts worden momenteel ondersteund in alle Azure-regio's.

## <a name="pricing-and-billing"></a>Prijzen en facturering
Alle opslagaccounts maken gebruik van een prijsmodel voor het opslaan van blobs op basis van laag van elke blob. Als u een opslagaccount gebruikt, zijn de volgende factureringsvoorwaarden van toepassing:

* **Opslagkosten**: de kosten voor het opslaan van gegevens hangen niet alleen af van de hoeveelheid opgeslagen gegevens, maar ook van de gebruikte opslaglaag. De kosten per GB nemen af als de laag minder dynamisch ('cooler') wordt.

* **Kosten van gegevenstoegang**: de kosten voor gegevenstoegang nemen toe als de laag minder dynamisch ('cooler') wordt. Voor gegevens in de lagen Cool Storage en Archive Storage worden kosten per GB in rekening gebracht aan gegevenstoegang voor leesbewerkingen.

* **Transactiekosten**: er gelden kosten per transactie voor alle lagen. Deze kosten nemen toe als de laag minder dynamisch wordt.

* **Kosten voor gegevensoverdracht met geo-replicatie**: deze kosten zijn alleen van toepassing op accounts waarvoor geo-replicatie is geconfigureerd, inclusief GRS en RA-GRS. Kosten voor gegevensoverdracht met geo-replicatie worden in rekening gebracht per GB.

* **Kosten voor uitgaande gegevensoverdracht**: uitgaande gegevensoverdracht (gegevens die buiten een Azure-regio worden overgedragen) worden gefactureerd voor bandbreedtegebruik per GB, net zoals bij opslagaccounts voor algemeen gebruik.

* **De opslaglaag wijzigen**: als u de accountopslaglaag wijzigt van 'cool' naar 'hot', worden kosten in rekening gebracht die overeenkomen met de kosten voor het lezen van alle bestaande gegevens in het opslagaccount. Bij een wijziging van de accountopslaglaag van dynamisch naar statisch, worden echter kosten in rekening gebracht die gelijk zijn aan die voor het schrijven van alle gegevens in de statische laag (alleen GPv2-accounts).

> [!NOTE]
> Zie de pagina [Prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor meer informatie over het prijsmodel voor Blob Storage-accounts. Zie de pagina [Prijsinformatie voor bandbreedte](https://azure.microsoft.com/pricing/details/data-transfers/) voor meer informatie over de kosten voor uitgaande gegevensoverdracht.

## <a name="quickstart-scenarios"></a>Snelstartscenario's

In deze sectie worden de volgende scenario‘s toegelicht, waarbij gebruik wordt gemaakt van Azure Portal:

* [Een GPv2-opslagaccount maken.](#create-a-gpv2-storage-account-using-the-azure-portal)
* [Een GPv1-opslagaccount of Blob Storage-account converteren naar een GPv2-opslagaccount.](#convert-a-gpv1-or-blob-storage-account-to-a-gpv2-storage-account-using-the-azure-portal)
* [De opslaglaag van een GPv2-opslagaccount wijzigen.](#change-the-storage-tier-of-a-gpv2-storage-account-using-the-azure-portal)
* [Een bloblaag instellen in een Blob-opslagaccount of GPv2-opslagaccount.](#change-the-storage-tier-of-a-blob-using-the-azure-portal)

U kunt de toegangslaag in de volgende voorbeelden niet instellen op Archive, omdat deze instelling voor het hele opslagaccount geldt. Archive Storage kan alleen worden ingesteld voor een specifieke blob.

### <a name="create-a-gpv2-storage-account-using-the-azure-portal"></a>Een GPv2-opslagaccount maken via Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer in het menu Hub achtereenvolgens **Een resource maken** > **Gegevens en opslag** > **Opslagaccount**.

3. Voer een naam in voor het opslagaccount.

    Deze naam moet uniek zijn. De naam wordt gebruikt als onderdeel van de URL die wordt gebruikt voor toegang tot de objecten in het opslagaccount.  

4. Selecteer **Resource Manager** als het implementatiemodel.

    Gelaagde opslag kan alleen worden gebruikt met Resource Manager-opslagaccounts. Resource Manager is het aanbevolen implementatiemodel voor nieuwe resources. Zie [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor meer informatie.  

5. Selecteer **Algemeen gebruik v2** in de vervolgkeuzelijst **Soort account**.

    Als u GPv2 selecteert, wordt de prestatielaag ingesteld op Standaard. Gelaagde opslag is niet beschikbaar bij de Premium-prestatielaag.

6. Selecteer de replicatieoptie voor het opslagaccount: **LRS**, **ZRS**, **GRS** of **RA-GRS**. **RA-GRS** is de standaardinstelling.

    LRS = locally redundant storage (lokaal redundante opslag), ZRS = zone-redundant storage (zone-redundante opslag), GRS = geo-redundant storage (geografisch redundante opslag) (twee regio's), RA-GRS = read-access geo-redundant storage (geografisch redundante opslag met leestoegang) (twee regio's met leestoegang tot de tweede).

    Zie [Azure Storage-replicatie](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie over Azure Storage-replicatieopties.

7. Selecteer de juiste opslaglaag voor wat u nodig hebt: stel de **Toegangslaag** in op **'Cool'** of **'Hot'**. **Hot** is de standaardinstelling.

8. Selecteer het abonnement waarin u het nieuwe opslagaccount wilt maken.

9. Geef een nieuwe resourcegroep op of selecteer een bestaande resourcegroep. Zie [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor meer informatie over resourcegroepen.

10. Selecteer de regio voor uw opslagaccount.

11. Klik op **Maken** om het opslagaccount te maken.

### <a name="convert-a-gpv1-or-blob-storage-account-to-a-gpv2-storage-account-using-the-azure-portal"></a>Een GPv1- of Blob Storage-account converteren naar een GPv2-opslagaccount met behulp van Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw opslagaccount, selecteer **Alle resources** en selecteer vervolgens uw opslagaccount.

3. Klik in de sectie Instellingen op **Configuratie**.

4. Klik onder **Soort account** op **Upgrade**.

5. Typ bij **Upgrade bevestigen** de naam van uw account. 

5. Klik onder aan de blade op Upgraden.

### <a name="change-the-storage-tier-of-a-gpv2-storage-account-using-the-azure-portal"></a>De opslaglaag voor een GPv2-opslagaccount wijzigen via Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw opslagaccount, selecteer **Alle resources** en selecteer vervolgens uw opslagaccount.

3. Klik in de Instellingen-blade op **Configuratie** om de accountconfiguratie te bekijken en/of te wijzigen.

4. Selecteer de juiste opslaglaag voor wat u nodig hebt: stel de **Toegangslaag** in op **'Cool'** of **'Hot'**.

5. Klik op Opslaan boven aan de blade.

### <a name="change-the-storage-tier-of-a-blob-using-the-azure-portal"></a>De opslaglaag voor een blob wijzigen via Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Als u naar de blob in uw opslagaccount wilt gaan, selecteert u achtereenvolgens **Alle resources**, uw opslagaccount, uw container en uw blob.

3. Klik op de blade met blob-eigenschappen op het vervolgkeuzemenu **Toegangslaag** om de opslaglaag **Hot**, **Cool** of **Archive** te selecteren.

5. Klik op Opslaan boven aan de blade.

> [!NOTE]
> Aan het wijzigen van de opslaglaag kunnen extra kosten zijn verbonden. Zie [Prijzen en facturering](#pricing-and-billing) voor meer informatie.


## <a name="evaluating-and-migrating-to-gpv2-storage-accounts"></a>Evalueren en migreren naar GPv2-opslagaccounts
Het doel van deze sectie is om gebruikers op soepele wijze te helpen migreren naar het gebruik van GPv2-opslagaccounts (in tegenstelling tot GPv1). Er zijn twee scenario's voor gebruikers:

* U beschikt over een bestaand GPv1-opslagaccount en wilt een wijziging evalueren in een GPv2-opslagaccount met de juiste opslaglaag.
* U hebt besloten om een GPv2-opslagaccount te gebruiken of u hebt er al een, en u wilt evaluearen of u een dynamische- of statische-opslaglaag moet gebruiken.

In beide gevallen moeten eerst de kosten worden geschat van het opslaan en openen van uw gegevens die zijn opgeslagen in een GPv2-opslagaccount en dient u deze te vergelijken met uw huidige kosten.

## <a name="evaluating-gpv2-storage-account-tiers"></a>Lagen voor GPv2-opslagaccounts evalueren

Voor het maken van een schatting van de kosten voor het opslaan en openen van gegevens die zijn opgeslagen in een GPv2-opslagaccount, moet u uw bestaande gebruikspatroon evalueren of een schatting maken van het verwachte gebruikspatroon. Doorgaans zijn de volgende gegevens hiervoor van belang:

* Wat is het gebruik van de opslag? Hoeveel gegevens worden er opgeslagen en hoe wijzigt dit maandelijks?

* Wat is het toegangspatroon voor de opslag? Op hoeveel gegevens in het account worden er lees- en/of schrijfbewerkingen uitgevoerd (inclusief nieuwe gegevens)? Hoeveel transacties worden gebruikt voor toegang tot gegevens? En wat voor soort transacties zijn dit?

## <a name="monitoring-existing-storage-accounts"></a>Bewaking van bestaande opslagaccounts

Voor het bewaken van uw bestaande opslagaccounts en het verzamelen van deze gegevens, kunt u gebruikmaken van Azure Opslaganalyse, dat logboekregistratie uitvoert en metrische gegevens biedt voor een opslagaccount. Opslaganalyse kan metrische gegevens opslaan die samengevoegde transactiestatistieken en capaciteitsgegevens bevat over serviceaanvragen voor GPv1- en GPv2-opslagaccounts en Blob Storage-accounts. Deze gegevens worden opgeslagen in bekende tabellen in hetzelfde opslagaccount.

Zie [About Storage Analytics Metrics](https://msdn.microsoft.com/library/azure/hh343258.aspx) (Metrische gegevens in opslaganalyse) en [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx) (Tabelschema van metrische gegevens in opslaganalyse) voor meer informatie

> [!NOTE]
> Blob Storage-accounts geven het service-eindpunt van de tabel alleen weer voor het opslaan en openen van de metrische gegevens voor dat account. 

Voor het controleren van het opslagverbruik voor Blob Storage moet u de metrische gegevens over capaciteit inschakelen.
Als dit is ingeschakeld, worden de capaciteitsgegevens van een Blob Storage-serviceaccount dagelijks geregistreerd en vastgelegd als een tabelvermelding die naar de *$MetricsCapacityBlob*-tabel binnen hetzelfde opslagaccount wordt geschreven.

Voor het controleren van gegevenstoegangspatronen voor Blob Storage, moet u de metrische gegevens die per uur worden verzameld voor de transactie inschakelen vanaf de API. Als deze methode is ingeschakeld, worden er elk uur per-API-transacties verzameld en geregistreerd als een tabelvermelding die is naar de *$MetricsHourPrimaryTransactionsBlob*-tabel binnen hetzelfde opslagaccount wordt geschreven. De *$MetricsHourSecondaryTransactionsBlob*-tabel registreert de transacties naar het secundaire eindpunt bij gebruik van RA-GRS-opslagaccounts.

> [!NOTE]
> Als u een algemeen opslagaccount hebt waarin u pagina-blobs en virtuele-machineschijven of wachtrijen, bestanden of tabellen, hebt opgeslagen naast blok- en toevoegblobgegevens, is dit schattingsproces niet van toepassing. De capaciteitsgegevens maken geen onderscheid tussen blok-blobs en andere typen en geven dus geen capaciteitsgegevens voor andere gegevenstypen. Als u deze typen gebruikt, is een alternatieve methode om te kijken naar de hoeveelheden op uw meest recente factuur.

Als u een goede schatting wilt maken van uw gegevensverbruik en toegangspatroon, raden we u aan voor de metrische gegevens een retentieperiode te kiezen die een goede afspiegeling is van uw normale gebruik en dat als uitgangspunt te nemen. Een optie is de metrische gegevens zeven dagen te bewaren en de gegevens elke week te verzamelen en aan het einde van de maand te analyseren. Een andere optie is de metrische gegevens van de afgelopen 30 dagen te bewaren en deze gegevens aan het einde van deze periode van 30 dagen te verzamelen en te analyseren.

Voor meer informatie over het inschakelen, verzamelen en weergeven van metrische gegevens, raadpleegt u [Enabling Azure Storage metrics and viewing metrics data](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Metrische gegevens voor Azure Storage inschakelen en metrische gegevens weergeven).

> [!NOTE]
> Het opslaan, openen en downloaden van analytische gegevens wordt op dezelfde manier in rekening gebracht als normale gebruikersgegevens.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Metrische gegevens over het gebruik inzetten voor het schatten van de kosten

#### <a name="storage-costs"></a>Opslagkosten

De meest recente vermelding in de metrische gegevenstabel voor capaciteit *$MetricsCapacityBlob* met de rijsleutel *'data'* toont de opslagcapaciteit die wordt gebruikt door gebruikersgegevens. De meest recente vermelding in de metrische gegevenstabel voor capaciteit *$MetricsCapacityBlob* met de rijsleutel *'analytics'* toont de opslagcapaciteit die wordt gebruikt door de analyselogboeken.

Deze totale capaciteit die wordt verbruikt door zowel gebruikersgegevens en analyselogboeken (indien ingeschakeld) kunnen vervolgens worden gebruikt om de opslagkosten te schatten voor gegevens in het opslagaccount. Deze manier kan ook worden gebruikt voor het schatten van de opslagkosten in GPv1-opslagaccounts.

#### <a name="transaction-costs"></a>Transactiekosten

Het totaal van *'TotalBillableRequests'*, in alle items voor een API in de metrische gegevenstabel voor transactie, geeft het totale aantal transacties voor die bepaalde API weer. *Bijvoorbeeld*: het totale aantal *GetBlob*-transacties in een bepaalde periode kan worden berekend door het totaal te nemen van het totale aantal factureerbare aanvragen voor alle items met de rijsleutel *user;GetBlob*.

Voor het schatten van de transactiekosten van Blob Storage-accounts moet u de transacties in drie groepen opdelen omdat ze verschillend zijn geprijsd.

* Schrijftransacties zoals *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* en *'CopyBlob'*.
* Verwijdertransacties zoals *'DeleteBlob'* en *'DeleteContainer'*.
* Alle andere transacties.

Voor het schatten van de transactiekosten voor GPv1-opslagaccounts moet u alle transacties verzamelen ongeacht de bewerking/API.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Gegevenstoegang en overdrachtskosten voor geo-replicatiegegevens

Opslaganalyse biedt geen informatie over de hoeveelheid gegevens die zijn gelezen en geschreven van en naar een opslagaccount, maar deze hoeveelheid kan min of meer worden geschat door te kijken naar de metrische gegevenstabel voor transacties. Het totaal van *'TotalIngress'* in alle items voor een API in de metrische gegevenstabel voor transacties, geeft de totale hoeveelheid inkomende gegevens in bytes voor die bepaalde API weer. Op dezelfde manier geeft het totaal van *'TotalEgress'* de totale hoeveelheid uitgaande gegevens in bytes weer.

Voor het schatten van de kosten voor het openen van gegevens in Blob Storage-accounts moet u de transacties in twee groepen opdelen.

* De hoeveelheid gegevens die is opgehaald van het opslagaccount, kan worden geschat door te kijken naar het totaal van *'TotalEgress'* voor met name de *'GetBlob'*- en *'CopyBlob'*-bewerkingen.

* De hoeveelheid gegevens die wordt geschreven naar het opslagaccount kan worden geschat door te kijken naar het totaal van *'TotalIngress'* voor met name de *'PutBlob'*-, *'PutBlock'*-, *'CopyBlob'*- en *'AppendBlock'*-bewerkingen.

De overdrachtskosten van geo-replicatiegegevens voor Blob Storage-accounts kan ook worden berekend met behulp van de schatting voor de hoeveelheid gegevens die wordt geschreven bij gebruik van een GRS- of RA-GRS-opslagaccount.

> [!NOTE]
> Ga voor een uitgebreider voorbeeld over het berekenen van de kosten voor het gebruik van Hot of Cool Storage-lagen naar de veelgestelde vraag *'Wat zijn warme, koude en archieftoegangslagen, en hoe bepaal ik welke laag ik moet gebruiken?'* op de [pagina met prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="migrating-existing-data"></a>Bestaande gegevens migreren

Upgraden van een GPv1-account naar GPv2 kan gemakkelijk zonder uitvaltijd of API-wijzigingen en zonder de noodzaak om gegevens te migreren. Daarom wordt het aanbevolen GPv1-accounts naar GPv2-accounts te migreren in plaats van naar Blob Storage-accounts.

Echter, als u naar een Blob Storage-account wilt migreren, kunt u de volgende instructies volgen.

Een Blob Storage-account is speciaal bedoeld voor het opslaan van blok-blobs en toevoeg-blobs. Bestaande opslagaccounts voor algemeen gebruik, waarin u naast blobs ook tabellen, wachtrijen, bestanden en schijven kunt opslaan, kunnen niet worden geconverteerd naar Blob Storage-accounts. Als u gebruik wilt maken van de toegangslagen, maakt u nieuwe Blob Storage-accounts en migreert u de bestaande gegevens naar deze nieuwe accounts.

U kunt de volgende methoden gebruiken om bestaande gegevens vanaf on-premises opslagapparaten, van cloudopslagproviders van derden of vanuit bestaande opslagaccounts voor algemeen gebruik in Azure te migreren naar Blob Storage-accounts:

### <a name="azcopy"></a>AzCopy

AzCopy is een Windows-opdrachtregelprogramma dat is  ontworpen voor het high-performance kopiëren van gegevens van en naar Azure Storage. U kunt AzCopy gebruiken om gegevens uit bestaande opslagaccounts voor algemeen gebruik te kopiëren naar een Blob Storage-account en om gegevens van on-premises opslagaccounts te uploaden naar een Blob Storage-account.

Zie [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie.

### <a name="data-movement-library"></a>Bibliotheek voor gegevensverplaatsing

De Azure Storage-bibliotheek voor gegevensverplaatsing voor .NET is gebaseerd op het basisframework voor gegevensverplaatsing voor AzCopy. De bibliotheek is ontworpen voor high-performance, betrouwbare en eenvoudige gegevensoverdracht vergelijkbaar met AzCopy. Hiermee kunt u in uw toepassing op systeemeigen wijze profiteren van de functies die AzCopy biedt, zonder dat u externe exemplaren van AzCopy hoeft uit te voeren of te controleren.

Zie [Bibliotheek voor gegevensverplaatsing van Azure Storage voor .Net](https://github.com/Azure/azure-storage-net-data-movement) voor meer informatie

### <a name="rest-api-or-client-library"></a>REST-API of clientbibliotheek

U kunt een aangepaste toepassing maken om gegevens naar een Blob Storage-account te migreren met behulp van een van de Azure-clientbibliotheken of de REST API voor Azure Storage-services. Azure Storage biedt uitgebreide clientbibliotheken voor meerdere talen en platforms, zoals  .NET, Java, C++, Node.JS, PHP, Ruby en Python. De clientbibliotheken bieden geavanceerde mogelijkheden, zoals pogingslogica, logboekregistratie en parallelle uploads. U kunt ook rechtstreeks met de REST API ontwikkelen. Deze kan worden aangeroepen in elke taal waarin HTTP-/HTTPS-verzoeken kunnen worden gemaakt.

Zie [Aan de slag met Azure Blob Storage](../blobs/storage-dotnet-how-to-use-blobs.md) voor meer informatie.

> [!IMPORTANT]
> Blobs die aan de clientzijde zijn versleuteld, bevatten versleutelingsgerelateerde metagegevens die samen met de blob zijn opgeslagen. Als u een blob met versleuteling aan de clientzijde kopieert, zorg er dan voor dat bij het kopiëren de blobmetagegevens behouden blijven, en dan met name de versleutelingsgerelateerde metagegevens. Als u een blob kopieert zonder versleutelingsgerelateerde metagegevens, kan de inhoud van de blob niet meer worden opgehaald. Zie [Azure Storage-versleuteling aan de clientzijde](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie over versleutelingsgerelateerde metagegevens.

## <a name="faq"></a>Veelgestelde vragen

**Zijn de bestaande opslagaccounts nog steeds beschikbaar?**

Ja, de bestaande opslagaccounts (GPv1) zijn nog steeds beschikbaar. De prijs en de functionaliteit hiervan zijn niet gewijzigd. U kunt voor GPv1 accounts echter geen opslaglagen kiezen. Dit zal in de toekomst niet veranderen.

**Waarom en wanneer is het een goed idee om GPv2-opslagaccounts te gebruiken?**

GPv2-opslagaccounts zijn speciaal bestemd voor het leveren van de laagste GB-opslagkosten en concurrerende transactie- en gegevenstoegangskosten. Het is een goed idee om GPv2-opslagaccounts te gebruiken voor het opslaan van blobs, omdat er in de toekomst nieuwe mogelijkheden zoals meldingen bij wijzigingen worden geïntroduceerd in dit accounttype. Op basis van uw bedrijfsvereisten bepaalt u echter helemaal zelf wanneer u uw gegevens bijwerkt. U kunt bijvoorbeeld uw transactiepatronen vóór de upgrade optimaliseren.

Downgrade van GPv2 worden niet ondersteund. Houd daarom rekening met alle prijsimplicaties voordat u uw accounts gaat upgraden naar GPv2.

**Kan ik mijn bestaande opslagaccount bijwerken naar een GPv2-opslagaccount?**

Ja. U kunt GPv1- of Blob Storage-accounts eenvoudig upgraden naar GPv2 in de portal, of met behulp van PowerShell of CLI. 

Downgrade van GPv2 worden niet ondersteund. Houd daarom rekening met alle prijsimplicaties voordat u uw accounts gaat upgraden naar GPv2.

**Kan ik objecten opslaan in beide opslaglagen van hetzelfde account?**

Ja. Het kenmerk **Acces Tier** dat op accountniveau is ingesteld, is de standaardlaag die van toepassing is op alle objecten in dat account zonder een expliciet ingestelde laag. Met laaginstelling op blobniveau kunt u echter de toegangslaag op objectniveau instellen, ongeacht de instelling van de toegangslaag voor het account. Blobs in een van de drie opslaglagen (Hot, Cool of Archive) kunnen binnen hetzelfde account aanwezig zijn.

**Kan ik de opslaglaag van mijn GPv2-opslagaccount wijzigen?**

Ja, u kunt de opslaglaag in het opslagaccount wijzigen door het kenmerk **Toegangslaag** in te stellen voor het opslagaccount. Als u de opslaglaag wijzigt, geldt dit voor alle objecten waarvoor geen expliciete laag is ingesteld en die zijn opgeslagen in het account. Bij het wijzigen van de opslaglaag van dynamisch naar statisch worden kosten in rekening gebracht voor zowel schrijfbewerkingen (per 10.000) (alleen voor GPv2-opslagaccounts). Bij het wijzigen van de opslaglaag van statisch naar dynamisch worden kosten in rekening gebracht voor zowel leesbewerkingen (per 10.000) als het ophalen van gegevens (per GB).

**Hoe vaak kan ik de opslaglaag van mijn Blob Storage-account wijzigen?**

Hoewel er geen beperking geldt voor het aantal keren dat u de opslaglaag kunt wijzigen, worden er voor het wijzigen van de opslaglaag van Cool naar Hot aanzienlijke kosten in rekening gebracht. Het wordt afgeraden de opslaglaag regelmatig te wijzigen.

**Gedragen blobs in de opslaglaag voor 'cool' blobs zich anders dan blobs in de opslaglaag voor 'hot' blobs?**

Blobs in de dynamische-opslaglaag voor GPv2- en Blob Storage-accounts hebben dezelfde latentie als blobs in GPv1-opslagaccounts. Blobs in de statische-opslaglaag hebben een gelijksoortige latentie (in milliseconden) als blobs in de dynamische laag. Blobs in de opslaglaag voor 'archive storage'-blobs kennen enkele uren latentie.

Blobs in de opslaglaag voor 'cool' blobs hebben een lagere SLA (Service Level Availability) dan blobs die zijn opgeslagen in de opslaglaag voor 'hot' blobs. Zie [Dienstovereenkomst voor Storage](https://azure.microsoft.com/support/legal/sla/storage) voor meer informatie.

**Kan ik pagina-blobs en virtuele-machineschijven opslaan in Blob Storage-accounts?**

Nee. Blob Storage-accounts ondersteunen alleen blok-blobs en toevoeg-blobs. Pagina-blobs worden niet ondersteund. Omdat virtuele machines van Azure gebruikmaken van pagina-blobs, kunnen virtuele-machineschijven niet worden opgeslagen in Blob Storage-accounts. Het is echter wel mogelijk om back-ups van de virtuele-machineschijven als blok-blobs op te slaan in een Blob Storage-account. Dit is een van de redenen om te overwegen GPv2 in plaats van Blob Storage-accounts te gebruiken.

**Moet ik mijn bestaande toepassingen wijzigen om GPv2-opslagaccounts te kunnen gebruiken?**

GPv2-opslagaccounts zijn 100 procent API-consistent met GPv1- en Blob Storage-accounts. Zolang uw toepassing gebruikmaakt van blok-blobs of toevoeg-blobs en u versie 2014-02-14 of hoger van de [REST-API voor Storage Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) gebruikt, zal uw toepassing geen problemen ondervinden. Als u een oudere versie van het protocol gebruikt, moet u uw toepassing bijwerken voor gebruik van de nieuwe versie, zodat deze naadloos werkt met beide typen opslagaccounts. Over het algemeen is het bijna altijd het beste om de meest recente versie te gebruiken, ongeacht het type opslagaccount dat u gebruikt.

Prijzen voor GPv2 zijn in het algemeen hoger dan GPv1 voor transacties en bandbreedte. Daarom kan het nodig zijn om uw transactiepatronen vóór de upgrade te optimaliseren, zodat de totale rekening niet hoger wordt.

**Wordt de gebruikerservaring gewijzigd?**

GPv2-opslagaccounts zijn vergelijkbaar met GPv1-opslagaccounts voor het ondersteunen van alle belangrijkste functies van Azure Storage, waaronder duurzaamheid, beschikbaarheid, schaalbaarheid, prestaties en beveiliging. Afgezien van de hierboven beschreven functionaliteit en beperkingen die specifiek zijn voor Blob Storage-accounts en de bijbehorende opslaglagen, verandert er voor u niets bij het bijwerken naar GPv2 of Blob Storage.

## <a name="next-steps"></a>Volgende stappen

### <a name="evaluate-blob-storage-accounts"></a>Blob Storage-accounts evalueren

[Beschikbaarheid van Blob Storage-accounts per regio controleren](https://azure.microsoft.com/regions/#services)

[Gebruik van de huidige opslagaccounts evalueren door metrische gegevens voor Azure Storage in te schakelen](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Prijzen voor Blob Storage per regio controleren](https://azure.microsoft.com/pricing/details/storage/)

[Prijzen voor gegevensoverdracht controleren](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-gpv2-storage-accounts"></a>GPv2-opslagaccounts gebruiken

[Aan de slag met Azure Blob Storage](../blobs/storage-dotnet-how-to-use-blobs.md)

[Gegevens verplaatsen naar en uit Azure Storage](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Uw opslagaccounts bekijken en verkennen](http://storageexplorer.com/)
