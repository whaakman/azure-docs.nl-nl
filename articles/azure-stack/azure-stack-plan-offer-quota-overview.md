---
title: Stack plan, aanbieding, quota en abonnement overzicht van Azure | Microsoft Docs
description: Als een cloud-operator wil ik Azure Stack plannen, aanbiedingen, quota's en -abonnementen te begrijpen.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 8/22/2017
ms.author: erikje
ms.openlocfilehash: 083ca2f0a06625810d2f90a682ba0b3110032e60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="plan-offer-quota-and-subscription-overview"></a>Overzicht van plannen, aanbiedingen, quota en abonnementen

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

[Azure Stack](azure-stack-poc.md) kunt u een groot aantal leveren services, zoals virtuele machines, SQL Server-databases, SharePoint, Exchange, en zelfs [Azure Marketplace-items](azure-stack-marketplace-azure-items.md). Als een Azure-Stack-operator, configureren en deze services in Azure-Stack leveren via plannen, aanbiedingen en quota's.

De aanbiedingen die een of meer abonnementen bevatten en elk plan bevat een of meer services. Door het maken van abonnementen en ze combineren in verschillende aanbiedingen, die u beheert
- welke services en bronnen gebruikers hebben toegang tot
- het bedrag van de resources die gebruikers kunnen gebruiken
- welke regio's hebben toegang tot de bronnen

Wanneer u een service leveren, moet u deze stappen op hoog niveau volgen:

1. Een service die u wilt ervoor zorgen dat uw gebruikers toevoegen.
2. Maak een plan dat een of meer services bevat. Wanneer u een plan maakt, wordt u selecteren of maken van quota die de limieten van elke service in het plan definiëren.
3. Maak een aanbieding met een of meer plannen (inclusief base plannen en optionele invoegtoepassing plannen).

Nadat u de aanbieding hebt gemaakt, kunnen uw gebruikers zich abonneren op deze toegang krijgt tot de services en bronnen biedt. Gebruikers kunnen zich abonneren op zoveel voorstellen als ze willen. Het volgende diagram toont een eenvoudig voorbeeld van een gebruiker die is geabonneerd op twee aanbiedingen. Elke aanbieding heeft een plan of twee en elk plan hen toegang geeft tot services.

![](media/azure-stack-key-features/image4.png)

## <a name="plans"></a>Abonnementen

Plannen voor zijn groepen van een of meer services. Als operator Azure Stack u [plannen maken](azure-stack-create-plan.md) te bieden aan uw gebruikers. Uw gebruikers abonneren op zijn beurt op uw aanbiedingen plannen en services die ze gebruiken. Bij het maken van abonnementen, Controleer uw quota instellen, uw plannen voor basis te definiëren en overwegen optionele invoegtoepassing plannen.

### <a name="quotas"></a>Quota

Om u te helpen bij het beheren van de cloudcapaciteit van de, selecteer of maak een quotum voor elke service in een plan. Quota's definiëren de bovenste limieten die een gebruikerabonnement kan inrichten of gebruiken. Een quotum kunt bijvoorbeeld een gebruiker tot vijf virtuele machines maken. Quota's kunnen beperken tal van bronnen, zoals virtuele machines, RAM-geheugen en CPU limieten.

Quota's kunnen worden geconfigureerd per regio. Een plan met compute services van een regio kan bijvoorbeeld een quotum van twee virtuele machines, 4 GB RAM-geheugen en 10 CPU-kernen hebben. In de Azure-Stack Development Kit, slechts één regio (met de naam *lokale*) is beschikbaar.

### <a name="base-plan"></a>Basisplan

Wanneer u een aanbieding maakt, kan de service-beheerder een basisplan bevatten. Deze base plannen zijn standaard opgenomen als een gebruiker zich op deze aanbieding abonneert. Wanneer een gebruiker zich abonneert, hebben ze toegang tot alle resourceproviders die zijn opgegeven in deze base plannen (met de bijbehorende quota's).

### <a name="add-on-plans"></a>Invoegtoepassing plannen

U kunt ook optionele invoegtoepassing plannen in een aanbieding opnemen. Plannen van de invoegtoepassing zijn niet standaard opgenomen in het abonnement. Plannen van de invoegtoepassing zijn aanvullende schema's (met quota's) beschikbaar in een aanbieding met een abonnee aan hun abonnementen toevoegen kunt. U kunt bijvoorbeeld een basisplan met beperkte bronnen voor een proefversie, en een invoegtoepassing plannen met meer resources aanbieden aan klanten die u kiest voor de service.

## <a name="offers"></a>Aanbiedingen

Aanbiedingen zijn een of meer abonnementen die u maakt zodat gebruikers zich op deze abonneren kunnen groepen. Bijvoorbeeld, bieden Alpha Plan een kan bevatten die een set van compute services en Plan B die een set van services voor opslag en netwerk. 

Wanneer u [maken van een aanbieding](azure-stack-create-offer.md), moet u ten minste één basisplan opnemen, maar u kunt ook extra plannen die gebruikers aan hun abonnement toevoegen kunnen maken.


## <a name="subscriptions"></a>Abonnementen

Een abonnement is hoe gebruikers toegang tot uw aanbiedingen. Als u een Azure-Stack-operator bij een serviceprovider, kopen uw gebruikers (tenants) uw services Abonneer u op uw aanbiedingen. Als u een Azure-Stack-operator in een organisatie, kunnen uw gebruikers (medewerkers) abonneren op de services die u zonder te betalen aanbieden. Elke combinatie van een gebruiker met een aanbieding is een unieke abonnement. Dus een gebruiker kan abonnementen op meerdere aanbiedingen hebben, maar elk abonnement is van toepassing op slechts één aanbieding. Plannen, aanbiedingen en quota's gelden alleen voor elke unieke abonnement – ze kunnen niet worden gedeeld tussen abonnementen. Elke bron die u een gebruiker maakt is gekoppeld aan één abonnement.


### <a name="default-provider-subscription"></a>Standaard provider-abonnement

De standaard Provider-abonnement wordt automatisch gemaakt wanneer u de Azure-Stack Development Kit implementeert. Dit abonnement kan worden gebruikt voor Azure Stack beheren, implementeren verdere resourceproviders en maken van abonnementen en aanbiedingen voor gebruikers. Omwille van beveiliging en licentieverlening mag deze niet worden gebruikt om uit te voeren van de klant workloads en toepassingen. 

## <a name="next-steps"></a>Volgende stappen

[Een plan maken](azure-stack-create-plan.md)
