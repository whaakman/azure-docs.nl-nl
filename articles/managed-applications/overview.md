---
title: Overzicht van Azure beheerde toepassingen | Microsoft Docs
description: Beschrijft de concepten voor Azure beheerde toepassingen
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/26/2017
ms.author: tomfitz
ms.openlocfilehash: a26cfc632dacb41435b3755409d3e91630f5f3eb
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="azure-managed-applications-overview"></a>Overzicht van door Azure beheerde toepassingen

Beheerde Azure-toepassingen kunnen u cloudoplossingen bieden die gemakkelijk voor consumenten te implementeren en te gebruiken. U implementeert de infrastructuur en blijvende ondersteuning. Als u een beheerde toepassing beschikbaar voor alle klanten, deze te publiceren in de Azure marketplace. Om deze beschikbaar te maken alleen gebruikers in uw organisatie, kunt u het publiceren naar een interne catalogus. 

Een beheerde toepassing is vergelijkbaar met een oplossingssjabloon in de Marketplace, met één belangrijk verschil. In een beheerde toepassing de bronnen aan een resourcegroep die wordt beheerd door de uitgever van de app ingericht. De resourcegroep is aanwezig in de consument abonnement, maar een identiteit in de publisher tenant toegang heeft tot de resourcegroep. Als de uitgever in geeft u de kosten voor ondersteuning van de oplossing.

## <a name="advantages-of-managed-applications"></a>Voordelen van beheerde toepassingen

Beheerde toepassingen de barrières voor consumenten via uw oplossingen. Ze hoeft geen kennis in de cloudinfrastructuur voor uw oplossing gebruiken. Gebruikers hebben beperkte toegang tot kritieke bronnen. Ze hoeft niet te hoeven maken over het maken van een fout bij het beheren van deze. 

Beheerde toepassingen kunnen u een continue relatie tot stand brengen met uw consumenten. Definieert u de voorwaarden voor het beheren van de toepassing en alle kosten worden afgehandeld via de Azure-facturering.

Hoewel klanten deze beheerde toepassingen in hun abonnementen implementeren, zijn ze om onderhouden, bijwerken of deze service. U kunt ervoor zorgen dat alle klanten die van goedgekeurde versies gebruikmaken. Klanten hoeven toepassingsspecifieke domein kennis om deze toepassingen te beheren. Klanten wordt automatisch toepassingsupdates zonder de noodzaak zorgen over het oplossen van problemen en het onderzoeken van problemen met de toepassingen te verkrijgen. 

Voor IT-teams kunnen beheerde toepassingen u vooraf goedgekeurde oplossingen aan te bieden voor gebruikers in de organisatie. U ervoor zorgen deze oplossingen die compatibel zijn met organisatie-standaarden.

## <a name="types-of-managed-applications"></a>Typen van beheerde toepassingen

U kunt uw beheerde toepassing publiceren extern of intern.

![Intern of extern publiceren](./media/overview/manage_app_options.png)

### <a name="service-catalog"></a>Servicecatalogus

De Servicecatalogus is een interne catalogus van de goedgekeurde oplossingen voor gebruikers in een organisatie. U kunt de catalogus gebruiken om ervoor te zorgen dat bepaalde organisatie normen terwijl ze bieden van oplossingen voor de organisaties. De catalogus werknemers gebruiken voor het detecteren van de uitgebreide set toepassingen die zijn aanbevolen en goedgekeurd door hun IT-afdelingen eenvoudig. Ze zien dat de beheerde toepassingen die andere personen in hun organisatie met hen delen.

Zie voor meer informatie over het publiceren van een Servicecatalogus beheerde toepassing [-catalogus-servicetoepassing maken](publish-service-catalog-app.md).

### <a name="marketplace"></a>Marketplace

Leveranciers willen factureren voor hun services kunnen beheerde toepassingen beschikbaar maken via de Azure marketplace. Nadat de leverancier van een toepassing publiceert, is beschikbaar voor gebruikers buiten de organisatie. Met deze benadering, beheerde serviceproviders (MSPs), onafhankelijke softwareleveranciers (ISV's) en system kunnen SI's (SIs) hun oplossingen aanbieden aan alle Azure-klanten.

Zie voor meer informatie over het publiceren van een beheerde toepassing in de Marketplace [marketplace-toepassing maken](publish-marketplace-app.md).

## <a name="resource-groups-for-managed-applications"></a>Resourcegroepen voor beheerde toepassingen

De resources voor een beheerde toepassing zich doorgaans in twee resourcegroepen bevinden. De consument beheert een resourcegroep en de uitgever van de resourcegroep beheert. Bij het definiëren van de beheerde toepassing geeft de uitgever de toegangsniveaus. De volgende afbeelding toont een scenario waarbij de uitgever verzoekt om de rol van eigenaar voor de beheerde resourcegroep. De uitgever wordt een alleen-lezen-vergrendeling op deze resourcegroep geplaatst voor de consument.

![Toegang tot bedrijfsbronnen-groep](./media/overview/access.png)

### <a name="application-resource-group"></a>Groep van toepassingen resource

Deze resourcegroep bevat een exemplaar van de beheerde toepassing. Deze resourcegroep mag slechts één resource bevatten. Het brontype van de beheerde toepassing is **Microsoft.Solutions/applications**.

De consument heeft volledige toegang tot de resourcegroep en gebruikt voor het beheren van de levenscyclus van de beheerde toepassing.

### <a name="managed-resource-group"></a>Beheerde resourcegroep

Deze resourcegroep bevat alle resources die voor de beheerde toepassing vereist zijn. Deze resourcegroep bevat bijvoorbeeld de virtuele machines, opslagaccounts en virtuele netwerken voor de oplossing. De consument heeft toegang tot deze resourcegroep beperkt, omdat de consument de afzonderlijke resources voor de beheerde toepassing niet wordt beheerd. De uitgever toegang tot deze resourcegroep overeenkomt met de rol die is opgegeven in de definitie van beheerde toepassingsservices. De uitgever kan bijvoorbeeld verzoeken om de rol van eigenaar of bijdrager voor deze resourcegroep.

Wanneer de gebruiker de beheerde toepassing verwijdert, wordt de beheerde resourcegroep ook verwijderd.

## <a name="next-steps"></a>Volgende stappen

* Voor een inleiding te definiëren en implementeren van beheerde toepassingen, Zie [maken en implementeren van een Azure beheerde toepassing met Azure CLI](managed-apps-quickstart-cli.md)
* Zie voor meer informatie over het publiceren van een interne applicatie [-catalogus-servicetoepassing maken](publish-service-catalog-app.md).
* Zie voor meer informatie over het uitgeven van beheerde toepassingen op de markt [marketplace-toepassing maken](publish-marketplace-app.md).
