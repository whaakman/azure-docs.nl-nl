---
title: "Niet-verbonden Azure-implementatie beslissingen voor Azure-Stack geïntegreerd systemen | Microsoft Docs"
description: Implementatie planningsbeslissingen voor implementaties met meerdere knooppunten verbonden met een Azure-Stack Azure bepalen.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 8708f13109767c7cb9e4a1bf0d2797d7c4fb9a23
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-disconnected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Azure-niet-verbonden implementatie planningsbeslissingen voor Azure-Stack geïntegreerd systemen
Nadat u hebt besloten [hoe u Azure-Stack wordt integreren in uw cloudomgeving hybride](azure-stack-deployment-decisions.md), kunt u uw Azure-Stack implementatie beslissingen voltooien.

Met de niet-verbonden met de optie Azure-implementatie, kunt u implementeren en gebruiken van Azure Stack zonder verbinding met Internet. Met een implementatie met een niet-verbonden bent u echter beperkt tot een AD FS-identity-store en het facturering model op basis van capaciteit. 

Selecteer deze optie als u:
- Beveiligings- of andere beperkingen waarvoor u Azure-Stack implementeren in een omgeving die niet is verbonden met Internet hebben.
- Wilt blokkeren gegevens (inclusief gebruiksgegevens) van Azure worden verzonden.
- Wilt gebruiken van Azure-Stack uitsluitend als een privécloud-oplossing die wordt geïmplementeerd op het Intranet van uw bedrijf en niet wilt hybride scenario's.

> [!TIP]
> Soms wordt wordt dit type omgeving ook wel 'Onderzeese scenario'.

Een niet-verbonden implementatie betekent niet strikt dat u geen later verbinding uw Azure-Stack-exemplaar naar Azure voor hybride tenantscenario VM's maken. Betekent dit dat u hebt geen verbinding met Azure tijdens de implementatie van of u niet wilt dat Azure Active Directory gebruiken als uw archief. Als u verbinding hebt met Azure na de implementatie wilt ongeacht wat u wilt gebruiken als uw identiteit archief, moet u de verbinding maken met Azure-implementatie-optie kiezen. 

## <a name="features-that-are-impaired-or-unavailable-in-disconnected-deployments"></a>Functies die gehinderd of niet beschikbaar in niet-verbonden implementaties zijn 
Azure-Stack is ontworpen om te werken het beste wanneer verbonden met Azure, dus is het belangrijk te weten dat er zijn sommige functies en functionaliteiten die gehinderd of helemaal niet beschikbaar in de modus zonder verbinding. 

|Functie|Impact in de modus zonder verbinding|
|-----|-----|
|Implementaties van virtuele machines met DSC-uitbreiding voor post-implementaties van virtuele machines configureren|Mensen met een handicap - DSC-extensie gezocht met het Internet naar de meest recente WMF.|
|Implementaties van virtuele machines met Docker-extensie Docker-opdrachten uit te voeren|Mensen met een handicap: Docker wordt gecontroleerd op de meest recente versie en deze controle mislukt.|
|Documentatie-koppelingen in de Stack Azure Portal|Niet beschikbaar-koppelingen zoals geven Feedback, hulp nodig hebt, Quick Start, enzovoort die gebruikmaken van een Internet-URL, werken niet.|
|Waarschuwing herstel/risicobeperking die verwijst naar een handleiding online herstel|Niet beschikbaar: elke waarschuwing herstel koppelingen die gebruikmaken van die een Internet-URL, werken niet.|
|Marketplace-syndicatie de mogelijkheid om te selecteren en de galerie pakketten toevoegen rechtstreeks vanuit Azure Marketplace|Niet beschikbaar: deze functie is vereist voor de verbinding met Azure en Azure Active Directory-account.|
|De implementatie van een Azure-Stack beheren met behulp van Azure Active Directory federation-accounts|Niet beschikbaar: deze functie is vereist voor de verbinding met Azure. AD FS met een lokaal exemplaar van Active Directory moet in plaats daarvan worden gebruikt.|
|Resourceproviders zoals WebApps en SQL|Niet beschikbaar - Resourceproviders zoals WebApps en SQL toegang tot Internet nodig voor inhoud.|
|Opdrachtregelinterface (CLI)|Mensen met een handicap: CLI heeft verminderde functionaliteit wat betreft verificatie en toewijzing van de principes van de Service.|
|Visual Studio: Cloud discovery|Cloud Discovery functioneel – verschillende clouds, ofwel worden gedetecteerd of zal niet werken.|
|Visual Studio: AD FS|Functioneel – alleen Visual Studio Enterprise AD FS ondersteunt.
Telemetrie|Niet-telemetriegegevens voor Azure-Stack als en eventuele van derden galerie-pakketten die afhankelijk van telemetriegegevens zijn beschikbaar.|
|Certificaten|Niet beschikbaar-verbinding met Internet is vereist voor de certificaatintrekkingslijst (CRL) en Online Certificate Status Protocol (OSCP)-services in de context van HTTPS.|
|Key-Vault|Mensen met een handicap: een algemene gebruiksvoorbeeld voor Sleutelkluis is om een toepassing lezen van geheimen tijdens runtime. Hiervoor moet de toepassing een service-principal in de map. In Azure Active Directory zijn gewone gebruikers (niet-beheerders) standaard toegestaan om toe te voegen, service-principals. In AD (met behulp van AD FS) zijn ze niet. Dit wordt een drempel in de end-to-end-ervaring geplaatst, omdat een moet altijd een directory-beheerder elke toepassing toevoegen via.| 

## <a name="learn-more"></a>Meer informatie
- Zie voor meer informatie over gebruiksvoorbeelden, kopen, partners en hardwareleveranciers OEM de [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) productpagina.
- Voor informatie over de roadmap en geo-beschikbaarheid voor Azure-Stack geïntegreerde systemen, Zie het white paper: [Azure Stack: een uitbreiding van Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Voor meer informatie over Microsoft Azure-Stack verpakken en prijzen [downloaden van de .pdf](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

