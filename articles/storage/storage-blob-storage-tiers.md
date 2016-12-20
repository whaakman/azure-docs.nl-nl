---
title: Azure Storage voor &quot;cool&quot; blobs | Microsoft Docs
description: Opslaglagen voor Azure Blob Storage bieden voordelige opslag voor objectgegevens op basis van toegangspatronen. De opslaglaag voor &quot;cool&quot; blobs is geoptimaliseerd voor gegevens die minder regelmatig worden geopend.
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
ms.date: 10/18/2016
ms.author: mihauss
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 998e95611bca6778de601239bcf9c81246dead83


---
# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure Blob Storage: opslaglagen voor 'hot' blobs en 'cool' blobs
## <a name="overview"></a>Overzicht
Azure Storage biedt nu twee opslaglagen voor Blob Storage (objectopslag), zodat u gegevens zeer voordelig kunt opslaan afhankelijk van hoe u deze gebruikt. De Azure **Hot Storage-laag** is geoptimaliseerd voor het opslaan van gegevens die regelmatig worden geopend. De Azure **Cool Storage-laag** is geoptimaliseerd voor het opslaan van gegevens die niet regelmatig worden geopend en een lange levensduur hebben. Voor gegevens in de Cool Storage-laag is een iets lagere beschikbaarheid toegestaan, maar ze vereisen nog steeds een hoge duurzaamheid en een gelijke tijdsduur voor toegang en doorvoer als gegevens in de Hot Storage-laag. Voor gegevens in de Cool Storage-laag zijn een SLA met een iets lagere beschikbaarheid en hogere toegangskosten aanvaardbaar vanwege de veel lagere opslagkosten.

Het aantal gegevens dat is opgeslagen in de cloud, groeit vandaag de dag exponentieel. Om de kosten voor uw groeiende opslagbehoeften te beheren, is het nuttig de gegevens te ordenen op basis van kenmerken als toegangsfrequentie en geplande bewaarperiode. Er bestaan grote verschillen in de manier waarop gegevens die in de cloud zijn opgeslagen, tijdens hun levensduur worden gegenereerd, benaderd en verwerkt. Sommige gegevens worden tijdens hun hele levensduur actief geopend en gewijzigd. Andere gegevens worden in het begin van hun levensduur zeer regelmatig geopend, terwijl dit naarmate de tijd verstrijkt, aanzienlijk minder vaak gebeurt. Weer andere gegevens in de cloud zijn inactief en worden, als ze eenmaal zijn opgeslagen, zelden tot nooit geopend.

Het is nuttig om voor elk van deze scenario‘s voor toegang tot gegevens die hierboven worden beschreven, een gedifferentieerde opslaglaag te maken die is geoptimaliseerd voor een specifiek toegangspatroon. Dankzij de introductie van de opslaglagen voor 'hot' blobs en 'cool' blobs in Azure Blob Storage wordt voorzien in deze behoefte aan gedifferentieerde opslaglagen met afzonderlijke prijsmodellen.

## <a name="blob-storage-accounts"></a>Blob Storage-accounts
**Blob Storage-accounts** zijn gespecialiseerde opslagaccounts voor het opslaan van ongestructureerde gegevens als blobs (objecten) in Azure Storage. Met Blob Storage-accounts kunt u nu kiezen tussen opslaglagen voor 'hot' blobs en 'cool' blobs. Zo kunt u minder regelmatig geopende gegevens opslaan tegen lagere opslagkosten en regelmatiger geopende gegevens tegen lagere toegangskosten. Blob Storage-accounts zijn vergelijkbaar met de bestaande opslagaccounts voor algemeen gebruik en bieden dezelfde hoogwaardige kenmerken op het gebied van duurzaamheid, beschikbaarheid, schaalbaarheid en prestaties waarover u nu al beschikt, inclusief 100 procent API-consistentie voor blok-blobs en toevoeg-blobs.

> [!NOTE]
> Blob Storage-accounts ondersteunen alleen blok-blobs en toevoeg-blobs. Pagina-blobs worden niet ondersteund.
> 
> 

Blob Storage-accounts maken het kenmerk **Toegangslaag** beschikbaar. Dit stelt u in staat om de opslaglaag te specificeren als **Hot** of **Cool**, afhankelijk van de gegevens die in het account zijn opgeslagen. Als er een wijziging optreedt in het gebruikspatroon van de gegevens, kunt u op elk gewenst moment schakelen tussen deze opslaglagen.

