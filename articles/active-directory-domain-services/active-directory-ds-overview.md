---
title: Overzicht van Azure Active Directory Domain Services | Microsoft Docs
description: Overzicht van Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 0d47178f-773e-45f9-9ff4-9e8cffa4ffa2
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2017
ms.author: ergreenl
ms.openlocfilehash: c424ae1865a710593e509372dc2424c0b8b516a0
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155164"
---
# <a name="azure-active-directory-ad-domain-services"></a>Azure Active Directory (AD) Domain Services
## <a name="overview"></a>Overzicht
Azure Infrastructure Services kunt u een breed scala aan computeroplossingen op flexibele wijze distribueren. Met Azure Virtual Machines kunt u vrijwel onmiddellijk implementeren en betaalt u alleen per minuut. Ondersteuning voor Windows, Linux, SQL Server, Oracle, IBM, SAP en BizTalk gebruikt, kunt u elke workload en elke taal op vrijwel elk besturingssysteem implementeren. Deze voordelen kunnen u oudere toepassingen die zijn geïmplementeerd on-premises migreren naar Azure, om op te slaan op operationele kosten.

Een belangrijk aspect van het migreren van on-premises toepassingen naar Azure met het verwerken van de behoeften van de identiteit van deze toepassingen. Directory-toepassingen mogelijk afhankelijk zijn van LDAP voor lees- of schrijftoegang tot de adreslijst van het bedrijf of die gebruikmaken van geïntegreerde Windows-verificatie (Kerberos of NTLM-verificatie) voor verificatie van eindgebruikers. Line-of-business (LOB)-toepassingen die worden uitgevoerd op Windows Server worden doorgaans geïmplementeerd op virtuele machines toegevoegd aan een domein, zodat ze kunnen worden beheerd veilig met behulp van Groepsbeleid. 'Lift-and-shift' on-premises toepassingen naar de cloud moeten deze afhankelijkheden op de zakelijke identiteit-infrastructuur worden omgezet.

Beheerders wordt vaak inschakelen op een van de volgende oplossingen om te voldoen aan de behoeften van de identiteit van de toepassingen die zijn geïmplementeerd in Azure:

* Een site-naar-site VPN-verbinding tussen de workloads die worden uitgevoerd in Azure Infrastructure Services en de adreslijst van het bedrijf on-premises implementeren.
* De zakelijke AD-domein/forest-infrastructuur uitbreiden door het instellen van de replica-domeincontrollers met behulp van virtuele machines van Azure.
* Implementeer een zelfstandig domein in Azure met behulp van domeincontrollers die zijn geïmplementeerd als virtuele machines van Azure.

Alle deze methoden te maken krijgen met hoge kosten en administratieve overhead. Beheerders zijn vereist voor het implementeren van domeincontrollers met behulp van virtuele machines in Azure. Daarnaast moeten ze beheren, beveiligen, patch, bewaken, back-up maken en oplossen van deze virtuele machines. De afhankelijkheid van VPN-verbindingen met de on-premises directory zorgt ervoor dat de werkbelastingen die zijn geïmplementeerd in Azure kwetsbaar voor tijdelijk afwijkende netwerkverbinding storingen of onderbrekingen. Deze netwerkstoringen leiden op zijn beurt tot lagere uptime en verminderde betrouwbaarheid voor deze toepassingen.

We ontworpen voor Azure AD Domain Services voor een beter alternatief.

### <a name="watch-an-introductory-video"></a>Een inleidende video

