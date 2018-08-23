---
title: Azure Active Directory Domain Services voor Azure Cloud Solution Providers | Microsoft Docs
description: Azure Active Directory Domain Services voor Azure Cloud Solution Providers.
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 56ccb219-11b2-4e43-9f07-5a76e3cd8da8
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/08/2017
ms.author: maheshu
ms.openlocfilehash: 8d87312591f44bac5fd9a4ff63eccc19333a870c
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "42055681"
---
# <a name="azure-active-directory-ad-domain-services-for-azure-cloud-solution-providers-csp"></a>Azure Active Directory (AD) Domain Services voor Azure Cloud Solution Providers (CSP)
In dit artikel wordt uitgelegd hoe u Azure AD Domain Services kunt gebruiken in een Azure CSP-abonnement.

## <a name="overview-of-azure-csp"></a>Overzicht van Azure CSP
Azure CSP is een programma voor Microsoft-Partners en biedt een licentie-kanaal voor de verschillende Microsoft-cloudservices. Azure CSP maakt partners verkopen beheren, de eigenaar van de facturering regelen, bieden technische ondersteuning en facturering en worden van de klant één aanspreekpunt. Bovendien biedt Azure CSP een uitgebreide reeks hulpprogramma's, met inbegrip van een selfservice portal en de bijbehorende API's. Deze hulpprogramma's inschakelen CSP-partners eenvoudig inrichten en beheren van Azure-resources en facturering bieden voor klanten en hun abonnementen.

