---
title: Hoe u gebruikers toewijzen aan toepassingen | Microsoft Docs
description: Inzicht in hoe gebruikers toegewezen aan een toepassing in uw tenant
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 9e27df857d560abdf5b6a54272bb022b6ed466ee
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53079361"
---
# <a name="how-to-assign-users-to-applications"></a>Gebruikers toewijzen aan toepassingen

In dit artikel kunt u inzicht in hoe gebruikers toegewezen aan een toepassing in uw tenant.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Hoe gebruikers toegewezen aan een toepassing in Azure AD?

Voor een gebruiker toegang tot een toepassing, moeten ze eerst worden toegewezen aan deze op een bepaalde manier. Toewijzing kan worden uitgevoerd door een beheerder, een gemachtigde zakelijke of soms de gebruiker zelf. Hieronder wordt beschreven hoe u gebruikers krijgen aan toepassingen toegewezen kunnen:

1.  Een beheerder [wijst een gebruiker](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) naar de toepassing rechtstreeks

2.  Een beheerder [wijst een groep](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) dat de gebruiker lid van de toepassing is, met inbegrip van:

    * Een groep die is gesynchroniseerd vanuit on-premises

    * Een statische groep gemaakt in de cloud

    * Een [dynamische beveiligingsgroep](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) gemaakt in de cloud

    * Een Office 365-groep gemaakt in de cloud

    * De [alle gebruikers](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) groep

3.  Een beheerder activeert [Self-service toegang tot toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) om toe te staan om toe te voegen een toepassing met behulp van een gebruiker de [Toegangsvenster](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **App toevoegen** functie **zonder goedkeuring van bedrijven**

4.  Een beheerder activeert [Self-service toegang tot toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) om toe te staan om toe te voegen een toepassing met behulp van een gebruiker de [Toegangsvenster](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **App toevoegen** onderdeel, maar alleen w**ith voorafgaande goedkeuring van een geselecteerde set fiatteur**

5.  Een beheerder activeert [groepsbeheer met Self-service](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) waarmee een gebruiker voor deelname aan een groep die een toepassing is toegewezen aan **zonder goedkeuring van bedrijven**

6.  Een beheerder activeert [groepsbeheer met Self-service](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) waarmee een gebruiker voor deelname aan een groep die een toepassing is toegewezen aan, maar alleen **met voorafgaande goedkeuring van een geselecteerde set fiatteur**

7.  Een beheerder een licentie toegewezen aan een gebruiker rechtstreeks voor een eerste toepassing van derden, zoals [Microsoft Office 365](https://products.office.com/)

8.  Een beheerder wijst een licentie toe aan een groep die de gebruiker een lid van een eerste toepassing van derden, zoals is [Microsoft Office 365](https://products.office.com/)

9.  Een [administrator toestemming heeft voor een toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) moet worden gebruikt door alle gebruikers en voer vervolgens een gebruiker zich aanmeldt bij de toepassing

10. Een gebruiker [toestemming heeft voor een toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) zich aanmeldt bij de toepassing

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](what-is-application-management.md)
