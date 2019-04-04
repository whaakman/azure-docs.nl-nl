---
title: Het bijwerken van een service in de cloud | Microsoft Docs
description: Informatie over het bijwerken van cloudservices in Azure. Meer informatie over hoe een update voor een cloudservice wordt voortgezet om beschikbaarheid te garanderen.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: c6a8b5e6-5c99-454c-9911-5c7ae8d1af63
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeconnoc
ms.openlocfilehash: ff4dd571911719e4f2ec27952785432960a56d42
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58917221"
---
# <a name="how-to-update-a-cloud-service"></a>Het bijwerken van een service in de cloud

Bijwerken van een cloudservice, met inbegrip van de functies en de Gast-besturingssysteem hebt, is drie stappen vereist. Eerst moeten de binaire bestanden en de configuratiebestanden voor de nieuwe service in de cloud of de versie van het besturingssysteem worden geüpload. Azure reserveert vervolgens reken- en resources voor de cloudservice op basis van de vereisten van de nieuwe versie van de cloud-service. Ten slotte voert Azure uit een rolling upgrade voor het bijwerken van de tenant incrementeel naar de nieuwe versie of de Gast-OS, behoud van de beschikbaarheid van uw. Dit artikel worden de details van deze laatste stap: de rolling upgrade.

## <a name="update-an-azure-service"></a>Een Azure-Service bijwerken
Azure organiseert uw rolinstanties in logische groeperingen upgrade-domeinen (UD) genoemd. Upgrade-domeinen (UD) zijn logische verzamelingen rolinstanties die als een groep worden bijgewerkt.  Azure-updates een cloud service één UD op een tijdstip, waardoor de instanties in andere ud's om door te gaan met het verkeer.

Het aantal upgradedomeinen is 5. U kunt een ander aantal upgradedomeinen opgeven door het kenmerk upgradeDomainCount te nemen in het definitiebestand van de service (.csdef). Zie voor meer informatie over het kenmerk upgradeDomainCount [WebRole Schema](/previous-versions/azure/reference/gg557553(v=azure.100)) of [WorkerRole Schema](/previous-versions/azure/reference/gg557552(v=azure.100)).

