---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 0eb47c8ec470ef05f3c6ae37bdc75e5bb1043eb0
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55736245"
---
Als u problemen ondervindt tijdens het uitvoeren van bewerkingen op galerieën met gedeelde installatiekopieën, definities van installatiekopieën en versies van installatiekopieën, voert u de mislukte opdracht opnieuw uit in de foutopsporingsmodus. Foutopsporingsmodus is geactiveerd door door te geven de **-debug** overschakelen met CLI en de **-Debug** overschakelen met PowerShell. Nadat u de fout hebt gevonden, gaat u als volgt dit document om op te lossen de fouten.


## <a name="unable-to-create-a-shared-image-gallery"></a>Kan niet worden gemaakt van een gedeelde Afbeeldingengalerie

Mogelijke oorzaken:

*Naam van de galerie is ongeldig.*

Toegestane tekens voor de naam van de galerie worden hoofdletters of kleine letters, cijfers, punten en perioden. Naam van de galerie geen streepjes erin. Wijzig de naam van de galerie en probeer het opnieuw. 

*Naam van de galerie is niet uniek binnen uw abonnement.*

Kies een andere naam van de galerie en probeer het opnieuw.


## <a name="unable-to-create-an-image-definition"></a>Kan de definitie van een installatiekopie maken 

Mogelijke oorzaken:

*naam van installatiekopie-definitie is ongeldig.*

Toegestane tekens in voor de definitie van installatiekopie zijn hoofdletters of kleine letters, cijfers, punten, streepjes en punten. Wijzig de naam van de installatiekopie en probeer het opnieuw.

*De verplichte eigenschappen voor het maken van de definitie van een installatiekopie worden niet ingevuld.*

De eigenschappen zoals naam, uitgever, aanbieding, sku en type besturingssysteem zijn verplicht. Controleer of als alle eigenschappen die worden doorgegeven.

Zorg ervoor dat de **OSType**, Linux of Windows van de definitie van de installatiekopie is hetzelfde als de bron installatiekopie die u gebruikt beheerde voor het maken van de versie van de installatiekopie. 


## <a name="unable-to-create-an-image-version"></a>Kan de versie van een installatiekopie maken 

Mogelijke oorzaken:

*Naam van installatiekopie-versie is ongeldig.*

Toegestane tekens in voor de versie van installatiekopie zijn cijfers en punten. Cijfers moet binnen het bereik van een 32-bits geheel getal zijn. Indeling: *MajorVersion.MinorVersion.Patch*. Wijzig de naam van de installatiekopie-versie en probeer het opnieuw.

*Beheerde broninstallatiekopie van welke versie van de installatiekopie wordt gemaakt, is niet gevonden.* 

Controleer of de installatiekopie van de bron bestaat en zich in dezelfde regio als de versie van de installatiekopie.

*De beheerde installatiekopie is niet uitgevoerd wordt ingericht.*

Zorg ervoor dat de Inrichtingsstatus van de beheerde bronafbeelding is **geslaagd**.

*De regio van de gegevensbron wordt nog niet ondersteund.*

Gebruik de onderstaande tabel om te zien of de beoogde bronregio wordt ondersteund:
<br>

| Galerie in of 'bronregio' maken   | Versie of "doelregio" repliceren |
|----------------------------------------|-------------------------------------------|
| US - west-centraal                        | Alle openbare Azure-Cloud-regio 's            |
| US - zuid-centraal                       |                                           |
| US - oost 2                              |                                           |
| Azië - zuidoost                         |                                           |
| Europa -west                            |                                           |

<br>

De lijst met doelservers regio moet de regio van de gegevensbron van de versie van de installatiekopie bevatten. Zorg ervoor dat u hebt de bronregio opgenomen in de lijst met doelregio's waar u Azure voor het repliceren van uw versie van de installatiekopie aan.

*Replicatie naar alle doelregio's niet voltooid.*

Gebruik de **--Vouw ReplicationStatus** vlag om te controleren als de replicatie naar alle regio's van het opgegeven doel is voltooid. Als dat niet het geval is, wacht u maximaal zes uur voor de taak is voltooid. Als dit mislukt, voert u de opdracht opnieuw uit voor het maken en repliceren van de versie van de installatiekopie. Als er een groot aantal doelregio's die versie van de installatiekopie wordt gerepliceerd naar, kunt u in dat de replicatie in fasen.

## <a name="unable-to-create-a-vm-or-a-scale-set"></a>Kan een virtuele machine of een schaalset maken 

Mogelijke oorzaken:

*De gebruiker bij het maken van een schaalset voor virtuele machine of virtuele machine geen leestoegang voor de versie van de installatiekopie.*

Neem contact op met de eigenaar van het abonnement en vraag deze om te lezen-toegang geven tot de versie van de installatiekopie of de bovenliggende resources (zoals de galerie met installatiekopieën van gedeelde of de definitie van installatiekopie) via [rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles) (RBAC). 

*Versie van de installatiekopie is niet gevonden.*

Controleer of dat de regio die u probeert te maken van de schaal van een virtuele machine of virtuele machine in is opgenomen in de lijst met doelregio's van de versie van de installatiekopie. Als de regio al in de lijst met doelregio's is, controleert u of als de replicatietaak is voltooid. U kunt de **- ReplicationStatus** vlag om te controleren als de replicatie naar alle regio's van het opgegeven doel is voltooid. 

*De virtuele machine of virtuele machine-schaalset maken veel tijd kost.*

Controleer de **OSType** van de installatiekopie heeft versie die u probeert te maken van de virtuele machine of virtuele-machineschaalset van hetzelfde **OSType** beheerde installatiekopie die u gebruikt voor het maken van de versie van de installatiekopie van de bron. 

## <a name="unable-to-share-resources"></a>Kan geen resources delen

Het delen van gedeelde afbeeldingengalerie, de definitie van installatiekopie en afbeelding versie resources tussen abonnementen is ingeschakeld met behulp van [rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles) (RBAC). 

## <a name="replication-is-slow"></a>Replicatie is traag

Gebruik de **--Vouw ReplicationStatus** vlag om te controleren als de replicatie naar alle regio's van het opgegeven doel is voltooid. Als dat niet het geval is, wacht u maximaal zes uur voor de taak is voltooid. Als dit mislukt, activeert u de opdracht opnieuw op te maken en repliceren van de versie van de installatiekopie. Als er een groot aantal doelregio's die versie van de installatiekopie wordt gerepliceerd naar, kunt u in dat de replicatie in fasen.

## <a name="azure-limits-and-quotas"></a>Azure-limieten en quota 

[Azure-limieten en quota's](https://docs.microsoft.com/azure/azure-subscription-service-limits) gelden voor alle gedeelde afbeeldingengalerie, de definitie van installatiekopie en afbeeldingsresources versie. Zorg ervoor dat u zich binnen de grenzen van uw abonnementen. 



