---
title: Hoe toepassingen worden weergegeven in het toegangsvenster | Microsoft Docs
description: Waarom een toepassing wordt weergegeven in het toegangsvenster
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewr: japere
ms.openlocfilehash: 83aa3b1b132c52944d350a9d2b5231857a10c7fb
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191751"
---
# <a name="how-applications-appear-on-the-access-panel"></a>Hoe toepassingen worden weergegeven in het toegangsvenster

Het toegangsvenster is een portal op Internet, waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) om te bekijken en starten van cloud-gebaseerde toepassingen die Azure AD-beheerder heeft deze toegang verleend aan. Deze toepassingen zijn namens de gebruiker in de Azure AD-portal geconfigureerd. De beheerder kan de toepassing voor de gebruiker rechtstreeks inrichten of aan een groep van een gebruiker deel uitmaakt van leidt tot de toepassing die wordt weergegeven in het toegangsvenster van de gebruiker.

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren

-   Als een toepassing is verwijderd uit een gebruiker of groep die de gebruiker een lid van is, probeert aan te melden en afmelden opnieuw in het toegangsvenster van de gebruiker na een paar minuten om te zien als de toepassing wordt verwijderd.

-   Als u een licentie is verwijderd uit een gebruiker of groep is de gebruiker dat lid is van dit duurt lang duren, afhankelijk van de grootte en complexiteit van de groep voor wijzigingen worden aangebracht. Kan extra tijd voordat het toegangsvenster aan te melden.

## <a name="problems-related-to-assigning-applications-to-users"></a>Problemen met betrekking tot toepassingen aan gebruikers toewijzen

Een gebruiker mogelijk ziet u een toepassing in hun Toegangsvenster omdat ze was eerder eraan is toegewezen. Hieronder vindt u enkele manieren om te controleren:

-   [Controleren of een gebruiker is toegewezen aan de toepassing](#check-if-a-user-is-assigned-to-the-application)

-   [Controleren of een gebruiker een licentie met betrekking tot de toepassing](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a>Controleren of een gebruiker is toegewezen aan de toepassing

Om te zien als een gebruiker is toegewezen aan de toepassing, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

6.  **Search** voor de naam van de betreffende toepassing.

7.  Klik op **gebruikers en groepen**.

8.  Controleer of de gebruiker is toegewezen aan de toepassing.

  * Als u wilt verwijderen van de gebruiker van de toepassing **klikt u op de rij** van de gebruiker en selecteer **verwijderen**.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Controleren of een gebruiker een licentie met betrekking tot de toepassing

Om te controleren of de toegewezen licenties van een gebruiker, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien welke licenties die de gebruiker die momenteel is toegewezen.

   * Als de gebruiker aan een Office-licentie is toegewezen, kunt dit eerste partij Office-toepassingen worden weergegeven in het toegangsvenster van de gebruiker.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Problemen met betrekking tot toepassingen aan groepen toewijzen

Een gebruiker mogelijk ziet u een toepassing in hun Toegangsvenster omdat ze deel uitmaken van een groep die de toepassing is toegewezen. Hieronder vindt u enkele manieren om te controleren:

-   [Controleren van groepslidmaatschappen van een gebruiker](#check-a-users-group-memberships)

-   [Controleren of een gebruiker lid is van een groep die is toegewezen aan een licentie](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Controleren van groepslidmaatschappen van een gebruiker

Als u wilt controleren van een groep lidmaatschap, de volgende stappen uit:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **groepen.**

8.  Controleer of de gebruiker deel uitmaakt van een groep die is toegewezen aan de toepassing is.

   * Als u wilt verwijderen van de gebruiker uit de groep **klikt u op de rij** van de groep en selecteer verwijderen.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a>Controleren of een gebruiker lid is van een groep die is toegewezen aan een licentie

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **groepen.**

8.  Klik op de rij van een specifieke groep.

9.  Klik op **licenties** om te zien die de groep licenties zijn toegewezen aan.

  * Als de groep is toegewezen aan een licentie voor Office, is dit mogelijk bepaalde eerste partij Office-toepassingen worden weergegeven in het toegangsvenster van de gebruiker.


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Als deze stappen voor probleemoplossing niet oplossen door het probleem doet

Open een ondersteuningsticket met de volgende informatie, indien beschikbaar:

-   Correlatie-ID van fout

-   UPN (e-mailadres van gebruiker)

-   Tenant-id

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens de fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](what-is-application-management.md)
