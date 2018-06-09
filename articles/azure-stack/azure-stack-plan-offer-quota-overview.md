---
title: Stack plan, aanbieding, quota en abonnement overzicht van Azure | Microsoft Docs
description: Als een cloud-operator wil ik Azure Stack plannen, aanbiedingen, quota's en -abonnementen te begrijpen.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: d8aef778807d3a8a61cf9eedaae24abce84a19ab
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248755"
---
# <a name="plan-offer-quota-and-subscription-overview"></a>Overzicht van plannen, aanbiedingen, quota en abonnementen

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

[Azure Stack](azure-stack-poc.md) kunt u een groot aantal leveren services, zoals virtuele machines, SQL Server-databases, SharePoint, Exchange, en zelfs [Azure Marketplace-items](azure-stack-marketplace-azure-items.md). Als een Azure-Stack-operator, configureren en deze services in Azure-Stack leveren via plannen, aanbiedingen en quota's.

De aanbiedingen die een of meer abonnementen bevatten en elk plan bevat een of meer services. Door het maken van abonnementen en ze combineren in verschillende aanbiedingen, kunt u het volgende beheren:

- Welke services en uw gebruikers hebben toegang tot bronnen.
- De hoeveelheid resources die gebruikers kunnen worden gebruikt.
- Welke regio's hebben toegang tot de bronnen.

Wanneer u een service leveren, volg deze stappen op hoog niveau:

1. Een service die u wilt ervoor zorgen dat uw gebruikers toevoegen.
2. Maak een plan dat een of meer services heeft. Bij het maken van een abonnement selecteren of maken van quota die de limieten van elke service in het plan definiëren.
3. Maak een aanbieding met een of meer abonnementen. De aanbieding kunt opnemen base plannen en optionele invoegtoepassing plannen.

Nadat u de aanbieding hebt gemaakt, kunnen uw gebruikers abonneren op deze toegang krijgt tot de services en bronnen die voorziet in de aanbieding. Gebruikers kunnen zich abonneren op zoveel voorstellen als ze willen. Het volgende diagram toont een eenvoudig voorbeeld van een gebruiker die is geabonneerd op twee aanbiedingen. Elke aanbieding heeft een plan of twee en elk plan hen toegang geeft tot services.

![Tenantabonnement met aanbiedingen en plannen](media/azure-stack-key-features/image4.png)

## <a name="plans"></a>Abonnementen

Plannen voor zijn groepen van een of meer services. Als operator Azure Stack u [plannen maken](azure-stack-create-plan.md) te bieden aan uw gebruikers. Uw gebruikers abonneren op zijn beurt op uw aanbiedingen plannen en services die ze gebruiken. Bij het maken van abonnementen, Controleer uw quota instellen, uw plannen voor basis te definiëren en overwegen optionele invoegtoepassing plannen.

### <a name="quotas"></a>Quota

Om u te helpen bij het beheren van de cloudcapaciteit van de, kunt u vooraf geconfigureerde quota gebruiken of een nieuwe quota voor elke service in een plan maken. Quota's definiëren de bovenste limieten die een gebruikerabonnement kan inrichten of gebruiken. Een quotum kunt bijvoorbeeld een gebruiker tot vijf virtuele machines (VM's) maken. U kunt extra quota instellen op de virtuele machines, zoals het RAM-geheugen en CPU-kernen.

U kunt quota's per regio configureren. Een plan dat compute services voor een regio biedt kan bijvoorbeeld een quotum van twee virtuele machines met 4 GB RAM en 8 CPU-kernen hebben.

>[!NOTE]
>In de Azure-Stack Development Kit, slechts één regio (met de naam *lokale*) is beschikbaar.

Meer informatie over [quotatypen in Azure-Stack](azure-stack-quota-types.md).

### <a name="base-plan"></a>Basisplan

Wanneer u een aanbieding maakt, kan de service-beheerder een basisplan bevatten. Deze base plannen zijn standaard opgenomen als een gebruiker zich op deze aanbieding abonneert. Wanneer een gebruiker zich abonneert, hebben die toegang tot alle resourceproviders die zijn opgegeven in deze base plannen (met de bijbehorende quota's.)

### <a name="add-on-plans"></a>Aanvullende plannen

Plannen van de invoegtoepassing zijn optionele plannen die u aan een aanbieding toevoegt. Plannen van de invoegtoepassing worden niet standaard opgenomen in het abonnement. Plannen van de invoegtoepassing zijn aanvullende schema's (met quota's) beschikbaar in een aanbieding met een abonnee aan hun abonnementen toevoegen kunt. U kunt bijvoorbeeld een basisplan met beperkte bronnen voor een proefversie, en een invoegtoepassing plannen met meer resources aanbieden aan klanten die u kiest voor de service.

## <a name="offers"></a>Aanbiedingen

Aanbiedingen zijn een of meer abonnementen die u maakt zodat gebruikers zich op deze abonneren kunnen groepen. Alpha bieden kan bijvoorbeeld Plan A, dat voorziet in een set van compute services en Plan B, die voorziet in een set van opslag en netwerk services bevatten.

Wanneer u [maken van een aanbieding](azure-stack-create-offer.md), moet u ten minste één basisplan opnemen, maar u kunt ook extra plannen die gebruikers aan hun abonnement toevoegen kunnen maken.

## <a name="subscriptions"></a>Abonnementen

Een abonnement is hoe gebruikers toegang tot uw aanbiedingen. Als u een Azure-Stack-operator voor een serviceprovider, kopen uw gebruikers (tenants) uw services Abonneer u op uw aanbiedingen. Als u een Azure-Stack-operator in een organisatie, kunnen uw gebruikers (medewerkers) abonneren op de services die u zonder te betalen aanbieden.

Elke combinatie van een gebruiker met een aanbieding is een unieke abonnement. Een gebruiker kan abonnementen op meerdere aanbiedingen hebben, maar elk abonnement is alleen van toepassing op één aanbieding. Plannen, aanbiedingen en quota's alleen van toepassing op een unieke abonnement – ze kunnen niet worden gedeeld tussen abonnementen. Elke bron die u een gebruiker maakt is gekoppeld aan één abonnement.

### <a name="default-provider-subscription"></a>Standaard provider-abonnement

De standaard Provider-abonnement wordt automatisch gemaakt wanneer u de Azure-Stack Development Kit implementeert. Dit abonnement kan worden gebruikt voor het beheren van Azure-Stack, extra resourceproviders implementeren en het maken van plannen en aanbiedingen voor gebruikers. Omwille van beveiliging en licentieverlening mag niet wordt gebruikt voor het uitvoeren van de klant workloads en toepassingen.

## <a name="next-steps"></a>Volgende stappen

[Een plan maken](azure-stack-create-plan.md)
