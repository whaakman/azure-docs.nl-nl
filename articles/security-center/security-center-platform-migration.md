---
title: Migratie van Azure Security Center-platform | Microsoft Docs
description: In dit document wordt een uitleg gegeven over enkele wijzigingen in de wijze waarop Azure Security Center-gegevens worden verzameld.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 80246b00-bdb8-4bbc-af54-06b7d12acf58
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: yurid
ms.openlocfilehash: 5ddf71dcd9c5a2b03e3b1441d8c9b4d91b6bad12
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-center-platform-migration"></a>Migratie van Azure Security Center-platform

Vanaf begin juni 2017 implementeert Azure Security Center belangrijke wijzigingen in de manier waarop beveiligingsgegevens worden verzameld en opgeslagen.  Deze wijzigingen bieden nieuwe mogelijkheden, zoals de mogelijkheid om eenvoudig beveiligingsgegevens te zoeken en een betere afstemming op andere Azure-services voor beheer en controle.

> [!NOTE]
> De platformmigratie heeft geen invloed op uw productieresources en u hoeft geen actie te ondernemen.


## <a name="whats-happening-during-this-platform-migration"></a>Wat gebeurt er tijdens deze platformmigratie?

Voorheen gebruikte Security Center de Azure Monitoring Agent voor het verzamelen van beveiligingsgegevens van uw virtuele machines. Dit is inclusief informatie over beveiligingsconfiguraties, die worden gebruikt voor het identificeren van kwetsbaarheden, en beveiligingsgebeurtenissen, die worden gebruikt voor het detecteren van bedreigingen. Deze gegevens werden opgeslagen in uw opslagaccounts in Azure.

Voortaan maakt Security Center gebruik van de Microsoft Monitoring Agent. Dit is dezelfde agent die wordt gebruikt door de Operations Management Suite en de Log Analytics-service. Gegevens die via deze agent worden verzameld, worden opgeslagen in een bestaande *Log Analytics*-[werkruimte](../log-analytics/log-analytics-manage-access.md) die is gekoppeld aan uw Azure-abonnement, of in nieuwe werkruimten, rekening houdend met de geolocatie van de virtuele machine.

## <a name="agent"></a>Agent

Als onderdeel van de overgang wordt de Microsoft Monitoring Agent (voor [Windows](../log-analytics/log-analytics-windows-agents.md) of [Linux](../log-analytics/log-analytics-linux-agents.md)) geïnstalleerd op alle virtuele machines van Azure waarop momenteel gegevens worden verzameld.  Als de Microsoft Monitoring Agent al op de virtuele machine is geïnstalleerd, maakt Security Center gebruik van de huidige geïnstalleerde agent.

Voor een bepaalde tijd (doorgaans een paar dagen) worden beide agents naast elkaar uitgevoerd om te zorgen voor een soepele overgang zonder verlies van gegevens. Dit stelt Microsoft in staat om te controleren of de nieuwe gegevenspijplijn operationeel is voordat wordt gestopt met het gebruik van de huidige pijplijn. Nadat dit is geverifieerd, wordt de Azure Monitoring Agent verwijderd van uw virtuele machines. U hoeft hier niets voor te doen. U ontvangt een e-mailbericht wanneer alle klanten die zijn gemigreerd.
 
Het is niet raadzaam de Azure Monitoring Agent handmatig te verwijderen tijdens de migratie, omdat dit kan leiden tot hiaten in de beveiligingsgegevens. Neem contact op met [Microsoft Customer Service and Support](https://support.microsoft.com/contactus/) als u meer hulp nodig hebt. 

De Microsoft Monitoring Agent voor Windows vereist het gebruik van TCP-poort 443. Lees de [Handleiding voor het oplossen van problemen met Azure Security Center](security-center-troubleshooting-guide.md) voor meer informatie.


> [!NOTE] 
> Omdat de Microsoft Monitoring Agent kan worden gebruikt door andere Azure-services voor beheer en controle, wordt de agent niet automatisch verwijderd wanneer u gegevens verzamelen uitschakelt in Security Center. U kunt de agent echter zo nodig handmatig verwijderen.

## <a name="workspace"></a>Werkruimte

Zoals eerder beschreven, worden gegevens die (namens Security Center) via de Microsoft Monitoring Agent worden verzameld, opgeslagen in een bestaande Log Analytics-werkruimte die is gekoppeld aan uw Azure-abonnement, of in nieuwe werkruimten, rekening houdend met de geolocatie van de virtuele machine.

In Azure Portal kunt u bladeren om een overzicht te zien van uw Log Analytics-werkruimten, inclusief alle werkruimten die door Security Center zijn gemaakt. Een gerelateerde resourcegroep wordt gemaakt voor nieuwe werkruimten. Werkruimten en resourcegroepen volgen beide deze naamconventie:

- Werkruimte: *Standaardwerkruimte-[abonnement-ID]-[geo]*
- Resourcegroep: *Standaardresoucegroep-[geo]* 
 
Voor werkruimten die zijn gemaakt door Security Center worden gegevens 30 dagen bewaard. Voor bestaande werkruimten is de bewaarperiode gebaseerd op de prijscategorie van de werkruimte.

> [!NOTE]
> Gegevens die eerder door Security Center werden verzameld, blijven aanwezig in uw opslagaccounts. Nadat de migratie is voltooid, kunt u deze opslagaccounts verwijderen.

### <a name="oms-security-solution"></a>OMS-beveiligingsoplossing 

Voor bestaande klanten die geen OMS-beveiligingsoplossing hebben geïnstalleerd, installeert Microsoft deze in hun werkruimte. De oplossing is echter alleen gericht op virtuele machines van Azure. U moet deze oplossing niet verwijderen, omdat er geen automatisch herstel is als dit wordt gedaan via de OMS-beheerconsole.


## <a name="other-updates"></a>Andere Updates

Samen met de platformmigratie rollen we een aantal kleine updates uit:

- Aanvullende versies van het besturingssysteem worden ondersteund. Bekijk de lijst [hier](security-center-faq.md#virtual-machines).
- De lijst met beveiligingslekken in het besturingssysteem wordt uitgebreid. Bekijk de lijst [hier](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- [Prijzen](https://azure.microsoft.com/pricing/details/security-center/) zijn pro rato per uur (voorheen was dit per dag). Voor sommige klanten zal dit leiden tot kostenbesparingen.
- Gegevensverzameling is vereist en wordt automatisch ingeschakeld voor klanten in de prijscategorie Standard.
- Azure Security Center start met het detecteren van antimalware-oplossingen die niet zijn geïmplementeerd via Azure-extensies. Detectie van Symantec Endpoint Protection en Defender voor Windows 2016 zal als eerste beschikbaar zijn.
- Preventiebeleid en -meldingen kunnen alleen worden alleen geconfigureerd op *abonnementsniveau*, maar de prijzen kunnen nog steeds worden ingesteld op *resourcegroepniveau*