> [!NOTE]
> Aan het wijzigen van de opslaglaag kunnen extra kosten zijn verbonden. Zie de sectie [Prijzen en facturering](storage-blob-storage-tiers.md#pricing-and-billing) voor meer informatie.
> 
> 

Enkele voorbeelden van gebruiksscenario's voor de opslaglaag voor 'hot' blobs:

* Gegevens die actief worden gebruikt of waarvan wordt verwacht dat ze regelmatig worden geopend (lees- en schrijfbewerkingen).
* Gegevens die tijdelijk worden opgeslagen voor verwerking en uiteindelijk voor migratie naar de Cool Storage-laag.

Enkele voorbeelden van gebruiksscenario's voor de Cool Storage-laag:

* Gegevenssets waarvan een back-up is gemaakt, die zijn gearchiveerd of die na een noodgeval zijn hersteld.
* Oudere media-inhoud die niet meer regelmatig wordt bekeken, maar onmiddellijk beschikbaar moet zijn wanneer deze wordt geopend.
* Grote gegevenssets die voordelig moeten worden opgeslagen, terwijl er meer gegevens worden verzameld voor toekomstige verwerking. (*bijvoorbeeld*: langetermijnopslag van wetenschappelijke gegevens, onbewerkte telemetriegegevens van een fabrikant)
* Oorspronkelijke (onbewerkte) gegevens die moeten worden bewaard, zelfs nadat deze zijn verwerkt tot hun uiteindelijke, bruikbare vorm. (*bijvoorbeeld*: onbewerkte mediabestanden na transcodering naar andere indelingen)
* Compliance- en archiveringsgegevens die gedurende lange tijd moeten worden opgeslagen en bijna nooit worden geopend. (*bijvoorbeeld*: beeldmateriaal van beveiligingscamera‘s, oude röntgenfoto‘s/MRI‘s in ziekenhuizen, geluidsopnamen en transcripties van verkoopgesprekken voor financiële diensten)

Zie [Over Azure Storage-accounts](storage-create-storage-account.md) voor meer informatie over opslagaccounts.

Voor toepassingen waarvoor alleen de opslag van blok- of toevoeg-blobs is vereist, wordt aangeraden gebruik te maken van Blob Storage-accounts. Zo profiteert u maximaal van het gedifferentieerde prijsmodel voor gelaagde opslag. We begrijpen dat dit onder bepaalde omstandigheden, waarbij opslagaccounts voor algemeen gebruik beter werken, wellicht niet mogelijk is. Bijvoorbeeld:

* U maakt gebruik van tabellen, wachtrijen of bestanden en wilt de blobs opslaan in hetzelfde opslagaccount. Let op: opslaan van deze items in hetzelfde account biedt geen technisch voordeel behalve dezelfde gedeelde sleutels.
* U moet nog steeds gebruikmaken van het klassieke implementatiemodel. Blob Storage-accounts zijn alleen beschikbaar via het Azure Resource Manager-implementatiemodel.
* U moet gebruikmaken van pagina-blobs. Blob Storage-accounts bieden geen ondersteuning voor pagina-blobs. Over het algemeen raden we het gebruik van blok-blobs aan, tenzij u specifiek behoefte hebt aan pagina-blobs.
* U gebruikt een versie van de [REST API voor Storage Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) die ouder is dan 2014-02-14 of een clientbibliotheek met een lagere versie dan 4.x en u uw toepassing niet kunt upgraden.

> [!NOTE]
> Blob Storage-accounts worden momenteel ondersteund in de meeste Azure-regio‘s. Verdere uitbreiding is gepland. Ga naar de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services) voor de bijgewerkte lijst met beschikbare regio‘s.
> 
> 

## <a name="comparison-between-the-storage-tiers"></a>Vergelijking tussen de opslaglagen
De volgende tabel maakt de verschillen tussen de twee opslaglagen inzichtelijk:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Opslaglaag voor 'hot' blobs</center></strong></td>
    <td><strong><center>Opslaglaag voor 'cool' blobs</center></strong></td
</tr>
<tr>
    <td><strong><center>Beschikbaarheid</center></strong></td>
    <td><center>99,9%</center></td>
    <td><center>99%</center></td>
</tr>
<tr>
    <td><strong><center>Beschikbaarheid<br>(RA-GRS-leesbewerkingen)</center></strong></td>
    <td><center>99,99%</center></td>
    <td><center>99,9%</center></td>
