---
title: Azure Active Directory Domain Services voor Azure Cloud Solution Providers | Microsoft Docs
description: Azure Active Directory Domain Services voor Azure Cloud Solution Providers.
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 56ccb219-11b2-4e43-9f07-5a76e3cd8da8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: maheshu
ms.openlocfilehash: 313c4853b0f585921739779bb764c50036c9ac8a
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-active-directory-ad-domain-services-for-azure-cloud-solution-providers-csp"></a>Azure Active Directory (AD) Domain Services voor Azure Cloud Solution Providers (CSP)
Dit artikel wordt uitgelegd hoe u Azure AD Domain Services kunt gebruiken in een Azure-CSP-abonnement.

## <a name="overview-of-azure-csp"></a>Overzicht van Azure CSP
Azure CSP is een programma voor Microsoft-Partners en biedt een licentie-kanaal voor verschillende Microsoft-cloudservices. Azure CSP kan partners omzetcijfers, eigenaar van de gefactureerd bieden ondersteuning voor technische en facturering en van de klant één aanspreekpunt worden. Daarnaast biedt Azure CSP een volledige set van hulpprogramma's, met inbegrip van een selfservice portal en de bijbehorende API's. Deze hulpprogramma's inschakelen CSP partners eenvoudig inrichten en beheren van Azure-resources en facturering voor klanten en hun abonnementen bieden.

