---
title: Het bijwerken van een service in de cloud | Microsoft Docs
description: Informatie over het bijwerken van cloudservices in Azure. Meer informatie over hoe een update op een cloudservice wordt uitgevoerd om beschikbaarheid te garanderen.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c6a8b5e6-5c99-454c-9911-5c7ae8d1af63
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 36d4ee9dabd39f4103d17455e47521b378af6ebb
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="how-to-update-a-cloud-service"></a>Het bijwerken van een service in de cloud

Bijwerken van een cloudservice, inclusief de functies en de gastbesturingssysteem, is een proces drie stappen. Eerst moeten de binaire bestanden en de configuratiebestanden voor de nieuwe cloudservice of de versie van het besturingssysteem worden geüpload. Vervolgens reserveert Azure compute- en netwerkbronnen voor de cloudservice op basis van de vereisten van de nieuwe versie van de cloud-service. Ten slotte voert Azure een uitrollende upgrade voor het bijwerken van de tenant stapsgewijs naar de nieuwe versie of een gastbesturingssysteem, behoud uw beschikbaarheid. Dit artikel worden de details van deze laatste stap – de rolling upgrade.

## <a name="update-an-azure-service"></a>Een Azure-Service bijwerken
Azure worden uw rolinstanties georganiseerd in logische groepen indelen upgradedomeinen (UD) genoemd. Upgradedomeinen (UD) zijn logische verzamelingen rolexemplaren die als een groep worden bijgewerkt.  Azure-updates een cloud service één UD tegelijk, waardoor exemplaren worden weergegeven in andere UDs om door te gaan voor verkeer.

