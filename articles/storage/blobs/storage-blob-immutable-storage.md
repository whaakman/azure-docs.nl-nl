---
title: Onveranderbare opslag voor Azure Storage-Blobs | Microsoft Docs
description: Azure Storage biedt ondersteuning voor blobopslag (object) waarmee gebruikers gegevens opslaan in een status bewaarinterval, niet kan worden gewijzigd voor een opgegeven interval WORM (Write Once, Read Many).
services: storage
author: xyh1
ms.service: storage
ms.topic: article
ms.date: 03/02/2019
ms.author: hux
ms.subservice: blobs
ms.openlocfilehash: 76d2794d45e9cd73ae8faf203ba29c4dbba64eae
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58004624"
---
# <a name="store-business-critical-data-in-azure-blob-storage"></a>Store kritieke zakelijke gegevens in Azure Blob-opslag

Onveranderbare opslag voor Azure Blob-opslag kan gebruikers kritieke zakelijke gegevensobjecten opslaan in een status WORM (Write Once, Read Many). Deze status maakt de gegevens niet kan worden gewist en niet kan worden gewijzigd voor een gebruiker opgegeven interval. BLOB-objecten kunnen worden gemaakt en lezen, maar niet gewijzigd of verwijderd, voor de duur van de retentie-interval. Onveranderbare opslag is voor algemeen gebruik v2 en Blob Storage-accounts in alle Azure-regio's ingeschakeld.

## <a name="overview"></a>Overzicht

Onveranderbare storage kunt organisatie in de gezondheidszorg, financiële instellingen en verwante bedrijfstakken--name handelaar organisaties--voor het veilig opslaan van gegevens. Het kan ook worden gebruikt in elk scenario om kritieke gegevens op basis van de wijziging of verwijdering te beveiligen. 

Typische toepassingen zijn onder andere:

