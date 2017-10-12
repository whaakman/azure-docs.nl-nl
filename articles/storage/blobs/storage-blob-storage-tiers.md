---
title: Azure Hot, Cool en Archive Storage voor blobs | Microsoft Docs
description: Hot, Cool en Archive Storage voor Azure Blob Storage-accounts.
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/05/2017
ms.author: mihauss
ms.openlocfilehash: 544b11d74a926fe62b8ceca51570ce9d2ee7e6e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-blob-storage-hot-cool-and-archive-preview-storage-tiers"></a>Azure Blob Storage: Hot, Cool en Archive (preview) Storage-lagen

## <a name="overview"></a>Overzicht

Azure Storage biedt twee opslaglagen voor de opslag van blob-objecten, zodat u gegevens zeer voordelig kunt opslaan afhankelijk van hoe u deze gebruikt. De Azure **Hot Storage-laag** is geoptimaliseerd voor het opslaan van gegevens die regelmatig worden geopend. De Azure **Cool Storage-laag** is geoptimaliseerd voor het opslaan van gegevens die niet regelmatig worden geopend en die gedurende minimaal een maand worden opgeslagen. De [Archive Storage-laag (preview)](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering) is geoptimaliseerd voor het opslaan van gegevens die zelden worden geraadpleegd en die gedurende ten minste zes maanden worden opgeslagen met flexibele latentievereisten (orde van grootte uur). De *Archive Storage*-laag kan alleen worden gebruikt op het niveau van blobs en niet voor het hele opslagaccount. Voor gegevens in de Cool Storage-laag is een iets lagere beschikbaarheid toegestaan, maar ze vereisen nog steeds een hoge duurzaamheid en een gelijke tijdsduur voor toegang en doorvoer als gegevens in de Hot Storage-laag. Voor gegevens in de Cool en Archive Storage-laag zijn een SLA met een iets lagere beschikbaarheid en hogere toegangskosten aanvaardbaar vanwege de veel lagere opslagkosten.

Het aantal gegevens dat is opgeslagen in de cloud, groeit vandaag de dag exponentieel. Voor een effectief beheer van de kosten voor uw groeiende opslagbehoeften is het een goed idee om de gegevens te ordenen op basis van kenmerken als toegangsfrequentie en geplande bewaarperiode. Er bestaan verschillen in de manier waarop gegevens die in de cloud zijn opgeslagen, tijdens hun levensduur worden gegenereerd, benaderd en verwerkt. Sommige gegevens worden tijdens hun hele levensduur actief geopend en gewijzigd. Andere gegevens worden in het begin van hun levensduur regelmatig geopend, terwijl dit naarmate de tijd verstrijkt, aanzienlijk minder vaak gebeurt. Weer andere gegevens in de cloud zijn inactief en worden, als ze eenmaal zijn opgeslagen, zelden tot nooit geopend.

Het is nuttig om voor elk van deze scenario‘s voor toegang tot gegevens een gedifferentieerde opslaglaag te maken die is geoptimaliseerd voor een specifiek toegangspatroon. Door middel van de lagen Hot, Cool en Archive Storage wordt in Azure Blob Storage voorzien in deze behoefte aan gedifferentieerde opslaglagen met afzonderlijke prijsmodellen.

## <a name="blob-storage-accounts"></a>Blob Storage-accounts

**Blob Storage-accounts** zijn gespecialiseerde opslagaccounts voor het opslaan van ongestructureerde gegevens als blobs (objecten) in Azure Storage. Met Blob Storage-accounts kunt u nu kiezen tussen Hot en Cool Storage-lagen op accountniveau, of Hot, Cool en Archive Storage-lagen op blob-niveau, op basis van toegangspatronen. Bewaar uw zelden gebruikte gegevens tegen de laagste opslagkosten, minder vaak geraadpleegde gegevens tegen lagere opslagkosten kosten dan voor Hot Storage en gegevens die regelmatiger worden gebruikt tegen de laagste toegangskosten. Blob Storage-accounts zijn vergelijkbaar met de bestaande opslagaccounts voor algemeen gebruik en bieden dezelfde hoogwaardige kenmerken op het gebied van duurzaamheid, beschikbaarheid, schaalbaarheid en prestaties waarover u nu al beschikt, inclusief 100 procent API-consistentie voor blok-blobs en toevoeg-blobs.

> [!NOTE]
> Blob Storage-accounts ondersteunen alleen blok-blobs en toevoeg-blobs. Pagina-blobs worden niet ondersteund.

Blob Storage-accounts maken het kenmerk **Toegangslaag** beschikbaar. Dit stelt u in staat om de opslaglaag te specificeren als **Hot** of **Cool**, afhankelijk van de gegevens die in het account zijn opgeslagen. Als er een wijziging optreedt in het gebruikspatroon van de gegevens, kunt u op elk gewenst moment schakelen tussen deze opslaglagen. De Archive Storage-laag (preview) kan alleen worden toegepast op blob-niveau.