Aantal upgradedomeinen is 5. U kunt een verschillend aantal upgradedomeinen opgeven door het kenmerk upgradeDomainCount in de service-definitiebestand (.csdef). Zie voor meer informatie over het kenmerk upgradeDomainCount [WebRole Schema](https://msdn.microsoft.com/library/azure/gg557553.aspx) of [WorkerRole Schema](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Wanneer u een update in plaats van een of meer rollen in uw service uitvoert, Azure-updates sets rolinstanties volgens het upgradedomein waartoe ze behoren. Alle exemplaren van in een opgegeven upgradedomein – stoppen, bijwerken, worden gezet online – back-Azure-updates gaat vervolgens naar het volgende domein. Door alleen de exemplaren die worden uitgevoerd in het huidige upgradedomein stoppen, ervoor Azure zorgt dat een update met het zo weinig mogelijk gevolgen voor de uitgevoerde service plaatsvindt. Zie voor meer informatie [hoe de update wordt voortgezet](#howanupgradeproceeds) verderop in dit artikel.

> [!NOTE]
> Terwijl de voorwaarden **bijwerken** en **upgrade** iets andere betekenis hebben in de context van Azure, ze door elkaar kunnen worden gebruikt voor de processen en beschrijvingen van de functies in dit document.
>
>

Uw service moet ten minste twee exemplaren van een rol voor deze rol worden bijgewerkt definiëren in-place zonder uitvaltijd. Als de service uit slechts één exemplaar van een rol bestaat, zijn uw service niet beschikbaar totdat de update in-place is voltooid.

In dit onderwerp worden de volgende informatie over Azure-updates:

* [Servicewijzigingen toegestaan tijdens het bijwerken](#AllowedChanges)
* [Hoe een upgrade wordt uitgevoerd](#howanupgradeproceeds)
* [Terugdraaien van een update](#RollbackofanUpdate)
* [Meerdere mutating bewerkingen op een actieve implementatie initiëren](#multiplemutatingoperations)
* [Distributie van functies tussen domeinen upgraden](#distributiondfroles)

<a name="AllowedChanges"></a>

## <a name="allowed-service-changes-during-an-update"></a>Servicewijzigingen toegestaan tijdens het bijwerken
De volgende tabel toont de toegestane wijzigingen in een service tijdens het bijwerken:

| Wijzigingen die zijn toegestaan als host fungeert, services en functies | InPlace-update | Gefaseerde (VIP's wisselen) | Verwijderen en opnieuw implementeren |
| --- | --- | --- | --- |
| Besturingssysteem |Ja |Ja |Ja |
| .NET-vertrouwensniveau |Ja |Ja |Ja |
| Grootte van virtuele machine<sup>1</sup> |Ja<sup>2</sup> |Ja |Ja |
| Instellingen voor de lokale opslag |Alleen verhogen<sup>2</sup> |Ja |Ja |
| Toevoegen of verwijderen van rollen in een service |Ja |Ja |Ja |
| Aantal exemplaren van een bepaalde rol |Ja |Ja |Ja |
| Aantal of type eindpunten voor een service |Ja<sup>2</sup> |Nee |Ja |
| Namen en waarden van configuratie-instellingen |Ja |Ja |Ja |
| Waarden (maar niet de namen van) van configuratie-instellingen |Ja |Ja |Ja |
| Nieuwe certificaten toevoegen |Ja |Ja |Ja |
| Wijzigen van bestaande certificaten |Ja |Ja |Ja |
| Implementeer nieuwe code |Ja |Ja |Ja |

<sup>1</sup> formaat wijzigen beperkt tot de subset met grootten beschikbaar voor de cloudservice.

<sup>2</sup> vereist Azure SDK 1.5 of hoger.

> [!WARNING]
> De grootte van de virtuele machine wijzigen, zullen lokale gegevens.
>
>

De volgende items worden niet ondersteund tijdens het bijwerken:

* De naam van een rol wijzigen. Verwijderen en voeg vervolgens de rol met de nieuwe naam.
* Het wijzigen van het aantal upgraden-domein.
* De grootte van de lokale resources wordt verminderd.

Als u andere updates aan de definitie van uw service, zoals het verlagen van de grootte van de lokale resource moet u een VIP wisselen update uitvoeren. Zie voor meer informatie [wisselen implementatie](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>

## <a name="how-an-upgrade-proceeds"></a>Hoe een upgrade wordt uitgevoerd
U kunt beslissen of u wilt bijwerken van de rollen in uw service of een enkele rol in de service. In beide gevallen worden alle exemplaren van elke rol die wordt bijgewerkt en deel uitmaken van het eerste upgradedomein is gestopt, bijgewerkt en weer online. Zodra deze weer online zijn, zijn de exemplaren in de tweede upgradedomein is gestopt, bijgewerkt en weer online. Een cloudservice kan maximaal één upgrade tegelijk actief hebben. De upgrade wordt altijd uitgevoerd op de meest recente versie van de cloudservice.

Het volgende diagram illustreert hoe de upgrade wordt uitgevoerd als u een van alle van de rollen in de service upgrade:

![Upgrade van service](media/cloud-services-update-azure-service/IC345879.png "Upgrade van service")

Deze volgende diagram ziet u hoe de update wordt uitgevoerd als u slechts één functie bijwerkt:

![Upgrade rol](media/cloud-services-update-azure-service/IC345880.png "Upgrade rol")  

De Azure-Infrastructuurcontroller tijdens een automatische update periodiek de status van de cloudservice om te bepalen wanneer dit veilig Doorloop de volgende UD geëvalueerd. Deze health-evaluatie wordt uitgevoerd op basis van per rol en overweegt alleen exemplaren in de meest recente versie (dat wil zeggen de exemplaren van UDs die al zijn gelopen). Controleert of dat een minimum aantal rolexemplaren, voor elke rol, hebben een goede definitieve status bereikt.

### <a name="role-instance-start-timeout"></a>Time-out voor de Start van de rol exemplaar
De Controller wacht 30 minuten voor elk rolexemplaar een gestart status bereikte. Als de time-outduur verstrijkt, blijft de Controller voor roulatie van het volgende rolexemplaar.

### <a name="impact-to-drive-data-during-cloud-service-upgrades"></a>Gevolgen voor de gegevens van de stations tijdens de Cloudservice upgrades

Bij het bijwerken van een service vanuit één exemplaar naar meerdere exemplaren wordt uw service worden verbroken tijdens de upgrade wordt uitgevoerd vanwege de manier waarop Azure upgrades-services. Service level agreement aansprakelijke beschikbaarheid van de service is alleen van toepassing op services die zijn geïmplementeerd met meer dan één exemplaar. De volgende lijst wordt beschreven hoe de gegevens op elk station wordt beïnvloed door elke Azure-service-upgrade scenario:

|Scenario|C-schijf|D-station|E-station|
|--------|-------|-------|-------|
|VM opnieuw wordt opgestart|Behouden|Behouden|Behouden|
|Portal opnieuw opstarten|Behouden|Behouden|Vernietigd|
|Portal terugzetten van de installatiekopie|Behouden|Vernietigd|Vernietigd|
|In-Place Upgrade|Behouden|Behouden|Vernietigd|
|Migratie van knooppunt|Vernietigd|Vernietigd|Vernietigd|

Houd er rekening mee dat in de bovenstaande lijst de station E: het basisstation van de rol vertegenwoordigt, en niet vastgelegd worden moet. Gebruik in plaats daarvan de **% RoleRoot %** omgevingsvariabele vertegenwoordigt het station.

Als u wilt de downtime minimaliseren bij een upgrade van een single instance-service, een nieuwe meerdere exemplaren service implementeren in de staging-server en geen VIP's wisselen.

<a name="RollbackofanUpdate"></a>

## <a name="rollback-of-an-update"></a>Terugdraaien van een update
Azure biedt flexibiliteit voor het beheren van services tijdens het bijwerken doordat u kunt aanvullende bewerkingen op een service starten na de initiële update-aanvraag wordt geaccepteerd door de Azure-Infrastructuurcontroller. Terugdraaien kan alleen worden uitgevoerd wanneer een update (configuratie wijzigen) of upgrade wordt het **Bezig** status op de implementatie. Een update of upgrade wordt beschouwd als wordt uitgevoerd, zolang er is ten minste één exemplaar van de service die nog niet zijn bijgewerkt naar de nieuwe versie. Als u wilt testen of een terugdraaien is toegestaan, Controleer de waarde van de vlag RollbackAllowed geretourneerd door [ophalen implementatie](https://msdn.microsoft.com/library/azure/ee460804.aspx) en [Cloud Service-eigenschappen ophalen](https://msdn.microsoft.com/library/azure/ee460806.aspx) bewerkingen, is ingesteld op true.

> [!NOTE]
> Het alleen zinvol om aan te roepen terugdraaien op een **in-place** bijwerken of bijwerken omdat het VIP wisselen upgrades hebben betrekking op één volledige actief exemplaar van uw service met een andere vervangen.
>
>

Terugdraaien van een update wordt uitgevoerd, heeft de volgende gevolgen voor de implementatie:

* Alle rolinstanties die was nog niet zijn bijgewerkt of upgrade naar de nieuwe versie worden niet bijgewerkt of bijgewerkt, omdat de instanties van de doelversie van de service al wordt uitgevoerd.
* Alle rolinstanties die u al had zijn bijgewerkt of bijgewerkt naar de nieuwe versie van het servicepakket (\*.cspkg) bestand of de serviceconfiguratie (\*.cscfg)-bestand (of beide bestanden) de vóór de upgrade versie van deze bestanden zijn hersteld.

Dit is functioneel bepaald door de volgende functies:

* De [terugdraaien bijwerken of bijwerken](https://msdn.microsoft.com/library/azure/hh403977.aspx) bewerking die kan worden aangeroepen voor een configuratie-update (geactiveerd door het aanroepen van [implementatie-configuratie wijzigen](https://msdn.microsoft.com/library/azure/ee460809.aspx)) of een upgrade (geactiveerd door het aanroepen van [ Upgrade-implementatie](https://msdn.microsoft.com/library/azure/ee460793.aspx)), zolang er is ten minste één exemplaar in de service die nog niet zijn bijgewerkt naar de nieuwe versie.
* Het element vergrendeld en het element RollbackAllowed worden geretourneerd als onderdeel van de hoofdtekst van de reactie van de [ophalen implementatie](https://msdn.microsoft.com/library/azure/ee460804.aspx) en [Cloud Service-eigenschappen ophalen](https://msdn.microsoft.com/library/azure/ee460806.aspx) bewerkingen:

  1. Het element vergrendeld, kunt u detecteren wanneer een mutating bewerking kan worden aangeroepen voor een bepaalde implementatie.
  2. Het element RollbackAllowed kunt u detecteren wanneer de [terugdraaien van de Update of Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) bewerking kan worden aangeroepen voor een bepaalde implementatie.

  U hebt om uit te voeren een terugdraaiactie heeft plaatsgevonden, niet zowel de vergrendeld als de RollbackAllowed-elementen. Deze achtervoegsels om te bevestigen dat RollbackAllowed is ingesteld op true. Deze elementen zijn alleen geretourneerd als deze methoden worden aangeroepen met behulp van de aanvraagheader ingesteld op ' x-ms-version: 2011-10-01 ' of een latere versie. Zie voor meer informatie over kopteksten versioning [Management Serviceversiebeheer](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Zijn er situaties waarin een terugdraaien van een update of upgrade wordt niet ondersteund, dit als volgt zijn:

* Verlaging van de lokale bronnen - als de update de lokale bronnen voor een functie van de Azure-platform verhoogt kunnen niet worden teruggedraaid.
* Quotumbeperkingen - hebben als de update is een schaal omlaag bewerking die u mogelijk niet meer voldoende quotum aangepaste compute om het van de terugdraaibewerking te voltooien. Elk Azure-abonnement heeft een quotum gekoppeld waarmee het maximum aantal kernen dat kan worden gebruikt door alle gehoste services die deel uitmaken van dat abonnement. Als u dat terugdraaien van een bepaalde update uw abonnement via quotum vervolgens die plaatst wordt een rollback niet ingeschakeld.
* Race condition - als de eerste update is voltooid, terugdraaien is niet mogelijk.

Een voorbeeld van wanneer het terugdraaien van een update mogelijk nuttig is wanneer u gebruikt de [Upgrade-implementatie](https://msdn.microsoft.com/library/azure/ee460793.aspx) bewerking in de handmatige modus voor het beheren van de snelheid waarmee een belangrijke in-place upgrade uw Azure service gehoste is geïmplementeerd.

Tijdens de implementatie van de upgrade u aanroepen [Upgrade-implementatie](https://msdn.microsoft.com/library/azure/ee460793.aspx) in de handmatige modus en doorloop upgradedomeinen beginnen. Als op een bepaald moment tijdens het controleren van de upgrade moet u rekening mee houden sommige rolinstanties in de eerste upgradedomeinen die u hebt niet meer reageren, kunt u bellen de [terugdraaien van de Update of Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) bewerking op de implementatie blijft nog op de exemplaren die was nog niet zijn bijgewerkt en de rollback-serverexemplaren die is bijgewerkt naar de vorige servicepakket en configuratie.

<a name="multiplemutatingoperations"></a>

## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Meerdere mutating bewerkingen op een actieve implementatie initiëren
In sommige gevallen wilt u mogelijk meerdere gelijktijdige mutating bewerkingen voor de implementatie van een doorlopende starten. Bijvoorbeeld u een service-update kunt uitvoeren en, terwijl deze update is teruggedraaid uit over uw service, die u wilt bijvoorbeeld een wijziging aanbrengen om te draaien terug in de update, een andere update toepassen of zelfs het verwijderen van de implementatie. Een aanvraag waarin dit wordt mogelijk nodig is als een service-upgrade bevat buggy code die ervoor zorgt dat een bijgewerkte rolinstantie herhaaldelijk vastloopt. In dit geval is de Azure-Infrastructuurcontroller niet mogelijk voortgang bij de toepassing die niet upgraden omdat een onvoldoende aantal exemplaren in de bijgewerkte domein zijn in orde. Deze status wordt aangeduid als een *vastgelopen implementatie*. U kunt de implementatie door het terugdraaien van de update of een nieuwe update toepassen via de bovenkant van het mislukken van de Registreer een.

Nadat de eerste aanvraag bijwerken of upgrade van de service is ontvangen door de Azure-Infrastructuurcontroller, kunt u verdere mutating bewerkingen starten. U hebt dat wil zeggen, niet te wachten op de eerste bewerking is voltooid voordat u kunt een andere mutating bewerking.

Een tweede update-bewerking starten terwijl de eerste update uitgevoerd wordt wordt vergelijkbaar met de rollback-bewerking uitgevoerd. Als de tweede update in de automatische modus is wordt het eerste upgradedomein bijgewerkt onmiddellijk mogelijk leiden tot exemplaren van meerdere upgradedomeinen wordt offline op een bepaald punt in tijd.

De mutating bewerkingen zijn als volgt: [implementatie-configuratie wijzigen](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Upgrade-implementatie](https://msdn.microsoft.com/library/azure/ee460793.aspx), [Update Implementatiestatus](https://msdn.microsoft.com/library/azure/ee460808.aspx), [implementatie verwijderen ](https://msdn.microsoft.com/library/azure/ee460815.aspx), en [terugdraaien van de Update of Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Twee bewerkingen [ophalen implementatie](https://msdn.microsoft.com/library/azure/ee460804.aspx) en [Cloud Service-eigenschappen ophalen](https://msdn.microsoft.com/library/azure/ee460806.aspx), de vergrendeld vlag waarmee kan worden onderzocht om te bepalen of een mutating bewerking kan worden aangeroepen voor een bepaalde implementatie te retourneren.

Als u wilt de versie van deze methoden die als resultaat van de vlag vergrendeld aanroepen, moet u aanvraagheader instellen op ' x-ms-version: 2011-10-01 ' of een later. Zie voor meer informatie over kopteksten versioning [Management Serviceversiebeheer](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>

## <a name="distribution-of-roles-across-upgrade-domains"></a>Distributie van functies tussen domeinen upgraden
Azure worden exemplaren van een rol gelijkmatig verdeeld over een bepaald aantal upgradedomeinen die kan worden geconfigureerd als onderdeel van het servicedefinitiebestand (.csdef). Het maximum aantal upgradedomeinen is 20 en de standaardwaarde is 5. Zie voor meer informatie over het wijzigen van het servicedefinitiebestand [Azure Service definitie Schema (.csdef-bestand)](cloud-services-model-and-package.md#csdef).

Bijvoorbeeld, als uw rol tien exemplaren heeft, bevat elk upgradedomein standaard twee exemplaren. Als uw rol 14 exemplaren heeft, bevat vier van de upgradedomeinen drie exemplaren en een vijfde domein bevat twee.

Upgradedomeinen worden aangeduid met een op nul gebaseerde index: het eerste upgradedomein heeft een 0-ID en het tweede upgradedomein heeft een ID van 1, enzovoort.

Het volgende diagram illustreert hoe een service dan bevat twee rollen worden gedistribueerd wanneer de service definieert twee upgradedomeinen. De service wordt uitgevoerd acht exemplaren van de Webrol en negen exemplaren van de werkrol.

![Distributie van Upgradedomeinen](media/cloud-services-update-azure-service/IC345533.png "distributie van Upgradedomeinen")

> [!NOTE]
> Houd er rekening mee dat Azure bepaalt hoe exemplaren worden toegewezen via upgradedomeinen. Het is niet mogelijk om op te geven welke exemplaren zijn toegewezen aan een domein.
>
>

## <a name="next-steps"></a>Volgende stappen
[Cloud Services beheren](cloud-services-how-to-manage-portal.md)  
[Het bewaken van Cloud-Services](cloud-services-how-to-monitor.md)  
[Cloud Services configureren](cloud-services-how-to-configure-portal.md)  
