---
title: Office 365 extern delen en samenwerking van Azure Active Directory B2B | Microsoft Docs
description: Worden resources te delen met externe partners met behulp van Office 365 en Azure Active Directory B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/24/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 7e0a65e4a5807cb9a6b39feecfd2d5b2643ea4a9
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39330902"
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Extern delen van Office 365 en Azure Active Directory B2B-samenwerking

Extern delen in Office 365 (OneDrive, SharePoint Online, gecombineerde groepen, enzovoort) en Azure Active Directory (Azure AD) B2B-samenwerking zijn technisch gezien hetzelfde te doen. Alle extern delen (met uitzondering van OneDrive/SharePoint Online), inclusief gasten in Office 365-groepen, al gebruikmaakt van de Azure AD B2B-samenwerking uitnodiging API's voor het delen van.

## <a name="how-does-azure-ad-b2b-differ-from-external-sharing-in-sharepoint-online"></a>Hoe verschilt Azure AD B2B van extern delen in SharePoint Online?

OneDrive/SharePoint Online heeft een afzonderlijke inschrijving manager. Ondersteuning voor extern delen in OneDrive/SharePoint Online gestart voordat de ondersteuning die Azure AD is ontwikkeld. Na verloop van tijd, OneDrive/SharePoint Online extern delen is samengevoegd verschillende functies en veel miljoenen gebruikers die gebruikmaken van het product van de ingebouwde patroon delen. Er zijn echter enkele subtiele verschillen tussen OneDrive/SharePoint Online extern delen werking en de werking van Azure AD B2B-samenwerking. U kunt meer informatie over het OneDrive/SharePoint Online extern delen in [extern delen overzicht](https://docs.microsoft.com/sharepoint/external-sharing-overview). Het proces is doorgaans anders dan Azure AD B2B op de volgende manieren:

- OneDrive/SharePoint Online gebruikers toegevoegd aan de directory nadat gebruikers hun uitnodiging hebt ingewisseld. Dus voordat u inschrijving zien u niet dat de gebruiker in Azure AD-portal. Als een andere site in de tussentijd nodigt van een gebruiker, wordt een uitnodiging voor een nieuwe gegenereerd. Wanneer u Azure AD B2B-samenwerking, worden gebruikers echter toegevoegd onmiddellijk op uitnodiging zodat ze overal worden weergegeven.

- De ervaring voor inschrijving in OneDrive/SharePoint Online ziet er anders uit de ervaring in Azure AD B2B-samenwerking. Nadat een gebruiker een uitnodiging wordt ingewisseld, is de ervaringen lijken.

- Azure AD B2B-samenwerking uitgenodigd gebruikers kunnen worden verzameld van OneDrive/SharePoint Online-dialoogvensters voor delen. OneDrive/SharePoint Online uitgenodigd gebruikers ook weergegeven in Azure AD wanneer ze hun uitnodiging inwisselen.

- De licentievereisten verschillen. Voor elke Azure AD-licentie betaalde, kunt u maximaal 5 gastgebruikers ook kunnen toegang krijgen tot uw betaalde Azure AD-functies. Zie voor meer informatie over licentieverlening [Azure AD B2B-licentieverlening](https://docs.microsoft.com/azure/active-directory/b2b/licensing-guidance) en ['Wat is een externe gebruiker?' in de SharePoint Online extern delen overzicht](https://docs.microsoft.com/sharepoint/external-sharing-overview#what-is-an-external-user).

Voor het beheren van extern delen in OneDrive/SharePoint Online met Azure AD B2B-samenwerking, OneDrive/SharePoint Online dat deze extern ingestelde instelling in om te delen **alleen met de externe gebruikers die al bestaan in uw organisatie delen toestaan Directory**. Gebruikers kunnen naar extern gedeelde sites en kies uit meer dan externe deelnemers die de beheerder heeft toegevoegd. De beheerder kan de externe deelnemers via de B2B-samenwerking uitnodiging API's kunt toevoegen.


![De OneDrive/SharePoint Online extern delen van instelling](media/o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [B2B-samenwerking gebruiker toe te voegen aan een rol](add-guest-to-role.md)
* [B2B-samenwerking uitnodigingen delegeren](delegate-invitations.md)
* [Dynamische groepen en B2B-samenwerking](use-dynamic-groups.md)