</tr>
<tr>
    <td><strong><center>Gebruikskosten</center></strong></td>
    <td><center>Hogere opslagkosten<br>Lagere toegangs- en transactiekosten</center></td>
    <td><center>Lagere opslagkosten<br>Hogere toegangs- en transactiekosten</center></td>
</tr>
<tr>
    <td><strong><center>Minimale objectgrootte<center></strong></td>
    <td colspan="2"><center>N.v.t.</center></td>
</tr>
<tr>
    <td><strong><center>Minimale opslagduur<center></strong></td>
    <td colspan="2"><center>N.v.t.</center></td>
</tr>
<tr>
    <td><strong><center>Latentie<br>(Tijd tot eerste byte)<center></strong></td>
    <td colspan="2"><center>milliseconden</center></td>
</tr>
<tr>
    <td><strong><center>Schaalbaarheids- en prestatiedoelen<center></strong></td>
    <td colspan="2"><center>Dezelfde als bij opslagaccounts voor algemeen gebruik</center></td>
</tr>
</tbody>
</table>

> [!NOTE]
> Blob Storage-accounts bieden ondersteuning voor dezelfde schaalbaarheids- en prestatiedoelen als opslagaccounts voor algemeen gebruik. Zie [Schaalbaarheids- en prestatiedoelen in Azure Storage](storage-scalability-targets.md) voor meer informatie.
> 
> 

## <a name="pricing-and-billing"></a>Prijzen en facturering
Blob Storage-accounts maken gebruik van een nieuw prijsmodel voor het opslaan van blobs op basis van de opslaglaag. Als u een Blob Storage-account gebruikt, zijn de volgende factureringsvoorwaarden van toepassing:

* **Opslagkosten**: de kosten voor het opslaan van gegevens hangen niet alleen af van de hoeveelheid opgeslagen gegevens, maar ook van de gebruikte opslaglaag. De kosten per GB voor de opslaglaag voor 'cool' blobs zijn lager dan die voor de opslaglaag voor 'hot' blobs.
* **Kosten voor gegevenstoegang**: voor gegevens in de opslaglaag voor 'cool' blobs worden kosten in rekening gebracht per GB aan gegevenstoegang voor lees- en schrijfbewerkingen.
* **Transactiekosten**: voor beide lagen worden kosten in rekening gebracht per transactie. De kosten per transactie zijn voor de opslaglaag voor 'cool' blobs echter hoger dan voor de opslaglaag voor 'hot' blobs.
* **Kosten voor gegevensoverdracht met geo-replicatie**: dit is alleen van toepassing op accounts waarvoor geo-replicatie is geconfigureerd, inclusief GRS en RA-GRS. Kosten voor gegevensoverdracht met geo-replicatie worden in rekening gebracht per GB.
* **Kosten voor uitgaande gegevensoverdracht**: uitgaande gegevensoverdracht (gegevens die buiten een Azure-regio worden overgedragen) worden gefactureerd voor bandbreedtegebruik per GB, net zoals bij opslagaccounts voor algemeen gebruik.
* **De opslaglaag wijzigen**: als u de opslaglaag wijzigt van 'cool' naar 'hot', worden voor elke overgang kosten in rekening gebracht die overeenkomen met de kosten voor het lezen van alle bestaande gegevens in het opslagaccount. Het wijzigen van de opslaglaag van 'hot' naar 'cool' is echter gratis.