- **Naleving van regelgeving**: Onveranderbare opslag voor Azure Blob-opslag helpt organisaties adres SEC 17a-4(f), CFTC 1.31(d) FINRA en andere voorschriften. Een technisch document door Cohasset koppelt wordt uitgelegd hoe onveranderbare opslag adressen deze reglementaire voorschriften is gedownload via de [Microsoft Service Trust Portal](https://aka.ms/AzureWormStorage). De [Azure Trust Center](https://www.microsoft.com/trustcenter/compliance/compliance-overview) bevat gedetailleerde informatie over onze nalevingscertificeringen.

- **Beveiligen van documenten vasthouden**: Onveranderbare opslag voor Azure Blob-opslag zorgt ervoor dat gegevens kan niet worden gewijzigd of verwijderd door een gebruiker, met inbegrip van gebruikers met beheerdersrechten account.

- **Juridische bewaring**: Onveranderbare opslag voor Azure Blob-opslag kan gebruikers voor het opslaan van gevoelige informatie die essentieel zijn voor gebruik met rechtszaak of bedrijven in de status van een fraudebestendig de gewenste duur totdat de blokkering is verwijderd. Deze functie is niet beperkt tot juridische use-cases, maar kan ook worden beschouwd als een geblokkeerd op basis van gebeurtenissen of een vergrendeling enterprise waar de noodzaak om gegevens op basis van gebeurtenistriggers of bedrijfsbeleid te beveiligen vereist is.

Onveranderbare storage ondersteunt het volgende:

- **[Ondersteuning voor op tijd gebaseerd bewaren groepsbeleid](#time-based-retention)**: Gebruikers kunnen beleid voor het opslaan van gegevens voor een opgegeven interval instellen. Wanneer een op tijd gebaseerd bewaarbeleid is kunnen ingesteld, blobs worden gemaakt en lezen, maar niet gewijzigd of verwijderd. Nadat de bewaarperiode is verlopen, blobs kunnen worden verwijderd, maar niet overschreven.

- **[Ondersteuning van Groepsbeleid juridisch](#legal-holds)**: Als de retentie-interval dat is niet bekend zijn, kunnen gebruikers juridische bewaring voor het opslaan van gegevens immutably totdat de juridische bewaring is uitgeschakeld ingesteld.  Wanneer een beleid voor juridische bewaring is ingesteld, kunnen blobs worden gemaakt en lezen, maar niet gewijzigd of verwijderd. Elke juridische bewaring is gekoppeld aan een gebruiker gedefinieerde alfanumerieke code (zoals een case-ID, gebeurtenisnaam van de, enzovoort) die wordt gebruikt als een tekenreeks-id. 

- **Ondersteuning voor alle lagen blob**: WORM beleidsregels zijn onafhankelijk van de Azure Blob storage-laag en zijn van toepassing op alle lagen: hot, cool en archive Storage. Gebruikers kunnen gegevens naar de prijscategorie meest kosten geoptimaliseerd voor hun workloads overgang behoud van gegevens onveranderbaarheid.

- **Configuratie van de container op serverniveau**: Gebruikers kunnen configureren voor op tijd gebaseerd bewaarbeleid en juridisch tags op het niveau van de container. Met behulp van eenvoudige container-niveau instellingen, kunnen gebruikers maken en op tijd gebaseerd bewaarbeleid vergrendelen, uit te breiden retentie intervallen, instellen en schakel juridische bewaring en meer. Dit beleid van toepassing op alle blobs in de container, bestaande en nieuwe.

- **Ondersteuning voor logboekregistratie controleren**: Elke container bevat een logboek. Het bevat maximaal vijf tijd gebaseerd bewaren-opdrachten voor het beleid voor het vergrendelde op tijd gebaseerd bewaren, met een maximum van drie logboeken voor extensies van retentie-interval. Het logboek bevat voor het bewaren op basis van tijd, de gebruikers-ID, het opdrachttype, de tijdstempels en de retentie-interval. Het logboek bevat de gebruikers-ID, het opdrachttype, de tijdstempels voor juridische bewaring en juridisch tags. Dit logboek worden bewaard gedurende de levensduur van de container, in overeenstemming met de seconde 17a-4(f)-regelgeving. De [Azure Activity Log](../../azure-monitor/platform/activity-logs-overview.md) ziet u een meer uitgebreide logboek van alle activiteiten van het besturingselement vlak; tijdens het inschakelen van [diagnostische logboeken van Azure](../../azure-monitor/platform/diagnostic-logs-overview.md) blijven behouden en gegevens vlak bewerkingen weergegeven. Het is de verantwoordelijkheid van de gebruiker voor het opslaan van deze logboeken permanent mogelijk zijn vereist voor wettelijke of andere doeleinden.

## <a name="how-it-works"></a>Hoe werkt het?

Onveranderbare opslag voor Azure Blob storage ondersteunt twee typen onveranderbare beleidsregels of WORM: tijd gebaseerd bewaren en juridische bewaring. Wanneer een op tijd gebaseerd bewaarbeleid of een juridische bewaring wordt toegepast op een container, wordt de status van alle bestaande blobs verplaatsen naar een onveranderbare WORM-status in minder dan 30 seconden. Alle nieuwe blobs die zijn geüpload naar deze container wordt ook verplaatsen naar de status van de onveranderbare. Bewerkingen voor bestaande en nieuwe objecten in de onveranderbare container mogen niet als alle blobs zijn verplaatst naar de onveranderbare staat, wordt de onveranderbare beleid bevestigd en alle overschrijven of verwijderen.

### <a name="time-based-retention"></a>Tijd gebaseerd bewaren

> [!IMPORTANT]
> Een op tijd gebaseerd bewaarbeleid moet *vergrendeld* voor de blob in een onveranderbare (schrijven en verwijderen die zijn beveiligd) staat voor seconde 17a-4(f) en andere voorschriften. Het is raadzaam dat u het beleid in een redelijk tijdsbestek, meestal binnen 24 uur vergrendelen. U kunt beter geen de *ontgrendeld* staat voor enig doel dan op korte termijn functie proefversies.

Wanneer een op tijd gebaseerd bewaarbeleid wordt toegepast op een container, alle blobs in de container blijft in de onveranderbare staat voor de duur van de *effectieve* bewaarperiode. De daadwerkelijke bewaarperiode voor bestaande blobs is gelijk aan het verschil tussen de tijd van de blob-wijziging en de gebruiker opgegeven retentie-interval.

Voor nieuwe blobs is de effectieve retentieperiode gelijk aan de door de gebruiker opgegeven retentieperiode. Omdat gebruikers het bewaarinterval uitbreiden kunnen, onveranderbare storage maakt gebruik van de meest recente waarde van de gebruiker opgegeven bewaarinterval voor het berekenen van de daadwerkelijke bewaarperiode.

> [!TIP]
> **Voorbeeld:** Een gebruiker maakt een op tijd gebaseerd bewaarbeleid met een retentie-interval van vijf jaar.
>
> De bestaande blob in die container, _testblob1_, één jaar geleden is gemaakt. De daadwerkelijke bewaarperiode voor _testblob1_ vier jaar is.
>
> Een nieuwe blob _testblob2_, nu is geüpload naar de container. De daadwerkelijke bewaarperiode voor deze nieuwe blob is vijf jaar.

### <a name="legal-holds"></a>Juridische bewaring

Als u een juridische bewaring instelt, worden alle bestaande en nieuwe blobs in de status van de onveranderbare blijven totdat de juridische bewaring is uitgeschakeld. Zie voor meer informatie over het instellen en schakel juridische bewaring de [aan de slag](#getting-started) sectie.

Een container kan zowel een juridische bewaring en een op tijd gebaseerd bewaarbeleid hebben op hetzelfde moment. Alle blobs in die container blijven in de status van de onveranderbare, totdat alle juridische bewaring zijn uitgeschakeld, zelfs als de daadwerkelijke bewaarperiode is verlopen. Daarentegen blijft een blob in een onveranderbare status totdat de daadwerkelijke bewaarperiode is verlopen, zelfs als alle juridische bewaring zijn gewist.

De volgende tabel bevat de typen van blob-bewerkingen die zijn uitgeschakeld voor de verschillende onveranderbare scenario's. Zie voor meer informatie de [Azure Blob Service-API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) documentatie.

|Scenario  |BLOB-status  |BLOB-bewerkingen niet toegestaan  |
|---------|---------|---------|
|Effectieve retentieperiode voor de blob is nog niet verlopen en/of wettelijke bewaring is ingesteld     |Onveranderbaar: beveiligd tegen verwijderen en schrijven         | Blob plaatsen<sup>1</sup>, plaatsen blok<sup>1</sup>, Put, lijst met geblokkeerde websites<sup>1</sup>, verwijderen van de metagegevens van de Blob-Container, Blob-verwijderen, Set, pagina, stelt u eigenschappen van de Blob, momentopname maken van Blob, incrementeel kopiëren van de Blob, Blok toevoegen         |
|Effectieve retentieperiode voor blob is verlopen     |Alleen beveiligd tegen schrijven (verwijderen is toegestaan)         |Blob plaatsen<sup>1</sup>, plaatsen blok<sup>1</sup>, Put, lijst met geblokkeerde websites<sup>1</sup>, instellen van de metagegevens van de Blob, pagina, instellen van Blob-eigenschappen, Blob-momentopname, incrementeel kopiëren van de Blob, blok toevoegen         |
|Alle juridische bevat uitgeschakeld en niet op tijd gebaseerd bewaarbeleid is ingesteld op de container     |Veranderlijk         |Geen         |
|Er is geen WORM-beleid is gemaakt (op basis van tijd bewaren of juridische bewaring)     |Veranderlijk         |Geen         |

<sup>1</sup> met deze toepassing kan deze bewerkingen te maken van een nieuwe blob eenmaal. Alle volgende overschrijven bewerkingen op een bestaand blobpad in een onveranderbare container zijn niet toegestaan.

## <a name="pricing"></a>Prijzen

Er is geen extra kosten voor het gebruik van deze functie. De prijs is onveranderbaar gegevens op dezelfde manier als normale, veranderlijke gegevens. Informatie over prijzen voor Azure Blob Storage, Zie de [Azure Storage-pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="getting-started"></a>Aan de slag

De meest recente versies van de [Azure-portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), en [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) onveranderbare storage ondersteuning voor Azure Blob-opslag. [Bibliotheek clientondersteuning](#client-libraries) is ook beschikbaar.

> [!NOTE]
>
> Onveranderbare opslag is alleen beschikbaar voor algemeen gebruik v2 en Blob Storage-Accounts. Deze account moet worden beheerd via [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Zie voor meer informatie over het bijwerken van een bestaand opslagaccount voor algemeen gebruik v1 [een storage-account upgraden](../common/storage-account-upgrade.md).

### <a name="azure-portal"></a>Azure Portal

1. Maak een nieuwe container of selecteer een bestaande container om de blobs op te slaan die u in onveranderbare toestand wilt bewaren.
 De container moet zich in een GPv2- of blob storage-account.
2. Selecteer **toegangsbeleid** in de containerinstellingen van de. Selecteer vervolgens **+ beleid toevoegen** onder **onveranderbare blob storage**.

    ![Containerinstellingen in de portal](media/storage-blob-immutable-storage/portal-image-1.png)

3. Als u tijd gebaseerd bewaren, schakelt **tijd gebaseerd bewaren** uit de vervolgkeuzelijst.

    !["Tijd gebaseerd bewaren' onder 'Beleidstype' geselecteerd](media/storage-blob-immutable-storage/portal-image-2.png)

4. Voer het retentie-interval in dagen (acceptabele waarden van 1-146000 dagen).

    ![Vak "Bewaarperiode update naar"](media/storage-blob-immutable-storage/portal-image-5-retention-interval.png)

    De initiële status van het beleid is ontgrendeld zodat u kunt de functie testen en wijzigingen aanbrengen aan het beleid voordat u het vergrendelen. Vergrendeling van het beleid is van essentieel belang voor u voldoet aan regelgeving zoals SEC 17 bis-4.

5. Vergrendelen van het beleid. Met de rechtermuisknop op het weglatingsteken (**...** ), en de volgende menu wordt weergegeven met aanvullende acties:

    !['Vergrendelen beleid' in het menu](media/storage-blob-immutable-storage/portal-image-4-lock-policy.png)

    Selecteer **beleid vergrendelen**. Het beleid nu is vergrendeld en kan niet worden verwijderd, kunt u alleen de extensies van de retentie-interval dat is toegestaan.

6. Als u juridische bewaring, schakelt **+ beleid toevoegen**. Selecteer **juridische bewaring** uit de vervolgkeuzelijst.

    !["Juridisch" in het menu onder "Beleidstype"](media/storage-blob-immutable-storage/portal-image-legal-hold-selection-7.png)

7. Maak een juridische bewaring met een of meer labels.

    !['Naam van de tag' vak onder het beleidstype](media/storage-blob-immutable-storage/portal-image-set-legal-hold-tags.png)

8. Schakel een juridische bewaring, hoeft u de toegepaste juridische bewaring id-tag te verwijderen.

### <a name="azure-cli"></a>Azure-CLI

De functie is opgenomen in de volgende opdrachtgroepen: `az storage container immutability-policy` en `az storage container legal-hold`. Voer `-h` op deze om te zien van de opdrachten.

### <a name="powershell"></a>PowerShell

De preview-module Az.Storage ondersteunt onveranderbare-opslag.  Schakel de functie door de volgende stappen uit:

1. Zorg ervoor dat u de nieuwste versie van PowerShellGet geïnstalleerd hebt: `Install-Module PowerShellGet –Repository PSGallery –Force`.
2. Verwijder eventuele vorige installatie van Azure PowerShell.
3. Azure PowerShell installeren: `Install-Module Az –Repository PSGallery –AllowClobber`.
4. De preview-versie van de Azure PowerShell Storage-module installeren: `Install-Module Az.Storage -AllowPrerelease -Repository PSGallery -AllowClobber`

De [voorbeeld PowerShell-code](#sample-powershell-code) verderop in dit artikel ziet u het gebruik van functies.

## <a name="client-libraries"></a>Clientbibliotheken

De volgende clientbibliotheken bieden ondersteuning voor onveranderbare opslag voor Azure Blob-opslag:

- [.NET-clientbibliotheek versie 7.2.0-preview en hoger](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/7.2.0-preview)
- [Node.js-clientbibliotheek versie 4.0.0 en hoger](https://www.npmjs.com/package/azure-arm-storage)
- [Python-clientbibliotheek versie 2.0.0 Release Candidate 2 en hoger](https://pypi.org/project/azure-mgmt-storage/2.0.0rc2/)
- [Java-clientbibliotheek](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/storage/resource-manager/Microsoft.Storage/preview/2018-03-01-preview)

## <a name="supported-values"></a>Ondersteunde waarden

- De minimale bewaarinterval is één dag. Het maximum is 146,000 dagen (400 jaar).
- Voor een opslagaccount is het maximum aantal containers met vergrendeld beleid voor onveranderbare 1000.
- Voor een opslagaccount is het maximum aantal containers met een instelling voor juridische bewaring 1000.
- Voor een container is het maximum aantal tags voor juridische bewaring 10.
- De maximale lengte van een juridische bewaring-tag is 23 alfanumerieke tekens. De minimale lengte is drie tekens.
- Voor een container is het maximum aantal toegestane retentie-interval uitbreidingen voor vergrendelde onveranderbare beleid drie.
- Voor een container met een vergrendelde onveranderbare beleid houdt een maximum van vijf logboeken van het beleid op basis van tijd bewaren en maximaal 10 juridische beleid logboeken worden bewaard voor de duur van de container.

## <a name="faq"></a>Veelgestelde vragen

**Kunt u de documentatie van de naleving van de WORM geven?**

Ja. Naleving van het document, Microsoft een toonaangevende onafhankelijke beoordeling-bedrijf dat is gespecialiseerd in records beheers- en bestuur Cohasset Associates, Azure onveranderbare Blob Storage en de compatibiliteit met specifieke vereisten evalueren behouden op de financiële dienstverleningssector. Cohasset gevalideerd dat Azure onveranderbare Blob Storage, wanneer gebruikt voor het behouden van Blobs op basis van tijd in een status WORM, voldoet aan de relevante opslagvereisten CFTC regel 1.31(c)-(d) FINRA regel 4511 en SEC regel 17 bis-4. Microsoft gericht deze reeks regels, zoals zij de meest uitgebreide richtlijnen voor records bewaartermijn voor financiële instellingen vertegenwoordigen. Het rapport Cohasset is beschikbaar in de [Microsoft Service Trust Center](https://aka.ms/AzureWormStorage).

**Is de functie voor alleen blok-blobs, of voor pagina- en toevoeg-blobs ook toepassing?**

Onveranderbare opslag kan worden gebruikt met elk blobtype, maar het beste voornamelijk voor blok-blobs te gebruiken. In tegenstelling tot blok-blobs, pagina-blobs en toevoeg-blobs moeten worden gemaakt buiten de container van een WORM, en vervolgens gekopieerd. Nadat u deze blobs naar een container WORM niet verder kopiëren *voegt* aan een toevoeg-blob of wijzigingen aan een pagina-blob zijn toegestaan.

**Moet ik altijd een nieuw opslagaccount maken om deze functie te gebruiken?**

U kunt onveranderbare storage gebruiken met een bestaande of nieuwe algemeen gebruik v2- of Blob Storage-accounts. Deze functie is bedoeld voor gebruik met blok-blobs in Blob-opslag- en GPv2-accounts.

**Kan ik een juridische bewaring en de tijd gebaseerd bewaarbeleid toepassen?**

Een container kan zowel een juridische bewaring en een op tijd gebaseerd bewaarbeleid hebben op hetzelfde moment. Alle blobs in die container blijven in de status van de onveranderbare, totdat alle juridische bewaring zijn uitgeschakeld, zelfs als de daadwerkelijke bewaarperiode is verlopen. Daarentegen blijft een blob in een onveranderbare status totdat de daadwerkelijke bewaarperiode is verlopen, zelfs als alle juridische bewaring zijn gewist.

**Beleid voor juridische bewaring zijn alleen bedoeld voor gerechtelijke procedure of zijn er andere scenario's voor gebruiken?**

Nee, juridische houdt, is alleen de algemene term die wordt gebruikt voor een bewaarbeleid voor niet-op basis van tijd. Het hoeft niet alleen worden gebruikt voor rechtszaak procedures die betrekking hebben. Juridische bewaring beleidsregels zijn handig voor het uitschakelen van overschrijven en verwijderen voor het beveiligen van belangrijke enterprise WORM gegevens, waar de bewaarperiode is onbekend. U kunt deze als een enterprise-beleid op uw WORM bedrijfskritieke beveiligen of gebruiken als een faseringsbeleid voordat een trigger voor de aangepaste gebeurtenis het gebruik van een op tijd gebaseerd bewaarbeleid vereist. 

**Wat gebeurt er als ik een container probeer te verwijderen die een *vergrendeld* retentiebeleid op basis van tijd of een juridische bewaring heeft?**

De bewerking verwijderen Container mislukken als er ten minste één blob bestaat met een vergrendelde op tijd gebaseerd bewaarbeleid of een juridische bewaring. De Container verwijderen-bewerking slaagt alleen als er geen blob met een actieve bewaarinterval bestaat en er geen juridische bewaring zijn. Voordat u de container kunt verwijderen, moet u de blobs verwijderen.

**Wat gebeurt er als ik een opslagaccount probeer te verwijderen met een WORM-container die een *vergrendeld* retentiebeleid op basis van tijd of een juridische bewaring heeft?**

De verwijdering van het opslagaccount mislukt als er minimaal één WORM-container is die een juridische bewaring of een blob met een actieve retentieperiode bevat.  Voordat u de storage-account kunt verwijderen, moet u alle WORM containers verwijderen. Zie voor informatie over het verwijderen van de beveiligingscontainer, de voorgaande vraag.

**Kan ik de gegevens verplaatsen tussen verschillende blob-lagen (dynamische toegang, statische toegang, archieftoegang) als de blob de status onveranderbaar heeft?**

Ja, kunt u de Blob-laag instellen-opdracht gebruiken om gegevens te verplaatsen voor de blob-lagen en tegelijk de bedrijfsgegevens in de status van de compatibele onveranderbare. Onveranderbare opslag wordt ondersteund op hot, cool en archive blob-lagen.

**Wat gebeurt er als ik niet meer betaal en mijn retentieperiode niet is verlopen?**

In het geval van niet-betaling geldt voor het bewaren van normale gegevens zoals bepaald in de voorwaarden en bepalingen van uw contract met Microsoft.

**Biedt u ook een evaluatie- of respijtperiode voor de zojuist beschreven functie?**

Ja. Wanneer een op tijd gebaseerd bewaarbeleid wordt gemaakt, is in een *ontgrendeld* staat. In deze status kunt u gewenste wijzigingen aanbrengen in de retentie-interval, zoals vergroten of verkleinen en zelfs verwijderen van het beleid. Nadat het beleid is vergrendeld, blijft het vergrendeld totdat het bewaarinterval is verlopen. Dit vergrendeld beleid voorkomt dat verwijderen en wijzigen van de retentie-interval. Wordt aangeraden dat u de *ontgrendeld* staat alleen ter evaluatie en een vergrendeling van het beleid binnen een periode van 24 uur. Deze procedures kunnen u voldoen aan de seconde 17a-4(f) en andere voorschriften.

**Kan ik de functie voor voorlopig verwijderen naast onveranderbare blob beleidsregels gebruiken?**

Ja. [Voorlopig verwijderen voor Azure Blob storage](storage-blob-soft-delete.md) geldt voor alle containers in een storage-account, onafhankelijk van een juridische bewaring of een op tijd gebaseerd bewaarbeleid. Het is raadzaam om voorlopig verwijderen voor extra beveiliging inschakelen voordat een onveranderbare WORM-beleidsregels worden toegepast en bevestigd. 

**Is de functie beschikbaar in landelijke en overheidsclouds?**

Onveranderbare storage is beschikbaar in openbare Azure-, China en overheid regio's. Als onveranderbare opslag niet beschikbaar in uw regio is, neem contact op met ondersteuning voor e-mailbericht azurestoragefeedback@microsoft.com.

## <a name="sample-powershell-code"></a>PowerShell-voorbeeldcode

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

De volgende PowerShell-voorbeeldscript wordt ter referentie. Dit script maakt een nieuw opslagaccount en -container. Deze vervolgens ziet u hoe in te stellen en wissen juridische bewaring, maken en vergrendelen van een op tijd gebaseerd bewaarbeleid (ook wel bekend als een beleid voor onveranderbaarheid), het bewaarinterval uitbreiden.

Instellen en testen van de Azure Storage-account:

```powershell
$ResourceGroup = "<Enter your resource group>”
$StorageAccount = "<Enter your storage account name>"
$container = "<Enter your container name>"
$container2 = "<Enter another container name>”
$location = "<Enter the storage account location>"

# Log in to the Azure Resource Manager account
Login-AzAccount
Register-AzResourceProvider -ProviderNamespace "Microsoft.Storage"

# Create your Azure resource group
New-AzResourceGroup -Name $ResourceGroup -Location $location

# Create your Azure storage account
New-AzStorageAccount -ResourceGroupName $ResourceGroup -StorageAccountName `
    $StorageAccount -SkuName Standard_LRS -Location $location -Kind StorageV2

# Create a new container
New-AzStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Create Container 2 with a storage account object
$accountObject = Get-AzStorageAccount -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount
New-AzStorageContainer -StorageAccount $accountObject -Name $container2

# Get a container
Get-AzStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Get a container with an account object
$containerObject = Get-AzStorageContainer -StorageAccount $accountObject -Name $container

# List containers
Get-AzStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount

# Remove a container (add -Force to dismiss the prompt)
Remove-AzStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container2

# Remove a container with an account object
Remove-AzStorageContainer -StorageAccount $accountObject -Name $container2

# Remove a container with a container object
$containerObject2 = Get-AzStorageContainer -StorageAccount $accountObject -Name $container2
Remove-AzStorageContainer -InputObject $containerObject2
```

Stel en juridische bewaring:

```powershell
# Set a legal hold
Add-AzStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag <tag1>,<tag2>,...

# with an account object
Add-AzStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag <tag3>

# with a container object
Add-AzStorageContainerLegalHold -Container $containerObject -Tag <tag4>,<tag5>,...

# Clear a legal hold
Remove-AzStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag <tag2>

# with an account object
Remove-AzStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag <tag3>,<tag5>

# with a container object
Remove-AzStorageContainerLegalHold -Container $containerObject -Tag <tag4>
```

Maken of bijwerken van beleid voor onveranderbaarheid:
```powershell
# with an account name or container name
Set-AzStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container -ImmutabilityPeriod 10

# with an account object
Set-AzStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -ImmutabilityPeriod 1 -Etag $policy.Etag

# with a container object
$policy = Set-AzStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 7

# with an immutability policy object
Set-AzStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -ImmutabilityPeriod 5
```

Beleid voor onveranderbaarheid ophalen:
```powershell
# Get an immutability policy
Get-AzStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container

# with an account object
Get-AzStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container

# with a container object
Get-AzStorageContainerImmutabilityPolicy -Container $containerObject
```

Beleid voor onveranderbaarheid vergrendelen (add - Force voor het verwijderen van de prompt):
```powershell
# with an immutability policy object
$policy = Get-AzStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
$policy = Lock-AzStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -force

# with an account name or container name
$policy = Lock-AzStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

# with an account object
$policy = Lock-AzStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -Etag $policy.Etag

# with a container object
$policy = Lock-AzStorageContainerImmutabilityPolicy -Container `
    $containerObject -Etag $policy.Etag -force
```

Beleid voor onveranderbaarheid uitbreiden:
```powershell

# with an immutability policy object
$policy = Get-AzStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container

$policy = Set-AzStorageContainerImmutabilityPolicy -ImmutabilityPolicy `
    $policy -ImmutabilityPeriod 11 -ExtendPolicy

# with an account name or container name
$policy = Set-AzStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -ImmutabilityPeriod 11 -Etag $policy.Etag -ExtendPolicy

# with an account object
$policy = Set-AzStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -ImmutabilityPeriod 12 -Etag `
    $policy.Etag -ExtendPolicy

# with a container object
$policy = Set-AzStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 13 -Etag $policy.Etag -ExtendPolicy
```

Een beleid voor Onveranderbaarheid van verwijderen (add - Force voor het verwijderen van de prompt):
```powershell
# with an immutability policy object
$policy = Get-AzStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
Remove-AzStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy

# with an account name or container name
Remove-AzStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

# with an account object
Remove-AzStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -Etag $policy.Etag

# with a container object
Remove-AzStorageContainerImmutabilityPolicy -Container $containerObject `
    -Etag $policy.Etag

```
