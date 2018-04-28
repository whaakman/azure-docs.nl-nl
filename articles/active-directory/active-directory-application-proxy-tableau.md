---
title: Azure Active Directory-toepassingsproxy en Tableau | Microsoft Docs
description: Informatie over het gebruik van toepassingsproxy van Azure Active Directory (Azure AD) voor externe toegang voor uw implementatie Tableau.  .
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 480e1cd72bcb42d7f39a92fc49e7c3d66ecf05f8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="azure-active-directory-application-proxy-and-tableau"></a>Azure Active Directory-toepassingsproxy en Tableau 

Azure Active Directory-toepassingsproxy en Tableau hebben hun zodat u zich eenvoudig kunt toepassingsproxy gebruiken voor externe toegang voor uw implementatie Tableau. Dit artikel wordt uitgelegd hoe u dit scenario configureert.  

## <a name="prerequisites"></a>Vereisten 

Het scenario in dit artikel wordt ervan uitgegaan dat u hebt:

- [Tableau](https://onlinehelp.tableau.com/current/server/en-us/proxy.htm#reverse-proxy-server) geconfigureerd. 

- Een [Application Proxy connector](active-directory-application-proxy-enable.md) geïnstalleerd. 

 

## <a name="enabling-application-proxy-for-tableau"></a>Toepassingsproxy voor Tableau inschakelen 

Als u wilt de toepassingsproxy gebruiken voor Tableau, moet u een e-mail sturen naar [ aadapfeedback@microsoft.com ](mailto:aadapfeedback@microsoft.com) ophalen van dit scenario is ingeschakeld.
In uw e-mailadres:

-   Toepassingsproxy inschakelen voor Tableau gebruiken als onderwerp
-   Het opnemen van uw tenant-ID in de hoofdtekst    

U krijgt een bevestiging wanneer u klaar bent voor het gebruik van de toepassing. Dit duurt normaal gesproken een week. U kunt echter de configuraties voltooien tijdens het wachten op voor de bevestiging.


 

## <a name="publish-your-applications-in-azure"></a>Uw toepassingen publiceren in Azure 

Als u wilt publiceren Tableau, moet u een toepassing publiceren in de Azure Portal.

Voor:

- Gedetailleerde instructies van de stappen 1-8, Zie [toepassingen publiceren met Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md). 
- Informatie over het zoeken naar Tableau waarden voor de velden toepassingsproxy Raadpleeg de documentatie van Tableau.  

**Voor het publiceren van uw app**: 


1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als globale beheerder. 

2. Selecteer **Azure Active Directory > bedrijfstoepassingen**. 

3. Selecteer **toevoegen** boven aan de blade. 

4. Selecteer **On-premises toepassing**. 

5. Vul de vereiste velden met informatie over uw nieuwe app. Gebruik de volgende richtlijnen voor de instellingen: 

    - **Interne URL**: deze toepassing moet een interne URL de URL van de Tableau is. Bijvoorbeeld `https://adventure-works.tableau.com`. 

    - **Methode voor verificatie vooraf**: Azure Active Directory (aanbevolen maar niet vereist). 

6. Selecteer **toevoegen** boven aan de blade. Uw toepassing wordt toegevoegd en het menu Snel starten wordt geopend. 

7. Selecteer in het menu Snel starten **een gebruiker toewijzen voor het testen van**, en ten minste één gebruiker toevoegen aan de toepassing. Zorg ervoor dat deze testaccount toegang heeft tot de on-premises toepassing. 

8. Selecteer **toewijzen** om op te slaan van de toewijzing van de gebruiker test. 

9. (Optioneel) Selecteer op de pagina app-beheer **eenmalige aanmelding**. Kies **geïntegreerde Windows-verificatie** uit de vervolgkeuzelijst en vul de vereiste velden op basis van uw configuratie Tableau. Selecteer **Opslaan**. 

 

## <a name="testing"></a>Testen 

Uw toepassing is nu gereed om te testen. Toegang tot de externe URL die u gebruikt voor het publiceren van Tableau, en meld u aan als een gebruiker die is toegewezen aan beide toepassingen.



## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure AD-toepassingsproxy [het verstrekken van veilige externe toegang tot on-premises toepassingen](active-directory-application-proxy-get-started.md).

