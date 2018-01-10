---
title: Overzicht van Azure Active Directory Domain Services | Microsoft Docs
description: Overzicht van Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 0d47178f-773e-45f9-9ff4-9e8cffa4ffa2
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: maheshu
ms.openlocfilehash: 2c00d324adfc1b71d436cb017214af98ff3002cf
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="azure-active-directory-ad-domain-services"></a>(AD) voor Azure Active Directory Domain Services
## <a name="overview"></a>Overzicht
Azure Infrastructure Services kunt u een breed scala aan oplossingen computergebruik in een flexibele manier te implementeren. Met Azure Virtual Machines, kunt u vrijwel onmiddellijk implementeren en betaalt u alleen per minuut. Ondersteuning voor Windows, Linux, SQL Server, Oracle, IBM, SAP en BizTalk gebruikt, kunt u elke werkbelasting, een andere taal, op vrijwel elk besturingssysteem implementeren. Deze voordelen kunnen u oudere toepassingen die zijn geïmplementeerd op lokale migreren naar Azure, op te slaan op de operationele kosten.

Een belangrijk aspect van het migreren van on-premises toepassingen naar Azure met het verwerken van de behoeften van de identiteit van deze toepassingen. Directory-compatibele toepassingen afhankelijk zijn van LDAP voor lees- of schrijftoegang tot de zakelijke directory of afhankelijk zijn van geïntegreerde Windows-verificatie (Kerberos of NTLM-verificatie) om te verifiëren van eindgebruikers. Line-of-business (LOB)-toepassingen die op Windows Server worden doorgaans geïmplementeerd op de machines verbonden met het domein, zodat deze kunnen worden beheerd veilig via Groepsbeleid. 'Lift-en-shift' on-premises toepassingen naar de cloud moeten deze afhankelijkheden op de zakelijke identiteit infrastructuur worden omgezet.

Beheerders schakelen vaak op een van de volgende oplossingen om te voldoen aan de behoeften van de identiteit van hun toepassingen die zijn geïmplementeerd in Azure:

* Een site-naar-site VPN-verbinding tussen de werkbelastingen die worden uitgevoerd in Azure Infrastructure Services en de zakelijke directory on-premises implementeren.
* De zakelijke AD-domein/forest-infrastructuur uitbreiden door het instellen van de replica van domeincontrollers met behulp van virtuele machines in Azure.
* Implementeer een zelfstandig domein in Azure met domeincontrollers die zijn geïmplementeerd als virtuele machines in Azure.

Alle deze benaderingen lijden onder blokkering van de hoge kosten en administratieve overhead. Beheerders zijn vereist voor het implementeren van domeincontrollers met behulp van virtuele machines in Azure. Daarnaast moeten ze beheren, beveiligen, patch, bewaken, back-up en oplossen van deze virtuele machines. De afhankelijkheid van VPN-verbindingen naar de on-premises directory wordt geïmplementeerd in Azure worden blootgesteld aan tijdelijk afwijkende netwerkverbinding haperingen of stroomonderbrekingen werkbelastingen. Deze netwerkstoringen op zijn beurt leiden tot lagere bedrijfstijd en verminderde betrouwbaarheid voor deze toepassingen.

We ontworpen Azure AD Domain Services bieden een eenvoudiger alternatief.

### <a name="watch-an-introductory-video"></a>Een inleidende video bekijken

