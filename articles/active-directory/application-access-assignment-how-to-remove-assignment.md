---
title: Het verwijderen van een gebruiker toegang tot een toepassing | Microsoft Docs
description: Begrijpen hoe een gebruiker toegang tot een toepassing te verwijderen
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: aec49b108c21b97646e622c47bf69728a81d6d33
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Het verwijderen van een gebruiker toegang tot een toepassing

In dit artikel helpt u om te begrijpen hoe een gebruiker toegang tot een toepassing te verwijderen.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Ik wil een specifieke gebruiker of groep van toewijzing aan een toepassing verwijderen

Als u wilt verwijderen van een gebruiker of groepstoewijzing aan een toepassing, volg de stappen in de [de toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artikel.

. ## ik wil alle toegang tot een toepassing voor elke gebruiker uitschakelen

Schakel alle gebruikersaanmeldingen tot een toepassing die de stappen die worden vermeld in de [gebruikersaanmeldingen een enterprise-App in Azure Active Directory uitschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artikel.

## <a name="i-want-to-delete-an-application-entirely"></a>Ik wil een toepassing volledig verwijderen

Naar **verwijderen van een toepassing**, volgt u deze instructies:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt verwijderen.

7.  Nadat de toepassing wordt geladen, klikt u op **verwijderen** pictogram van de bovenste toepassing **overzicht** deelvenster.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Ik wil alle bewerkingen van toekomstige gebruikers toestemming voor elke toepassing uitschakelen

Toestemming van de gebruiker uitschakelt voor uw hele directory te voorkomen dat eindgebruikers ermee akkoord dat elke toepassing. Beheerders kunnen nog steeds op behalves van de gebruiker toestemming. Meer informatie over de toepassing toestemming en waarom u niet wilt doen, of lezen [wat gebruikers- en toestemming van de beheerder](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

Naar **uitschakelen alle toekomstige gebruikers toestemming bewerkingen in uw hele directory**, volgt u deze instructies:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **gebruikersinstellingen**.

6.  Alle toekomstige gebruikers toestemming bewerkingen uitschakelen door in te stellen de **gebruikers kunnen toestaan dat apps toegang tot hun gegevens** in-of uitschakelen op **Nee** en klik op de **opslaan** knop.


# <a name="next-steps"></a>Volgende stappen
[Toegang tot apps beheren](active-directory-managing-access-to-apps.md)
