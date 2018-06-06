---
title: Wat is licentieverlening in Azure Active Directory op basis van groep? | Microsoft Docs
description: Beschrijving van Azure Active Directory op basis van een groep licenties, hoe het werkt en aanbevolen procedures
services: active-directory
keywords: Azure AD-licentieverlening
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: fundamentals
ms.topic: article
ms.workload: identity
ms.date: 05/21/2018
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: 6fe473d606df4ea0c37c2e6b2b1c9146056d0106
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713470"
---
# <a name="group-based-licensing-basics-in-azure-active-directory"></a>Licentieverlening basisbeginselen op basis van een groep in Azure Active Directory

Microsoft-cloudservices, zoals Office 365, Enterprise Mobility + Security Dynamics 365 en andere vergelijkbare producten betaald licenties vereist. Deze licenties zijn toegewezen aan elke gebruiker die toegang krijgen tot deze services moet. Beheerders gebruiken voor het beheren van licenties voor een van de beheerportals (Office of Azure) en PowerShell-cmdlets. Azure Active Directory (Azure AD) is de onderliggende infrastructuur die ondersteuning biedt voor identiteitsbeheer voor alle Microsoft-cloudservices. Azure AD bevat informatie over toewijzing licentiestatussen voor gebruikers.

Tot op heden worden licenties kunnen alleen op het niveau van afzonderlijke gebruiker, waardoor grootschalige management moeilijk toegewezen. Bijvoorbeeld, als u wilt toevoegen of verwijderen op basis van veranderingen in de organisatie, zoals gebruikers toevoegen of verlaat de organisatie of een afdeling gebruikerslicenties moet een beheerder vaak schrijven een complexe PowerShell-script. Dit script worden afzonderlijke aanroepen naar de cloudservice.

Als u wilt deze uitdagingen bevat nu Azure AD op basis van een groep licenties. U kunt een of meer productlicenties toewijzen aan een groep. Azure AD zorgt ervoor dat de licenties zijn toegewezen aan alle leden van de groep. Nieuwe leden die deelnemen aan de groep zijn toegewezen de juiste licenties. Wanneer ze de groep verlaten, worden die licenties verwijderd. Hierdoor is de noodzaak voor het automatiseren van Licentiebeheer via PowerShell te spelen op wijzigingen in de organisatie en de afdelingen structuur op per-gebruiker.

>[!NOTE]
>Deze functie is momenteel in de openbare preview. Wees voorbereid om te herstellen of verwijderen van eventuele wijzigingen. De functie is beschikbaar in een abonnement voor Azure Active Directory (Azure AD) tijdens de openbare preview. Wanneer de functie algemeen beschikbaar wordt, kunnen sommige aspecten van de functie echter een of meer Azure Active Directory Premium-licenties nodig.

## <a name="features"></a>Functies

Hier volgen de belangrijkste functies van de groep gebaseerde licentieverlening:

- Licenties kunnen worden toegewezen aan een beveiligingsgroep in Azure AD. Beveiligingsgroepen kunnen gesynchroniseerde on-premises worden met behulp van Azure AD Connect. U kunt ook beveiligingsgroepen maken rechtstreeks in Azure AD (ook wel groepen alleen in de cloud) of automatisch via het onderdeel van de dynamische groep Azure AD.

- Wanneer een productlicentie is toegewezen aan een groep, kan de beheerder een of meer service-abonnementen in het product kunt uitschakelen. Dit gebeurt meestal wanneer de organisatie is nog niet klaar om te beginnen met een service die deel uitmaken van een product. De beheerder kan bijvoorbeeld Office 365 toewijzen aan een afdeling, maar de Yammer-service tijdelijk uitschakelen.

- Alle Microsoft cloud-services die op gebruikersniveau licentieverlening vereisen worden ondersteund. Dit omvat alle Office 365-producten, Enterprise Mobility + beveiligings- en Dynamics 365.

- Op basis van een groep-licentieverlening is momenteel alleen beschikbaar via [de Azure-portal](https://portal.azure.com). Als u andere beheerportals voornamelijk voor gebruikers- en groepsbeheer, zoals Office 365-portal gebruikt kunt u blijven om dit te doen. Maar u moet de Azure portal gebruiken voor het beheren van licenties op groepeerniveau van de.

- Azure AD worden beheerd, licentie-wijzigingen die het gevolg zijn van wijzigingen voor een groepslidmaatschap. Licentie wijzigingen zijn meestal effectieve binnen enkele minuten een wijziging lidmaatschap.

- Een gebruiker kan lid zijn van meerdere groepen met licentie-beleidsregels die zijn opgegeven. Een gebruiker kan ook een aantal licenties die rechtstreeks zijn toegewezen, buiten groepen hebben. De status van de resulterende gebruiker is een combinatie van alle toegewezen product en service-licenties.

- In sommige gevallen kunnen niet licenties worden toegewezen aan een gebruiker. Bijvoorbeeld, worden er mogelijk niet voldoende beschikbare licenties in de tenant of conflicterende services kunnen worden toegewezen op hetzelfde moment. Beheerders hebben toegang tot informatie over gebruikers voor wie Azure AD niet volledig groep licenties verwerken kan. Ze kunnen los problemen op basis van die gegevens.

- Een betaald of proefversie abonnement voor Azure AD Basic of Premium-editie is tijdens de openbare preview vereist in de tenant te gebruiken op basis van een groep Licentiebeheer.

## <a name="your-feedback-is-welcome"></a>Uw feedback is Welkom!

Als u feedback geven of functieaanvragen hebt, neem ze delen met ons op [het Azure AD-beheerder forum](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over andere scenario's voor Licentiebeheer via Groepsbeleid licentieverlening:

* [Aan de slag met Azure Active Directory-licenties](active-directory-licensing-get-started-azure-portal.md)
* [Licenties toewijzen aan een groep in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Opsporen en oplossen van licentieproblemen met een voor een groep in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Het migreren van afzonderlijke gebruikers met een licentie aan op basis van een groep licentieverlening in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory-licentieverlening voor aanvullende scenario's op basis van groep](active-directory-licensing-group-advanced.md)
