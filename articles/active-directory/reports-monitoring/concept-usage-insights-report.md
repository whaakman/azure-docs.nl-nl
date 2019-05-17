---
title: Gebruik en inzichten rapport in de Azure Active Directory-portal | Microsoft Docs
description: Inleiding tot gebruik en inzichten rapport in de Azure Active Directory-portal
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 3fba300d-18fc-4355-9924-d8662f563a1f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 3fe1e72d277bffd4bc9b38ac377e33b341967e17
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65806354"
---
# <a name="usage-and-insights-report-in-the-azure-active-directory-portal"></a>Gebruik en inzichten rapport in de Azure Active Directory-portal

Met het rapport gebruiks- en inzichten krijgt u een toepassingsgerichte weergave van uw gegevens aanmelden. U vindt antwoorden op de volgende vragen:

*   Wat zijn de bovenkant toepassingen gebruikt in mijn organisatie?
*   Welke toepassingen de meeste mislukte aanmeldingen hebben? 
*   Wat zijn de belangrijkste aanmelden fouten voor elke toepassing?

## <a name="prerequisites"></a>Vereisten 

Voor toegang tot de gegevens uit het rapport gebruiks- en inzichten, hebt u het volgende nodig:

* Een Azure AD-tenant
* Een Azure AD premium (P1/P2)-licentie om de gegevens aanmelden weer te geven
* Een gebruiker in de globale beheerder, Beveiligingsbeheerder, beveiligingslezer of rol van beveiligingslezer rapport. Bovendien kan elke gebruiker (niet-beheerders) toegang tot hun eigen aanmeldingen. 

## <a name="access-the-usage-and-insights-report"></a>Toegang tot het gebruik en de insights-rapport

1. Navigeer naar [Azure Portal](https://portal.azure.com).
2. Selecteer de juiste map en selecteer vervolgens **Azure Active Directory** en kies **bedrijfstoepassingen**.
3. Uit de **activiteit** sectie, selecteer **gebruik & insights** om het rapport te openen. 

![Gebruik en de Insights-rapport](./media/concept-usage-insights-report/main-menu.png)
                                     

## <a name="use-the-report"></a>Het rapport gebruiken

Het gebruik en de insights-rapport bevat een lijst van toepassingen met een of meer sign in poging en kunt u sorteren op het aantal geslaagde aanmeldingen, mislukte aanmeldingen en het slagingspercentage voor.

Als u op load meer aan de onderkant van de lijst kunt u om aanvullende toepassingen op de pagina weer te geven. U kunt het datumbereik om weer te geven van alle toepassingen die zijn gebruikt binnen het bereik selecteren.

U kunt ook de focus instellen op een specifieke toepassing. Selecteer **aanmeldingsactiviteiten bekijken** om te zien van het teken van de activiteit na verloop van tijd voor de toepassing, evenals de ernstigste fouten.  

Als u een dag in de grafiek voor het gebruik van toepassing selecteert, krijgt u een gedetailleerd overzicht van de aanmeldingsactiviteiten voor de toepassing.  

![Gebruik en de Insights-rapport](./media/concept-usage-insights-report/usage-and-insights-report.png)

## <a name="next-steps"></a>Volgende stappen

* [Rapport aanmeldingen](concept-sign-ins.md)