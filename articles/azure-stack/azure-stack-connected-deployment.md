---
title: Beslissingen voor de gekoppelde Azure-implementatie voor Azure Stack-geïntegreerde systemen | Microsoft Docs
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
ms.lastreviewed: 11/05/2018
ms.openlocfilehash: a39ab2eb6d08f478ed68aed2bc753e5225185fe3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56175384"
---
# <a name="azure-connected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Azure-verbonden implementatie planningsbeslissingen voor Azure Stack-geïntegreerde systemen
Nadat u hebt besloten [hoe u Azure Stack wordt integreren in uw hybride cloudomgeving](azure-stack-connection-models.md), kunt u uw beslissingen voor de implementatie van Azure Stack voltooien.

Implementeren Azure Stack met Azure bent verbonden betekent dat u kunt Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) voor uw archief. U kunt ook kiezen uit een van beide factureringsmodel: betalen als u-gebruik of op basis van capaciteit. Een gekoppelde implementatie is de standaardoptie omdat Hiermee kunnen klanten om op te halen van de meeste waarde ligt buiten het Azure Stack, met name voor hybride cloud-scenario's die betrekking hebben op Azure en Azure Stack. 

## <a name="choose-an-identity-store"></a>Kies een archief voor de klantidentiteit
Met een gekoppelde implementatie, kunt u kiezen tussen Azure AD of AD FS voor het opslaan van uw identiteit. Implementatie van een niet-verbonden kunt zonder internetverbinding, alleen AD FS.

