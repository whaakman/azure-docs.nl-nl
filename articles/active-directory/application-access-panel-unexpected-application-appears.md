---
title: Hoe toepassingen worden weergegeven op het toegangsvenster | Microsoft Docs
description: Waarom een toepassing wordt weergegeven in het deelvenster toegang
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
ms.reviewr: japere
ms.openlocfilehash: 7ff6817bafdfe1943d70639c7f3c69c417f5f94a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-applications-appear-on-the-access-panel"></a>Hoe toepassingen worden weergegeven op het toegangsvenster

Het Toegangspaneel is een portal op Internet waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) weergeven en starten van cloud-gebaseerde toepassingen die de Azure AD-beheerder heeft deze toegang verleend. Deze toepassingen zijn geconfigureerd namens de gebruiker in de Azure AD-portal. De beheerder kan de toepassing aan de gebruiker rechtstreeks inrichten of aan een groep een gebruiker deel uitmaakt van de toepassing die zijn opgenomen in het deelvenster van de gebruiker toegang krijgt.

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren

-   Als een toepassing is verwijderd uit een gebruiker of groep die de gebruiker lid van is, probeert aan te melden en afmelden opnieuw in het deelvenster van de gebruiker toegang na een paar minuten om te zien als de toepassing wordt verwijderd.

-   Als u een licentie is verwijderd uit een gebruiker of groep die de gebruiker is dat lid van deze kan lang duren, afhankelijk van de omvang en complexiteit van de groep voor wijzigingen worden aangebracht. Voor extra tijd voordat u zich in het deelvenster toegang toestaan.

## <a name="problems-related-to-assigning-applications-to-users"></a>Problemen met betrekking tot het toewijzen van toepassingen aan gebruikers

Een gebruiker mogelijk ziet u een toepassing op hun Toegangsvenster omdat ze eerder waren is toegewezen aan deze. Hieronder vindt u een aantal manieren om te controleren:

-   [Controleren of een gebruiker is toegewezen aan de toepassing](#check-if-a-user-is-assigned-to-the-application)

-   [Controleer of een gebruiker zich onder een licentie die is gerelateerd aan de toepassing](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a>Controleren of een gebruiker is toegewezen aan de toepassing

Als u wilt controleren of een gebruiker is toegewezen aan de toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

6.  **Search** voor de naam van de betreffende toepassing.

7.  Klik op **gebruikers en groepen**.

8.  Controleer of de gebruiker is toegewezen aan de toepassing.

  * Als u wilt verwijderen van de gebruiker van de toepassing **klikt u op de rij** van de gebruiker en selecteer **verwijderen**.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Controleer of een gebruiker zich onder een licentie die is gerelateerd aan de toepassing

Als u wilt controleren toegewezen licenties van een gebruiker, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de gebruiker momenteel is toegewezen.

   * Als de gebruiker is toegewezen aan een Office licentie deze inschakelen eerste partij Office-toepassingen worden weergegeven op het toegangsvenster van de gebruiker.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Problemen met betrekking tot het toewijzen van toepassingen aan groepen

Een gebruiker mogelijk ziet u een toepassing op hun Toegangsvenster omdat ze deel uitmaken van een groep die de toepassing is toegewezen. Hieronder vindt u een aantal manieren om te controleren:

-   [Controleer de groepslidmaatschappen van een gebruiker](#check-a-users-group-memberships)

-   [Controleer of een gebruiker lid is van een groep die is toegewezen aan een licentie is](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Controleer de groepslidmaatschappen van een gebruiker

Als u wilt controleren lidmaatschap van een groep, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **groepen.**

8.  Controleer of uw gebruiker deel van een groep die is toegewezen aan de toepassing uitmaakt.

   * Als u wilt verwijderen van de gebruiker uit de groep **klikt u op de rij** van het Groepsbeleid en selecteer verwijderen.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a>Controleer of een gebruiker lid is van een groep die is toegewezen aan een licentie is

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **groepen.**

8.  Klik op de rij van een specifieke groep.

9.  Klik op **licenties** om te zien die de groep licenties is toegewezen.

  * Als de groep is toegewezen aan een Office-licentie hierdoor bepaalde eerste partij Office-toepassingen worden weergegeven op het toegangsvenster van de gebruiker.


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Als bovenstaande stappen voor probleemoplossing niet de los het probleem doet

een ondersteuningsticket opent met de volgende informatie, indien beschikbaar:

-   Correlatie fout-ID

-   UPN (e-mailadres van de gebruiker)

-   Tenant-id

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](active-directory-enable-sso-scenario.md)
