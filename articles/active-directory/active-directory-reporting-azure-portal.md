---
title: Azure Active Directory-rapportage | Microsoft Docs
description: Biedt een algemeen overzicht van Azure Active Directory-rapportage.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 738c8f4a56586b87f03973ec258b0a3023affa60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-reporting"></a>Azure Active Directory-rapportage

Met Azure Active Directory-rapportage krijgt u inzicht in hoe uw omgeving presteert.  
Met de gegevens kunt u het volgende doen:

- Vaststellen hoe uw apps en services door uw gebruikers worden gebruikt
- Potentiële risico's detecteren die invloed hebben op de status van uw omgeving
- Problemen oplossen waardoor uw gebruikers hun werk niet kunnen doen  

De rapportagearchitectuur is afhankelijk van twee belangrijke zaken:

- Beveiligingsrapporten
- Activiteitsrapporten

![Rapportage](./media/active-directory-reporting-azure-portal/01.png)



## <a name="security-reports"></a>Beveiligingsrapporten

Met de beveiligingsrapporten in Azure Active Directory kunt u de identiteiten van uw organisatie beschermen.  
Er zijn in Azure Active Directory twee soorten beveiligingsrapporten:

- **Gebruikers voor wie wordt aangegeven dat ze risico lopen**: in het rapport [Gebruikers voor wie wordt aangegeven dat ze risico lopen](active-directory-reporting-security-user-at-risk.md) krijgt u een overzicht van gebruikersaccounts die mogelijk zijn aangetast.

- **Riskante aanmeldingen**: in het beveiligingsrapport [Riskante aanmeldingen](active-directory-reporting-security-risky-sign-ins.md) krijgt u een idee van aanmeldingspogingen die mogelijk zijn uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. 

**Welke Azure AD-licentie heb ik nodig voor toegang tot een beveiligingsrapport?**  
Alle edities van Azure Active Directory bieden rapporten over gebruikers voor wie wordt aangegeven dat ze risico lopen en rapporten over riskante aanmeldingen.  
Het detailniveau van rapporten verschilt wel per editie: 

- In de edities **Azure Active Directory Free en Basic** hebt u toegang tot een lijst die gebruikers bevat voor wie wordt aangegeven dat ze risico lopen, evenals riskante aanmeldingen. 

- De editie **Azure Active Directory Premium 1** bevat een uitgebreider model waarmee u ook bepaalde onderliggende risicogebeurtenissen kunt onderzoeken die voor elk rapport zijn gedetecteerd. 

- De editie **Azure Active Directory Premium 2** biedt u de meest gedetailleerde informatie over de onderliggende risicogebeurtenissen. Deze editie stelt u ook in staat beveiligingsbeleidsregels te configureren die automatisch op de geconfigureerde risiconiveaus reageren.


## <a name="activity-reports"></a>Activiteitsrapporten

Er zijn in Azure Active Directory twee soorten activiteitsrapporten:

- **Audittrails**: het [activiteitenrapport voor audittrails](active-directory-reporting-activity-audit-logs.md) biedt u toegang tot de geschiedenis van elke taak die in uw tenant is uitgevoerd.

- **Aanmeldingen**: met het [activiteitenrapport voor aanmeldingen](active-directory-reporting-activity-sign-ins.md) kunt u bepalen wie de taken heeft uitgevoerd die in het audittrailrapport zijn gerapporteerd.



De **audittrailrapporten** bieden records van systeemactiviteiten in het kader van naleving.
Met de geleverde gegevens kunt u onder andere de volgende veelvoorkomende vragen beantwoorden:

- Iemand in mijn tenant heeft toegang gekregen tot een beheerdersgroep. Wie heeft diegene toegang verleend? 

- Ik wil een lijst weergeven met gebruikers die zich in een specifieke app hebben aangemeld sinds ik de app onlangs heb toegevoegd en wil weten of de app het goed doet

- Ik wil weten hoe vaak er in mijn tenant een wachtwoord opnieuw is ingesteld


**Welke Azure AD-licentie heb ik nodig voor toegang tot audittrailrapporten?**  
Het audittrailrapport is beschikbaar voor functies waarvoor u licenties hebt. Als u een licentie voor een specifieke functie hebt, hebt u ook toegang tot de audittrailgegevens hiervan.

Zie **Comparing generally available features of the Free, Basic, and Premium editions** (Algemeen beschikbare functies van de Free-, Basic- en Premium-edities vergelijken) in [Azure Active Directory features and capabilities](https://www.microsoft.com/cloud-platform/azure-active-directory-features) (Functies en mogelijkheden van Azure Active Directory) voor meer informatie.   



Het **aanmeldactiviteitenrapport** helpt u antwoorden te vinden op vragen zoals:

- Wat is het aanmeldingspatroon van een gebruiker?
- Hoeveel keer hebben gebruikers zich aangemeld gedurende een week?
- Wat is de status van deze aanmeldingen?


**Welke Azure AD-licentie heb ik nodig voor toegang tot het aanmeldactiviteitenrapport?**  
Uw tenant moet beschikken over een Azure AD Premium-licentie om het rapport met aanmeldactiviteiten te kunnen openen.


## <a name="programmatic-access"></a>Toegang op programmeerniveau

De rapportage van Azure Active Directory biedt u naast de gebruikersinterface ook [toegang op programmeerniveau](active-directory-reporting-api-getting-started-azure-portal.md) tot de rapportagegegevens. De gegevens van deze rapporten kunnen zeer nuttig zijn voor uw toepassingen, zoals SIEM-systemen, audit- en business intelligence-hulpprogramma's. De API's van Azure AD Reporting bieden toegang tot de gegevens op programmeerniveau via een set op REST-gebaseerde API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen. 


## <a name="next-steps"></a>Volgende stappen

Als u meer wilt weten over de verschillende soorten rapporten in Azure Active Directory, raadpleegt u:

- [Rapport voor Gebruikers voor wie wordt aangegeven dat ze risico lopen](active-directory-reporting-security-user-at-risk.md)
- [Rapport voor riskante aanmeldingen](active-directory-reporting-security-risky-sign-ins.md)
- [Rapport voor audittrails](active-directory-reporting-activity-audit-logs.md)
- [Rapport voor aanmeldlogboeken](active-directory-reporting-activity-sign-ins.md)

Als u meer weten wilt over het openen van de rapportagegegevens met de rapportage-API, raadpleegt u: 

- [Aan de slag met de Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started-azure-portal.md)


<!--Image references-->
[1]: ./media/active-directory-reporting-azure-portal/ic195031.png