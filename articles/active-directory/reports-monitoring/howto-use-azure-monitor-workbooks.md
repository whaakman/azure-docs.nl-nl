---
title: Azure Monitor werkmappen gebruiken voor Azure Active Directory-rapporten | Microsoft Docs
description: Informatie over het gebruik van Azure Monitor werkmappen voor Azure Active Directory-rapporten.
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
ms.openlocfilehash: 6d461725d0d4c86ee7aa5b450d42c01ca77ffb16
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107668"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Het gebruik van Azure Monitor werkmappen voor Azure Active Directory-rapporten

U wilt doen:

- Het effect van uw [beleid voor voorwaardelijke toegang](../conditional-access/overview.md) op uw gebruikers aanmelden ervaring?

- Problemen met aanmelden oplossen om een beter beeld van uw organisatie aanmelden status en het oplossen van problemen snel?

- Weten wie verouderde verificaties te melden bij uw omgeving gebruikt? (Door [blokkeren van verouderde](../conditional-access/block-legacy-authentication.md), kunt u de beveiliging van uw tenant te verbeteren.)

Om te helpen om deze vragen, biedt Active Directory werkmappen voor bewaking. [Azure Monitor werkmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) combineren van tekst, analytics-query's, metrische gegevens en parameters in uitgebreide interactieve rapporten. 

Dit artikel:

- Wordt ervan uitgegaan dat u bekend bent met het [interactieve rapporten maken met behulp van de Monitor werkmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).

- Wordt uitgelegd hoe Monitor werkmappen gebruiken om het effect van uw beleid voor voorwaardelijke toegang om op te lossen aanmeldingen te identificeren, en verouderde verificaties te begrijpen.
 


## <a name="prerequisites"></a>Vereisten

Voor Monitor werkmappen, hebt u het volgende nodig:

- Een Active Directory-tenant met een premium-licentie voor (P1 of P2). Meer informatie over het [een premium-licentie nodig](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium).

- Een [Log Analytics-werkruimte](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

## <a name="workbook-access"></a>Werkmap-toegang 

Voor toegang tot werkmappen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer in het navigatiedeelvenster links **Azure Active Directory**.

3. In de **bewaking** sectie, selecteer **Insights**. 

    ![Selecteer Insights](./media/howto-use-azure-monitor-workbooks/41.png)

4. Selecteer een rapport of een sjabloon of Selecteer op de werkbalk **Open**. 

    ![Selecteer openen](./media/howto-use-azure-monitor-workbooks/42.png)


## <a name="sign-in-analysis"></a>Analyse van aanmelden

Voor toegang tot de werkmap aanmelden analyse de **gebruik** sectie, selecteer **aanmeldingen**. 

Deze werkmap bevat de volgende aanmelding trends in de:

- Alle aanmeldingen

- Geslaagd

- In behandeling gebruikersactie

- Fout

U kunt elke trend filteren op de volgende categorieën:

- Tijdsbereik

- Apps

- Gebruikers

![Analyse van aanmelden](./media/howto-use-azure-monitor-workbooks/43.png)


Voor elke trend krijgt u een uitsplitsing van de volgende categorieën:

- Locatie

    ![Aanmeldingen per locatie](./media/howto-use-azure-monitor-workbooks/45.png)

- Apparaat

    ![Aanmeldingen per apparaat](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Aanmeldingen met behulp van verouderde verificatie 


Voor toegang tot de werkmap voor aanmeldingen die gebruikmaken van [verouderde](../conditional-access/block-legacy-authentication.md), in de **gebruik** sectie, selecteer **-aanmeldingen met behulp van verouderde verificatie**. 

Deze werkmap bevat de volgende aanmelding trends in de:

- Alle aanmeldingen

- Geslaagd


U kunt elke trend filteren op de volgende categorieën:

- Tijdsbereik

- Apps

- Gebruikers

- Protocollen

![-Aanmeldingen door verouderde-verificatie](./media/howto-use-azure-monitor-workbooks/47.png)


Voor elke trend krijgt u een uitsplitsing per-app en -protocol.

![Legacy-verificatie-aanmeldingen door de app en het protocol](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Aanmeldingen voor voorwaardelijke toegang 


Voor toegang tot de werkmap voor aanmeldingen per [beleid voor voorwaardelijke toegang](../conditional-access/overview.md), in de **voorwaardelijke toegang** sectie, selecteer **aanmeldingen voor voorwaardelijke toegang**. 

Deze werkmap bevat de trends voor uitgeschakelde aanmeldingen. U kunt elke trend filteren op de volgende categorieën:

- Tijdsbereik

- Apps

- Gebruikers

![Aanmeldingen via voorwaardelijke toegang](./media/howto-use-azure-monitor-workbooks/49.png)


Voor uitgeschakelde aanmeldingen krijgt u een uitsplitsing van de status voor voorwaardelijke toegang.

![Status van voorwaardelijke toegang](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)








## <a name="sign-ins-by-grant-controls"></a>-Aanmeldingen door besturingselementen verlenen

Voor toegang tot de werkmap voor aanmeldingen per [verlenen besturingselementen](../conditional-access/controls.md), in de **voorwaardelijke toegang** sectie, selecteer **-aanmeldingen door besturingselementen verlenen**. 

Deze werkmap bevat de volgende uitgeschakelde aanmelden trends in:

- MFA vereisen
 
- Gebruiksvoorwaarden vereisen

- Privacyverklaring is vereist

- Overige


U kunt elke trend filteren op de volgende categorieën:

- Tijdsbereik

- Apps

- Gebruikers

![-Aanmeldingen door besturingselementen verlenen](./media/howto-use-azure-monitor-workbooks/50.png)


Voor elke trend krijgt u een uitsplitsing per-app en -protocol.

![Uitsplitsing van recente aanmeldingen](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Analyse van storingen in aanmeldingen

Gebruik de **foutanalyses voor aanmeldingen** werkmap oplossen door het volgende:

- Aanmeldingen
- Beleid voor voorwaardelijke toegang
- Verouderde verificatie 


Toegang krijgen tot de aanmeldingen van voorwaardelijke toegang tot gegevens, in de **oplossen** sectie, selecteer **-aanmeldingen met behulp van verificatie van de oude**. 

Deze werkmap bevat de volgende aanmelding trends in de:

- Alle aanmeldingen

- Geslaagd

- In behandeling zijnde actie

- Fout


U kunt elke trend filteren op de volgende categorieën:

- Tijdsbereik

- Apps

- Gebruikers

![Probleemoplossing-aanmeldingen](./media/howto-use-azure-monitor-workbooks/52.png)


Voor hulp bij het oplossen van aanmeldingen, Azure Monitor biedt u een uitsplitsing van de door de volgende categorieën:

- Meestvoorkomende fouten

    ![Overzicht van de meestvoorkomende fouten](./media/howto-use-azure-monitor-workbooks/53.png)

- Aanmeldingen die wachten op actie van de gebruiker

    ![Overzicht van aanmeldingen die wachten op actie van de gebruiker](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Volgende stappen

[Interactieve rapporten maken met behulp van de Monitor werkmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).