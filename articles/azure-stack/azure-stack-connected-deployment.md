---
title: "Beslissingen voor de gekoppelde Azure-implementatie voor Azure-Stack geïntegreerd systemen | Microsoft Docs"
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
ms.openlocfilehash: c1a3b2107abdc3ef19a314616518c494687d81bf
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-connected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Azure verbonden implementatie planningsbeslissingen voor Azure-Stack geïntegreerd systemen
Nadat u hebt besloten [hoe u Azure-Stack wordt integreren in uw cloudomgeving hybride](azure-stack-deployment-decisions.md), kunt u uw Azure-Stack implementatie beslissingen voltooien.

Implementatie Azure-Stack is verbonden met Azure betekent dat u kunt Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) voor uw archief. U kunt ook kiezen uit de facturering model: pay-as-gebruik of op basis van capaciteit. Een gekoppelde implementatie is de standaardoptie omdat kunnen klanten het belangrijkst onvoldoende Stack Azure, met name voor hybride cloud-scenario's die betrekking hebben op de Azure- en Azure-Stack ophalen. 

## <a name="choose-an-identity-store"></a>Kies een identiteit store
Een implementatie voor verbonden kunt u kiezen tussen Azure AD of AD FS voor uw archief. Een implementatie met een niet-verbonden kan zonder internetverbinding alleen gebruiken AD FS.

Uw identiteit store keuze heeft geen gevolgen voor de virtuele machines (VM's) van de tenant. Tenant-VM's kunnen ervoor kiezen welke identiteit store ze willen het verbinding maken met afhankelijk van hoe ze worden geconfigureerd: Azure AD, Windows Server Active Directory-domein, werkgroep, enzovoort. Dit is geen verband met de Azure-Stack identiteit provider beslissing. 

Bijvoorbeeld, als u IaaS tenant-VM's boven in de Azure-Stack implementeren en wilt laten deelnemen aan een zakelijke Active Directory-domein en accounts van daaruit gebruiken, kunt u nog steeds doen dit. U bent niet vereist voor het gebruik van de Azure AD identity store die u hier selecteert voor deze accounts.

### <a name="azure-ad-identity-store"></a>Azure AD identity store
Wanneer u Azure AD gebruikt voor uw archief twee Azure AD-accounts vereist: een globale beheerdersaccount en een rekening. Deze accounts kunnen worden de accounts op dezelfde of verschillende accounts. Tijdens het gebruik van dezelfde gebruikersaccount mogelijk eenvoudiger en is handig als u een beperkt aantal Azure-accounts hebt, kunnen uw bedrijfsbehoeften voorstellen met behulp van twee accounts:

1. **Globale beheerdersaccount** (alleen vereist voor verbonden implementaties). Dit is een Azure-account die wordt gebruikt voor het maken van toepassingen en service-principals voor het Azure-Stack infrastructuurservices in Azure Active Directory. Dit account moet beheerdersmachtigingen directory voor de map die onder uw Azure-Stack-systeem worden geïmplementeerd. Deze worden de operator' cloud' globale beheerdersrechten voor de Azure AD-tenant en wordt gebruikt: 
    - Inrichten en het delegeren van toepassingen en service-principals voor alle Azure-Stack-services die nodig zijn om te communiceren met Azure Active Directory en Graph API. 
    - Als de Service-beheerdersaccount. Dit is de eigenaar van de provider standaardabonnement (die u kunt later wijzigen). U kunt zich aanmelden bij de Azure-Stack-beheerportal aan dit account en kunt gebruiken voor aanbiedingen en planningen maken, quota instellen en andere beheertaken in Azure-Stack.
2. **Factureringsaccount** (vereist voor zowel verbonden als implementaties verbroken). Deze Azure-account wordt gebruikt voor de facturering relatie tussen uw Azure-Stack geïntegreerd-systeem en de back-end van Azure commerce tot stand brengen. Dit is het account dat wordt gefactureerd voor Azure-Stack-kosten. Dit account wordt ook gebruikt voor marketplace-syndicatie en andere hybride scenario's. 

### <a name="ad-fs-identity-store"></a>AD FS IDENTITEIT STORE
Selecteer deze optie als u wilt uw eigen identiteit store, zoals uw zakelijke Active Directory, gebruiken voor uw Service Administrator-accounts.  

## <a name="choose-a-billing-model"></a>Kies een factureringsmodel selecteren
U kunt een **Pay-as-gebruik** of de **capaciteit** facturering model. Het facturering model implementaties pay-as-gebruik moet kunnen rapporteren over het gebruik via een verbinding met Azure ten minste één keer voor elke 30 dagen, dus als er verbinding wordt niet beschikbaar, het facturering model capaciteit is de enige optie. 

### <a name="pay-as-you-use"></a>Pay-as-you-use
Met het facturering model Pay-as-gebruik gebruik verrekend met een Azure-abonnement. U betaalt alleen wanneer u de Azure-Stack-services gebruikt. Als dit het model dat u kiest, moet u een Azure-abonnement en de account-ID die is gekoppeld aan dat abonnement (bijvoorbeeld serviceadmin@contoso.onmicrosoft.com). EA CSP en CSL abonnementen worden ondersteund. Rapportage over het gebruik wordt geconfigureerd tijdens [Azure Stack registratie](azure-stack-registration.md).

> [!NOTE]
> In de meeste gevallen Enterprise-klanten gebruikt EA abonnementen en serviceproviders CSP of CSL abonnementen wordt gebruikt.

Als u wilt gebruiken een CSP-abonnement, raadpleegt u de tabel hieronder om u te identificeren welke CSP-abonnement moet worden gebruikt, zoals de juiste aanpak is afhankelijk van het exacte CSP-scenario:

|Scenario|Opties voor domein- en -abonnement|
|-----|-----|
|U bent een directe of indirecte CSP Partner en werkt u de Azure-Stack|Gebruik een CSL (Common Layer voor de Service)-abonnement.|
|U bent een directe of indirecte CSP Partner en werkt u de Azure-Stack|Partner Center Maak in een Azure AD-tenant met een beschrijvende naam, bijvoorbeeld <your organization>CSPAdmin en een Azure-CSP-abonnement gekoppeld.|
|U bent een indirecte CSP wederverkoper en werkt u de Azure-Stack|Vraag uw indirecte CSP-Provider te maken, met behulp van de Partner Center, een Azure AD-tenant voor uw organisatie en een Azure-CSP-abonnement gekoppeld.|

### <a name="capacity-based-billing"></a>Capaciteit gebaseerde facturering
Als u besluit het facturering Capaciteitsmodel gebruiken, moet u een Azure-Stack capaciteit plannen SKU op basis van de capaciteit van uw systeem aanschaffen. U moet weten van het aantal fysieke kernen in uw Azure-Stack het juiste aantal kopen. 

Capaciteit facturering vereist een Enterprise Agreement (EA) voor registratie bij de Azure-abonnement. De reden is dat registratie stelt syndication waarvoor een Azure-abonnement is vereist. Het abonnement wordt niet gebruikt voor het gebruik van Azure-Stack.

## <a name="learn-more"></a>Meer informatie
- Zie voor meer informatie over gebruiksvoorbeelden, kopen, partners en hardwareleveranciers OEM de [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) productpagina.
- Voor informatie over de roadmap en geo-beschikbaarheid voor Azure-Stack geïntegreerde systemen, Zie het white paper: [Azure Stack: een uitbreiding van Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Voor meer informatie over Microsoft Azure-Stack verpakken en prijzen [downloaden van de .pdf](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 
