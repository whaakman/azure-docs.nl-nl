---
title: Basisbeginselen van Azure Stack beheer | Microsoft Docs
description: Meer informatie over wat u moet weten voor het beheer van Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 856738a7-1510-442a-88a8-d316c67c757c
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: fa77faac195de3be7bf7b2785eb589b030a6e6ce
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-stack-administration-basics"></a>Basisbeginselen van Azure Stack-beheer

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Er zijn verschillende dingen die u weten moet als u geen ervaring met Azure-Stack-beheer. In deze richtlijnen biedt een overzicht van uw rol als een Azure-Stack-operator en wat u moet uw gebruikers ze snel productief worden uitgelegd.

## <a name="understand-the-builds"></a>De builds begrijpen

### <a name="integrated-systems"></a>Geïntegreerde systemen

Als u een Azure-Stack geïntegreerd systeem, worden de bijgewerkte versies van Azure-Stack verspreid via updatepakketten. U kunt deze pakketten te importeren en ze toepassen met behulp van de tegel Updates in de beheerdersportal.
 
### <a name="development-kit"></a>Development kit

Als u de Azure-Stack Development Kit, raadpleegt u de [wat is Azure Stack?](azure-stack-poc.md) artikel om ervoor te zorgen dat u het doel van de development kit en de beperkingen begrijpt. Als een 'sandbox,' kunt u Azure-Stack, evalueren en ontwikkelen en testen van uw apps in een niet-productieomgeving moet u de development kit. (Zie voor informatie over de implementatie de [Azure Stack Development Kit implementatie](azure-stack-deploy-overview.md) Quick Start.)

Zoals Azure innoveren we snel. We je regelmatig nieuwe builds vrijgeven. Als u de development kit uitvoert en u verplaatsen naar de laatste build wilt, moet u [implementeren Azure Stack](azure-stack-redeploy.md). U kunt geen updatepakketten toepassen. Dit proces duurt, maar het voordeel is dat u de nieuwste functies kunt uitproberen. De documentatie development kit op onze website weerspiegelt de meest recente release-build.

## <a name="learn-about-available-services"></a>Meer informatie over de beschikbare services

U moet een afhankelijkheid van welke services u beschikbaar voor uw gebruikers maken kunt. Azure-Stack ondersteunt een subset van de Azure-services. De lijst met ondersteunde services blijft ontwikkelen.

**Fundamentele services**

Azure-Stack omvat standaard de volgende 'fundamentele services' wanneer u Azure-Stack implementeert:

- Compute
- Storage
- Netwerken
- Key Vault

U kunt deze fundamentele Services Infrastructure-as-a-Service (IaaS) aanbieden aan uw gebruikers met een minimale configuratie.

**Extra services**

Op dit moment ondersteunen we de volgende aanvullende Platform-as-a-Service (PaaS)-services:

- App Service
- Azure Functions
- SQL- en MySQL-databases

Deze services vereisen aanvullende configuratie voordat u ze beschikbaar stelt aan uw gebruikers. Zie 'De zelfstudies' en de secties 'services How-to guides\Offer' Azure Stack operator documentatie voor meer informatie.

**Roadmap voor service**