Als u een update in plaats van een of meer rollen in uw service uitvoert, Azure-updates sets rolinstanties op basis van het upgradedomein waartoe ze behoren. Alle exemplaren van in een bepaalde upgradedomein – stoppen, bijwerken, worden gezet online: back-Azure-updates gaat vervolgens naar het volgende domein. Door alleen de exemplaren die worden uitgevoerd in het huidige upgradedomein stoppen, ervoor Azure zorgt dat een update zich met de geringst mogelijke gevolgen voor de uitgevoerde service voordoet. Zie voor meer informatie, [hoe de update wordt voortgezet](#howanupgradeproceeds) verderop in dit artikel.

> [!NOTE]
> Terwijl de voorwaarden **bijwerken** en **upgrade** iets andere betekenis hebben in de context van Azure, ze kunnen worden door elkaar worden gebruikt voor de processen en beschrijvingen van de functies in dit document.
>
>

Uw service moet ten minste twee exemplaren van een rol voor die rol worden bijgewerkt definiëren in-place zonder uitvaltijd. Als de service uit slechts één exemplaar van een rol bestaat, is uw service niet beschikbaar totdat de InPlace-update is voltooid.

In dit onderwerp bevat informatie over de volgende informatie over Azure-updates:

* [Wijzigingen in de service toegestaan tijdens het bijwerken](#AllowedChanges)
* [Hoe een upgrade wordt uitgevoerd](#howanupgradeproceeds)
* [Terugdraaien van een update](#RollbackofanUpdate)
* [Meerdere mutating bewerkingen op een actieve implementatie starten](#multiplemutatingoperations)
* [Distributie van functies in meerdere domeinen upgraden](#distributiondfroles)

<a name="AllowedChanges"></a>

## <a name="allowed-service-changes-during-an-update"></a>Wijzigingen in de service toegestaan tijdens het bijwerken
De volgende tabel toont de toegestane wijzigingen in een service tijdens het bijwerken:

| Wijzigingen die zijn toegestaan op die als host fungeert, services en functies | InPlace-update | Gefaseerde (wisselen van VIP) | Verwijderen en opnieuw implementeren |
| --- | --- | --- | --- |
| Besturingssysteem |Ja |Ja |Ja |
| .NET-vertrouwensniveau |Ja |Ja |Ja |
| Grootte van virtuele machine<sup>1</sup> |Ja<sup>2</sup> |Ja |Ja |
| Instellingen voor de lokale opslag |Alleen verhogen<sup>2</sup> |Ja |Ja |
| Toevoegen of verwijderen van rollen in een service |Ja |Ja |Ja |
| Aantal exemplaren van een bepaalde rol |Ja |Ja |Ja |
| Type van eindpunten voor een service of nummer |Ja<sup>2</sup> |Nee |Ja |
| Namen en waarden van configuratie-instellingen |Ja |Ja |Ja |
| De waarden (maar niet de naam) van configuratie-instellingen |Ja |Ja |Ja |
| Nieuwe certificaten toevoegen |Ja |Ja |Ja |
| Wijzigen van bestaande certificaten |Ja |Ja |Ja |
| Implementeer nieuwe code |Ja |Ja |Ja |

<sup>1</sup> formaat wijzigen beperkt tot de subset met grootten die beschikbaar zijn voor de cloudservice.

<sup>2</sup> vereist Azure SDK 1.5 of hoger.

> [!WARNING]
> De grootte van de virtuele machine te wijzigen, lokale gegevens gaan verloren.
>
>

De volgende items worden niet ondersteund tijdens het bijwerken:

* De naam van een rol wijzigen. Verwijdert en vervolgens voegt de rol met de nieuwe naam.
* Wijzigen van het aantal upgraden.
* Het verkleinen van de lokale bronnen.

Als u andere updates aan de definitie van uw service, zoals het verkleinen van de lokale bron, moet u in plaats daarvan een update voor het wisselen van VIP uitvoeren. Zie voor meer informatie, [implementatie wisselen](/previous-versions/azure/reference/ee460814(v=azure.100)).

<a name="howanupgradeproceeds"></a>

## <a name="how-an-upgrade-proceeds"></a>Hoe een upgrade wordt uitgevoerd
U kunt beslissen of u wilt alle van de rollen in uw service of een enkele rol in de service bijwerken. In beide gevallen worden alle exemplaren van elke rol die wordt bijgewerkt en die deel uitmaken van het eerste upgradedomein is gestopt, bijgewerkt en weer online. Zodra deze weer online zijn, worden de exemplaren in de tweede upgradedomein is gestopt, bijgewerkt en weer online. Een service in de cloud kan maximaal één upgrade actieve bewerking tegelijk hebben. De upgrade wordt altijd uitgevoerd tegen de nieuwste versie van de cloudservice.

Het volgende diagram illustreert hoe de upgrade wordt uitgevoerd als u een upgrade alle van de rollen in de service uitvoert:

![Upgrade van service](media/cloud-services-update-azure-service/IC345879.png "-service upgraden")

Deze volgende diagram ziet u hoe de update wordt uitgevoerd als u slechts één functie bijwerkt:

![Upgrade rol](media/cloud-services-update-azure-service/IC345880.png "Upgrade rol")  

Tijdens het automatisch bijwerken evalueert de Azure-Infrastructuurcontroller periodiek de status van de cloudservice om te bepalen wanneer het is veilig om u te helpen de volgende UD. Deze status evaluatie wordt uitgevoerd op basis van per rol en rekening gehouden met alleen-exemplaren in de meest recente versie (dat wil zeggen de instanties van ud's die al zijn gelopen). Er wordt gecontroleerd of een minimum aantal rolexemplaren, voor elke rol, hebben een goede definitieve status bereikt.

### <a name="role-instance-start-timeout"></a>Time-out voor de Start van de rol-exemplaar
De Infrastructuurcontroller wacht 30 minuten voor elk rolexemplaar te bereiken een status gestart. Als de duur van de time-out is verstreken, blijft de Infrastructuurcontroller walking met het volgende exemplaar van de rol.

### <a name="impact-to-drive-data-during-cloud-service-upgrades"></a>Impact op het station gegevens tijdens de Service in de Cloud wordt bijgewerkt

Bij het upgraden van een service van één exemplaar naar meerdere exemplaren wordt uw service worden verbroken tijdens de upgrade wordt uitgevoerd vanwege de manier waarop Azure upgrades-services. Service level agreement aansprakelijke beschikbaarheid van de service is alleen van toepassing op services die zijn geïmplementeerd met meer dan één exemplaar. De volgende lijst wordt beschreven hoe de gegevens op elke schijf wordt beïnvloed door een upgradescenario voor elke Azure-service:

|Scenario|C-schijf|D-station|E Drive|
|--------|-------|-------|-------|
|Virtuele machine opnieuw opstarten|Behouden|Behouden|Behouden|
|Portal opnieuw opstarten|Behouden|Behouden|Vernietigd|
|Portal terugzetten|Behouden|Vernietigd|Vernietigd|
|In-Place Upgrade|Behouden|Behouden|Vernietigd|
|Migratie van knooppunt|Vernietigd|Vernietigd|Vernietigd|

Houd er rekening mee dat in de bovenstaande lijst, het e-station het basisstation van de rol vertegenwoordigt, en niet vastgelegd worden moet. Gebruik in plaats daarvan de **RoleRoot %** omgevingsvariabele om weer te geven van het station.

Een nieuwe service met meerdere exemplaren implementeren naar de staging-server om te beperken de downtime bij het upgraden van een service met één instantie, en geen VIP's wisselen.

<a name="RollbackofanUpdate"></a>

## <a name="rollback-of-an-update"></a>Terugdraaien van een update
Azure biedt flexibiliteit voor het beheren van services doordat u aanvullende bewerkingen van een service starten nadat de initiële update-aanvraag is geaccepteerd door de Azure-Infrastructuurcontroller tijdens het bijwerken. Terugdraaien kan alleen worden uitgevoerd wanneer een update (configuratiewijziging) of de upgrade wordt het **Bezig** staat op de implementatie. Een update of upgrade wordt beschouwd als te worden uitgevoerd, zolang er is ten minste één exemplaar van de service nog niet naar de nieuwe versie bijgewerkt is. Als u wilt testen of een terugdraaien is toegestaan, Controleer de waarde van de vlag RollbackAllowed, die wordt geretourneerd door [ophalen implementatie](/previous-versions/azure/reference/ee460804(v=azure.100)) en [Cloud Service-eigenschappen ophalen](/previous-versions/azure/reference/ee460806(v=azure.100)) bewerkingen, is ingesteld op true.

> [!NOTE]
> Alleen is het handig om aan te roepen terugdraaien op een **ter plekke** bijwerken of bijwerken omdat het VIP wisselen upgrades betrekking hebben op een hele actief exemplaar van uw service met een andere vervangen.
>
>

Terugdraaien van een update wordt uitgevoerd, heeft de volgende gevolgen voor de implementatie:

* Alle rolinstanties die nog niet zijn bijgewerkt of een upgrade uitgevoerd naar de nieuwe versie zijn niet bijgewerkt of upgrade hebt uitgevoerd, omdat de doelversie van de service al op deze instanties worden uitgevoerd.
* Alle rolinstanties die al waren zijn bijgewerkt of een upgrade uitgevoerd naar de nieuwe versie van het servicepakket (\*.cspkg) bestand of de configuratie van de service (\*cscfg-bestand)-bestand (of beide bestanden) worden hersteld naar de versie van vóór de upgrade van deze bestanden.

Dit is functioneel bepaald door de volgende functies:

* De [terugdraaien van de Update of Upgrade](/previous-versions/azure/reference/hh403977(v=azure.100)) bewerking, die kan worden aangeroepen in een configuratie-update (geactiveerd door het aanroepen van [implementatieconfiguratie wijzigen](/previous-versions/azure/reference/ee460809(v=azure.100))) of een upgrade (geactiveerd door het aanroepen van [ Upgrade-implementatie](/previous-versions/azure/reference/ee460793(v=azure.100))), zolang er is ten minste één exemplaar in de service die nog niet naar de nieuwe versie bijgewerkt is.
* Het element vergrendeld en het element RollbackAllowed die worden geretourneerd als onderdeel van de hoofdtekst van het antwoord van de [ophalen implementatie](/previous-versions/azure/reference/ee460804(v=azure.100)) en [Cloud Service-eigenschappen ophalen](/previous-versions/azure/reference/ee460806(v=azure.100)) bewerkingen:

  1. Het element vergrendeld, kunt u om te detecteren wanneer een mutating bewerking kan worden aangeroepen op een bepaalde implementatie.
  2. Het element RollbackAllowed kunt u om te detecteren wanneer de [terugdraaien van de Update of Upgrade](/previous-versions/azure/reference/hh403977(v=azure.100)) bewerking kan worden aangeroepen voor een bepaalde implementatie.

  Om uit te voeren een terugdraaiactie, hoeft u niet om zowel de vergrendeld en de elementen RollbackAllowed te controleren. Deze achtervoegsels om te bevestigen dat RollbackAllowed is ingesteld op true. Deze elementen zijn alleen geretourneerd als deze methoden zijn aangeroepen met behulp van de aanvraagheader ingesteld op ' x-ms-version: 2011-10-01 ' of een latere versie. Zie voor meer informatie over versiebeheer kopteksten [Service Management versiebeheer](/previous-versions/azure/gg592580(v=azure.100)).

Er zijn bepaalde situaties waarbij een ongedaan maken van een update of upgrade wordt niet ondersteund, dit als volgt zijn:

* Vermindering van de lokale bronnen - als de update de lokale bronnen voor een functie van het Azure-platform verhoogt is niet toegestaan voor het terugdraaien.
* Quotumbeperkingen - zijn als de update is een bewerking die u mogelijk niet meer omlaag schalen voldoende rekenquota om het van de terugdraaibewerking te voltooien. Elk Azure-abonnement heeft een quotum dat is gekoppeld aan deze die Hiermee geeft u het maximum aantal kernen dat kan worden gebruikt door alle gehoste services die deel uitmaken van dat abonnement. Als uw abonnement uitvoeren van een terugdraaien van een bepaalde update via quotum en die plaatst wordt een terugdraaiactie niet ingeschakeld.
* Racevoorwaarde - als de eerste update is voltooid, terugdraaien is niet mogelijk.

Een voorbeeld van wanneer het terugdraaien van een update mogelijk nuttig is als u gebruikt de [Upgrade-implementatie](/previous-versions/azure/reference/ee460793(v=azure.100)) bewerking in de handmatige modus voor het beheren van de snelheid waarmee een grote in-place upgrade naar uw Azure service gehoste wordt uitgerold.

Tijdens de implementatie van de upgrade die u aanroept [Upgrade-implementatie](/previous-versions/azure/reference/ee460793(v=azure.100)) in de modus voor handmatige en begint te lopen upgradedomeinen. Als op een bepaald moment tijdens het controleren van de upgrade moet u Houd er rekening mee sommige rolinstanties in de eerste upgrade-domeinen die u onderzoekt hebt reageert, roept u de [terugdraaien van de Update of Upgrade](/previous-versions/azure/reference/hh403977(v=azure.100)) bewerking op de implementatie, laat gewoon de exemplaren van die was nog niet zijn bijgewerkt en de rollback-exemplaren die is bijgewerkt naar de vorige servicepakket en configuratie.

<a name="multiplemutatingoperations"></a>

## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Meerdere mutating bewerkingen op een actieve implementatie starten
In sommige gevallen kunt u meerdere gelijktijdige mutating bewerkingen voor de implementatie van een doorlopende initiëren. Bijvoorbeeld, u kunt een service-update uitvoeren en, terwijl deze update is gemaakt voor uw service, die u wilt bijvoorbeeld een wijziging aanbrengen om te draaien terug in de update, een andere update van toepassing, of zelfs verwijderen van de implementatie. Een aanvraag waarin dit kan het nodig zijn, is als een service-upgrade bevat buggy code die ervoor zorgt dat een bijgewerkte rolinstantie herhaaldelijk vastloopt. In dit geval wordt de Azure-Infrastructuurcontroller pas weer voortgang bij het toepassen van die worden bijgewerkt omdat een onvoldoende aantal exemplaren in de bijgewerkte domein in orde zijn. Deze status wordt aangeduid als een *vastgelopen implementatie*. U kunt de implementatie van de update terugdraaien of het toepassen van een nieuwe update via de bovenkant van het mislukken van de Registreer een.

Nadat de eerste aanvraag naar de update of upgrade van de service is ontvangen door de Azure-Infrastructuurcontroller, kun u verdere mutating bewerkingen. Dat wil zeggen, hoeft u niet te wachten op de eerste bewerking is voltooid voordat u kunt een andere mutating bewerking.

Een tweede updatebewerking starten terwijl de eerste update wordt momenteel is wordt vergelijkbaar met de terugdraaibewerking uitgevoerd. Als de tweede update in de automatische modus is het eerste upgradedomein wordt een upgrade uitgevoerd onmiddellijk mogelijk leiden tot instanties van meerdere upgradedomeinen offline op een bepaald moment in-time.

De mutating bewerkingen zijn als volgt: [Implementatieconfiguratie wijzigen](/previous-versions/azure/reference/ee460809(v=azure.100)), [implementatie upgraden](/previous-versions/azure/reference/ee460793(v=azure.100)), [implementatiestatus van de Update](/previous-versions/azure/reference/ee460808(v=azure.100)), [implementatie verwijderen](/previous-versions/azure/reference/ee460815(v=azure.100)), en [terugdraaien Update- of Upgrade](/previous-versions/azure/reference/hh403977(v=azure.100)).

Twee bewerkingen [ophalen implementatie](/previous-versions/azure/reference/ee460804(v=azure.100)) en [Cloud Service-eigenschappen ophalen](/previous-versions/azure/reference/ee460806(v=azure.100)), retourneren de vergrendeld-vlag waarmee kan worden onderzocht om te bepalen of een mutating bewerking kan worden aangeroepen op een bepaalde implementatie.

Als u wilt de versie van deze methoden die als de vlag geblokkeerde resultaat aanroepen, moet u de aanvraagheader instellen op ' x-ms-version: 2011-10-01 ' of een hoger. Zie voor meer informatie over versiebeheer kopteksten [Service Management versiebeheer](/previous-versions/azure/gg592580(v=azure.100)).

<a name="distributiondfroles"></a>

## <a name="distribution-of-roles-across-upgrade-domains"></a>Distributie van functies in meerdere domeinen upgraden
Azure wordt exemplaren van een rol gelijkmatig verspreid over een bepaald aantal upgradedomeinen, die kan worden geconfigureerd als onderdeel van het servicedefinitiebestand (.csdef). Het maximum aantal upgradedomeinen is 20 en de standaardwaarde is 5. Zie voor meer informatie over het wijzigen van het servicedefinitiebestand [Definitieschema voor Azure-Service (.csdef-bestand)](cloud-services-model-and-package.md#csdef).

Bijvoorbeeld, als uw rol tien exemplaren heeft, bevat elk upgradedomein standaard twee exemplaren. Als uw rol 14-exemplaren heeft, bevat vier van de upgradedomeinen drie exemplaren en een vijfde domein bevat twee.

Upgradedomeinen worden aangeduid met een op nul gebaseerde index: het eerste upgradedomein heeft een 0-ID en het tweede upgradedomein ID 1, enzovoort.

Het volgende diagram illustreert hoe een service bevat twee rollen worden gedistribueerd wanneer de service definieert twee upgradedomeinen. De service wordt uitgevoerd acht instanties van de Webrol en negen instanties van de werkrol.

![Distributie van Upgradedomeinen](media/cloud-services-update-azure-service/IC345533.png "distributie van Upgradedomeinen")

> [!NOTE]
> Houd er rekening mee dat Azure bepaalt hoe de exemplaren worden toegewezen via upgradedomeinen. Het is niet mogelijk om op te geven welke instanties worden toegewezen aan een domein.
>
>

## <a name="next-steps"></a>Volgende stappen
[Cloudservices beheren](cloud-services-how-to-manage-portal.md)  
[Cloud Services bewaken](cloud-services-how-to-monitor.md)  
[Cloudservices configureren](cloud-services-how-to-configure-portal.md)  