> [!NOTE]
> Aan het wijzigen van de opslaglaag kunnen extra kosten zijn verbonden. Zie de sectie [Prijzen en facturering](#pricing-and-billing) voor meer informatie.

### <a name="hot-access-tier"></a>Hot Storage-toegangslaag

Enkele voorbeelden van gebruiksscenario's voor de opslaglaag voor 'hot' blobs:

* Gegevens die actief worden gebruikt of waarvan wordt verwacht dat ze regelmatig worden geopend (lees- en schrijfbewerkingen).
* Gegevens die tijdelijk worden opgeslagen voor verwerking en uiteindelijk voor migratie naar de Cool Storage-laag.

### <a name="cool-access-tier"></a>Cool Storage-toegangslaag

Enkele voorbeelden van gebruiksscenario's voor de Cool Storage-laag:

* Gegevenssets waarvan voor de korte termijn een back-up is gemaakt en die na een noodgeval zijn hersteld.
* Oudere media-inhoud die niet meer regelmatig wordt bekeken, maar onmiddellijk beschikbaar moet zijn wanneer deze wordt geopend.
* Grote gegevenssets die voordelig moeten worden opgeslagen, terwijl er meer gegevens worden verzameld voor toekomstige verwerking. (*Bijvoorbeeld*: langetermijnopslag van wetenschappelijke gegevens, onbewerkte telemetriegegevens van een productiefaciliteit)

### <a name="archive-access-tier-preview"></a>Archive Storage-toegangslaag (preview)

De laag [Archive Storage](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering) heeft de laagste kosten voor opslag en hogere kosten voor het ophalen van gegevens vergeleken met Hot en Cool Storage.

Een blob in Archive Storage kan niet worden gelezen, gekopieerd, overschreven of gewijzigd. Ook kunt u geen momentopnamen maken van een blob in Archive Storage. U kunt echter bestaande bewerkingen gebruiken voor een blob om deze te verwijderen, weer te geven in een lijst, de eigenschappen/metagegevens van de blob weer te geven of de opslaglaag van de blob wijzigen. Als u gegevens wilt lezen die aanwezig zijn in Archive Storage, moet u de laag van de blob eerst wijzigen in Hot of Cool. Dit proces staat bekend als rehydratatie en het voltooien ervan kan maximaal 15 uur duren voor blobs kleiner dan 50 GB. De extra tijd die nodig is voor grotere blobs, varieert naargelang de doorvoerlimiet van blobs.

Tijdens rehydratatie kunt u aan de blob-eigenschap 'archive status' zien of de laag is gewijzigd. De status is "rehydrate-pending-to-hot" of "rehydrate-pending-to-cool", afhankelijk van de doellaag. Als de bewerking is voltooid, wordt de blob-eigenschap "archive status" verwijderd en geeft de blob-eigenschap "access tier" aan of de opslaglaag Hot of Cool is.  

Enkele voorbeelden van gebruiksscenario's voor de Archive Storage-laag:

* Gegevenssets waarvan voor de lange termijn een back-up is gemaakt, die zijn gearchiveerd of die na een noodgeval zijn hersteld.
* Oorspronkelijke (onbewerkte) gegevens die moeten worden bewaard, zelfs nadat deze zijn verwerkt tot hun uiteindelijke, bruikbare vorm. (*Bijvoorbeeld*: onbewerkte mediabestanden na transcodering naar andere indelingen)
* Compliance- en archiveringsgegevens die gedurende lange tijd moeten worden opgeslagen en bijna nooit worden geopend. (*Bijvoorbeeld*: beeldmateriaal van beveiligingscamera‘s, oude röntgenfoto‘s/MRI‘s in ziekenhuizen, geluidsopnamen en transcripties van verkoopgesprekken voor financiële diensten)

### <a name="recommendations"></a>Aanbevelingen

Zie [Over Azure Storage-accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie over opslagaccounts.

Voor toepassingen waarvoor alleen de opslag van blok- of toevoeg-blobs is vereist, wordt aangeraden gebruik te maken van Blob Storage-accounts. Zo profiteert u maximaal van het gedifferentieerde prijsmodel voor gelaagde opslag. We begrijpen dat dit onder bepaalde omstandigheden, waar opslagaccounts voor algemeen gebruik beter werken, wellicht niet mogelijk is. Bijvoorbeeld:

* U maakt gebruik van tabellen, wachtrijen of bestanden en wilt de blobs opslaan in hetzelfde opslagaccount. Let op: opslaan van deze items in hetzelfde account biedt geen technisch voordeel behalve dezelfde gedeelde sleutels.

* U moet nog steeds gebruikmaken van het klassieke implementatiemodel. Blob Storage-accounts zijn alleen beschikbaar via het Azure Resource Manager-implementatiemodel.

* U moet gebruikmaken van pagina-blobs. Blob Storage-accounts bieden geen ondersteuning voor pagina-blobs. Over het algemeen raden we het gebruik van blok-blobs aan, tenzij u specifiek behoefte hebt aan pagina-blobs.

* U gebruikt een versie van de [REST API voor Storage Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) die ouder is dan 2014-02-14 of een clientbibliotheek met een lagere versie dan 4.x en u uw toepassing niet kunt upgraden.

> [!NOTE]
> Blob Storage-accounts worden momenteel ondersteund in alle Azure-regio's.
 

## <a name="blob-level-tiering-feature-preview"></a>Toegangslagen op blob-niveau (preview)

De functie Toegangslagen op blob-niveau maakt het mogelijk om de laag van uw gegevens nu op objectniveau te wijzigen met behulp van een eenmalige bewerking met de naam [Blob-laag instellen](/rest/api/storageservices/set-blob-tier). U kunt de toegangslaag van een blob gemakkelijk wijzigen van Hot in Cool of Archive, plus alle mogelijke varianten. U kunt zo snel inspelen op veranderende gebruikspatronen zonder dat u gegevens tussen accounts hoeft te verplaatsen. Alle laagwijzigingen vinden direct plaats, behalve wanneer een blob wordt gerehydrateerd uit Archive Storage. Blobs in alle drie de opslaglagen kunnen naast elkaar bestaan binnen hetzelfde account. Elke blob waaraan niet expliciet een lag is toegewezen laag, neemt de laag over die is ingesteld als toegangslaag voor het account.

Als u deze functies in preview wilt gebruiken, volgt u de instructies in dit Engelstalige blog: [Announcing the public preview of Azure Archive Blob Storage and Blob-Level Tiering](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering).

Hieronder staan enkele beperkingen die van toepassing zijn tijdens de preview van toegangslagen op blob-niveau:

* Alleen nieuwe Blob Storage-accounts die zijn gemaakt in VS Oost 2 en waarvoor de preview-inschrijving is geslaagd, ondersteunen archiefopslag.

* Alleen nieuwe Blob Storage-accounts die zijn gemaakt in openbare regio's en waarvoor de preview-inschrijving is geslaagd, ondersteunen toegangslagen op blob-niveau.

* Toegangslagen op blob-niveau en archiefopslag worden alleen ondersteund in [LRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#locally-redundant-storage)-opslag. Ondersteuning voor [GRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#geo-redundant-storage) en [RA-GRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#read-access-geo-redundant-storage) volgt in de toekomst.

* U kunt de laag van een blob met momentopnamen niet wijzigen.

* U mag een blob in archiefopslag niet kopiëren of er een momentopname van maken.

## <a name="comparison-of-the-storage-tiers"></a>Vergelijking van de opslaglagen

In de volgende tabel ziet u een vergelijking van de Hot en Cool Storage-lagen. Archive Storage op blob-niveau is in preview, dus hier zijn nog geen serviceovereenkomsten voor.

| | **Hot Storage-laag** | **Cool Storage-laag** |
| ---- | ----- | ----- |
| **Beschikbaarheid** | 99,9% | 99% |
| **Beschikbaarheid** <br> **(RA-GRS-leesbewerkingen)**| 99,99% | 99,9% |
| **Gebruikskosten** | Hogere opslagkosten, lagere toegangs- en transactiekosten | Lagere opslagkosten, hogere toegangs- en transactiekosten |
| **Minimale objectgrootte** | N.v.t. | N.v.t. |
| **Minimale opslagduur** | N.v.t. | N.v.t. |
| **Latentie** <br> **(Tijd tot eerste byte)** | milliseconden | milliseconden |
| **Schaalbaarheids- en prestatiedoelen** | Dezelfde als bij opslagaccounts voor algemeen gebruik | Dezelfde als bij opslagaccounts voor algemeen gebruik |

> [!NOTE]
> Blob Storage-accounts bieden ondersteuning voor dezelfde schaalbaarheids- en prestatiedoelen als opslagaccounts voor algemeen gebruik. Zie [Schaalbaarheids- en prestatiedoelen in Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie.


## <a name="pricing-and-billing"></a>Prijzen en facturering
Blob Storage-accounts maken gebruik van een prijsmodel voor het opslaan van blobs op basis van de opslaglaag. Als u een Blob Storage-account gebruikt, zijn de volgende factureringsvoorwaarden van toepassing:

* **Opslagkosten**: de kosten voor het opslaan van gegevens hangen niet alleen af van de hoeveelheid opgeslagen gegevens, maar ook van de gebruikte opslaglaag. De kosten per GB voor de opslaglaag voor 'cool' blobs zijn lager dan die voor de opslaglaag voor 'hot' blobs.

* **Kosten voor gegevenstoegang**: voor gegevens in de opslaglaag voor 'cool' blobs worden kosten in rekening gebracht per GB aan gegevenstoegang voor lees- en schrijfbewerkingen.

* **Transactiekosten**: voor beide lagen worden kosten in rekening gebracht per transactie. De kosten per transactie zijn voor de opslaglaag voor 'cool' blobs echter hoger dan voor de opslaglaag voor 'hot' blobs.

* **Kosten voor gegevensoverdracht met geo-replicatie**: dit is alleen van toepassing op accounts waarvoor geo-replicatie is geconfigureerd, inclusief GRS en RA-GRS. Kosten voor gegevensoverdracht met geo-replicatie worden in rekening gebracht per GB.

* **Kosten voor uitgaande gegevensoverdracht**: uitgaande gegevensoverdracht (gegevens die buiten een Azure-regio worden overgedragen) worden gefactureerd voor bandbreedtegebruik per GB, net zoals bij opslagaccounts voor algemeen gebruik.

* **De opslaglaag wijzigen**: als u de opslaglaag wijzigt van 'cool' naar 'hot', worden voor elke overgang kosten in rekening gebracht die overeenkomen met de kosten voor het lezen van alle bestaande gegevens in het opslagaccount. Het wijzigen van de opslaglaag van 'hot' naar 'cool' is echter gratis.

> [!NOTE]
> Ga naar de pagina [Prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor meer informatie over het prijsmodel voor Blob Storage-accounts. Ga naar de pagina [Prijsinformatie voor bandbreedte](https://azure.microsoft.com/pricing/details/data-transfers/) voor meer informatie over de kosten voor uitgaande gegevensoverdracht.

## <a name="quickstart"></a>Snelstartgids

In deze sectie worden de volgende scenario‘s toegelicht, waarbij gebruik wordt gemaakt van Azure Portal:

* Het maken van een Blob Storage-account.
* Het beheren van een Blob Storage-account.

U kunt de toegangslaag nu instellen op Archive in de volgende voorbeelden omdat deze instelling voor het hele opslagaccount geldt. Archive Storage kan alleen worden ingesteld voor een specifieke blob.

### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Een Blob Storage-account maken met behulp van Azure Portal

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).

2. Selecteer in het menu Hub achtereenvolgens **Nieuw** > **Gegevens en opslag** > **Opslagaccount**.

3. Voer een naam in voor het opslagaccount.
   
    Deze naam moet uniek zijn. De naam wordt gebruikt als onderdeel van de URL die wordt gebruikt voor toegang tot de objecten in het opslagaccount.  

4. Selecteer **Resource Manager** als het implementatiemodel.
   
    Gelaagde opslag kan alleen worden gebruikt met Resource Manager-opslagaccounts. Dit is het aanbevolen implementatiemodel voor nieuwe resources. Zie [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor meer informatie.  

5. Selecteer **Blob Storage** in de vervolgkeuzelijst Soort account.
   
    Hier selecteert u het type opslagaccount. Gelaagde opslag is niet beschikbaar in de algemene opslag. Dit is alleen beschikbaar in het type account voor Blob Storage.     
   
    Houd er rekening mee dat, wanneer u deze optie selecteert, de prestatielaag wordt ingesteld op Standaard. Gelaagde opslag is niet beschikbaar bij de Premium-prestatielaag.

6. Selecteer de replicatieoptie voor het opslagaccount: **LRS**, **GRS** of **RA-GRS**. **RA-GRS** is de standaardinstelling.
   
    LRS = locally redundant storage (lokaal redundante opslag), GRS = geo-redundant storage (geografisch redundante opslag) (2 regio's), RA-GRS = read-access geo-redundant storage (geografisch redundante opslag met leestoegang) (2 regio's met leestoegang tot de tweede).
   
    Zie [Azure Storage-replicatie](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie over Azure Storage-replicatieopties.

7. Selecteer de juiste opslaglaag voor wat u nodig hebt: stel de **Toegangslaag** in op **'Cool'** of **'Hot'**. **Hot** is de standaardinstelling. 

8. Selecteer het abonnement waarin u het nieuwe opslagaccount wilt maken.

9. Geef een nieuwe resourcegroep op of selecteer een bestaande resourcegroep. Zie [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) voor meer informatie over resourcegroepen.

10. Selecteer de regio voor uw opslagaccount.

11. Klik op **Maken** om het opslagaccount te maken.

### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>De opslaglaag voor een Blob Storage-account wijzigen via Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw opslagaccount, selecteer Alle resources en selecteer vervolgens uw opslagaccount.

3. Klik in de Instellingen-blade op **Configuratie** om de accountconfiguratie te bekijken en/of te wijzigen.

4. Selecteer de juiste opslaglaag voor wat u nodig hebt: stel **Toegangslaag** in op **Cool** of **Hot**.

5. Klik op Opslaan boven aan de blade.

> [!NOTE]
> Aan het wijzigen van de opslaglaag kunnen extra kosten zijn verbonden. Zie de sectie [Prijzen en facturering](#pricing-and-billing) voor meer informatie.


## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Evalueren en migreren naar Blob Storage-accounts
Het doel van deze sectie is om gebruikers op soepele wijze te helpen migreren naar het gebruiken van Blob Storage-accounts. Er zijn twee scenario's voor gebruikers:

* U beschikt over een bestaand algemene opslagaccount en wilt een wijziging evalueren in een Blob Storage-account met de juiste opslaglaag.
* U hebt besloten om een Blob Storage-account te gebruiken of u hebt er al een, en u wilt beoordelen of een opslaglaag voor 'hot' blobs of een opslaglaag voor 'cool' blobs moet gebruiken.

In beide gevallen moeten eerst de kosten worden geschat van het opslaan en openen van uw gegevens die zijn opgeslagen in een Blob Storage-account en dient u deze te vergelijken met uw huidige kosten.

## <a name="evaluating-blob-storage-account-tiers"></a>Lagen voor Blob Storage-accounts evalueren

Voor het maken van een schatting van de kosten voor het opslaan en openen van gegevens die zijn opgeslagen in een Blob Storage-account, moet u uw bestaande gebruikspatroon evalueren of een schatting maken van het verwachte gebruikspatroon. Doorgaans zijn de volgende gegevens hiervoor van belang:

* Wat is het gebruik van de opslag? Hoeveel gegevens worden er opgeslagen en hoe wijzigt dit maandelijks?

* Wat is het toegangspatroon voor de opslag? Op hoeveel gegevens in het account worden er lees- en/of schrijfbewerkingen uitgevoerd (inclusief nieuwe gegevens)? Hoeveel transacties worden gebruikt voor toegang tot gegevens? En wat voor soort transacties zijn dit?

## <a name="monitoring-existing-storage-accounts"></a>Bewaking van bestaande opslagaccounts

Voor het bewaken van uw bestaande opslagaccounts en het verzamelen van deze gegevens, kunt u gebruikmaken van Azure Storage Analytics dat logboekregistratie uitvoert en metrische gegevens biedt voor een opslagaccount. Storage Analytics kan metrische gegevens opslaan die samengevoegde transactiestatistieken en capaciteitsgegevens bevat over Blob Storage-serviceaanvragen voor algemene opslagaccounts en Blob Storage-accounts. Deze gegevens worden opgeslagen in bekende tabellen in hetzelfde opslagaccount.

Raadpleeg voor meer informatie [About Storage Analytics Metrics](https://msdn.microsoft.com/library/azure/hh343258.aspx) (Metrische gegevens in Storage Analytics) en [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx) (Tabelschema van metrische gegevens in Storage Analytics).

> [!NOTE]
> Blob Storage-accounts geven het service-eindpunt van de tabel alleen weer voor het opslaan en openen van de metrische gegevens voor dat account.

Voor het controleren van het opslagverbruik voor de Blob Storage-service moet u de metrische gegevens over capaciteit inschakelen.
Als dit is ingeschakeld, worden de capaciteitsgegevens van een Blob Storage-serviceaccount dagelijks geregistreerd en vastgelegd als een tabelvermelding die naar de *$MetricsCapacityBlob*-tabel binnen hetzelfde opslagaccount wordt geschreven.

Voor het controleren van het gegevenstoegangspatroon voor de Blob Storage-service, moet u de metrische gegevens die per uur worden verzameld voor de transactie inschakelen op een API-niveau. Als dit is ingeschakeld, worden er elk uur per-API-transacties verzameld en geregistreerd als een tabelvermelding die is naar de *$MetricsHourPrimaryTransactionsBlob*-tabel binnen hetzelfde opslagaccount wordt geschreven. De *$MetricsHourSecondaryTransactionsBlob*-tabel registreert de transacties naar het secundaire eindpunt bij gebruik van RA-GRS-opslagaccounts.

> [!NOTE]
> Als u een algemeen opslagaccount hebt waarin u pagina-blobs en virtuele-machineschijven hebt opgeslagen naast blok- en toevoegblobgegevens, is dit schattingsproces niet van toepassing. Dit komt omdat er geen enkele manier is om onderscheid te maken tussen metrische gegevens voor capaciteit en voor transactie op basis van het type blob voor alleen blok- en toevoeg-blobs die kunnen worden gemigreerd naar een Blob Storage-account.

Als u een goede schatting wilt maken van uw gegevensverbruik en toegangspatroon, raden we u aan voor de metrische gegevens een retentieperiode te kiezen die een goede afspiegeling is van uw normale gebruik en dat als uitgangspunt te nemen. Een optie is de metrische gegevens 7 dagen te bewaren en de gegevens elke week te verzamelen en aan het einde van de maand te analyseren. Een andere optie is de metrische gegevens van de afgelopen 30 dagen te bewaren en deze gegevens aan het einde van deze periode van 30 dagen te verzamelen en te analyseren.

Voor meer informatie over het inschakelen, verzamelen en weergeven van metrische gegevens, raadpleegt u [Enabling Azure Storage metrics and viewing metrics data](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Metrische gegevens voor Azure Storage inschakelen en metrische gegevens weergeven).

> [!NOTE]
> Het opslaan, openen en downloaden van analytische gegevens wordt op dezelfde manier in rekening gebracht als normale gebruikersgegevens.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Metrische gegevens over het gebruik inzetten voor het schatten van de kosten

### <a name="storage-costs"></a>Opslagkosten

De meest recente vermelding in de metrische gegevenstabel voor capaciteit *$MetricsCapacityBlob* met de rijsleutel *'data'* toont de opslagcapaciteit die wordt gebruikt door gebruikersgegevens. De meest recente vermelding in de metrische gegevenstabel voor capaciteit *$MetricsCapacityBlob* met de rijsleutel *'analytics'* toont de opslagcapaciteit die wordt gebruikt door de analyselogboeken.

Deze totale capaciteit die wordt verbruikt door zowel gebruikersgegevens en analyselogboeken (indien ingeschakeld) kunnen vervolgens worden gebruikt om de opslagkosten te schatten voor gegevens in het opslagaccount. Deze manier kan ook worden gebruikt voor het schatten van de opslagkosten voor blok- en toevoegblobs in algemene opslagaccounts.

### <a name="transaction-costs"></a>Transactiekosten

Het totaal van *'TotalBillableRequests'*, in alle items voor een API in de metrische gegevenstabel voor transactie, geeft het totale aantal transacties voor die bepaalde API weer. *Bijvoorbeeld*: het totale aantal *GetBlob*-transacties in een bepaalde periode kan worden berekend door het totaal te nemen van het totale aantal factureerbare aanvragen voor alle items met de rijsleutel *user;GetBlob*.

Voor het schatten van de transactiekosten van Blob Storage-accounts moet u de transacties in drie groepen opdelen omdat ze verschillend zijn geprijsd.

* Schrijftransacties zoals *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* en *'CopyBlob'*.
* Verwijdertransacties zoals *'DeleteBlob'* en *'DeleteContainer'*.
* Alle andere transacties.

Voor het schatten van de transactiekosten voor algemene opslagaccounts moet u alle transacties verzamelen ongeacht de bewerking/API.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Gegevenstoegang en overdrachtskosten voor geo-replicatiegegevens

Opslaganalyse biedt geen informatie over de hoeveelheid gegevens die zijn gelezen en geschreven van en naar een opslagaccount, maar deze hoeveelheid kan min of meer worden geschat door te kijken naar de metrische gegevenstabel voor transacties. Het totaal van *'TotalIngress'* in alle items voor een API in de metrische gegevenstabel voor transacties, geeft de totale hoeveelheid inkomende gegevens in bytes voor die bepaalde API weer. Op dezelfde manier geeft het totaal van *'TotalEgress'* de totale hoeveelheid uitgaande gegevens in bytes weer.

Voor het schatten van de kosten voor het openen van gegevens in Blob Storage-accounts moet u de transacties in twee groepen opdelen. 

* De hoeveelheid gegevens die is opgehaald van het opslagaccount, kan worden geschat door te kijken naar het totaal van *'TotalEgress'* voor met name de *'GetBlob'*- en *'CopyBlob'*-bewerkingen.

* De hoeveelheid gegevens die wordt geschreven naar het opslagaccount kan worden geschat door te kijken naar het totaal van *'TotalIngress'* voor met name de *'PutBlob'*-, *'PutBlock'*-, *'CopyBlob'*- en *'AppendBlock'*-bewerkingen.

De overdrachtskosten van geo-replicatiegegevens voor Blob Storage-accounts kan ook worden berekend met behulp van de schatting voor de hoeveelheid gegevens die wordt geschreven bij gebruik van een GRS- of RA-GRS-opslagaccount.

> [!NOTE]
> Ga voor een uitgebreider voorbeeld over het berekenen van de kosten voor het gebruik van Hot of Cool Storage-lagen naar de veelgestelde vraag *'Wat zijn warme, koude en archieftoegangslagen, en hoe bepaal ik welke laag ik moet gebruiken?'* op de [pagina met prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
 
## <a name="migrating-existing-data"></a>Bestaande gegevens migreren

Een Blob Storage-account is speciaal bedoeld voor het opslaan van blok-blobs en toevoeg-blobs. Bestaande opslagaccounts voor algemeen gebruik, waarin u naast blobs ook tabellen, wachtrijen, bestanden en schijven kunt opslaan, kunnen niet worden geconverteerd naar Blob Storage-accounts. Als u gebruik wilt maken van de toegangslagen, maakt u nieuwe Blob Storage-accounts en migreert u de bestaande gegevens naar deze nieuwe accounts.

U kunt de volgende methoden gebruiken om bestaande gegevens vanaf on-premises opslagapparaten, van cloudopslagproviders van derden of vanuit bestaande opslagaccounts voor algemeen gebruik in Azure te migreren naar Blob Storage-accounts:

### <a name="azcopy"></a>AzCopy

AzCopy is een Windows-opdrachtregelprogramma dat is  ontworpen voor het high-performance kopiëren van gegevens van en naar Azure Storage. U kunt AzCopy gebruiken om gegevens uit bestaande opslagaccounts voor algemeen gebruik te kopiëren naar een Blob Storage-account en om gegevens van on-premises opslagaccounts te uploaden naar een Blob Storage-account.

Zie [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie.

### <a name="data-movement-library"></a>Bibliotheek voor gegevensverplaatsing

De Azure Storage-bibliotheek voor gegevensverplaatsing voor .NET is gebaseerd op het basisframework voor gegevensverplaatsing van AzCopy. De bibliotheek is ontworpen voor high-performance, betrouwbare en eenvoudige gegevensoverdracht vergelijkbaar met AzCopy. Hierdoor kunt u in uw toepassing op systeemeigen wijze maximaal profiteren van de functies die AzCopy biedt, zonder dat u externe exemplaren van AzCopy hoeft uit te voeren of te controleren.

Zie [Bibliotheek voor gegevensverplaatsing van Azure Storage voor .Net](https://github.com/Azure/azure-storage-net-data-movement) voor meer informatie.

### <a name="rest-api-or-client-library"></a>REST-API of clientbibliotheek

U kunt een aangepaste toepassing maken om gegevens naar een Blob Storage-account te migreren met behulp van een van de Azure-clientbibliotheken of de REST API voor Azure Storage-services. Azure Storage biedt uitgebreide clientbibliotheken voor meerdere talen en platforms, zoals  .NET, Java, C++, Node.JS, PHP, Ruby en Python. De clientbibliotheken bieden geavanceerde mogelijkheden, zoals pogingslogica, logboekregistratie en parallelle uploads. U kunt ook rechtstreeks met de REST API ontwikkelen. Deze kan worden aangeroepen in elke taal waarin HTTP-/HTTPS-verzoeken kunnen worden gemaakt.

Zie [Aan de slag met Azure Blob Storage](storage-dotnet-how-to-use-blobs.md) voor meer informatie.

> [!NOTE]
> Blobs die aan de clientzijde zijn versleuteld, bevatten versleutelingsgerelateerde metagegevens die samen met de blob zijn opgeslagen. Het is absoluut essentieel dat de metagegevens in de blob (en dan met name de versleutelingsgerelateerde metagegevens) bij het kopiëren behouden blijven. Als u de blobs kopieert zonder deze metagegevens, kan de inhoud van de blob niet meer worden opgehaald. Zie [Azure Storage-versleuteling aan de clientzijde](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie over versleutelingsgerelateerde metagegevens.
 
## <a name="faq"></a>Veelgestelde vragen

1. **Zijn de bestaande opslagaccounts nog steeds beschikbaar?**
   
    Ja, de bestaande opslagaccounts zijn nog steeds beschikbaar. De prijs en de functionaliteit hiervan zijn niet gewijzigd.  U kunt voor deze opslagaccounts echter geen opslaglagen kiezen. Dit zal in de toekomst niet veranderen.

2. **Waarom en wanneer is het een goed idee om Blob Storage-accounts te gebruiken?**
   
    Blob Storage-accounts zijn helemaal gericht op het opslaan van blobs en maken het introduceren van nieuwe blobfuncties mogelijk. Het is een goed idee om Blob Storage-accounts te gebruiken voor het opslaan van blobs, omdat er in de toekomst nieuwe mogelijkheden (bijvoorbeeld hiërarchische opslag en lagen) worden geïntroduceerd in dit accounttype. Op basis van uw bedrijfsvereisten bepaalt u echter helemaal zelf wanneer u uw gegevens migreert.

3. **Kan ik mijn bestaande opslagaccount converteren naar een Blob Storage-account?**
   
    Nee. Een Blob Storage-account is een ander soort opslagaccount. Daarom moet u dit account afzonderlijk maken en dient u de gegevens naar dit account te migreren zoals eerder is uitgelegd.

4. **Kan ik objecten opslaan in beide opslaglagen van hetzelfde account?**
   
    Het kenmerk *'Toegangslaag'* geeft de opslaglaag aan die op accountniveau is ingesteld en is van toepassing op alle objecten in dat account. Met de functie Toegangslagen op blob-niveau (preview) kunt u nu echter de toegangslaag voor specifieke blobs instellen en zo de instelling van de toegangslaag voor het account overschrijven. 

5. **Kan ik de opslaglaag van mijn Blob Storage-account wijzigen?**
   
    Ja. U kunt de opslaglaag in het opslagaccount wijzigen door het kenmerk *'Toegangslaag'* in te stellen op het opslagaccount. Als u de opslaglaag wijzigt, is dit van toepassing op alle objecten die in het account zijn opgeslagen. Als u de opslaglaag wijzigt van Hot naar Cool, worden er geen kosten in rekening gebracht. Als u echter de toegangslaag wijzigt van Cool naar Hot, worden er kosten berekend per GB voor het lezen van alle gegevens in het account.

6. **Hoe vaak kan ik de opslaglaag van mijn Blob Storage-account wijzigen?**
   
    Hoewel er geen beperking geldt voor het aantal keren dat u de opslaglaag kunt wijzigen, worden er voor het wijzigen van de opslaglaag van Cool naar Hot aanzienlijke kosten in rekening gebracht. We raden u daarom aan de opslaglaag niet te vaak te wijzigen.

7. **Gedragen blobs in de opslaglaag voor 'cool' blobs zich anders dan blobs in de opslaglaag voor 'hot' blobs?**
   
    Blobs in de opslaglaag voor 'hot' blobs hebben dezelfde latentie als blobs in opslagaccounts voor algemeen gebruik. Blobs in de opslaglaag voor 'cool' blobs hebben een gelijksoortige latentie (in milliseconden) als blobs in opslagaccounts voor algemeen gebruik.
   
    Blobs in de opslaglaag voor 'cool' blobs hebben een lagere SLA (Service Level Availability) dan blobs die zijn opgeslagen in de opslaglaag voor 'hot' blobs. Zie [SLA voor opslag](https://azure.microsoft.com/support/legal/sla/storage) voor meer informatie.

8. **Kan ik pagina-blobs en virtuele-machineschijven opslaan in Blob Storage-accounts?**
   
    Blob Storage-accounts ondersteunen alleen blok-blobs en toevoeg-blobs. Pagina-blobs worden niet ondersteund. Omdat virtuele machines van Azure gebruikmaken van pagina-blobs, kunnen virtuele-machineschijven niet worden opgeslagen in Blob Storage-accounts. Het is echter wel mogelijk om back-ups van de virtuele-machineschijven als blok-blobs op te slaan in een Blob Storage-account.

9. **Moet ik mijn bestaande toepassingen wijzigen om Blob Storage-accounts te kunnen gebruiken?**
   
    Blob Storage-accounts zijn voor 100 procent API-consistent met opslagaccounts voor algemeen gebruik voor blok- en toevoeg-blobs. Zolang uw toepassing gebruikmaakt van blok-blobs of toevoeg-blobs en u versie 2014-02-14 of hoger van de [REST-API voor Storage Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) gebruikt, zal uw toepassing geen problemen ondervinden. Als u een oudere versie van het protocol gebruikt, moet u uw toepassing bijwerken voor gebruik van de nieuwe versie, zodat deze naadloos werkt met beide typen opslagaccounts. Over het algemeen is het bijna altijd het beste om de meest recente versie te gebruiken, ongeacht het type opslagaccount dat u gebruikt.

10. **Is de gebruikerservaring gewijzigd?**
    
    Blob Storage-accounts zijn vergelijkbaar met opslagaccounts voor algemeen gebruik voor het opslaan van blok- en toevoeg-blobs. Hierbij maken ze maximaal gebruik van de kracht van Azure Storage op het gebied van duurzaamheid, beschikbaarheid, schaalbaarheid, prestaties en beveiliging. Afgezien van de hierboven beschreven functionaliteit en beperkingen die specifiek zijn voor Blob Storage-accounts en de bijbehorende opslaglagen, verandert er voor u niets.

## <a name="next-steps"></a>Volgende stappen

### <a name="evaluate-blob-storage-accounts"></a>Blob Storage-accounts evalueren

[Beschikbaarheid van Blob Storage-accounts per regio controleren](https://azure.microsoft.com/regions/#services)

[Gebruik van de huidige opslagaccounts evalueren door metrische gegevens voor Azure Storage in te schakelen](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Prijzen voor Blob Storage per regio controleren](https://azure.microsoft.com/pricing/details/storage/)

[Prijzen voor gegevensoverdracht controleren](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Blob Storage-accounts gebruiken

[Aan de slag met Azure Blob Storage](storage-dotnet-how-to-use-blobs.md)

[Gegevens verplaatsen naar en uit Azure Storage](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Uw opslagaccounts bekijken en verkennen](http://storageexplorer.com/)