---
title: Azure Stack Development Kit basisbeginselen | Microsoft Docs
description: Beschrijft hoe u algemene beheertaken uitvoeren voor de Azure Stack Development Kit (ASDK).
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
ms.date: 02/19/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 9a07a829aac9659ac7ab8d04b64a1ea1a9a2de78
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428054"
---
# <a name="asdk-administration-basics"></a>Basisprincipes van beheer ASDK 
Er zijn verschillende dingen die u weten moet als u geen ervaring met Azure Stack Development Kit (ASDK)-beheer. Deze handleiding biedt een overzicht van uw rol als Azure Stack-operators in de evaluatieomgeving en hoe kunt zorgen dat uw testgebruikers snel productiever kunt worden.

Eerst moet u controleren de [wat is Azure Stack Development Kit?](asdk-what-is.md) artikel om ervoor te zorgen dat u leert wat het doel van de ASDK en beperkingen. U moet de development kit gebruiken als een "sandbox" waar u Azure Stack kunt ontwikkelen en testen van uw apps in een niet-productieomgeving kunt evalueren. 

Zoals Azure innovates Azure Stack snel, zodat we regelmatig nieuwe versies van de ASDK brengen. U kunt de ASDK echter niet bijwerken, zoals u implementaties voor geïntegreerde Azure Stack-systemen kunt. Dus als u verplaatsen naar de nieuwste build wilt, moet u volledig [opnieuw implementeren van de ASDK](asdk-redeploy.md). U kunt geen updatepakketten toepassen. Dit proces duurt, maar het voordeel is kunt u de nieuwste functies uitproberen zodra deze beschikbaar komen. 

## <a name="what-account-should-i-use"></a>Welk account moet ik gebruiken?
Er zijn enkele aandachtspunten voor gebruikersaccounts die u houden moet rekening bij het beheren van Azure Stack. Met name in implementaties met behulp van Windows Server Active Directory Federation Services (AD FS) als de id-provider in plaats van Azure Active Directory (Azure AD). De volgende aandachtspunten voor gebruikersaccounts van toepassing op zowel geïntegreerde Azure Stack-systemen en ASDK implementaties:

|Account|Azure AD|AD FS|
|-----|-----|-----|
|Lokale Administrator (. \Administrator)|ASDK host beheerder|ASDK host beheerder|
|AzureStack\AzureStackAdmin|ASDK host beheerder<br><br>Kan worden gebruikt voor aanmelding bij de beheerportal van Azure Stack<br><br>Toegang tot het weergeven en beheren van Service Fabric-ringen|ASDK host beheerder<br><br>Geen toegang tot de Azure Stack-beheerportal<br><br>Toegang tot het weergeven en beheren van Service Fabric-ringen<br><br>Niet langer eigenaar van de standaard Provider abonnement (DPS)|
|AzureStack\CloudAdmin|Toegang tot en toegestane opdrachten binnen het eindpunt van de bevoegdheden uitvoeren|Toegang tot en toegestane opdrachten binnen het eindpunt van de bevoegdheden uitvoeren<br><br>Kan niet aanmelden bij de host ASDK<br><br>Eigenaar van de Provider standaardabonnement (DPS)|
|Globale Azure AD-beheerder|Tijdens de installatie gebruikt<br><br>Eigenaar van de Provider standaardabonnement (DPS)|Niet van toepassing|
|

## <a name="what-tools-do-i-use-to-manage"></a>Welke hulpprogramma's kan ik gebruiken om te beheren?
U kunt de [Azure Stack-Beheerdersportal](https://adminportal.local.azurestack.external) of PowerShell voor het beheren van Azure Stack. De eenvoudigste manier voor meer informatie over de basisconcepten is via de portal. Als u PowerShell gebruiken wilt, moet u voor het installeren van [PowerShell voor Azure Stack](asdk-post-deploy.md#install-azure-stack-powershell) en [de hulpprogramma's voor Azure Stack downloaden vanuit GitHub](asdk-post-deploy.md#download-the-azure-stack-tools).

Azure Stack maakt gebruik van Azure Resource Manager als een onderliggende implementatie, beheer en organisatie-mechanisme. Als u Azure Stack beheren en ter ondersteuning van gebruikers, moet u meer informatie over Azure Resource Manager. U vindt meer informatie lezen van de [aan de slag met Azure Resource Manager-whitepaper](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf).

## <a name="your-typical-responsibilities"></a>Uw gebruikelijke verantwoordelijkheden
Uw gebruikers willen om services te gebruiken. Vanuit het perspectief is uw belangrijkste rol aan ze beschikbaar stellen van deze services. Met behulp van de ASDK, u kunt meer te weten welke services te bieden, en hoe u deze services beschikbaar is per [maken, plannen, aanbiedingen en quota's](asdk-offer-services.md). U moet ook items toevoegen aan de marketplace, zoals installatiekopieën voor virtuele machines. De eenvoudigste manier is om te [marketplace-items downloaden](asdk-marketplace-item.md) van Azure naar Azure Stack.

> [!NOTE]
> Als u testen van uw plannen, aanbiedingen en services wilt, moet u de [gebruikersportal](https://portal.local.azurestack.external); niet de [beheerdersportal](https://adminportal.local.azurestack.external).

Naast het leveren van services, moet u alle reguliere rechten van een Azure Stack-operators te houden van de ASDK actief en werkend uitvoeren. Deze rechten omvatten zaken zoals het volgende:
- Voeg gebruikersaccounts toe voor Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS)-implementaties.
- Toegang op rollen gebaseerd beheer (RBAC)-rollen (dit is niet beperkt tot alleen beheerders) toewijzen
- Status van de groepsbeleidstructuur controleren
- Netwerk- en opslagresources beheren
- Mislukte development kit host computerhardware vervangen 

## <a name="where-to-get-support"></a>Waar u ondersteuning krijgen
Voor de development kit, de enige ondersteuning-mogelijkheid is om vragen met betrekking tot ondersteuning in te stellen de [Microsoft Azure Stack-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). Als u klikt u op het pictogram Help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerdersportal en klik vervolgens op **nieuwe ondersteuningsaanvraag**, Hiermee opent u de forums-site rechtstreeks. Deze forums worden regelmatig gecontroleerd. 

> [!IMPORTANT]
> Omdat de ASDK een evaluatieomgeving is, is er geen officiële ondersteuning aangeboden via Microsoft de klant ondersteuning klantenondersteuning (CSS).

## <a name="next-steps"></a>Volgende stappen
[De ASDK implementeren](asdk-install.md)