Uw keuze van de store identiteit heeft geen gevolgen voor de tenant virtuele machines (VM's). Tenant-VM's kunnen kiezen welke identiteitsarchief die ze willen het verbinding maken met afhankelijk van hoe ze worden geconfigureerd: Azure AD, Windows Server Active Directory-domein, werkgroep, enzovoort. Dit is niet verwant aan de beslissing van Azure Stack-id-provider. 

Bijvoorbeeld, als u IaaS tenant-VM's boven op Azure Stack implementeren en deze toevoegen aan een zakelijke Active Directory-domein en de accounts van daaruit gebruiken wilt, kunt u nog steeds doen dit. U bent niet verplicht met de Azure AD identity-store die u hier selecteert voor deze accounts.

### <a name="azure-ad-identity-store"></a>Azure AD-identiteitenarchief
Wanneer u Azure AD gebruiken voor uw archief voor de klantidentiteit heeft twee Azure AD-accounts nodig: een globale beheerdersaccount en een factureringsaccount. Deze accounts mag bestaan uit de accounts van dezelfde of verschillende accounts. Terwijl met behulp van hetzelfde gebruikersaccount, mogelijk eenvoudiger en handig als u een beperkt aantal Azure-accounts hebt, kunnen uw bedrijfsbehoeften voorstellen met behulp van twee accounts:

1. **Globale beheerdersaccount** (alleen vereist voor de verbonden implementaties). Dit is een Azure-account die wordt gebruikt voor het maken van toepassingen en service-principals voor infrastructuurservices van Azure Stack in Azure Active Directory. Dit account moet beheerdersmachtigingen directory naar de map die onder uw Azure Stack-systeem worden geïmplementeerd. Dit wordt dan de 'cloud-operator' globale beheerder voor de Azure AD-tenant om te worden gebruikt: 
    - Inrichten en het delegeren van toepassingen en service-principals voor alle Azure Stack-services die moeten communiceren met Azure Active Directory en Graph API. 
    - Als de Service Administrator-account. Dit is de eigenaar van de provider standaardabonnement (die u kunt later wijzigen). U kunt u aanmelden bij de Azure Stack-beheerportal met dit account en deze kunt gebruiken om te maken van aanbiedingen en plannen, quota instellen en andere administratieve taken uitvoeren in Azure Stack.
2. **Factureringsaccount** (vereist voor zowel verbonden als implementaties verbroken). Deze Azure-account wordt gebruikt om de facturering relatie tussen uw geïntegreerde Azure Stack-systeem en de back-end van Azure commerce stand te brengen. Dit is het account dat in rekening voor Azure Stack-kosten gebracht wordt. Dit account wordt ook worden gebruikt voor het bieden van items in de marketplace en andere hybride scenario's. 

### <a name="ad-fs-identity-store"></a>AD FS-identiteitenarchief
Selecteer deze optie als u wilt uw eigen identiteit-store, zoals uw zakelijke Active Directory, gebruiken voor uw Service Administrator-accounts.  

## <a name="choose-a-billing-model"></a>Kies een model voor facturering
U kunt een **betalen als u-gebruik** of de **capaciteit** factureringsmodel. Betalen als u-gebruik facturering modelimplementaties moeten kunnen rapporteren over het gebruik door een verbinding met Azure ten minste één keer voor elke 30 dagen zijn. Daarom is het facturering model voor betalen als u-gebruik is alleen beschikbaar voor implementaties van verbonden.  

### <a name="pay-as-you-use"></a>Pay-as-you-use
Met het factureringsmodel betalen als u-gebruik wordt verrekend met een Azure-abonnement. U betaalt alleen wanneer u de Azure Stack-services gebruiken. Als dit het model dat u kiest is, moet u een Azure-abonnement en de account-ID die is gekoppeld aan dat abonnement (bijvoorbeeld serviceadmin@contoso.onmicrosoft.com). EA, CSP en CSL abonnementen worden ondersteund. Rapportage over het gebruik wordt geconfigureerd tijdens [Azure Stack-registratie](azure-stack-registration.md).

> [!NOTE]
> In de meeste gevallen EA-abonnementen wordt gebruikt door zakelijke klanten en serviceproviders CSP of CSL abonnementen wordt gebruikt.

Als u een CSP-abonnement gebruiken wilt, raadpleegt u de onderstaande tabel voor het identificeren van welke CSP-abonnement moet worden gebruikt, zoals de juiste benadering is afhankelijk van de exacte CSP-scenario:

|Scenario|Opties voor domein- en -abonnement|
|-----|-----|
|U bent een **directe CSP-Partner** of een **indirecte CSP-Provider**, en u de Azure Stack wordt uitgevoerd|Gebruik een CSL (Common Layer voor de Service)-abonnement.<br>     of<br>Maak een Azure AD-tenant met een beschrijvende naam in het Partnercentrum. Bijvoorbeeld &lt;uw organisatie > CSPAdmin met een Azure CSP-abonnement dat is gekoppeld.|
|U bent een **indirecte CSP-Reseller**, en u de Azure Stack wordt uitgevoerd|Vraag uw indirecte CSP-Provider te maken van een Azure AD-tenant voor uw organisatie met een Azure CSP-abonnement dat is gekoppeld met behulp van de Partner Center.|

### <a name="capacity-based-billing"></a>Capaciteit op basis van facturering
Als u besluit het factureringsmodel capaciteit gebruiken, moet u een Azure Stack-capaciteit plannen SKU op basis van de capaciteit van uw systeem aanschaffen. U moet het aantal fysieke kernen in uw Azure-Stack om aan te schaffen het juiste aantal weten. 

Capaciteit facturering vereist een Enterprise Agreement (EA) Azure-abonnement voor registratie. De reden is dat registratie stelt u de beschikbaarheid van items in de Marketplace, met een Azure-abonnement vereist. Het abonnement wordt niet gebruikt voor het gebruik van Azure Stack.

## <a name="learn-more"></a>Meer informatie
- Zie voor informatie over gebruiksvoorbeelden, aanschaffen, partners en leveranciers van OEM-hardware, de [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) productpagina.
- Geïntegreerde systemen, Zie het technische document voor informatie over de roadmap en geografische beschikbaarheid voor Azure Stack: [Azure Stack: Een uitbreiding van Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Voor meer informatie over Microsoft Azure Stack verpakking en prijzen [downloaden van de .pdf](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Volgende stappen
[Datacenter-netwerkintegratie](azure-stack-network.md)