De [portal Partnercentrum](https://docs.microsoft.com/azure/cloud-solution-provider/overview/partner-center-overview) fungeert als een beginpunt voor alle Azure CSP-partners. Het biedt mogelijkheden voor uitgebreide klant, geautomatiseerde verwerking en meer. Azure CSP-partners kunt Partner Center mogelijkheden met behulp van een web gebaseerde gebruikersinterface of met behulp van PowerShell en verschillende API-aanroepen.

Het volgende diagram illustreert de werking van het CSP-model op hoog niveau. Contoso heeft een Azure AD Active Directory. Ze hebben een partnerschap met een CSP, die u implementeert en beheert, resources in het Azure CSP-abonnement. Contoso mogelijk ook reguliere (direct) Azure-abonnementen, die rechtstreeks op Contoso worden gefactureerd.

![Overzicht van het CSP-model](./media/csp/csp_model_overview.png)

De CSP-partner tenant heeft drie speciale agent groepen - Admin-agents, Helpdesk-agents en verkoop-agents. De groep Admin agents is toegewezen aan de tenant administrator-rol in Azure AD-directory van Contoso. Als gevolg hiervan is een gebruiker die behoort tot de CSP-partner-beheerdersgroep agents beheerdersbevoegdheden tenant in Azure AD-directory van Contoso. Wanneer de bepalingen van de CSP-partner een Azure CSP-abonnement voor Contoso, hun groep Administrator agents wordt toegewezen aan de eigenaarsrol voor dat abonnement. Als gevolg hiervan zijn de CSP-partner admin agents de vereiste machtigingen voor het inrichten van Azure-resources zoals virtuele machines, virtuele netwerken en Azure AD Domain Services namens Contoso.

Zie voor meer informatie de [overzicht van Azure CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview)

## <a name="benefits-of-using-azure-ad-domain-services-in-an-azure-csp-subscription"></a>Voordelen van het gebruik van Azure AD Domain Services in een Azure CSP-abonnement
Azure AD Domain Services biedt Windows Server AD compatibele services in Azure, zoals LDAP, Kerberos/NTLM-verificatie, domein, Groepsbeleid en DNS. In de decennia zijn veel toepassingen gebouwd om te werken aan de hand AD met behulp van deze mogelijkheden. Veel onafhankelijke softwareleveranciers (ISV's) zijn gebouwd en geïmplementeerde toepassingen bij hun klanten. Deze toepassingen zijn kostbare ter ondersteuning van omdat waarvoor vaak toegang tot de verschillende omgevingen waarin deze toepassingen worden geïmplementeerd. Met Azure CSP-abonnementen hebt u een alternatief eenvoudiger met de schaal en flexibiliteit van Azure.

Azure AD Domain Services biedt nu ondersteuning voor Azure CSP-abonnementen. U kunt nu uw toepassing in een Azure CSP-abonnement dat is gekoppeld aan Azure AD-adreslijst van uw klant implementeren. Als gevolg hiervan uw werknemers (ondersteuningsmedewerkers) kunnen beheren, te beheren en onderhouden van de virtuele machines waarop uw toepassing is geïmplementeerd, met bedrijfsreferenties van uw organisatie. Bovendien kunt u een beheerd domein van Azure AD Domain Services voor Azure AD-adreslijst van uw klant inrichten. Uw toepassing is verbonden met het beheerde domein van uw klant. Daarom mogelijkheden binnen uw toepassing die afhankelijk van Kerberos/NTLM, LDAP zijn, of de [System.DirectoryServices API](https://msdn.microsoft.com/library/system.directoryservices) werken naadloos op basis van de directory van uw klant. Uw eindgebruikers merken aanzienlijk kunnen profiteren van uw toepassing als een service, zonder dat u hoeft zich geen zorgen over het onderhouden van de infrastructuur van die de toepassing wordt geïmplementeerd op te gebruiken.

Alle facturering voor Azure-resources die u in dat abonnement, met inbegrip van Azure AD Domain Services, in rekening voor u gebracht wordt verbruikt. U beheren volledige controle over de relatie met de klant als het gaat om verkopen, facturering en technische ondersteuning enzovoort. Dankzij de flexibiliteit van de CSP van de Azure-platform, een klein team van ondersteuning voor agents veel dergelijke klanten die instanties hebben kunt onderhouden van uw toepassing geïmplementeerd.


## <a name="csp-deployment-models-for-azure-ad-domain-services"></a>CSP-implementatiemodellen voor Azure AD Domain services
Er zijn twee manieren waarop u Azure AD Domain Services met een Azure CSP-abonnement gebruiken kunt. Kies de juiste is, op basis van de beveiliging en eenvoud overwegingen die uw klanten hebben.

### <a name="direct-deployment-model"></a>Directe implementatiemodel
In dit implementatiemodel is Azure AD Domain Services ingeschakeld binnen een virtueel netwerk die behoren tot het Azure CSP-abonnement. De CSP-partner admin agents hebben de volgende bevoegdheden:
* Globale beheerder-bevoegdheden in Azure AD-map van de klant.
* Eigenaarsbevoegdheden abonnement op het Azure CSP-abonnement.

![Directe implementatiemodel](./media/csp/csp_direct_deployment_model.png)

In dit implementatiemodel kunnen de CSP-provider admin agents identiteiten beheren voor de klant. Deze agents beheerder hebben de mogelijkheid om het inrichten van nieuwe gebruikers, groepen, toepassingen in Azure AD-directory van de klant enzovoort toevoegen. Dit implementatiemodel mogelijk is geschikt voor kleinere organisaties die niet de beheerder van een toegewezen identiteit hebben of voorkeur voor de CSP-partner voor het beheren van identiteiten namens hen.


### <a name="peered-deployment-model"></a>Gekoppelde implementatiemodel
In dit implementatiemodel is Azure AD Domain Services ingeschakeld binnen een virtueel netwerk die horen bij de klant - dat wil zeggen, een directe Azure-abonnement voor zijn betaald door de klant. Toepassingen binnen een virtueel netwerk die behoren tot de CSP-abonnement van de klant vervolgens door de CSP-partner kunt implementeren. De virtuele netwerken kunnen vervolgens worden verbonden met behulp van peering op virtueel netwerk. Als gevolg hiervan kunt de werkbelastingen/toepassingen die zijn geïmplementeerd door de CSP-partner in het abonnement op Azure CSP verbinden met de klant beheerde domein in van de klant direct Azure-abonnement ingericht.

![Gekoppelde implementatiemodel](./media/csp/csp_peered_deployment_model.png)

Dit implementatiemodel biedt een scheiding van bevoegdheden en kunt de CSP-partner helpdesk agents aan het Azure-abonnement beheren en implementeren en beheren van resources binnen deze. De CSP-partner helpdesk agents hoeft echter niet aan de globale beheerder-bevoegdheden hebben op van de klant Azure AD-directory. Van de klant identiteitsbeheerders kunnen doorgaan met het beheren van identiteiten voor hun organisatie.

Kan dit implementatiemodel is geschikt voor scenario's waarin een ISV (independent software vendor) een gehoste biedt versie van hun on-premises toepassing, die ook verbinding maken met de klant moet de AD.


## <a name="administering-azure-ad-domain-services-managed-domains-in-csp-subscriptions"></a>Beheer van Azure AD Domain Services beheerde domeinen in CSP-abonnementen
De volgende belangrijke overwegingen zijn van toepassing bij het beheren van een beheerd domein van een abonnement op Azure CSP:

* **Agents voor CSP-beheerder kunnen een beheerd domein met hun referenties worden ingericht:** Azure AD Domain Services biedt ondersteuning voor Azure CSP-abonnementen. Gebruikers die behoren tot een CSP-partner-beheerdersgroep agents kunnen daarom een nieuw Azure AD Domain Services beheerde domein inrichten.

* **CSP's kunnen maken van nieuwe beheerde domeinen voor hun klanten met behulp van PowerShell script:** Zie [hoe om in te schakelen van Azure AD Domain Services met behulp van PowerShell](active-directory-ds-enable-using-powershell.md) voor meer informatie.

* **CSP-beheerder agents doorlopende beheertaken in het beheerde domein met hun referenties niet uitvoeren:** CSP beheergebruikers routinematige taken binnen het beheerde domein met hun referenties niet uitvoeren. Deze gebruikers niet tot de Azure AD-directory van de klant en hun referenties zijn niet beschikbaar in Azure AD-directory van de klant. Azure AD Domain Services heeft daarom geen toegang tot de wachtwoord-hashes van Kerberos en NTLM voor deze gebruikers. Als gevolg hiervan worden niet dergelijke gebruikers geverifieerd in Azure AD Domain Services beheerde domeinen.

  > [!WARNING]
  > **U moet een gebruikersaccount in de directory van de klant aan het doorlopende beheertaken uitvoeren in het beheerde domein maken.**
  > U kunt zich niet bij het beheerde domein met behulp van de beheerder van een CSP-referenties. De referenties van een gebruikersaccount die horen bij de klant Azure AD-directory gebruiken om dit te doen. U moet deze referenties voor taken, zoals virtuele machines toevoegen aan het beheerde domein, beheren van DNS, beheer van de groep beleid, enzovoort.
  >

* **Het gebruikersaccount dat is gemaakt voor het doorlopende beheer moet worden toegevoegd aan de groep 'AAD DC Administrators':** heeft bevoegdheden voor bepaalde taken voor gedelegeerd beheer in het beheerde domein van de groep 'AAD DC Administrators'. Deze taken omvatten het configureren van DNS, het maken van organisatie-eenheden, beheren van Groepsbeleid enzovoort. Voor CSP-partner die taken uitvoeren op een beheerd domein moet een gebruikersaccount worden gemaakt binnen de Azure AD-directory van de klant. De referenties voor dit account moeten worden gedeeld met de CSP-partner admin-agents. Dit gebruikersaccount moet ook worden toegevoegd aan de groep 'AAD DC Administrators' om in te schakelen configuratietaken in het beheerde domein moet worden uitgevoerd met behulp van dit gebruikersaccount.


## <a name="next-steps"></a>Volgende stappen
* [Schrijf u in het Azure CSP-programma](https://docs.microsoft.com/partner-center/enrolling-in-the-csp-program) en beginnen met het maken van zakelijke via Azure CSP.
* Bekijk de lijst [Azure-services beschikbaar zijn in Azure CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services).
* [Azure AD Domain Services inschakelen met behulp van PowerShell](active-directory-ds-enable-using-powershell.md)
* [Aan de slag met Azure AD Domain Services](active-directory-ds-getting-started.md)
