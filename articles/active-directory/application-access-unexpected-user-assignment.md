---
title: Hoe gebruikers toewijzen voor toepassingen | Microsoft Docs
description: Begrijpen hoe gebruikers worden toegewezen aan een toepassing in uw tenant
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 916238ba402a2555bac620d7f08e02799d981ae0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-assign-users-to-applications"></a>Gebruikers toewijzen aan toepassingen

In dit artikel helpt u bij het begrijpen hoe gebruikers worden toegewezen aan een toepassing in uw tenant.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Hoe gebruikers worden toegewezen aan een toepassing in Azure AD?

Voor een gebruiker toegang tot een toepassing, moeten ze eerst worden toegewezen aan deze op een bepaalde manier. Toewijzing kan worden uitgevoerd door een beheerder, een gemachtigde bedrijven of soms wordt de gebruiker zelf. Beschrijving van de manieren waarop gebruikers kunnen worden toegewezen aan toepassingen hieronder:

1.  Een beheerder [wijst een gebruiker](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) tot de toepassing rechtstreeks

2.  Een beheerder [wijst een groep](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) dat de gebruiker lid van de toepassing is, met inbegrip van:

  * Een groep die on-premises is gesynchroniseerd

  * Een statische beveiligingsgroep gemaakt in de cloud

  * Een [dynamische beveiligingsgroep](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) gemaakt in de cloud

  * Een Office 365-groep gemaakt in de cloud

  * De [alle gebruikers](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) groep

3.  Een beheerder activeert [Self-service toegang tot de toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) om toe te staan om toe te voegen met een toepassing met behulp van een gebruiker de [toepassing Toegangspaneel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **App toevoegen** functie **zonder goedkeuring van bedrijven**

4.  Een beheerder activeert [Self-service toegang tot de toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) om toe te staan om toe te voegen met een toepassing met behulp van een gebruiker de [toepassing Toegangspaneel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **App toevoegen** onderdeel, maar alleen w**ith voorafgaande goedkeuring van een geselecteerde verzameling business goedkeurders**

5.  Een beheerder activeert [groepsbeheer met Self-service](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) zodat een gebruiker lid worden van een groep die een toepassing is toegewezen aan **zonder goedkeuring van bedrijven**

6.  Een beheerder activeert [groepsbeheer met Self-service](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) zodat een gebruiker lid worden van een groep die een toepassing is toegewezen aan, maar alleen **met voorafgaande goedkeuring van een geselecteerde verzameling business goedkeurders**

7.  Een beheerder een licentie toegewezen aan een gebruiker rechtstreeks voor een eerste toepassing van derden, zoals [Microsoft Office 365](http://products.office.com/)

8.  Een beheerder een licentie aan een groep toewijst die de gebruiker lid is van een eerste toepassing van derden, zoals is [Microsoft Office 365](http://products.office.com/)

9.  Een [beheerder instemt met een toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) moet worden gebruikt door alle gebruikers en vervolgens een gebruiker zich aanmeldt bij de toepassing

10. Een gebruiker [instemt met een toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) zich aanmeldt bij de toepassing

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](active-directory-enable-sso-scenario.md)