> [!NOTE]
> De kosten voor het wijzigen van de opslaglaag van 'cool' naar 'hot' zijn tot 30 juni 2016 niet van toepassing. Dit is om gebruikers in staat te stellen de nieuwe opslaglagen uit te proberen en de functionaliteit na het op de markt brengen van het product te valideren. Vanaf 1 juli 2016 zijn er kosten van toepassing op alle overgangen van Cool naar Hot. Ga naar de pagina [Azure Storage-prijzen](https://azure.microsoft.com/pricing/details/storage/) voor meer informatie over het prijsmodel voor Blob Storage-accounts. Ga naar de pagina [Prijsinformatie voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) voor meer informatie over de kosten voor uitgaande gegevensoverdracht.
> 
> 

## <a name="quick-start"></a>Snel starten
In deze sectie worden de volgende scenario‘s toegelicht, waarbij gebruik wordt gemaakt van Azure Portal:

* Het maken van een Blob Storage-account.
* Het beheren van een Blob Storage-account.

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken
#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Een Blob Storage-account maken met behulp van Azure Portal
1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub achtereenvolgens **Nieuw** > **Gegevens en opslag** > **Opslagaccount**.
3. Voer een naam in voor het opslagaccount.
   
    Deze naam moet uniek zijn. De naam wordt gebruikt als onderdeel van de URL die wordt gebruikt voor toegang tot de objecten in het opslagaccount.  
4. Selecteer **Resource Manager** als het implementatiemodel.
   
    Gelaagde opslag kan alleen worden gebruikt met Resource Manager-opslagaccounts. Dit is het aanbevolen implementatiemodel voor nieuwe resources. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
5. Selecteer **Blob Storage** in de vervolgkeuzelijst Soort account.
   
    Hier selecteert u het type opslagaccount. Gelaagde opslag is niet beschikbaar in de algemene opslag. Dit is alleen beschikbaar in het type account voor Blob Storage.     
   
    Houd er rekening mee dat, wanneer u deze optie selecteert, de prestatielaag wordt ingesteld op Standaard. Gelaagde opslag is niet beschikbaar bij de Premium-prestatielaag.
6. Selecteer de replicatieoptie voor het opslagaccount: **LRS**, **GRS** of **RA-GRS**. **RA-GRS** is de standaardinstelling.
   
    LRS = locally redundant storage (lokaal redundante opslag), GRS = geo-redundant storage (geografisch redundante opslag) (2 regio's), RA-GRS = read-access geo-redundant storage (geografisch redundante opslag met leestoegang) (2 regio's met leestoegang tot de tweede).
   
    Zie [Azure Storage-replicatie](storage-redundancy.md) voor meer informatie over Azure Storage-replicatieopties.
7. Selecteer de juiste opslaglaag voor wat u nodig hebt: stel de **Toegangslaag** in op **'Cool'** of **'Hot'**. **Hot** is de standaardinstelling.
8. Selecteer het abonnement waarin u het nieuwe opslagaccount wilt maken.
9. Geef een nieuwe resourcegroep op of selecteer een bestaande resourcegroep. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie over resourcegroepen.
10. Selecteer de regio voor uw opslagaccount.
11. Klik op **Maken** om het opslagaccount te maken.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>De opslaglaag voor een Blob Storage-account wijzigen via Azure Portal
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar uw opslagaccount, selecteer Alle resources en selecteer vervolgens uw opslagaccount.
3. Klik in de Instellingen-blade op **Configuratie** om de accountconfiguratie te bekijken en/of te wijzigen.
4. Selecteer de juiste opslaglaag voor wat u nodig hebt: stel de **Toegangslaag** in op **'Cool'** of **'Hot'**.
5. Klik op Opslaan boven aan de blade.

> [!NOTE]
> Aan het wijzigen van de opslaglaag kunnen extra kosten zijn verbonden. Zie de sectie [Prijzen en facturering](storage-blob-storage-tiers.md#pricing-and-billing) voor meer informatie.
> 
> 

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Evalueren en migreren naar Blob Storage-accounts
Het doel van deze sectie is om gebruikers op soepele wijze te helpen migreren naar het gebruiken van Blob Storage-accounts. Er zijn twee scenario's voor gebruikers:

* U beschikt over een bestaand algemene opslagaccount en wilt een wijziging evalueren in een Blob Storage-account met de juiste opslaglaag.
* U hebt besloten om een Blob Storage-account te gebruiken of u hebt er al een, en u wilt beoordelen of een opslaglaag voor 'hot' blobs of een opslaglaag voor 'cool' blobs moet gebruiken.

In beide gevallen moeten eerst de kosten worden geschat van het opslaan en openen van uw gegevens die zijn opgeslagen in een Blob Storage-account en dient u deze te vergelijken met uw huidige kosten.

### <a name="evaluating-blob-storage-account-tiers"></a>Lagen voor Blob Storage-accounts evalueren
Voor het maken van een schatting van de kosten voor het opslaan en openen van gegevens die zijn opgeslagen in een Blob Storage-account, moet u uw bestaande gebruikspatroon evalueren of een schatting maken van het verwachte gebruikspatroon. Doorgaans zijn de volgende gegevens hiervoor van belang:

* Wat is het gebruik van de opslag? Hoeveel gegevens worden er opgeslagen en hoe wijzigt dit maandelijks?
* Wat is het toegangspatroon voor de opslag? Op hoeveel gegevens in het account worden er lees- en/of schrijfbewerkingen uitgevoerd (inclusief nieuwe gegevens)? Hoeveel transacties worden gebruikt voor toegang tot gegevens? En wat voor soort transacties zijn dit?

#### <a name="monitoring-existing-storage-accounts"></a>Bewaking van bestaande opslagaccounts
Voor het bewaken van uw bestaande opslagaccounts en het verzamelen van deze gegevens, kunt u gebruikmaken van Azure Storage Analytics dat logboekregistratie uitvoert en metrische gegevens biedt voor een opslagaccount.
Storage Analytics kan metrische gegevens opslaan die samengevoegde transactiestatistieken en capaciteitsgegevens bevat over Blob Storage-serviceaanvragen voor algemene opslagaccounts en Blob Storage-accounts.
Deze gegevens worden opgeslagen in bekende tabellen in hetzelfde opslagaccount.

Raadpleeg voor meer informatie [Informatie over metrische gegevens in Storage Analytics](https://msdn.microsoft.com/library/azure/hh343258.aspx) en [Tabelschema van metrische gegevens in Storage Analytics](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> Blob Storage-accounts geven het service-eindpunt van de tabel alleen weer voor het opslaan en openen van de metrische gegevens voor dat account.
> 
> 

Voor het controleren van het opslagverbruik voor de Blob Storage-service moet u de metrische gegevens over capaciteit inschakelen.
Als dit is ingeschakeld, worden de capaciteitsgegevens van een Blob Storage-serviceaccount dagelijks geregistreerd en worden deze geregistreerd als een tabelvermelding die naar de *$MetricsCapacityBlob*-tabel binnen hetzelfde opslagaccount wordt geschreven.

Voor het controleren van het gegevenstoegangspatroon voor de Blob Storage-service, moet u de metrische gegevens die per uur worden verzameld voor de transactie inschakelen op een API-niveau.
Als dit is ingeschakeld, worden er elk uur per-API-transacties verzameld en geregistreerd als een tabelvermelding die is naar de *$MetricsHourPrimaryTransactionsBlob*-tabel binnen hetzelfde opslagaccount wordt geschreven. De *$MetricsHourSecondaryTransactionsBlob*-tabel registreert de transacties naar het secundaire eindpunt in het geval van RA-GRS-opslagaccounts.

> [!NOTE]
> Als u een algemeen opslagaccount hebt waarin u pagina-blobs en virtuele-machineschijven hebt opgeslagen naast blok- en toevoegblobgegevens, is dit schattingsproces niet van toepassing. Dit komt omdat er geen enkele manier is om onderscheid te maken tussen metrische gegevens voor capaciteit en voor transactie op basis van het type blob voor alleen blok- en toevoeg-blobs die kunnen worden gemigreerd naar een Blob Storage-account.
> 
> 

Als u een goede schatting wilt maken van uw gegevensverbruik en toegangspatroon, raden wij u aan voor de metrische gegevens een retentieperiode te kiezen die een goede afspiegeling is van uw normale gebruik en dat als uitgangspunt te nemen.
Een optie is de metrische gegevens 7 dagen te bewaren en de gegevens elke week te verzamelen en aan het einde van de maand te analyseren.
Een andere optie is de metrische gegevens van de afgelopen 30 dagen te bewaren en deze gegevens aan het einde van de periode van de 30 dagen te verzamelen en analyseren.

Voor meer informatie over het inschakelen, verzamelen en weergeven van metrische gegevens, raadpleegt u [Metrische gegevens voor Azure Storage en metrische weergavegegevens inschakelen](storage-enable-and-view-metrics.md).

> [!NOTE]
> Het opslaan, openen en downloaden van analytische gegevens wordt op dezelfde manier in rekening gebracht als normale gebruikersgegevens.
> 
> 

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Metrische gegevens over het gebruik inzetten voor het schatten van de kosten
##### <a name="storage-costs"></a>Opslagkosten
De meest recente vermelding in de metrische gegevenstabel voor capaciteit *$MetricsCapacityBlob* met de rijsleutel *'data'* toont de opslagcapaciteit die wordt gebruikt door gebruikersgegevens.
De meest recente vermelding in de metrische gegevenstabel voor capaciteit *$MetricsCapacityBlob* met de rijsleutel *'analytics'* toont de opslagcapaciteit die wordt gebruikt door de analyselogboeken.

Deze totale capaciteit die wordt verbruikt door zowel gebruikersgegevens en analyselogboeken (indien ingeschakeld) kunnen vervolgens worden gebruikt om de opslagkosten te schatten voor gegevens in het opslagaccount.
Deze manier kan ook worden gebruikt voor het schatten van de opslagkosten voor blok- en toevoegblobs in algemene opslagaccounts.

##### <a name="transaction-costs"></a>Transactiekosten
Het totaal van *'TotalBillableRequests'*, in alle items voor een API in de metrische gegevenstabel voor transactie, geeft het totale aantal transacties voor die bepaalde API weer. *bijvoorbeeld*, het totale aantal *'GetBlob'*-transacties in een bepaalde periode kunnen worden berekend door het totaal te nemen van het totale aantal factureerbare aanvragen voor alle items met de rijsleutel *'user;GetBlob'*.

Voor het schatten van de transactiekosten van Blob Storage-accounts moet u de transacties in drie groepen opdelen omdat ze verschillend zijn geprijsd.

* Schrijftransacties zoals *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* en *'CopyBlob'*.
* Verwijdertransacties zoals *'DeleteBlob'* en *'DeleteContainer'*.
* Alle andere transacties.

Voor het schatten van de transactiekosten voor algemene opslagaccounts moet u alle transacties verzamelen ongeacht de bewerking/API.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Gegevenstoegang en overdrachtskosten voor geo-replicatiegegevens
Opslaganalyse biedt geen informatie over de hoeveelheid gegevens die zijn gelezen en geschreven van en naar een opslagaccount, maar deze hoeveelheid kan min of meer worden geschat door te kijken naar de metrische gegevenstabel voor transacties.
Het totaal van *'TotalIngress'* in alle items voor een API in de metrische gegevenstabel voor transacties, geeft de totale hoeveelheid inkomende gegevens in bytes voor die bepaalde API weer.
Op dezelfde manier geeft het totaal van *'TotalEgress'* de totale hoeveelheid uitgaande gegevens in bytes weer.

Voor het schatten van de kosten voor het openen van gegevens in Blob Storage-accounts moet u de transacties in twee groepen opdelen.

* De hoeveelheid gegevens die is opgehaald van het opslagaccount, kan worden geschat door te kijken naar het totaal van *'TotalEgress'* voor met name de *'GetBlob'*- en *'CopyBlob'*-bewerkingen.
* De hoeveelheid gegevens die wordt geschreven naar het opslagaccount kan worden geschat door te kijken naar het totaal van *'TotalIngress'* voor met name de *'PutBlob'*-, *'PutBlock'*-, *'CopyBlob'*- en *'AppendBlock'*-bewerkingen.

De overdrachtskosten van geo-replicatiegegevens voor Blob Storage-accounts kan ook worden berekend met behulp van de schatting voor de hoeveelheid gegevens die wordt geschreven in het geval van een GRS- of RA-GRS-opslagaccount.

> [!NOTE]
> Bekijk voor een uitgebreider voorbeeld over het berekenen van de kosten voor het gebruik van opslaglaag voor 'cool' blobs of de opslaglaag voor 'hot' blobs de veelgestelde vraag *'Wat zijn toegangslagen voor 'hot' en 'cool' blobs en hoe bepaal ik welke ik moet gebruiken?'* op de [pagina met prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
> 
> 

### <a name="migrating-existing-data"></a>Bestaande gegevens migreren
Een Blob Storage-account is speciaal bedoeld voor het opslaan van blok-blobs en toevoeg-blobs. Bestaande opslagaccounts voor algemeen gebruik, waarin u naast blobs ook tabellen, wachtrijen, bestanden en schijven kunt opslaan, kunnen niet worden geconverteerd naar Blob Storage-accounts. Als u gebruik wilt maken van de toegangslagen, maakt u nieuwe Blob Storage-accounts en migreert u de bestaande gegevens naar deze nieuwe accounts.
U kunt de volgende methoden gebruiken om bestaande gegevens vanaf on-premises opslagapparaten, van cloudopslagproviders van derden of vanuit bestaande opslagaccounts voor algemeen gebruik in Azure te migreren naar Blob Storage-accounts:

#### <a name="azcopy"></a>AzCopy
AzCopy is een Windows-opdrachtregelprogramma dat is  ontworpen voor het high-performance kopiëren van gegevens van en naar Azure Storage. U kunt AzCopy gebruiken om gegevens uit bestaande opslagaccounts voor algemeen gebruik te kopiëren naar een Blob Storage-account en om gegevens van on-premises opslagaccounts te uploaden naar een Blob Storage-account.

Zie [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md) voor meer informatie.

#### <a name="data-movement-library"></a>Bibliotheek voor gegevensverplaatsing
De Azure Storage-bibliotheek voor gegevensverplaatsing voor .NET is gebaseerd op het basisframework voor gegevensverplaatsing van AzCopy. De bibliotheek is ontworpen voor high-performance, betrouwbare en eenvoudige gegevensoverdracht vergelijkbaar met AzCopy. Hierdoor kunt u in uw toepassing op systeemeigen wijze maximaal profiteren van de functies die AzCopy biedt, zonder dat u externe exemplaren van AzCopy hoeft uit te voeren of te controleren.

Zie [Bibliotheek voor gegevensverplaatsing van Azure Storage voor .Net](https://github.com/Azure/azure-storage-net-data-movement) voor meer informatie.

#### <a name="rest-api-or-client-library"></a>REST-API of clientbibliotheek
U kunt een aangepaste toepassing maken om gegevens naar een Blob Storage-account te migreren met behulp van een van de Azure-clientbibliotheken of de REST API voor Azure Storage-services. Azure Storage biedt uitgebreide clientbibliotheken voor meerdere talen en platforms, zoals  .NET, Java, C++, Node.JS, PHP, Ruby en Python. De clientbibliotheken bieden geavanceerde mogelijkheden, zoals pogingslogica, logboekregistratie en parallelle uploads. U kunt ook rechtstreeks met de REST API ontwikkelen. Deze kan worden aangeroepen in elke taal waarin HTTP-/HTTPS-verzoeken kunnen worden gemaakt.

Zie [Aan de slag met Azure Blob Storage](storage-dotnet-how-to-use-blobs.md) voor meer informatie.

> [!NOTE]
> Blobs die aan de clientzijde zijn versleuteld, bevatten versleutelingsgerelateerde metagegevens die samen met de blob zijn opgeslagen. Het is absoluut essentieel dat de metagegevens in de blob (en dan met name de versleutelingsgerelateerde metagegevens) bij het kopiëren behouden blijven. Als u de blobs kopieert zonder deze metagegevens, kan de inhoud van de blob niet meer worden opgehaald. Zie [Azure Storage-versleuteling aan de clientzijde](storage-client-side-encryption.md) voor meer informatie over versleutelingsgerelateerde metagegevens.
> 
> 

## <a name="faqs"></a>Veelgestelde vragen
1. **Zijn de bestaande opslagaccounts nog steeds beschikbaar?**
   
    Ja, de bestaande opslagaccounts zijn nog steeds beschikbaar. De prijs en de functionaliteit hiervan zijn niet gewijzigd.  U kunt voor deze opslagaccounts echter geen opslaglagen kiezen. Dit zal in de toekomst niet veranderen.
2. **Waarom en wanneer is het een goed idee om Blob Storage-accounts te gebruiken?**
   
    Blob Storage-accounts zijn helemaal gericht op het opslaan van blobs en maken het introduceren van nieuwe blobfuncties mogelijk. Het is een goed idee om Blob Storage-accounts te gebruiken voor het opslaan van blobs, omdat er in de toekomst nieuwe mogelijkheden (bijvoorbeeld hiërarchische opslag en lagen) worden geïntroduceerd in dit accounttype. Op basis van uw bedrijfsvereisten bepaalt u echter helemaal zelf wanneer u uw gegevens migreert.
3. **Kan ik mijn bestaande opslagaccount converteren naar een Blob Storage-account?**
   
    Nee. Een Blob Storage-account is een ander soort opslagaccount. Daarom moet u dit account afzonderlijk maken en dient u de gegevens naar dit account te migreren zoals hierboven wordt uitgelegd.
4. **Kan ik objecten opslaan in beide opslaglagen van hetzelfde account?**
   
    Het kenmerk *'Toegangslaag'* dat de opslaglaag aangeeft, wordt op accountniveau ingesteld en is van toepassing op alle objecten in het account. U kunt het kenmerk toegangslaag niet instellen op objectniveau.
5. **Kan ik de opslaglaag van mijn Blob Storage-account wijzigen?**
   
    Ja. U kunt de opslaglaag in het opslagaccount wijzigen door het kenmerk *'Toegangslaag'* in te stellen op het opslagaccount. Als u de opslaglaag wijzigt, is dit van toepassing op alle objecten die in het account zijn opgeslagen. Als u de opslaglaag wijzigt van 'hot' naar 'cool', worden er geen kosten in rekening gebracht. Als u echter de toegangslaag wijzigt van 'cool' naar 'hot', worden er kosten berekend per GB voor het lezen van alle gegevens in het account.
6. **Hoe vaak kan ik de opslaglaag van mijn Blob Storage-account wijzigen?**
   
    Let op: hoewel er geen beperking bestaat voor het aantal malen dat u de opslaglaag kunt wijzigen, worden er voor het wijzigen van de opslaglaag van 'cool' naar 'hot' aanzienlijke kosten in rekening gebracht. We raden u daarom aan de opslaglaag niet te vaak te wijzigen.
7. **Gedragen blobs in de opslaglaag voor 'cool' blobs zich anders dan blobs in de opslaglaag voor 'hot' blobs?**
   
    Blobs in de opslaglaag voor 'hot' blobs hebben dezelfde latentie als blobs in opslagaccounts voor algemeen gebruik. Blobs in de opslaglaag voor 'cool' blobs hebben een gelijksoortige latentie (in milliseconden) als blobs in opslagaccounts voor algemeen gebruik.
   
    Blobs in de opslaglaag voor 'cool' blobs hebben een lagere SLA (Service Level Availability) dan blobs die zijn opgeslagen in de opslaglaag voor 'hot' blobs. Zie [SLA voor opslag](https://azure.microsoft.com/support/legal/sla/storage) voor meer informatie.
8. **Kan ik pagina-blobs en virtuele-machineschijven opslaan in Blob Storage-accounts?**
   
    Blob Storage-accounts ondersteunen alleen blok-blobs en toevoeg-blobs. Pagina-blobs worden niet ondersteund. Omdat virtuele machines van Azure gebruikmaken van pagina-blobs, kunnen virtuele-machineschijven niet worden opgeslagen in Blob Storage-accounts. Het is echter wel mogelijk om back-ups van de virtuele-machineschijven als blok-blobs op te slaan in een Blob Storage-account.
9. **Moet ik mijn bestaande toepassingen wijzigen om Blob Storage-accounts te kunnen gebruiken?**
   
    Blob Storage-accounts zijn voor 100 procent API-consistent met opslagaccounts voor algemeen gebruik voor blok- en toevoeg-blobs. Zolang uw toepassing gebruikmaakt van blok-blobs of toevoeg-blobs en u versie 2014-02-14 van de [REST API voor Storage Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) gebruikt, zal uw toepassing geen problemen ondervinden. Als u een oudere versie van het protocol gebruikt, moet u uw toepassing bijwerken voor gebruik van de nieuwe versie, zodat deze naadloos werkt met beide typen opslagaccounts. Over het algemeen is het bijna altijd het beste om de meest recente versie te gebruiken, ongeacht het type opslagaccount dat u gebruikt.
10. **Is de gebruikerservaring gewijzigd?**
    
    Blob Storage-accounts zijn vergelijkbaar met opslagaccounts voor algemeen gebruik voor het opslaan van blok- en toevoeg-blobs. Hierbij maken ze maximaal gebruik van de kracht van Azure Storage op het gebied van duurzaamheid, beschikbaarheid, schaalbaarheid, prestaties en beveiliging. Afgezien van de hierboven beschreven functionaliteit en beperkingen die specifiek zijn voor Blob Storage-accounts en de bijbehorende opslaglagen, verandert er voor u niets.

## <a name="next-steps"></a>Volgende stappen
### <a name="evaluate-blob-storage-accounts"></a>Blob Storage-accounts evalueren
[Beschikbaarheid van Blob Storage-accounts per regio controleren](https://azure.microsoft.com/regions/#services)

[Gebruik van de huidige opslagaccounts evalueren door metrische gegevens voor Azure Storage in te schakelen](storage-enable-and-view-metrics.md)

[Prijzen voor Blob Storage per regio controleren](https://azure.microsoft.com/pricing/details/storage/)

[Prijzen voor gegevensoverdracht controleren](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Blob Storage-accounts gebruiken
[Aan de slag met Azure Blob Storage](storage-dotnet-how-to-use-blobs.md)

[Gegevens verplaatsen naar en uit Azure Storage](storage-moving-data.md)

[Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md)

[Uw opslagaccounts bekijken en verkennen](http://storageexplorer.com/)




<!--HONumber=Dec16_HO1-->