>[!VIDEO https://www.youtube.com/embed/T1Nd9APNceQ]

## <a name="introducing-azure-ad-domain-services"></a>Inleiding tot Azure AD Domain Services

Azure AD Domain Services beheerd domein van services zoals domeindeelname, groep-beleid, LDAP, Kerberos/NTLM-verificatie, die volledig compatibel zijn met Windows Server Active Directory biedt. U kunt deze domain-services zonder de noodzaak om te implementeren, beheren en domeincontrollers in de cloud patch verbruiken. Azure AD Domain Services kan worden geïntegreerd met uw bestaande Azure AD-tenant, waardoor het mogelijk dat gebruikers zich aanmelden met hun bedrijfsreferenties. Bovendien kunt u bestaande groepen en accounts van gebruikers toegang te krijgen tot bronnen, zodat de soepeler 'lift-en-verschuiving' van een on-premises resources voor Azure-infrastructuurservices.

Azure AD Domain Services-functionaliteit werkt naadloos samen ongeacht of uw Azure AD-tenant alleen in de cloud of gesynchroniseerd met uw lokale Active Directory.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure AD Domain Services voor organisaties die alleen in de cloud

Een alleen-Azure AD-tenant (vaak aangeduid als 'beheerde tenants') beschikt niet over een lokale identiteit footprint. Met andere woorden, zijn gebruikersaccounts, hun wachtwoorden en groepslidmaatschappen alle systeemeigen naar de cloud - dat wil zeggen, gemaakt en beheerd in Azure AD. Kijk eens naar dat Contoso een alleen is-Azure AD-tenant. Zoals u in de volgende afbeelding, heeft de beheerder van Contoso een virtueel netwerk geconfigureerd in Azure Infrastructure Services. Toepassingen en serverwerkbelastingen worden geïmplementeerd in dit virtuele netwerk in Azure virtuele machines. Aangezien Contoso een alleen-tenant is, worden alle gebruikers-id's, hun referenties en groepslidmaatschappen gemaakt en beheerd in Azure AD.

![Overzicht van Azure AD Domain Services](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso IT-beheerder kan Azure AD Domain Services inschakelt voor hun Azure AD-tenant en kies domeinservices beschikbaar maken in dit virtuele netwerk. Daarna Azure AD Domain Services voorziet in een beheerd domein en is beschikbaar in het virtuele netwerk. Alle gebruikersaccounts, groepslidmaatschappen en de referenties van de gebruiker is beschikbaar in de Contoso Azure AD-tenant zijn ook beschikbaar in deze nieuwe domein. Deze functie kunnen gebruikers in de organisatie aan te melden bij het domein met hun bedrijfsreferenties - bijvoorbeeld, wanneer u op afstand verbinding met machines domein via Extern bureaublad. Beheerders kunnen de toegang tot bronnen in het domein met bestaande groepslidmaatschappen inrichten. Toepassingen die zijn geïmplementeerd in virtuele machines op het virtuele netwerk kunnen functies zoals domeindeelname, LDAP lees-, LDAP-binding, NTLM en Kerberos-verificatie en Groepsbeleid gebruiken.

Enkele van de meest kenmerkende aspecten van het beheerde domein die is ingericht door Azure AD Domain Services zijn als volgt:

* Contoso IT-beheerder niet hoeft te beheren, patch of dit domein of de domeincontrollers voor dit beheerde domein controleren.
* Er is niet nodig voor het beheren van AD-replicatie voor dit domein. Accounts van gebruikers, groepslidmaatschappen en referenties van van Contoso Azure AD-tenant zijn automatisch beschikbaar binnen dit beheerde domein.
* Omdat het domein wordt beheerd door Azure AD Domain Services, Contoso van IT-beheerder heeft geen domein- of Ondernemingsadministrator-bevoegdheden voor dit domein.

### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure AD Domain Services voor hybride organisaties
Organisaties met een hybride IT-infrastructuur gebruiken een combinatie van cloud-bronnen en lokale bronnen. Deze organisaties synchroniseren van identiteitsgegevens van hun on-premises directory met hun Azure AD-tenant. Hybride organisaties gezocht voor het migreren van meer van de on-premises toepassingen naar de cloud, met name oudere directory-bewuste toepassingen, Azure AD Domain Services kunnen nuttig zijn voor deze.

Litware Corporation heeft geïmplementeerd [Azure AD Connect](../active-directory/active-directory-aadconnect.md), om te synchroniseren van identiteitsgegevens van hun on-premises directory met hun Azure AD-tenant. De gegevens van de identiteit die wordt gesynchroniseerd bevat gebruikersaccounts, hun referentie-hashes voor verificatie (Wachtwoordsynchronisatie) en groepslidmaatschappen.

> [!NOTE]
> **Wachtwoordsynchronisatie is verplicht voor hybride organisaties gebruikmaken van Azure AD Domain Services**. Deze vereiste is omdat de referenties van gebruikers in het beheerde domein verstrekt door Azure AD Domain Services zijn nodig voor deze gebruikers verifiëren via NTLM of Kerberos-verificatiemethoden.
>
>

![Azure AD Domain Services voor Litware Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

De voorgaande illustratie ziet hoe organisaties met een hybride IT-infrastructuur, zoals Litware Corporation, Azure AD Domain Services kunnen gebruiken. De toepassingen en serverwerkbelastingen waarvoor domeinservices van Litware zijn geïmplementeerd in een virtueel netwerk in Azure Infrastructure Services. Van Litware IT-beheerder kan Azure AD Domain Services inschakelt voor hun Azure AD-tenant en kies een beheerd domein beschikbaar maken in dit virtuele netwerk. Aangezien Litware een organisatie met een hybride IT-infrastructuur is, worden gebruikersaccounts, groepen en referenties gesynchroniseerd met hun Azure AD-tenant vanuit hun on-premises directory. Deze functie kunnen gebruikers zich aanmelden bij het domein met hun bedrijfsreferenties - bijvoorbeeld, wanneer u extern verbinding maakt met machines toegevoegd aan het domein via Extern bureaublad. Beheerders kunnen de toegang tot bronnen in het domein met bestaande groepslidmaatschappen inrichten. Toepassingen die zijn geïmplementeerd in virtuele machines op het virtuele netwerk kunnen functies zoals domeindeelname, LDAP lees-, LDAP-binding, NTLM en Kerberos-verificatie en Groepsbeleid gebruiken.

Enkele van de meest kenmerkende aspecten van het beheerde domein die is ingericht door Azure AD Domain Services zijn als volgt:

* Het beheerde domein is een zelfstandig domein. Het is niet een uitbreiding van van Litware lokaal domein.
* Van Litware IT-beheerder niet hoeft te beheren, patch, of domeincontrollers voor dit beheerde domein bewaken.
* Er is niet nodig voor het beheren van AD-replicatie aan dit domein. Accounts van gebruikers, groepslidmaatschappen en referenties van van Litware on-premises adreslijst worden gesynchroniseerd naar Azure AD via Azure AD Connect. Deze accounts van gebruikers, groepslidmaatschappen en referenties zijn automatisch beschikbaar binnen het beheerde domein.
* Omdat het domein wordt beheerd door Azure AD Domain Services, Litware van IT-beheerder heeft geen domein- of Ondernemingsadministrator-bevoegdheden voor dit domein.

## <a name="benefits"></a>Voordelen
Met Azure AD Domain Services, kunt u profiteren van de volgende voordelen:

* **Eenvoudige** – u kunt voldoen aan de behoeften van de identiteit van virtuele machines geïmplementeerd op Azure-infrastructuurservices met een paar eenvoudige klikken. U hoeft niet te implementeren en beheren van infrastructuur voor identiteiten in de Azure of setup verbinding terug naar uw on-premises identity-infrastructuur.
* **Geïntegreerde** : Azure AD Domain Services is nauw geïntegreerd met uw Azure AD-tenant. U kunt nu Azure AD gebruiken als een map voor geïntegreerde enterprise cloud-gebaseerde die caters aan de behoeften van uw moderne toepassingen en de traditionele directory-bewuste toepassingen.
* **Compatibel** : Azure AD Domain Services is gebaseerd op de beproefde enterprise hoogwaardige infrastructuur van Windows Server Active Directory. Daarom kunnen uw toepassingen afhankelijk van een grotere mate van compatibiliteit met Windows Server Active Directory-functies. Niet alle functies die beschikbaar zijn in Windows Server AD zijn momenteel beschikbaar zijn in Azure AD Domain Services. Beschikbare functies zijn echter compatibel is met de bijbehorende Windows Server AD-functies die u op in uw on-premises infrastructuur vertrouwen. De mogelijkheden van de join LDAP, Kerberos, NTLM, Groepsbeleid en domein vormen een goed ontwikkelde aanbieding die zijn getest en verfijnd via verschillende versies van Windows Server.
* **Rendabele** – met Azure AD Domain Services, kunt u voorkomen dat de infrastructuur en werkbelasting die is gekoppeld aan het beheer van infrastructuur voor identiteiten ter ondersteuning van traditionele directory-bewuste toepassingen. U kunt deze toepassingen voor Azure-infrastructuurservices verplaatsen en profiteren van lagere operationele kosten.


## <a name="next-steps"></a>Volgende stappen
### <a name="learn-more-about-azure-ad-domain-services"></a>Meer informatie over Azure AD Domain Services
* [Functies](active-directory-ds-features.md)
* [Implementatiescenario 's](active-directory-ds-scenarios.md)
* [Als Azure AD Domain Services aansluit op uw use cases weten](active-directory-ds-comparison.md)
* [Begrijpen hoe Azure AD Domain Services wordt gesynchroniseerd met uw Azure AD-directory](active-directory-ds-synchronization.md)

### <a name="get-started-with-azure-ad-domain-services"></a>Aan de slag met Azure AD Domain Services
* [Azure AD Domain Services met Azure portal inschakelen](active-directory-ds-getting-started.md)
