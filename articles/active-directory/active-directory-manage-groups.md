---
title: Groepen gebruiken voor het beheren van toegang tot bronnen in Azure Active Directory | Microsoft Docs
description: Klik hier voor meer informatie over het gebruik van groepen in Azure Active Directory voor het beheren van de gebruikerstoegang tot on-premises en cloudtoepassingen en -resources.
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 714120d0-cdf9-465d-afee-39bef591c6b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: aaccc501526d313a572692ff8f2f5c9da38849d3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="manage-access-to-resources-with-azure-active-directory-groups"></a>Toegang tot resources beheren met Azure Active Directory-groepen
Azure Active Directory (Azure AD) is een uitgebreide identiteits- en toegangsbeheer-beheeroplossing die een set krachtige mogelijkheden biedt voor het beheer van toegang tot on-premises en cloudtoepassingen en bronnen, met inbegrip van Microsoft online services zoals Office 365 en een wereld van Microsoft SaaS-toepassingen. In dit artikel biedt een overzicht, maar als u wilt beginnen met behulp van Azure AD nu gegroepeerd, volg de instructies in [beveiligingsgroepen beheren in Azure AD](active-directory-groups-create-azure-portal.md). Als u wilt laten zien hoe u PowerShell kunt gebruiken voor het beheren van groepen in Azure Active directory kunt u meer informatie in [Azure Active Directory-cmdlets voor groepsbeheer](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

> [!NOTE]
> Voor het gebruik van Azure Active Directory, moet u een Azure-account. Als u geen account hebt, kunt u [aanmelden voor een gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/).
>
>

Een van de belangrijkste functies is de mogelijkheid voor het beheren van toegang tot bronnen in Azure AD. Deze resources kunnen deel uitmaken van de map in het geval van machtigingen voor het beheren van objecten door middel van rollen in de directory, of de bronnen die extern zijn voor de map, zoals SaaS-toepassingen, de Azure-services, en de SharePoint-sites of de lokale bronnen. Er zijn vier manieren waarop die een gebruiker kan worden toegewezen rechten aan een resource:

1. Rechtstreekse toewijzing

    Gebruikers kunnen worden toegewezen aan een resource rechtstreeks door de eigenaar van de bron.
2. Groepslidmaatschap

    Een groep kan worden toegewezen aan een resource van de resource-eigenaar en door dit te doen, het verlenen van de leden van die groepstoegang tot de resource. Lidmaatschap van de groep kan vervolgens worden beheerd door de eigenaar van de groep. De eigenaar van de resource delegeert effectief, de machtiging gebruikers toewijzen aan de resource toe aan de eigenaar van de groep.
3. Op basis van een regel

    De resource-eigenaar kunt u een regel gebruiken om uit te drukken welke gebruikers moeten toegang worden toegewezen aan een resource. Het resultaat van de regel is afhankelijk van de kenmerken in die regel en hun waarden gebruikt voor specifieke gebruikers en doet, delegeert de resource-eigenaar effectief het recht voor het beheren van toegang tot de resource toe aan de gezaghebbende bron voor de kenmerken die worden gebruikt in de regel. De resource-eigenaar wordt nog steeds de regel zelf beheert en bepaalt welke kenmerken en waarden toegang tot de resource bieden.
4. Externe authority

    De toegang tot een bron is afgeleid van een externe bron; bijvoorbeeld, een groep die worden gesynchroniseerd vanuit een gezaghebbende bron, zoals een on-premises adreslijst of een SaaS-app zoals werkdag. De groep voor toegang tot de bron van de resource-eigenaar toegewezen en de externe bron beheert de leden van de groep.

   ![Overzicht van diagram voor het beheren van toegang](./media/active-directory-access-management-groups/access-management-overview.png)

## <a name="watch-a-video-that-explains-access-management"></a>Bekijk een video waarin wordt uitgelegd toegangsbeheer
U kunt een korte video met uitleg over meer over deze bekijken:

**Azure AD: Inleiding tot dynamisch lidmaatschap voor groepen**

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]
>
>

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Hoe heeft toegang tot beheer in Azure Active Directory work?
In het midden van de Azure AD is de oplossing voor het beheer van toegang tot de beveiligingsgroep. Toegang tot bronnen beheren met behulp van een beveiligingsgroep is een bekende paradigma, waardoor een flexibele en begrijpelijke manier om toegang te bieden aan een resource voor de gewenste groep gebruikers. De resource-eigenaar (of de beheerder van de directory) kan een groep voor een bepaalde toegang tot de resources die ze eigenaar worden toegewezen. De toegang wordt aangeboden door de leden van de groep en de resource-eigenaar kan het recht voor het beheren van de lijst met leden van een groep naar iemand anders, zoals een afdeling of een sitebeheerder helpdesk delegeren.

![Azure Active Directory access management-diagram](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

De eigenaar van een groep kan ook die groep beschikbaar maken voor self-service aanvragen. Hierbij kunt een eindgebruiker zoeken naar en zoeken van de groep en maak een aanvraag om toe te voegen, effectief zoeken machtiging voor toegang tot de resources die worden beheerd door de groep. De eigenaar van de groep kunt instellen met de groep zodat join aanvragen automatisch worden goedgekeurd of moeten worden goedgekeurd door de eigenaar van de groep. Wanneer een gebruiker een aanvraag voor het lid worden van een groep maakt, wordt de join-aanvraag doorgestuurd naar de eigenaren van de groep. Als een van de eigenaren van de aanvraag goedkeurt, wordt de aanvragende gebruiker wordt geïnformeerd en wordt de gebruiker lid is van de groep. Als een van de eigenaren van de aanvraag weigert, wordt de aanvragende gebruiker een melding maar niet aan de groep toegevoegd.

## <a name="getting-started-with-access-management"></a>Aan de slag met toegangsbeheer
Klaar om aan de slag te gaan? Probeer een aantal van de basistaken die u met Azure AD-groepen doen kunt. Gebruik deze mogelijkheden voor speciale toegang tot verschillende groepen gebruikers voor verschillende resources in uw organisatie. Een lijst met eerste basisstappen worden hieronder vermeld.

* [Maken van een eenvoudige regel voor het configureren van dynamische lidmaatschappen voor een groep](active-directory-groups-create-azure-portal.md)
* [Een groep gebruiken voor het beheren van toegang tot SaaS-toepassingen](active-directory-accessmanagement-group-saasapps.md)
* [Een groep maken beschikbaar voor eindgebruikers selfservice](active-directory-accessmanagement-self-service-group-management.md)
* [Synchroniseren van een lokale groep naar Azure met Azure AD Connect](active-directory-aadconnect.md)
* [Eigenaars voor een groep beheren](active-directory-accessmanagement-managing-group-owners.md)

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van toegangsbeheer hebt begrepen zijn hier enkele aanvullende geavanceerde mogelijkheden beschikbaar in Azure Active Directory voor het beheren van toegang tot de toepassingen en bronnen.

* [Met behulp van kenmerken Geavanceerde regels maken](active-directory-groups-dynamic-membership-azure-portal.md)
* [Beveiligingsgroepen beheren in Azure AD](active-directory-groups-create-azure-portal.md)
* [Toegewezen groepen instellen in Azure AD](active-directory-accessmanagement-dedicated-groups.md)
* [Graph API-verwijzing voor groepen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)
* [Azure Active Directory cmdlets for configuring group settings](active-directory-accessmanagement-groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
