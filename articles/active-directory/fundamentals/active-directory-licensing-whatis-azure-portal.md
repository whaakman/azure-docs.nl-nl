---
title: Wat is licentieverlening in Azure Active Directory op basis van groep? | Microsoft Docs
description: Meer informatie over Azure Active Directory-groep op basis van licentieverlening, met inbegrip van hoe het werkt en best practices.
services: active-directory
keywords: Azure AD-licenties
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.topic: conceptual
ms.workload: identity
ms.date: 10/29/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 3f23b28c1b20155e50fddf17db90cd2a53c04855
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209816"
---
# <a name="what-is-group-based-licensing-in-azure-active-directory"></a>Wat is licentieverlening in Azure Active Directory op basis van groep?

Voor betaalde Microsoft-cloudservices, zoals Office 365, Enterprise Mobility + Security, Dynamics 365 en andere vergelijkbare producten, zijn licenties vereist. Deze licenties worden toegewezen aan elke gebruiker die toegang tot deze services nodig heeft. Om licenties te beheren gebruiken beheerders een van de beheerportals (Office of Azure) en PowerShell-cmdlets. Azure AD (Azure Active Directory) is de onderliggende infrastructuur die ondersteuning biedt voor identiteitsbeheer in alle Microsoft-cloudservices. In Azure AD wordt informatie opgeslagen over de statussen van licentietoewijzingen voor gebruikers.

Tot nu toe konden licenties alleen worden toegewezen op het niveau van de individuele gebruiker, wat grootschalig beheer moeilijk maakt. Als u bijvoorbeeld gebruikerslicenties wilt toevoegen of verwijderen op basis van wijzigingen in de organisatie, zoals wanneer gebruikers toetreden tot de organisatie of tot een afdeling of deze verlaten, moet een beheerder vaak een complex PowerShell-script schrijven. Met dit script worden afzonderlijke aanroepen naar de cloudservice gemaakt.

Azure AD bevat nu licenties op basis van groepen om met dergelijke uitdagingen om te gaan. U kunt een of meer productlicenties toewijzen aan een groep. De licenties worden in Azure AD toegewezen aan alle leden van de groep. Wanneer er nieuwe gebruikers lid worden van de groep, worden aan hen de juiste licenties toegewezen. Wanneer zij de groep weer verlaten, worden deze licenties verwijderd. Hierdoor is er geen noodzaak meer om licentiebeheer via PowerShell te automatiseren om wijzigingen in de organisatie- en afdelingsstructuur per gebruiker weer te geven.


## <a name="features"></a>Functies

Dit zijn de belangrijkste functies van licenties op basis van groepen:

- Licenties kunnen worden toegewezen aan elke beveiligingsgroep in Azure AD. Beveiligingsgroepen kunnen worden gesynchroniseerd vanuit on-premises, met behulp van Azure AD Connect. U kunt beveiligingsgroepen ook rechtstreeks in Azure AD maken (ook wel groepen met het kenmerk Alleen-cloud genoemd), of automatisch via de functie voor dynamische Azure AD-groepen.

- Wanneer een productlicentie is toegewezen aan een groep, kan de beheerder een of meer serviceplannen in het product uitschakelen. Dit gebeurt meestal wanneer de organisatie nog niet klaar is om een van de services in het product te gebruiken. Zo kan de beheerder, bijvoorbeeld, Office 365 toewijzen aan een afdeling, maar de Yammer-service tijdelijk uitschakelen.

- Alle Microsoft-cloudservices waarvoor licenties op gebruikersniveau zijn vereist, worden ondersteund. Dit omvat alle Office 365-producten, Enterprise Mobility + Security en Dynamics 365.

- Licenties op basis van groepen is momenteel alleen beschikbaar via [Azure Portal](https://portal.azure.com). Als u voornamelijk andere beheerportals gebruikers voor het beheer van gebruikers en groepen, zoals de Office 365-portal, kunt u dit gewoon blijven doen. U moet Azure Portal wel gebruiken om licenties op groepsniveau te beheren.

- Wijzigingen in licenties die het resultaat zijn van wijzigingen in groepslidmaatschappen, worden automatisch beheerd in Azure AD. Wijzigingen in licenties worden meestal binnen enkele minuten na een lidmaatschapswijziging van kracht.

- Een gebruiker kan lid zijn van meerdere groepen waarvoor licentiebeleid is opgesteld. Een gebruiker kan ook een aantal licenties hebben die rechtstreeks zijn toegewezen, buiten een groep. De resulterende gebruikersstatus is een combinatie van alle toegewezen product- en servicelicenties. Als een gebruiker licentie uit meerdere bronnen toegewezen, kan de licentie wordt slechts één keer worden gebruikt.

- In sommige gevallen kunnen er geen licenties worden toegewezen aan een gebruiker. Bijvoorbeeld wanneer er niet voldoende licenties beschikbaar zijn in de tenant, of wanneer tegelijkertijd conflicterende services zijn toegewezen. Beheerders hebben toegang tot informatie over gebruikers voor wie de groepslicenties niet volledig zijn verwerkt in Azure AD. Ze kunnen vervolgens een herstelbewerking uitvoeren op basis van deze informatie.

- Een betaalde versie of proefversie abonnement voor Azure AD Basic of een betaalde versie of proefversie Office 365 Enterprise E3, Office 365 A3 en hoger edities is vereist in de tenant te gebruiken op basis van een groep Licentiebeheer. Deze functie is de licentie voor elke unieke gebruiker die lid is van de groepen die zijn toegewezen licentie vereist. U hoeft te licenties toewijzen aan gebruikers voor deze lid te zijn van groepen die licentie zijn toegewezen, maar hebt u het minimale aantal licenties in de tenant om te kunnen krijgen alle gebruikers. Bijvoorbeeld, als u had een totaal van 1000 unieke gebruikers in alle groepen met licenties die zijn toegewezen in uw tenant, dan moet u ten minste 1000 licenties om te voldoen aan de licentievereiste.

## <a name="your-feedback-is-welcome"></a>We stellen uw feedback op prijs!

Als u feedback of functieaanvragen hebt, kunt u deze met ons delen op het [Azure AD-beheerforum](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over scenario’s voor licentiebeheer via licenties op basis van groepen, raadpleegt u:

* [Licenties toewijzen aan een groep in Azure Active Directory](../users-groups-roles/licensing-groups-assign.md)
* [Licentieproblemen voor een groep vaststellen en oplossen in Azure Active Directory](../users-groups-roles/licensing-groups-resolve-problems.md)
* [Gebruikers met een afzonderlijke licentie migreren naar licenties op basis van groepen in Azure Active Directory](../users-groups-roles/licensing-groups-migrate-users.md)
* [Het migreren van gebruikers tussen productlicenties groepsgebaseerde licentieverlening in Azure Active Directory gebruiken](../users-groups-roles/licensing-groups-change-licenses.md)
* [Aanvullende scenario’s voor Azure Active Directory-licenties op basis van groepen](../users-groups-roles/licensing-group-advanced.md)
* [PowerShell-voorbeelden voor Groepslicenties in Azure Active Directory](../users-groups-roles/licensing-ps-examples.md)