Azure-Stack blijven voegt ondersteuning voor Azure-services. Zie voor de verwachte roadmap de [Azure Stack: een uitbreiding van Azure](https://go.microsoft.com/fwlink/?LinkId=842846&clcid=0x409) technisch document. U kunt ook bewaken de [Azure Stack blogberichten](https://azure.microsoft.com/blog/tag/azure-stack-technical-preview) voor nieuwe meldingen.

## <a name="what-tools-do-i-use-to-manage"></a>Welke hulpprogramma's kan ik gebruiken om te beheren?
 
U kunt de [beheerdersportal](azure-stack-manage-portals.md) of PowerShell voor het beheren van Azure-Stack. De eenvoudigste manier om meer informatie over de basisconcepten is via de portal. Als u gebruiken van PowerShell wilt, zijn er voorbereidende stappen. U moet [installeren](azure-stack-powershell-install.md) PowerShell [downloaden](azure-stack-powershell-download.md) aanvullende modules en [configureren](azure-stack-powershell-configure-admin.md) PowerShell.

Azure Stack maakt gebruik van Azure Resource Manager als de onderliggende implementatie, beheer en organisatie-mechanisme. Als u Azure-Stack beheren en ondersteuning aan gebruikers, moet u meer informatie over Resource Manager. Zie de [aan de slag met Azure Resource Manager](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf) technisch document.

## <a name="your-typical-responsibilities"></a>Uw gebruikelijke verantwoordelijkheden

Gebruikers willen services gebruiken. Uw belangrijkste functie is deze services hen ter beschikking vanuit het perspectief. U moet bepalen welke services te bieden en deze services beschikbaar maken door te plannen, aanbiedingen en quota's maken. Zie voor meer informatie [overzicht van services in Azure-Stack van aanbieding](azure-stack-offer-services-overview.md). 

U moet ook items toevoegen aan [de marketplace](azure-stack-marketplace.md), zoals installatiekopieën van virtuele machines. De eenvoudigste manier is het [downloaden marketplace-items van Azure naar Azure Stack](azure-stack-download-azure-marketplace-item.md).

> [!NOTE]
> Als u testen van uw abonnementen, aanbiedingen en services wilt, moet u de [gebruikersportal](azure-stack-manage-portals.md); niet de beheerdersportal.

Naast het leveren van services, moet u alle normale taken van een operator te houden van Azure-Stack actief en werkend uitvoeren. Deze taken omvatten het volgende:

- Gebruikersaccounts toevoegen (voor [Azure Active Directory](azure-stack-add-new-user-aad.md) implementatie of voor [Active Directory Federation Services](azure-stack-add-users-adfs.md) implementatie)
- [Toegang op rollen gebaseerde toegangsbeheer (RBAC) rollen toewijzen](azure-stack-manage-permissions.md) (dit is niet beperkt tot administrators.)
- [Status van de groepsbeleidstructuur controleren](azure-stack-monitor-health.md)
- Beheren [netwerk](azure-stack-viewing-public-ip-address-consumption.md) en [opslag](azure-stack-manage-storage-accounts.md) resources
- Vervang de beschadigde hardware, bijvoorbeeld [een foutieve schijf vervangen](azure-stack-replace-disk.md).

## <a name="what-to-tell-your-users"></a>Wat u uw gebruikers vertelt

U moet uw gebruikers laten weten hoe werken met services in Azure-Stack, verbinding maken met de omgeving en abonneren op aanbiedingen. Dat kunt u uw gebruikers kunt u naast eventuele aangepaste documentatie gebruikers naar de Azure-documentatie voor Stack gebruikers site verwijzen.

**Begrijpen hoe werken met services in Azure-Stack**

Er is informatie die uw gebruikers begrijpen moeten voordat ze werken met services en bouwen van apps in Azure-Stack. Er zijn bijvoorbeeld specifieke vereisten voor PowerShell en API-versie. Er zijn ook enkele functie delta's tussen een service in Azure en de equivalente service in Azure-Stack. Zorg ervoor dat uw gebruikers Raadpleeg de volgende artikelen:

- [Belangrijke overwegingen: met behulp van services of het ontwikkelen van apps voor Azure-Stack](user/azure-stack-considerations.md)
- [Overwegingen voor virtuele Machines in Azure Stack](user/azure-stack-vm-considerations.md)
- [Opslag: de verschillen en overwegingen met betrekking tot](user/azure-stack-acs-differences.md)

De informatie in deze artikelen bevat een overzicht van de verschillen tussen een service in Azure en Azure-Stack. Er is een aanvulling op de informatie die beschikbaar is voor een Azure-service in de globale Azure-documentatie.

**Verbinding maken met Azure-Stack als een gebruiker**

In een ontwikkelingsomgeving kit als een gebruiker geen toegang tot de extern bureaublad op de host van de kit ontwikkeling moeten ze configureren een virtueel particulier netwerk (VPN)-verbinding toegang te krijgen tot Azure-Stack. Zie [verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md). 

Uw gebruikers wilt weten hoe [toegang tot de gebruikersportal ](user/azure-stack-use-portal.md) of verbinding maken via PowerShell. In een geïntegreerde systeemomgeving varieert portal adres van de gebruiker per implementatie. U moet uw gebruikers voorzien van de juiste URL.

Als u PowerShell, gebruikers hebben mogelijk resourceproviders registreren voordat ze kunnen de services gebruiken. (Een service een resourceprovider beheert. For example, de netwerken resourceprovider-resources, zoals virtuele netwerken, netwerkinterfaces en netwerktaakverdelers beheert.) Ze moeten [installeren](user/azure-stack-powershell-install.md) PowerShell [downloaden](user/azure-stack-powershell-download.md) aanvullende modules en [configureren](user/azure-stack-powershell-configure-user.md) PowerShell (waaronder registratie resourceprovider).

**Abonneren op een aanbieding**

Voordat een gebruiker toegang services tot, moeten zij [abonneren op een aanbieding](azure-stack-subscribe-plan-provision-vm.md) die u hebt gemaakt als een operator.

## <a name="where-to-get-support"></a>Waar u ondersteuning krijgen

### <a name="integrated-systems"></a>Geïntegreerde systemen

Voor een geïntegreerde is er een gecoördineerde escalatie en omzettingsproces tussen Microsoft en onze hardwarepartners oorspronkelijke leveranciers (OEM).

Als er een probleem met de cloud services, wordt ondersteuning geboden via Microsoft Customer ondersteunen Services (CSS). Als u klikt u op het pictogram Help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerdersportal en klik vervolgens op **nieuw ondersteuningsverzoek**, een site waar u kunt een ondersteuningsaanvraag rechtstreeks openen wordt geopend.

Als er een probleem met de implementatie, patch en bijwerken, hardware (inclusief veld replaceable units) en alle hardware huisstijl software, zoals software die wordt uitgevoerd op de host van de levenscyclus van hardware contact op met uw hardwareleverancier OEM eerst.

Voor andere doeleinden, contact op met Microsoft CSS.

### <a name="development-kit"></a>Development kit

Voor de development kit, u kunt vragen ondersteuning-gerelateerde in de [Microsoft forums](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). Als u klikt u op het pictogram Help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerdersportal en klik vervolgens op **nieuw ondersteuningsverzoek**, Hiermee opent u de site forums rechtstreeks. Deze forums worden regelmatig bewaakt. Omdat de development kit een evaluatieomgeving is, is geen officiële ondersteuning die worden aangeboden via Microsoft CSS.

## <a name="next-steps"></a>Volgende stappen

- [Regio management in Azure-Stack](azure-stack-region-management.md)


