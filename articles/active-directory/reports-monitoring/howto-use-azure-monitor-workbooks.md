---
title: Azure Monitor werkmappen gebruiken voor Azure Active Directory-rapporten | Microsoft Docs
description: Informatie over het gebruik van de Azure Monitor-werkmappen voor Azure Active Directory-rapporten
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 4066725c-c430-42b8-a75b-fe2360699b82
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 2c9b3d0ef110fea0629af345a71d0d7b7cce7313
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013586"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Procedure: Azure Monitor werkmappen gebruiken voor Azure Active Directory-rapporten

U wilt doen:

- Begrijp de gevolgen van uw [beleid voor voorwaardelijke toegang](../conditional-access/overview.md) op uw gebruikers aanmelden?

- Problemen met aanmelden voor een beter overzicht van uw organisatie aanmelden status evenals snel problemen oplossen?

- Weten wie verouderde verificaties aan te melden op uw omgeving gebruikt? Door [blokkeren van verouderde](../conditional-access/block-legacy-authentication.md), kunt u de beveiliging van uw tenant te verbeteren.


[Azure Monitor werkmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) combineren van tekst, Analytics-query's, Azure metrische gegevens en parameters in uitgebreide interactieve rapporten. Azure Active Directory biedt u werkmappen voor het bewaken van die u helpen bij het vinden van antwoorden op de bovenstaande vragen.

Dit artikel:

- Wordt ervan uitgegaan dat u bekend bent met het [interactieve rapporten maken met Azure Monitor werkmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).

- Wordt uitgelegd hoe u kunt de Azure Monitor-werkmappen over het controleren van de bovenstaande vragen beantwoorden.
 


## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze functie te gebruiken:

- Een Azure Active Directory-tenant met een licentie premium (P1/P2). Meer informatie over het [een premium-licentie nodig](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium).

- Een [Log Analytics-werkruimte](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

## <a name="access-workbooks"></a>Toegang tot werkmappen 

Voor toegang tot werkmappen:

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com).

2. Klik op de navigatiebalk links **Azure Active Directory**.

3. In de **bewaking** sectie, klikt u op **Insights**. 

    ![Inzichten](./media/howto-use-azure-monitor-workbooks/41.png)

4. Klik op een rapport of een sjabloon of **Open** in de werkbalk. 

    ![Gallery](./media/howto-use-azure-monitor-workbooks/42.png)


## <a name="sign-in-analysis"></a>Analyse van aanmelden

Voor toegang tot de werkmap aanmelden analyse, klikt u op **aanmeldingen** in de **gebruik** sectie. 

Deze werkmap bevat de volgende aanmelding trends in de:

- Alle aanmeldingen

- Geslaagd

- Gebruikersactie in behandeling

- Fout

U kunt elke trend door te filteren:

- Tijdsbereik

- Apps

- Gebruikers

![Gallery](./media/howto-use-azure-monitor-workbooks/43.png)


Voor elke trend krijgt u een uitsplitsing van:

- Location

    ![Gallery](./media/howto-use-azure-monitor-workbooks/45.png)

- Apparaat

    ![Gallery](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Aanmeldingen met behulp van verouderde verificatie 


Voor het gebruik van de toegang tot de aanmeldingen [verouderde](../conditional-access/block-legacy-authentication.md) werkmap, klikt u op **-aanmeldingen met behulp van verouderde verificatie** in de **gebruik** sectie. 

Deze werkmap bevat de volgende aanmelding trends in de:

- Alle aanmeldingen

- Geslaagd


U kunt elke trend door te filteren:

- Tijdsbereik

- Apps

- Gebruikers

- Protocollen 

![Gallery](./media/howto-use-azure-monitor-workbooks/47.png)


Voor elke trend krijgt u een uitsplitsing per-app en -protocol.

![Gallery](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Aanmeldingen voor voorwaardelijke toegang 


Voor toegang tot de aanmeldingen per [beleid voor voorwaardelijke toegang](../conditional-access/overview.md) werkmap, klikt u op **aanmeldingen voor voorwaardelijke toegang** in de **voorwaardelijke toegang** sectie. 

Deze werkmap wordt de trend voor uitgeschakelde aanmeldingen.

U kunt elke trend door te filteren:

- Tijdsbereik

- Apps

- Gebruikers

![Gallery](./media/howto-use-azure-monitor-workbooks/49.png)


Voor de uitgeschakelde aanmeldingen krijgt u een uitsplitsing van de status voor voorwaardelijke toegang.

![Status voor voorwaardelijke toegang](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)








## <a name="sign-ins-by-grant-controls"></a>-Aanmeldingen door besturingselementen verlenen

Voor toegang tot de aanmeldingen per [verlenen besturingselementen](../conditional-access/controls.md) werkmap, klikt u op **-aanmeldingen door besturingselementen verlenen** in de **voorwaardelijke toegang** sectie. 

Deze werkmap bevat de volgende uitgeschakelde aanmelden trends in:

- MFA vereisen
 
- Gebruiksvoorwaarden vereisen

- Privacyverklaring is vereist

- Overige


U kunt elke trend door te filteren:

- Tijdsbereik

- Apps

- Gebruikers

![Gallery](./media/howto-use-azure-monitor-workbooks/50.png)


Voor elke trend krijgt u een uitsplitsing per-app en -protocol.

![Gallery](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Analyse van storingen in aanmeldingen

Gebruik de **foutanalyses voor aanmeldingen** werkmap oplossen met:

- Aanmeldingen
- Voorwaardelijk toegangsbeleid
- Verouderde verificatie. 


Voor toegang tot de aanmeldingen van voorwaardelijke toegang tot gegevens, klikt u op **-aanmeldingen met behulp van verificatie van de oude** in de **oplossen** sectie. 

Deze werkmap bevat de volgende aanmelding trends in de:

- Alle aanmeldingen

- Geslaagd

- Wachtend op actie

- Fout


U kunt elke trend door te filteren:

- Tijdsbereik

- Apps

- Gebruikers

![Gallery](./media/howto-use-azure-monitor-workbooks/52.png)


Voor het oplossen van aanmeldingen, krijgt u een uitsplitsing van:

- Meestvoorkomende fouten

    ![Gallery](./media/howto-use-azure-monitor-workbooks/53.png)

- Aanmeldingen die wachten op actie van de gebruiker

    ![Gallery](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Volgende stappen

* [Interactieve rapporten maken met Azure Monitor-werkmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks)