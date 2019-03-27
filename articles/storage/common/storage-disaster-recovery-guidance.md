---
title: Disaster recovery en storage-account failover (preview) - Azure Storage
description: Azure Storage ondersteunt account failover (preview) voor geo-redundant storage-accounts. Met failover van de account, kunt u het failoverproces initiëren voor uw opslagaccount als het primaire eindpunt niet beschikbaar is.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/25/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: f0963e7f558de7b591576a49a74750d6697d7127
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486044"
---
# <a name="disaster-recovery-and-storage-account-failover-preview-in-azure-storage"></a>Disaster recovery en storage-account failover (preview) in Azure Storage

Microsoft streeft ernaar om ervoor te zorgen dat Azure-services altijd beschikbaar zijn. Echter, niet-geplande storingen optreden. Als uw toepassing tolerantie vereist, Microsoft adviseert geografisch redundante opslag, zodat uw gegevens in een tweede regio worden gerepliceerd. Klanten moeten bovendien een herstel na noodgevallen plannen in plaats voor het verwerken van een regionale service-onderbreking hebben. Een belangrijk onderdeel van een herstelplan na noodgevallen wordt voorbereid om te fungeren als failover voor het secundaire eindpunt in het geval dat het primaire eindpunt niet meer beschikbaar is. 

Azure Storage ondersteunt account failover (preview) voor geo-redundant storage-accounts. Met failover van de account, kunt u het failoverproces initiëren voor uw opslagaccount als het primaire eindpunt niet beschikbaar is. De failover werkt het secundaire eindpunt als u wilt worden van het primaire eindpunt voor uw opslagaccount. Nadat de failover voltooid is, kunnen clients begint te schrijven naar de nieuwe primaire eindpunt.

In dit artikel beschrijft de concepten en proces betrokken bij een failover van het account en wordt beschreven hoe u uw opslagaccount voorbereiden op herstel met zo min mogelijk gevolgen van de klant. Zie voor meer informatie over de failover van een account in de Azure portal of PowerShell starten, [Start de failover van een account (preview)](storage-initiate-account-failover.md).

## <a name="choose-the-right-redundancy-option"></a>Kies de juiste redundantieoptie

Alle opslagaccounts worden gerepliceerd voor redundantie. Welke optie voor redundantie u voor uw account kiest is afhankelijk van de mate van flexibiliteit die u nodig hebt. Kies voor de bescherming tegen regionale uitval, geografisch redundante opslag, met of zonder de mogelijkheid om leestoegang van de secundaire regio:  

**Geografisch redundante opslag (GRS)** repliceert uw gegevens asynchroon in twee geografische regio's die ten minste grote afstand van elkaar. Als de primaire regio een storing heeft, klikt u vervolgens fungeert de secundaire regio als een redundante bron voor uw gegevens. U kunt een failover voor het transformeren van het secundaire eindpunt in het primaire eindpunt starten.

**Geo-redundante opslag met leestoegang (RA-GRS)** levert geografisch redundante opslag met het bijkomende voordeel van leestoegang tot het secundaire eindpunt. Als er een storing optreedt in het primaire eindpunt, blijven geconfigureerd voor RA-GRS en die zijn ontworpen voor hoge beschikbaarheid kunnen lezen vanuit het secundaire eindpunt. Microsoft raadt aan RA-GRS voor maximale flexibiliteit voor uw toepassingen.

Andere Azure Storage-opties voor gegevensredundantie zijn zone-redundante opslag (ZRS), waarmee uw gegevens in meerdere beschikbaarheidszones in één regio worden gerepliceerd en lokaal redundante opslag (LRS), waarmee uw gegevens in één datacentrum in één regio worden gerepliceerd. Als uw storage-account is geconfigureerd voor ZRS of LRS, kunt u dat account voor het gebruik van GRS of RA-GRS converteren. Configureren van uw account voor geografisch redundante opslag, worden extra kosten in rekening gebracht. Zie voor meer informatie, [Azure Storage-replicatie](storage-redundancy.md).

> [!WARNING]
> Geografisch redundante opslag wordt een risico van gegevensverlies. Gegevens worden gerepliceerd naar de secundaire regio asynchroon, wat betekent dat er is een vertraging tussen wanneer de gegevens die zijn geschreven naar de primaire regio worden geschreven naar de secundaire regio. Na een storing schrijven bewerkingen naar het primaire eindpunt die nog niet is gerepliceerd naar het secundaire eindpunt gaan verloren. 

