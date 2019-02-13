---
title: Beslissingen voor de niet-verbonden Azure-implementatie voor Azure Stack-geïntegreerde systemen | Microsoft Docs
description: Implementatie planningsbeslissingen voor implementaties met meerdere knooppunten verbonden met een Azure Stack Azure bepalen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: wfayed
ms.lastreviewed: 12/11/2018
ms.openlocfilehash: 4481bcf7a794423f98f45e4a21a139dbe4c32b4f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56210778"
---
# <a name="azure-disconnected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Implementatie van Azure verbroken planningsbeslissingen voor Azure Stack-geïntegreerde systemen
Nadat u hebt besloten [hoe u Azure Stack wordt integreren in uw hybride cloudomgeving](azure-stack-connection-models.md), kunt u uw beslissingen voor de implementatie van Azure Stack voltooien.

U kunt implementeren en gebruiken van Azure Stack zonder een verbinding met internet. Met de implementatie van een niet-verbonden bent u echter beperkt tot een AD FS-identiteitsarchief en het factureringsmodel op basis van capaciteit. Omdat voor multitenancy is vereist voor het gebruik van Azure Active Directory (Azure AD), wordt multitenancy wordt niet ondersteund voor implementaties van niet-verbonden. 

Kies deze optie als u:
- Beveiligings- of andere beperkingen waarvoor u Azure Stack implementeren in een omgeving die niet is verbonden met Internet hebben.
- Wilt blokkeren (met inbegrip van gebruiksgegevens) gegevens worden verzonden naar Azure.
- Wilt u Azure Stack gebruiken als een privécloud-oplossing die is geïmplementeerd op het Intranet van uw bedrijf en niet zijn geïnteresseerd in hybride scenario's.

> [!TIP]
> Soms, dit type omgeving wordt ook wel een *Onderzeese scenario*.

Een niet-verbonden implementatie beperkt u niet later opnieuw verbinding maken met uw Azure Stack-exemplaar naar Azure voor hybride tenant-VM's. Betekent dit dat u hebt geen verbinding met Azure tijdens de implementatie van of u niet wilt dat Azure AD gebruiken als uw archief.

## <a name="features-that-are-impaired-or-unavailable-in-disconnected-deployments"></a>Functies die gehinderd of niet beschikbaar in niet-verbonden implementaties worden 
Azure Stack is ontworpen om te werken het beste wanneer verbonden met Azure, dus is het belangrijk te weten dat er zijn enkele functies en functionaliteit die verminderde of helemaal niet beschikbaar is in de modus zonder verbinding. 

|Functie|Impact in de modus zonder verbinding|
|-----|-----|
|VM-implementatie met DSC-extensie voor de virtuele machine na de implementatie configureren|Mensen met een handicap - DSC-extensie met Internet zoekt naar de meest recente WMF.|
|VM-implementatie met Docker-extensie om uit te voeren Docker-opdrachten|Mensen met een handicap: Docker controleert Internet voor de meest recente versie en deze controle mislukt.|
|Documentatie voor koppelingen in de Azure Stack-Portal|Niet-beschikbaar: koppelingen zoals Feedback geven, Help, Snelstart, enz. die gebruikmaken van een Internet-URL werkt niet.|
|Waarschuwing herbemiddeling/risicobeperking die verwijst naar een online herstel-handleiding|Niet beschikbaar: een herstel van waarschuwing is gekoppeld die gebruikmaken van die een Internet-URL werkt niet.|
|Marketplace, de mogelijkheid om te selecteren en toevoegen van galerie-pakketten rechtstreeks vanuit de Azure Marketplace|Mensen met een handicap: wanneer u Azure Stack in een niet-verbonden modus (zonder een verbinding met Internet) implementeert, kunt u items voor de marketplace niet downloaden met behulp van de Azure Stack-portal. Echter, kunt u de [marketplace syndication hulpprogramma](https://docs.microsoft.com/azure/azure-stack/azure-stack-download-azure-marketplace-item) voor de marketplace-items downloaden op een computer die verbinding heeft met internet en deze vervolgens overbrengen naar uw Azure Stack-omgeving.|
|Met behulp van Azure Active Directory federation-accounts voor het beheren van een Azure Stack-implementatie|Niet beschikbaar: deze functie vereist een verbinding met Azure. AD FS met een lokale Active Directory-exemplaar moet in plaats daarvan worden gebruikt.|
|App-services|Mensen met een handicap - Web-Apps mogelijk toegang tot Internet voor bijgewerkte inhoud.|
|Opdrachtregelinterface (CLI)|Mensen met een handicap: CLI heeft beperkte wat betreft verificatie en de inrichting van Service-principals.|
|Visual Studio – Cloud discovery|Cloud Discovery mensen met een handicap: ofwel worden gedetecteerd door verschillende clouds of werkt niet helemaal.|
|Visual Studio – AD FS|Verminderde – alleen AD FS biedt ondersteuning voor Visual Studio Enterprise.
Telemetrie|Niet-telemetriegegevens voor Azure Stack als en alle galeriepakketten van derden die afhankelijk van telemetrische gegevens zijn beschikbaar.|
|Certificaten|Niet beschikbaar: verbinding met Internet is vereist voor het certificaat certificaatintrekkingslijst (CRL) en Online Certificate Status Protocol (OSCP)-services in de context van HTTPS.|
|Key-Vault|Mensen met een handicap: een gebruikelijk voor Key Vault is dat een toepassing lezen van geheimen tijdens runtime. Hiervoor moet de toepassing een service-principal in de map. In Azure Active Directory, wordt normale gebruikers (niet-beheerders) worden standaard toegestaan om toe te voegen, service-principals. In AD (met behulp van AD FS) zijn ze niet. Dit wordt een drempel in de end-to-end-ervaring geplaatst, omdat een moet altijd via een directory-beheerder om toe te voegen elke toepassing.| 

## <a name="learn-more"></a>Meer informatie
- Zie voor informatie over gebruiksvoorbeelden, aanschaffen, partners en leveranciers van OEM-hardware, de [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) productpagina.
- Geïntegreerde systemen, Zie het technische document voor informatie over de roadmap en geografische beschikbaarheid voor Azure Stack: [Azure Stack: Een uitbreiding van Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Voor meer informatie over Microsoft Azure Stack verpakking en prijzen [downloaden van de .pdf](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Volgende stappen
[Datacenter-netwerkintegratie](azure-stack-network.md)
