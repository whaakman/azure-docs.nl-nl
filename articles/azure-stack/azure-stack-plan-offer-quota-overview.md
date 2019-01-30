---
title: Azure Stack-abonnement, aanbieding, quota en abonnementen overzicht | Microsoft Docs
description: Als een cloud-operator, die ik wil meer Azure Stack plannen, aanbiedingen, quota en abonnementen.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/12/2018
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 10/12/2018
ms.openlocfilehash: 4157c5d43b74cf7556114c5688e2ea717f1d0af3
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242652"
---
# <a name="plan-offer-quota-and-subscription-overview"></a>Overzicht van plannen, aanbiedingen, quota en abonnementen

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

[Azure Stack](azure-stack-poc.md) kunt leveren van een groot aantal services, zoals virtuele machines, SQL Server-databases, SharePoint, Exchange, en zelfs [items voor de Azure Marketplace](azure-stack-marketplace-azure-items.md). Als Azure Stack-operators, configureren en deze services in Azure Stack met behulp van plannen, aanbiedingen en quota's leveren.

Biedt een of meer abonnementen bevatten en elke-abonnement bevat een of meer services. Door te plannen maken en deze combineren in verschillende aanbiedingen, kunt u het volgende beheren:

- Welke services en uw gebruikers toegang hebben tot resources.
- De hoeveelheid resources die gebruikers kunnen gebruiken.
- Welke regio's hebben toegang tot de resources.

Wanneer u een service leveren, volgt u deze stappen op hoog niveau:

1. Een service die u wilt ervoor zorgen dat uw gebruikers toevoegen.
2. Maak een plan waarop een of meer services. Bij het maken van een abonnement selecteren of maken van quota's die de resourcelimieten van elke service in de planning definiëren.
3. Maak een aanbieding met een of meer abonnementen. De aanbieding zijn basisplannen en optioneel aanvullende plannen.

Nadat u het aanbieding hebt gemaakt, worden uw gebruikers kunnen abonneren op deze toegang tot de services en resources vindt u de aanbieding. Gebruikers kunnen zich abonneren op net zoveel aanbiedingen als ze willen. De volgende afbeelding toont een eenvoudig voorbeeld van een gebruiker die is geabonneerd op twee aanbiedingen. Ieder aanbod heeft een abonnement of twee en elk abonnement biedt hen toegang tot services.

![Tenantabonnement met aanbiedingen en plannen](media/azure-stack-key-features/image4.png)

## <a name="plans"></a>Abonnementen

De abonnementen zijn groepen van een of meer services. Als Azure Stack-operators, u [plannen maken](azure-stack-create-plan.md) te bieden aan uw gebruikers. Uw gebruikers abonneren op zijn beurt op uw aanbiedingen te gebruiken van de abonnementen en services die ze bevatten. Bij het maken van abonnementen, zorg ervoor dat u uw quota instellen, uw basisplannen te definiëren en u kunt overwegen om optioneel aanvullende plannen.

### <a name="quotas"></a>Quota

Voor hulp bij het beheren van de cloudcapaciteit van de, kunt u vooraf geconfigureerde *quota*, of een nieuw quotum voor elke service in een plan maken. Quota's definiëren de bovenste limieten die een gebruikersabonnement kunt inrichten of gebruiken. Een quotum kan bijvoorbeeld toestaan dat een gebruiker te maken van maximaal vijf virtuele machines (VM's).

U kunt quota's per regio configureren. Een plan waarmee compute-services voor een regio kan bijvoorbeeld een quotum van twee virtuele machines hebben.

>[!NOTE]
>In de Azure Stack Development Kit, slechts één regio (met de naam *lokale*) beschikbaar is.

Meer informatie over [quotatypen in Azure Stack](azure-stack-quota-types.md).

### <a name="base-plan"></a>Basisplan

Bij het maken van een aanbieding, kan de service-beheerder een basisplan bevatten. Deze basisplannen zijn standaard opgenomen als een gebruiker zich op deze aanbieding abonneert. Wanneer een gebruiker zich abonneert, hebben ze toegang tot alle resourceproviders die zijn opgegeven in deze basisplannen (met de bijbehorende quota).

### <a name="add-on-plans"></a>Aanvullende plannen

Aanvullende plannen zijn optioneel plannen die u aan een aanbieding toevoegt. Aanvullende plannen zijn niet opgenomen in de standaardinstelling in het abonnement. Aanvullende plannen zijn aanvullende plannen (met quota's) beschikbaar zijn in een aanbieding die een abonnement aan hun abonnementen toevoegen kunt. U kunt bijvoorbeeld een basisplan met beperkte resources voor een proefversie, en een aanvullende plan met meer resources voor klanten die zich conformeerde aan de service wilt aanbieden.

## <a name="offers"></a>Aanbiedingen

Aanbiedingen zijn groepen van een of meer abonnementen die u maakt zodat gebruikers kunnen zich hierop abonneren. Alpha bieden kunt bijvoorbeeld een Plan, waarmee u een set van compute-services en plannen B, die voorziet in een set services voor opslag en netwerk bevatten.

Wanneer u [maken van een aanbieding](azure-stack-create-offer.md), moet u ten minste één basisplan opnemen, maar u kunt ook aanvullende plannen die gebruikers aan hun abonnement toevoegen kunnen maken.

## <a name="subscriptions"></a>Abonnementen

Een abonnement is hoe gebruikers toegang tot uw aanbiedingen. Als u Azure Stack-operators voor een serviceprovider bent, kopen uw gebruikers (tenants) uw services met een abonnement op uw aanbiedingen. Als u Azure Stack-operators in een organisatie bent, worden uw gebruikers (werknemers) kunnen abonneren op de services die te zonder te hoeven betalen bieden.

Elke combinatie van een gebruiker met een aanbieding is een unieke abonnement. Een gebruiker kan abonnementen op meerdere aanbiedingen hebben, maar elk abonnement is alleen van toepassing tot één aanbieding. Plannen, aanbiedingen en quota's alleen van toepassing op een unieke abonnement – ze kunnen niet worden gedeeld tussen abonnementen. Elke bron die een gebruiker maakt, is gekoppeld aan één abonnement.

### <a name="default-provider-subscription"></a>Standaard-providerabonnement

Het abonnement van de provider standaard wordt automatisch gemaakt wanneer u de Azure Stack Development Kit implementeren. Dit abonnement kan worden gebruikt om het beheren van Azure Stack, aanvullende resourceproviders te implementeren en plannen en aanbiedingen voor gebruikers maken. Omwille van beveiliging en licentieverlening moet deze niet worden gebruikt om uit te voeren van de klantwerkbelastingen en toepassingen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het plannen en aanbiedingen [maken van een plan](azure-stack-create-plan.md).