>[!VIDEO https://www.youtube.com/embed/T1Nd9APNceQ]

## <a name="introducing-azure-ad-domain-services"></a>Maak kennis met Azure AD Domain Services

Azure AD Domain Services biedt beheerde domeinservices zoals domain-join, groep-beleid, LDAP, Kerberos/NTLM-verificatie, die volledig compatibel met Windows Server Active Directory. U kunt deze domain-services zonder de noodzaak om te implementeren, beheren en vullen van de domeincontrollers in de cloud gebruiken. Azure AD Domain Services kan worden geïntegreerd met uw bestaande Azure AD-tenant, waardoor het mogelijk dat gebruikers zich aanmelden met hun bedrijfsreferenties. Bovendien kunt u bestaande groepen en accounts voor gebruikers voor het beveiligen van toegang tot resources, zodat een soepeler 'lift-and-shift' van de on-premises resources met Azure Infrastructure Services.

Azure AD Domain Services-functionaliteit werkt naadloos samen ongeacht of uw Azure AD-tenant alleen in de cloud of gesynchroniseerd met uw on-premises Active Directory.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure AD Domain Services voor organisaties die alleen in de cloud

Een alleen-cloud Azure AD-tenant (vaak aangeduid als 'beheerde tenants') heeft geen een on-premises identity-footprint. Accounts voor gebruikers, hun wachtwoorden en groepslidmaatschappen zijn met andere woorden, alle systeemeigen naar de cloud - dat wil zeggen, gemaakt en beheerd in Azure AD. Kijk eens naar dat Contoso een alleen-cloud is Azure AD-tenant. Zoals wordt weergegeven in de volgende afbeelding, heeft de beheerder van Contoso een virtueel netwerk geconfigureerd in Azure Infrastructure Services. Toepassingen en werkbelastingen van de server zijn geïmplementeerd in dit virtuele netwerk in virtuele machines van Azure. Omdat Contoso een alleen-cloud-tenant is, worden alle gebruikers-id's, hun referenties en groepslidmaatschappen gemaakt en beheerd in Azure AD.

![Overzicht van Azure AD Domain Services](./media/active-directory-domain-services-overview/aadds-overview.png)

Het Contoso IT-beheerder kan Azure AD Domain Services inschakelen voor hun Azure AD-tenant en kies domeinservices beschikbaar maken in dit virtuele netwerk. Daarna worden Azure AD Domain Services een beheerd domein wordt ingericht en maakt deze beschikbaar zijn in het virtuele netwerk. Alle gebruikersaccounts, groepslidmaatschappen en de referenties van de gebruiker is beschikbaar in het Contoso Azure AD-tenant zijn ook beschikbaar in deze nieuwe domein. Deze functie kunnen gebruikers in de organisatie zich aanmeldt bij het domein met hun zakelijke referenties - bijvoorbeeld: bij het verbinden op afstand met domein computers via Extern bureaublad. Beheerders kunnen de toegang tot resources in het domein met bestaande groepslidmaatschappen inrichten. Toepassingen die zijn geïmplementeerd in virtuele machines op het virtuele netwerk kunnen functies zoals domeindeelname, LDAP lezen, LDAP-binding, NTLM en Kerberos-verificatie en Group Policy gebruiken.

Enkele van de meest kenmerkende aspecten van het beheerde domein die is ingericht met Azure AD Domain Services zijn als volgt:

* Het Contoso IT-beheerder niet hoeft te beheren, patch of controleren van dit domein of alle domeincontrollers voor dit beheerde domein.
* Er is niet nodig voor het beheren van AD-replicatie voor dit domein. Accounts voor gebruikers, groepslidmaatschappen en referenties van het Contoso Azure AD-tenant zijn automatisch beschikbaar binnen dit beheerde domein.
* Omdat het domein wordt beheerd door Azure AD Domain Services, Contoso van IT-beheerder heeft geen domeinbeheerder of Ondernemingsadministrator bevoegdheden voor dit domein.

### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure AD Domain Services voor hybride organisaties
Organisaties met een hybride IT-infrastructuur gebruiken een combinatie van cloudresources en on-premises bronnen. Zulke organisaties synchroniseren van identiteitsgegevens van hun on-premises directory met hun Azure AD-tenant. Hybride organisaties zoeken voor het migreren van meer van hun on-premises toepassingen naar de cloud, met name verouderde directory-toepassingen, Azure AD Domain Services kunnen nuttig zijn voor deze.

Litware Corporation heeft geïmplementeerd [Azure AD Connect](../active-directory/hybrid/whatis-hybrid-identity.md)voor het synchroniseren van identiteitsgegevens uit hun on-premises directory met hun Azure AD-tenant. De gegevens van de identiteit die is gesynchroniseerd bevat gebruikersaccounts, hun referentie-hashes voor verificatie (Wachtwoordsynchronisatie van) en groepslidmaatschappen.

> [!NOTE]
> **Wachtwoordsynchronisatie is verplicht voor hybride organisaties het gebruik van Azure AD Domain Services**. Deze vereiste is omdat de referenties van gebruikers in het beheerde domein geleverd door Azure AD Domain Services, zijn nodig om deze gebruikers via NTLM of Kerberos-verificatiemethoden te verifiëren.
>
>

![Azure AD Domain Services voor Litware Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

De voorgaande afbeelding ziet u hoe organisaties met een hybride IT-infrastructuur, zoals Litware Corporation, Azure AD Domain Services kunnen gebruiken. De toepassingen en de serverworkloads waarvoor domeinservices van Litware zijn geïmplementeerd in een virtueel netwerk in Azure Infrastructure Services. Litware van IT-beheerder kan Azure AD Domain Services inschakelen voor hun Azure AD-tenant en kies een beheerd domein om beschikbaar te maken in dit virtuele netwerk. Aangezien Litware een organisatie met een hybride IT-infrastructuur is, worden gebruikersaccounts, groepen en referenties van hun on-premises directory met hun Azure AD-tenant gesynchroniseerd. Deze functie kan gebruikers zich aanmelden bij het domein met hun zakelijke referenties - bijvoorbeeld wanneer het op afstand verbinding maken met machines toegevoegd aan het domein via Extern bureaublad. Beheerders kunnen de toegang tot resources in het domein met bestaande groepslidmaatschappen inrichten. Toepassingen die zijn geïmplementeerd in virtuele machines op het virtuele netwerk kunnen functies zoals domeindeelname, LDAP lezen, LDAP-binding, NTLM en Kerberos-verificatie en Group Policy gebruiken.

Enkele van de meest kenmerkende aspecten van het beheerde domein die is ingericht met Azure AD Domain Services zijn als volgt:

* Het beheerde domein is een zelfstandig domein. Het is niet een uitbreiding van Litware van on-premises domein.
* Litware van IT-beheerder niet hoeft te beheren, patch, of bewaken van domeincontrollers voor dit beheerde domein.
* Er is niet nodig voor het beheren van AD-replicatie voor dit domein. Accounts voor gebruikers, groepslidmaatschappen en referenties van Litware van on-premises directory zijn gesynchroniseerd met Azure AD via Azure AD Connect. Deze accounts van gebruikers, groepslidmaatschappen en referenties zijn automatisch beschikbaar in het beheerde domein.
* Omdat het domein wordt beheerd door Azure AD Domain Services, Litware van IT-beheerder heeft geen domeinbeheerder of Ondernemingsadministrator bevoegdheden voor dit domein.

## <a name="benefits"></a>Voordelen
Met Azure AD Domain Services, kunt u profiteren van de volgende voordelen:

* **Eenvoudige** – u kunt voldoen aan de behoeften van de identiteit van virtuele machines die zijn geïmplementeerd in Azure Infrastructure services met een paar eenvoudige klikken. U hoeft niet te implementeren en beheren van infrastructuur voor identiteiten in Azure of configuratie nodig connectiviteit met uw on-premises infrastructuur.
* **Geïntegreerde** – Azure AD Domain Services is nauw geïntegreerd met uw Azure AD-tenant. U kunt nu Azure AD gebruiken als een geïntegreerde cloud-gebaseerde enterprise-map die caters aan de behoeften van uw moderne toepassingen en de traditionele directory-toepassingen.
* **Compatibel** – Azure AD Domain Services is gebouwd op de bewezen geavanceerde infrastructuur van Windows Server Active Directory. Daarom kunnen uw toepassingen afhankelijk van een grotere mate van compatibiliteit met Windows Server Active Directory-functies. Niet alle functies die beschikbaar zijn in Windows Server AD zijn momenteel beschikbaar in Azure AD Domain Services. Beschikbare functies zijn echter compatibel is met de bijbehorende Windows Server AD-functies die kunt u op in uw on-premises infrastructuur vertrouwen. De mogelijkheden van de join LDAP, Kerberos, NTLM, Groepsbeleid en domein vormen een goed ontwikkelde aanbieding die is getest en verfijnd via verschillende versies van Windows Server.
* **Rendabele** – met Azure AD Domain Services, kunt u voorkomen dat de infrastructuur en werkbelasting die is gekoppeld aan het beheren van infrastructuur voor identiteiten ter ondersteuning van traditionele directory-toepassingen. U kunt deze toepassingen met Azure Infrastructure Services verplaatsen en profiteren van lagere operationele kosten.


## <a name="next-steps"></a>Volgende stappen
### <a name="learn-more-about-azure-ad-domain-services"></a>Meer informatie over Azure AD Domain Services
* [Functies](active-directory-ds-features.md)
* [Implementatiescenario 's](active-directory-ds-scenarios.md)
* [Ontdek als Azure AD Domain Services aansluit bij uw use cases](active-directory-ds-comparison.md)
* [Begrijpen hoe Azure AD Domain Services worden gesynchroniseerd met uw Azure AD-directory](active-directory-ds-synchronization.md)

### <a name="get-started-with-azure-ad-domain-services"></a>Aan de slag met Azure AD Domain Services
* [Azure AD Domain Services met behulp van de Azure-portal inschakelen](active-directory-ds-getting-started.md)