De [Partnercentrum portal](https://docs.microsoft.com/azure/cloud-solution-provider/overview/partner-center-overview) fungeert als een toegangspunt voor alle Azure-CSP-partners. Het biedt uitgebreide klant beheermogelijkheden en geautomatiseerde verwerking. Azure CSP partners kunnen Partnercentrum mogelijkheden gebruiken met behulp van een webgebaseerde gebruikersinterface of met behulp van PowerShell en verschillende API-aanroepen.

Het volgende diagram illustreert de werking van het CSP-model op hoog niveau. Contoso heeft een Azure AD Active Directory. Hebben ze een verbinding met een CSP die u implementeert en beheert resources in de Azure-CSP-abonnement. Contoso kan ook reguliere (direct) Azure-abonnementen die worden gefactureerd rechtstreeks naar Contoso hebben.

![Overzicht van het CSP-model](./media/csp/csp_model_overview.png)

De CSP-partner tenant heeft drie speciale agent groepen - Admin agents, Helpdesk agents en verkoop-agents. De groep Admin agents is toegewezen aan de tenant-beheerdersrol in Azure AD-directory van Contoso. Een gebruiker die horen bij de CSP-partner agents administratorgroep heeft als gevolg hiervan beheerdersbevoegdheden tenant in Azure AD-directory van Contoso. Wanneer de bepalingen van de partner CSP een Azure-CSP-abonnement voor Contoso, de groep Administrator agents wordt toegewezen aan de rol van eigenaar voor dat abonnement. Als gevolg hiervan zijn de CSP-partner admin agents de vereiste machtigingen voor het inrichten van Azure-resources zoals virtuele machines, virtuele netwerken en Azure AD Domain Services namens Contoso.

Zie voor meer informatie de [Azure CSP-overzicht](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview)

## <a name="benefits-of-using-azure-ad-domain-services-in-an-azure-csp-subscription"></a>Voordelen van het gebruik van Azure AD Domain Services in een Azure-CSP-abonnement
Azure AD Domain Services biedt een compatibel Windows Server AD-services in Azure, zoals LDAP, Kerberos/NTLM-verificatie, domein, Groepsbeleid en DNS. Via de tientallen jaren zijn veel toepassingen gebouwd om te werken met AD met behulp van deze mogelijkheden. Veel onafhankelijke softwareleveranciers (ISV's) zijn samengesteld en geïmplementeerde toepassingen op bedrijven hun klanten. Deze toepassingen zijn kostbare ter ondersteuning van omdat die vaak is toegang vereist tot de verschillende omgevingen waarin deze toepassingen worden geïmplementeerd. Met de CSP van de Azure-abonnementen hebt u een eenvoudigere alternatief met de schaal en de flexibiliteit van Azure.

Azure AD Domain Services ondersteunt nu de CSP van de Azure-abonnementen. U kunt nu uw toepassing in een Azure-CSP-abonnement dat is gekoppeld aan Azure AD-adreslijst van uw klant implementeren. Als gevolg hiervan uw werknemers (ondersteuningsmedewerkers) kunnen beheren, te beheren en onderhouden van de virtuele machines waarop uw toepassing wordt geïmplementeerd met zakelijke referenties van uw organisatie. Bovendien kunt u een beheerd domein van Azure AD Domain Services voor Azure AD-adreslijst van uw klant inrichten. Uw toepassing is verbonden met het beheerde domein van uw klant. Daarom mogelijkheden in uw toepassing die afhankelijk van Kerberos/NTLM, LDAP zijn, of de [System.DirectoryServices API](https://msdn.microsoft.com/library/system.directoryservices) werk naadloos tegen de directory van uw klant. Uw klanten end profiteren aanzienlijk van uw toepassing als een service zonder te hoeven maken over het onderhouden van de infrastructuur die de toepassing wordt geïmplementeerd op verbruikt.

Alle facturering voor Azure-resources u verbruikt in dat het abonnement, met inbegrip van Azure AD Domain Services, wordt in rekening gebracht voor u. U onderhouden volledige controle over de relatie met de klant als het gaat om de verkoop, facturering, technische ondersteuning enzovoort. De flexibiliteit van de CSP van de Azure-platform, een klein team van ondersteuning agents veel dergelijke klanten die exemplaren kunt onderhouden van uw toepassing geïmplementeerd.


## <a name="csp-deployment-models-for-azure-ad-domain-services"></a>CSP implementatiemodellen voor Azure AD Domain services
Er zijn twee manieren waarop u Azure AD Domain Services met een CSP van de Azure-abonnement gebruiken kunt. Kies de juiste is op basis van de overwegingen voor beveiliging en eenvoud die uw klanten hebben.

### <a name="direct-deployment-model"></a>Directe implementatiemodel
In dit implementatiemodel is Azure AD Domain Services ingeschakeld binnen een virtueel netwerk die horen bij de Azure-CSP-abonnement. De CSP-partner admin agents hebben de volgende bevoegdheden:
* Globale administrator-bevoegdheden in Azure AD-directory van de klant.
* Abonnement eigenaar bevoegdheden op de Azure-CSP-abonnement.

![Directe implementatiemodel](./media/csp/csp_direct_deployment_model.png)

In dit implementatiemodel beheren van de CSP-provider admin agents identiteiten voor de klant. Deze agents beheerder hebben de mogelijkheid voor het inrichten van nieuwe gebruikers, groepen, het toevoegen van toepassingen in Azure AD-directory van de klant enzovoort. Dit implementatiemodel is mogelijk geschikt voor kleinere organisaties die niet de beheerder van een toegewezen identiteit hebben of liever voor de CSP-partner voor het beheren van identiteiten in hun naam.


### <a name="peered-deployment-model"></a>Implementatiemodel brengen
In dit implementatiemodel is Azure AD Domain Services ingeschakeld binnen een virtueel netwerk die horen bij de klant - dat wil zeggen, een directe Azure-abonnement voor zijn betaald door de klant. De CSP-partner vervolgens de toepassingen binnen een virtueel netwerk die horen bij de CSP-abonnement van de klant kunt implementeren. De virtuele netwerken kunnen vervolgens worden verbonden met het virtuele netwerk van Azure-peering. Als gevolg hiervan kunt de werkbelastingen/toepassingen die zijn geïmplementeerd door de CSP-partner in het Azure-CSP-abonnement verbinden met de klant beheerd domein ingericht in van de klant direct Azure-abonnement.

![Implementatiemodel brengen](./media/csp/csp_peered_deployment_model.png)

Dit implementatiemodel biedt een scheiding van bevoegdheden en maakt de CSP-partner helpdesk agents naar het Azure-abonnement beheren en implementeren en beheren van resources binnen het. De CSP-partner helpdesk agents hoeft echter niet globale beheerdersrechten hebben op van de klant Azure AD-directory. Identiteiten beheren voor hun organisatie kunnen beheerders van de klant identiteit blijven.

Dit implementatiemodel is mogelijk geschikt voor scenario's waarbij een ISV (onafhankelijke softwareleverancier) een gehoste biedt versie van hun on-premises toepassing, die ook verbinding maken met de klant moet de AD.


## <a name="administering-azure-ad-domain-services-managed-domains-in-csp-subscriptions"></a>Beheer van Azure AD Domain Services beheerde domeinen in het CSP-abonnementen
De volgende belangrijke overwegingen bij het beheren van een beheerd domein in een Azure-CSP-abonnement van toepassing:

* **CSP admin agents een beheerd domein met hun referenties kunnen inrichten:** Azure AD Domain Services ondersteunt de CSP van de Azure-abonnementen. Gebruikers die behoren tot een CSP-partner-administratorgroep agents kunnen daarom een nieuwe Azure AD Domain Services beheerd domein inrichten.

* **CSP's kunnen maken van nieuwe beheerde domeinen voor klanten met behulp van PowerShell script:** Zie [hoe het inschakelen van Azure AD Domain Services met behulp van PowerShell](active-directory-ds-enable-using-powershell.md) voor meer informatie.

* **CSP admin agents niet lopende beheertaken uitvoeren op het beheerde domein met hun referenties:** beheergebruikers CSP routinematige beheertaken binnen het beheerde domein met hun referenties kunnen niet uitvoeren. Deze gebruikers extern zijn voor Azure AD-directory van de klant en hun referenties zijn niet beschikbaar in Azure AD-directory van de klant. Azure AD Domain Services heeft daarom geen toegang tot de wachtwoord-hashes van Kerberos en NTLM voor deze gebruikers. Als gevolg hiervan kunnen niet dergelijke gebruikers worden geverifieerd in Azure AD Domain Services beheerd-domeinen.

  > [!WARNING]
  > **U moet een gebruikersaccount in de directory van de klant lopende beheertaken uitvoeren op het beheerde domein maken.**
  > U kan niet aanmelden bij het beheerde domein met een CSP-beheerder de referenties van gebruiker. De referenties van een gebruikersaccount die horen bij de klant Azure AD-directory gebruiken om dit te doen. In dat geval moet u deze referenties in voor taken zoals het toevoegen van virtuele machines aan het beheerde domein, DNS implementeren, beheren van beleid, enzovoort.
  >

* **Het gebruikersaccount dat is gemaakt voor het doorlopende beheer moet worden toegevoegd aan de groep 'AAD DC Administrators':** bevoegdheden van de groep 'AAD DC Administrators' voor bepaalde taken gedelegeerd beheer op het beheerde domein. Deze taken omvatten het configureren van DNS, het maken van de organisatie-eenheden, het beheren van Groepsbeleid enzovoort. Voor een CSP-partner dergelijke taken uitvoeren op een beheerd domein, moet een gebruikersaccount worden gemaakt in Azure AD-directory van de klant. De referenties voor dit account moeten worden gedeeld met de CSP-partner admin agents. Dit gebruikersaccount moet ook worden toegevoegd aan de groep 'AAD DC Administrators' configuratietaken op het beheerde domein moet worden uitgevoerd met dit gebruikersaccount inschakelen.


## <a name="next-steps"></a>Volgende stappen
* [Deelnemen aan het programma Azure CSP](https://partnercenter.microsoft.com/partner/programs) en beginnen met het maken van transacties via het Azure-CSP.
* Bekijk de lijst met [Azure-services beschikbaar zijn in Azure CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services).
* [Azure AD Domain Services inschakelen met behulp van PowerShell](active-directory-ds-enable-using-powershell.md)
* [Aan de slag met Azure AD Domain Services](active-directory-ds-getting-started.md)
