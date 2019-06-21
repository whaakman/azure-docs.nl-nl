---
title: Azure Service Fabric code pakketfouten vaststellen | Microsoft Docs
description: Meer informatie over het oplossen van veelvoorkomende fouten van code-pakket met Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: grzuber
manager: gkhanna
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/09/2019
ms.author: grzuber
ms.openlocfilehash: 235952388d2c044cc141b3020c67944c4250ea3d
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276949"
---
# <a name="diagnose-common-code-package-errors-with-service-fabric"></a>Veelvoorkomende fouten van code-pakket met Service Fabric onderzoeken

Dit artikel ziet u wat het betekent voor een codepakket onverwacht is beëindigd, en biedt inzicht in mogelijke oorzaken van algemene foutcodes, evenals de stappen voor probleemoplossing die mogelijk het probleem mogelijk verhelpen.

## <a name="when-does-a-process-or-container-terminate-unexpectedly"></a>Wanneer een proces of container onverwacht wordt beëindigd?

Wanneer Service Fabric ontvangt een verzoek om te beginnen een codepakket, begint het voorbereiden van de omgeving op het lokale systeem op basis van de configuratieopties instellen in de App en Service-manifesten. Deze voorbereidingen omvat mogelijk netwerkeindpunten of resources te reserveren, firewallregels configureren of resource governance beperkingen instellen. Nadat de omgeving goed is geconfigureerd, probeert de Service Fabric om de codepakket. Deze stap wordt beschouwd als geslaagd als de runtime van het besturingssysteem of de container rapporteert dat het proces of container met succes is geactiveerd. Als activering niet gelukt is, ziet u een bericht health in SFX waarin wordt gemeld

```
There was an error during CodePackage activation. Service host failed to activate. Error: 0xXXXXXXXX
```

Zodra het codepakket is pas, begint Service Fabric met de bewaking van hun levensduur. Een proces of container kunt op dit moment beëindigen op elk gewenst moment voor een aantal oorzaken hebben. Het kan zijn niet initialiseren van een DLL-bestand, het besturingssysteem kan beschikt niet over voldoende ruimte voor bureaublad-heap, enzovoort. Als uw codepakket is beëindigd, ziet u een bericht health in SFX waarin wordt gemeld

```
The process/container terminated with exit code: XXXXXXXX. Please look at your application logs/dump or debug your code package for more details. For information about common termination errors, please visit https://aka.ms/service-fabric-termination-errors
```

De afsluitcode opgegeven in de gezondheid van dit bericht is de enige aanwijzing geleverd door het proces/container over waarom deze beëindigd en het kan zijn gegenereerd door een willekeurig niveau van de stack. Het is moeilijk te begrijpen als deze afsluitcode is gerelateerd, bijvoorbeeld een OS-fout, een probleem met .NET, of door uw code is gegenereerd. Als gevolg hiervan in dit artikel kan worden gebruikt als een beginpunt voor het vaststellen van de bron van de afsluitcodes beëindiging en mogelijke oplossingen, rekening houdend met het er rekening mee dat deze algemene oplossingen voor veelvoorkomende scenario's zijn en kunnen niet van toepassing op de fout u ziet.

## <a name="how-can-i-tell-if-service-fabric-terminated-my-code-package"></a>Hoe kan ik zien als Service Fabric mijn codepakket beëindigd?

Service Fabric kan zijn verantwoordelijk voor het beëindigen van uw codepakket voor een aantal redenen. Het kan bijvoorbeeld besluiten te plaatsen van het codepakket op een ander knooppunt voor de taakverdeling doeleinden. Als uw codepakket door Service Fabric is beëindigd als u een van de afsluitcodes in de volgende tabel ziet, kunt u zien:

>[!NOTE]
> Als uw proces/container wordt beëindigd met de afsluitcode dan die in de volgende tabel, is Service Fabric niet verantwoordelijk voor het beëindigen.

Afsluitcode | Description
--------- | -----------
7147 | Deze foutcodes geven aan dat Service Fabric zonder problemen wordt afgesloten het proces/container door het verzenden van een signaal Ctrl + C.
7148 | Deze foutcodes geven aan dat de Service Fabric het proces/container beëindigd. Soms deze foutcode geeft aan dat het proces/container tijdig heeft niet gereageerd na het verzenden van een signaal Ctrl + C, dus er worden verwijderd.


## <a name="other-common-error-codes-and-their-potential-fixes"></a>Andere algemene foutcodes en hun mogelijke oplossingen

Afsluitcode | Hexadecimale waarde | Korte beschrijving | Hoofdoorzaak | Potential Fix
--------- | --------- | ----------------- | ---------- | -------------
3221225794 | 0xc0000142 | STATUS_DLL_INIT_FAILED | Deze fout betekent mogelijk dat de machine is uitgevoerd buiten de bureaublad-heap-ruimte. Deze oorzaak is het vooral waarschijnlijk hebt u een groot aantal processen die behoren tot de toepassing die wordt uitgevoerd op het knooppunt. | Als uw programma is niet gemaakt om te reageren op Ctrl + C signalen, kunt u de instelling 'EnableActivateNoWindow' in het Manifest van de Cluster inschakelen. Deze instelling is ingeschakeld, zou uw codepakket zou uitvoeren zonder een GUI-venster en Ctrl + C signalen niet ontvangt, maar zou verminderen de hoeveelheid ruimte van bureaublad-heap die elk proces worden verbruikt. Als uw codepakket Ctrl + C signalen ontvangen moet, kunt u de grootte van het bureaublad van uw knooppunt-heap verhogen.
3762504530 | 0xe0434352 | N/A | Deze waarde is de foutcode voor een niet-verwerkte uitzondering van beheerde code (dat wil zeggen, .NET). | Als u deze afsluitcode ziet, betekent dit dat uw toepassing heeft een uitzondering veroorzaakt die nog steeds niet-verwerkte en het proces beëindigd. Foutopsporing in Logboeken en dumpbestanden voor foutopsporing van uw toepassing, moet de eerste stap bij het bepalen van wat de fout heeft veroorzaakt.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [diagnose van andere algemene scenario's](service-fabric-diagnostics-common-scenarios.md)
* Ophalen van een meer gedetailleerd overzicht van Azure Monitor-logboeken en mogelijkheden van het startpakket door te lezen [wat Azure Monitor-Logboeken is?](../operations-management-suite/operations-management-suite-overview.md)
* Meer informatie over Azure Monitor logboeken [waarschuwingen](../log-analytics/log-analytics-alerts.md) voor detectie en diagnostiek.
* Familiarized ophalen met de [zoeken en uitvoeren van query's](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van Azure Monitor-Logboeken