## <a name="design-for-high-availability"></a>Ontwerp voor hoge beschikbaarheid

Het is belangrijk om uw toepassing voor hoge beschikbaarheid vanaf het begin te ontwerpen. Verwijzen naar deze Azure-resources voor richtlijnen bij het ontwerpen van uw toepassing en het plannen voor herstel na noodgevallen:

* [Flexibele toepassingen ontwerpen voor Azure](https://docs.microsoft.com/azure/architecture/resiliency/): Een overzicht van de belangrijkste concepten voor het ontwerpen van maximaal beschikbare toepassingen in Azure.
* [Controlelijst voor beschikbaarheid](https://docs.microsoft.com/azure/architecture/checklist/availability): Een controlelijst om te verifiëren dat uw toepassing de aanbevolen procedures voor ontwerp voor hoge beschikbaarheid implementeert.
* [Maximaal beschikbare toepassingen met RA-GRS ontwerpen](storage-designing-ha-apps-with-ragrs.md): Ontwerprichtlijnen voor het bouwen van toepassingen om te profiteren van RA-GRS.
* [Zelfstudie: Maken van een maximaal beschikbare toepassing met blobopslag](../blobs/storage-create-geo-redundant-storage.md): Een zelfstudie die laat zien hoe u een maximaal beschikbare toepassing bouwt die automatisch tussen de eindpunten als fouten en herstelbewerkingen schakelt worden gesimuleerd. 

Bovendien, houd rekening met deze aanbevolen procedures voor het onderhouden van hoge beschikbaarheid voor uw Azure Storage-gegevens:

* **Schijven:** Gebruik [Azure Backup](https://azure.microsoft.com/services/backup/) back-up van de VM-schijven die worden gebruikt door uw Azure virtual machines. Ook kunt u overwegen [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) ter bescherming van uw virtuele machines in het geval van een regionaal noodgeval.
* **Blok-blobs:** Schakel [voorlopig verwijderen](../blobs/storage-blob-soft-delete.md) ter bescherming tegen op objectniveau verwijderingen en overschreven of blok-blobs kopiëren naar een ander opslagaccount in een andere regio met [AzCopy](storage-use-azcopy.md), [Azure PowerShell ](storage-powershell-guide-full.md), of de [-bibliotheek voor gegevensverplaatsing van Azure Data](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* **Bestanden:** Gebruik [AzCopy](storage-use-azcopy.md) of [Azure PowerShell](storage-powershell-guide-full.md) uw bestanden naar een ander opslagaccount in een andere regio te kopiëren.
* **Tabellen:** gebruiken [AzCopy](storage-use-azcopy.md) om tabelgegevens te exporteren naar een ander opslagaccount in een andere regio.

## <a name="track-outages"></a>Bijhouden van uitval

Klanten kunnen zich abonneren op de [Azure Service Health Dashboard](https://azure.microsoft.com/status/) om bij te houden van de status en de status van Azure Storage en andere Azure-services.

Microsoft raadt ook aan het ontwerpen van uw toepassing om voor te bereiden voor de kans op fouten schrijven. Uw toepassing moet schrijven storingen op een manier waarbij u geïnformeerd wordt met de mogelijkheid van een storing in de primaire regio worden blootgesteld.

## <a name="understand-the-account-failover-process"></a>Informatie over het failoverproces van account

Failover van de klant beheerde account (preview) kunt u failover wilt uitvoeren voor uw hele opslagaccount naar de secundaire regio als de primaire niet beschikbaar is voor een bepaalde reden. Wanneer u een failover op de secundaire regio afgedwongen, kunnen clients beginnen schrijven van gegevens naar het secundaire eindpunt nadat de failover voltooid is. De failover duurt meestal ongeveer een uur.

### <a name="how-an-account-failover-works"></a>De werking van de failover van een account

Onder normale omstandigheden, een client schrijft gegevens naar een Azure Storage-account in de primaire regio en die gegevens asynchroon worden gerepliceerd naar de secundaire regio. De volgende afbeelding ziet u het scenario als de primaire regio beschikbaar is:

![Clients schrijven gegevens naar het opslagaccount in de primaire regio](media/storage-disaster-recovery-guidance/primary-available.png)

Als het primaire eindpunt niet beschikbaar is voor een bepaalde reden, is de client niet langer kunnen wegschrijven naar het opslagaccount. De volgende afbeelding ziet u het scenario waarbij de primaire is niet meer beschikbaar, maar er is geen recovery heeft plaatsgevonden:

![De primaire niet beschikbaar is, zodat clients kunnen geen gegevens schrijven](media/storage-disaster-recovery-guidance/primary-unavailable-before-failover.png)

De klant initieert de account-failover naar het secundaire eindpunt. Het failoverproces updates de DNS-vermelding die is geleverd door Azure Storage zodat het secundaire eindpunt de nieuwe primaire eindpunt voor uw storage-account wordt, zoals wordt weergegeven in de volgende afbeelding:

![Klant initieert account failover naar de secundaire eindpunt](media/storage-disaster-recovery-guidance/failover-to-secondary.png)

Schrijftoegang is hersteld voor GRS en RA-GRS-accounts zodra de DNS-vermelding is bijgewerkt en aanvragen worden omgeleid naar de nieuwe primaire eindpunt. Bestaande storage service-eindpunten voor blobs, tabellen, wachtrijen en bestanden blijven hetzelfde na de failover.

> [!IMPORTANT]
> Nadat de failover voltooid is, wordt het opslagaccount dat is geconfigureerd om lokaal redundante in de nieuwe primaire eindpunt. Als u wilt hervatten replicatie naar de nieuwe secundaire, configureer het account voor het gebruik van geografisch redundante opslag opnieuw (RA-GRS- of GRS).
>
> Houd er rekening mee dat een LRS-account converteren naar RA-GRS- of GRS worden kosten in rekening gebracht. Deze kosten geldt voor het bijwerken van het opslagaccount in de nieuwe primaire regio voor gebruik van RA-GRS- of GRS na een failover.  

### <a name="anticipate-data-loss"></a>Anticiperen op verlies van gegevens

> [!CAUTION]
> De failover van een account omvat doorgaans gegevens verloren gaan. Het is belangrijk dat u begrijpt wat de gevolgen van een account-failover wordt gestart.  

Omdat gegevens asynchroon van de primaire regio naar de secundaire regio geschreven worden, is er altijd een vertraging optreden voordat een schrijfbewerking naar de primaire regio worden gerepliceerd naar de secundaire regio. Als de primaire regio niet beschikbaar is, kunnen de meest recente schrijfbewerkingen niet nog zijn gerepliceerd naar de secundaire regio.

Wanneer u een failover afgedwongen, gaat alle gegevens in de primaire regio verloren als de secundaire regio, wordt de nieuwe primaire regio en het opslagaccount dat is geconfigureerd als lokaal redundant. Alle gegevens die al worden gerepliceerd naar de secundaire wordt bijgehouden wanneer de failover plaatsvindt. Geen gegevens geschreven naar de primaire die ook niet is gerepliceerd naar de secundaire is echter permanent verloren gaan. 

De **tijd van laatste synchronisatie** eigenschap tijdstip aangeeft waarop de meest recente gegevens uit de primaire regio is gegarandeerd zijn geschreven voor de secundaire regio. Alle gegevens die zijn geschreven voordat u de tijd van laatste synchronisatie is beschikbaar op de secundaire server, terwijl de gegevens die worden geschreven als de tijd van laatste synchronisatie niet mogelijk is geschreven naar de secundaire en verloren gaan kan. Deze eigenschap in het geval van een storing om een schatting van de hoeveelheid gegevensverlies, er worden door het starten van de failover van een account te gebruiken. 

Als een best practice ontwerp uw toepassing zodat u de tijd van laatste synchronisatie gebruiken kunt voor het evalueren van de verwachte gegevens verloren gaan. Bijvoorbeeld, als u zich alle schrijfbewerkingen, en vervolgens kunt u het tijdstip van uw laatste schrijfbewerkingen naar de laatste vergelijken tijd om te bepalen welke schrijfbewerkingen niet zijn gesynchroniseerd naar de secundaire synchroniseren.

### <a name="use-caution-when-failing-back-to-the-original-primary"></a>Wees voorzichtig wanneer u terug naar de oorspronkelijke primaire database is mislukt

Nadat u een failover van de primaire naar de secundaire regio, wordt uw storage-account is geconfigureerd als lokaal redundante in de nieuwe primaire regio. U kunt het account voor geo-redundantie opnieuw configureren door het bijwerken van het gebruik van GRS of RA-GRS. Wanneer het account is geconfigureerd voor geo-redundantie opnieuw na een failover, onmiddellijk de nieuwe primaire regio repliceren van gegevens naar de nieuwe secundaire regio, die de primaire voordat u de oorspronkelijke failover. Het kan echter een bepaalde periode voordat de bestaande gegevens in de primaire volledig wordt gerepliceerd naar de nieuwe secundaire duren.

Nadat het opslagaccount dat is geconfigureerd voor geo-redundantie, is het mogelijk om te starten van een andere failover van de nieuwe primaire terug naar de nieuwe secundaire. In dit geval de oorspronkelijke primaire regio voordat de failover wordt de primaire regio opnieuw en is geconfigureerd als lokaal redundant. Alle gegevens in de primaire regio, nadat failover is uitgevoerd (de oorspronkelijke secundaire) wordt vervolgens verloren gaan. Als de meeste van de gegevens in het opslagaccount niet is gerepliceerd naar de nieuwe secundaire voordat u een failback, kunt u een verlies van belangrijke gegevens kan afnemen. 

Een belangrijke om gegevensverlies te voorkomen, controleert u de waarde van de **tijd van laatste synchronisatie** eigenschap alvorens de failback. Vergelijk de tijd van laatste synchronisatie op de laatste keer dat de gegevens is geschreven naar de nieuwe primaire evalueren van de verwachte gegevens verloren gaan. 

## <a name="initiate-an-account-failover"></a>Een failover van account initiëren

U kunt een failover van de account van de Azure-portal, PowerShell, Azure CLI of de Azure Storage resourceprovider API op te starten. Zie voor meer informatie over het starten van een failover [Start de failover van een account (preview)](storage-initiate-account-failover.md).

## <a name="about-the-preview"></a>Over de Preview-versie

failover van de account is beschikbaar in Preview-versie voor alle klanten met GRS of RA-GRS met Azure Resource Manager-implementaties. Algemeen gebruik v1, algemeen gebruik v2 en Blob storage-accounttypen worden ondersteund. failover van de account is momenteel beschikbaar in deze regio's:

- US - west 2
- US - west-centraal

De Preview-versie is bedoeld voor gebruik in alleen niet-productieomgevingen. Productie service level agreements (Sla's) zijn momenteel niet beschikbaar.

### <a name="register-for-the-preview"></a>Registreren voor de Preview-versie

Als u wilt registreren voor de Preview-versie, moet u de volgende opdrachten uitvoeren in PowerShell. Zorg ervoor dat de tijdelijke aanduiding tussen vierkante haken vervangen door uw eigen abonnement-ID:

```powershell
Connect-AzureRmAccount -SubscriptionId <subscription-id>
Register-AzureRmProviderFeature -FeatureName CustomerControlledFailover -ProviderNamespace Microsoft.Storage
```

1-2 dagen voor het ontvangen van goedkeuring voor de Preview-versie kan duren. Om te controleren dat de registratie is goedgekeurd, voert u de volgende opdracht uit:

```powershell
Get-AzureRmProviderFeature -FeatureName CustomerControlledFailover -ProviderNamespace Microsoft.Storage
```

### <a name="additional-considerations"></a>Aanvullende overwegingen 

Bekijk de aanvullende overwegingen beschreven in deze sectie om te begrijpen hoe uw toepassingen en services kunnen worden beïnvloed wanneer u een failover tijdens de preview-periode afgedwongen.

#### <a name="azure-virtual-machines"></a>Virtuele machines van Azure

Virtuele Azure-machines (VM's) kan geen failover als onderdeel van een account voor failover. Als de primaire regio niet beschikbaar, en u over naar de secundaire regio schakelt, moet u virtuele machines na de failover opnieuw maken. 

#### <a name="azure-unmanaged-disks"></a>Azure-niet-beheerde schijven

Als een best practice, Microsoft raadt aan niet-beheerde schijven converteren naar beheerde schijven. Echter als u nodig hebt voor failover van een account met een niet-beheerde schijven zijn gekoppeld aan Azure-VM's, moet u de virtuele machine af voordat de failover wordt gestart.

Niet-beheerde schijven worden opgeslagen als pagina-blobs in Azure Storage. Wanneer een virtuele machine wordt uitgevoerd in Azure, zijn alle niet-beheerde schijven die zijn gekoppeld aan de virtuele machine in lease gegeven. De failover van een account kan niet worden voortgezet wanneer er een lease op een blob. Als u de failover, de volgende stappen uit:

1. Voordat u begint, noteer de namen van alle niet-beheerde schijven, hun logische eenhedengetallen (LUN) en de virtuele machine waaraan ze zijn gekoppeld. In dat geval wordt het eenvoudiger te koppelen van de schijven na de failover. 
2. De virtuele machine af.
3. Verwijderen van de virtuele machine, maar behoud van de VHD-bestanden voor de niet-beheerde schijven. Houd er rekening mee de tijd waarop u de virtuele machine verwijderd.
4. Wacht totdat de **tijd van laatste synchronisatie** is bijgewerkt en is hoger dan de tijd waarop u de virtuele machine verwijderd. Deze stap is belangrijk, omdat als het secundaire eindpunt niet volledig met de VHD-bestanden bijgewerkt is wanneer de failover is uitgevoerd, klikt u vervolgens de virtuele machine mogelijk niet goed in de nieuwe primaire regio.
5. Start de failover van het account.
6. Wacht totdat de account-failover voltooid is en de secundaire regio is de nieuwe primaire regio geworden.
7. Een virtuele machine in de nieuwe primaire regio maken en koppelen van de VHD's.
8. Start de nieuwe virtuele machine.

Houd er rekening mee dat alle gegevens die zijn opgeslagen in een tijdelijke schijf verloren wanneer de virtuele machine wordt afgesloten.

### <a name="unsupported-features-or-services"></a>Niet-ondersteunde functies of services
De volgende functies of services worden niet ondersteund voor failover-account voor de preview-versie:

- Azure File Sync biedt geen ondersteuning voor failover van storage-account. Storage-accounts met Azure-bestandsshares wordt gebruikt als cloudeindpunten in Azure File Sync moeten failover niet mogelijk is. In dat geval wordt oorzaak synchroniseren niet meer werkt en kan ook leiden tot onverwachte gegevensverlies in het geval van nieuwe gelaagde bestanden.  
- Opslagaccounts met behulp van Azure Data Lake Storage Gen2 hiërarchische naamruimte kunnen geen failover mogelijk is.
- Een opslagaccount met gearchiveerde blobs kan geen failover mogelijk is. Gearchiveerde blobs in een afzonderlijk opslagaccount die u niet van plan bent failover onderhouden.
- Een met blok-blobs voor premium storage-account kan geen failover mogelijk is. Storage-accounts die ondersteuning bieden voor blok-blobs voor premium bieden momenteel geen ondersteuning voor geo-redundantie.
- Nadat de failover voltooid is de volgende functies worden niet meer werken als oorspronkelijk is ingeschakeld: [Gebeurtenisabonnementen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-overview), [Lifecycle-beleid](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts), [Storage Analytics logboekregistratie](https://docs.microsoft.com/rest/api/storageservices/about-storage-analytics-logging).

## <a name="copying-data-as-an-alternative-to-failover"></a>Kopiëren van gegevens als alternatief voor failover

Als uw storage-account is geconfigureerd voor RA-GRS, hebt u leestoegang tot uw gegevens met behulp van het secundaire eindpunt. Als u liever niet wilt uitvoeren op basis na een storing in de primaire regio, kunt u hulpprogramma's zoals [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), of de [-bibliotheek voor gegevensverplaatsing van Azure Data](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) naar gegevens uit uw opslagaccount in de secundaire regio kopiëren naar een ander opslagaccount in een regio niet beïnvloed. U kunt vervolgens wijst u uw toepassingen naar dit opslagaccount voor zowel lezen en schrijven van beschikbaarheid.

## <a name="microsoft-managed-failover"></a>Beheerd door Microsoft failover

In extreme omstandigheden waarin een regio verbroken vanwege een grote ramp is, kan Microsoft een regionale failover initiëren. In dit geval is geen actie van uw kant vereist. Pas nadat de failover beheerd door Microsoft is voltooid, hoeft u schrijftoegang te uw storage-account. Uw toepassingen kunnen lezen van de secundaire regio als uw storage-account is geconfigureerd voor RA-GRS. 

## <a name="see-also"></a>Zie ook

* [Start de failover van een account (preview)](storage-initiate-account-failover.md)
* [Maximaal beschikbare toepassingen met RA-GRS ontwerpen](storage-designing-ha-apps-with-ragrs.md)
* [Zelfstudie: Een maximaal beschikbare toepassing met Blob-opslag maken](../blobs/storage-create-geo-redundant-storage.md) 
