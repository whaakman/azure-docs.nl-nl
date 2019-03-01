---
title: Gebruikers met gebruiker analytics in Azure Sentinel Preview onderzoeken | Microsoft Docs
description: Meer informatie over het onderzoeken van gebruikers met gebruiker analytics in Azure Sentinel.
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 5b06aef0-16be-4ca0-83e4-a33795a82353
ms.service: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: b9944764345f97c561f3f82f9cce9d37e204f134
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992976"
---
# <a name="investigate-users-with-user-analytics"></a>Gebruikers met gebruiker analytics onderzoeken

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Gebruikers zijn de entiteiten die u wilt controleren. Als u wilt, kunt u inzicht krijgt in uw gebruikers Azure Sentinel naadloos geïntegreerd met Azure Advanced Threat Protection. Deze integratie kunt u gedrag van gebruikers analyseren en prioriteiten te stellen welke gebruikers dat u het eerst onderzoeken moet, op basis van hun waarschuwingen en patronen van verdachte activiteiten in Azure Sentinel en Microsoft 365.

Azure Sentinel verrijkt uw gebruikersgegevens met analyses van Microsoft 365 zodat uitgebreide analyse en risico analyse van gebruikers voor alle gebruikers in Azure Active Directory. 

## <a name="how-it-works"></a>Hoe werkt het?

1. Op basis van de beveiliging van uw analytische regels, wanneer een overeenkomst wordt gedetecteerd, de waarschuwingen naar Azure ATP Azure Sentinel verzendt.
1. Azure ATP controleert welke gebruikersentiteiten zijn met betrekking tot de waarschuwingen en berekent de prioriteit onderzoek voor gebruikers.
3. Azure ATP berekend vervolgens de score van de gebruikers nadat deze is verrijkt met gegevens uit uw regels analytics voor Azure Sentinel.


## <a name="prerequisites"></a>Vereisten

- Een licentie voor Azure Advanced Threat Protection 
- Als u wilt de functie inschakelt, moet u globale beheerdersmachtigingen heeft op de tenant waarin u geïnstalleerd Azure Sentinel

> [!NOTE]
> - Voor elke Azure ATP-tenant, kunt u verbinding maken met één exemplaar van de Azure-Sentinel.
> - Analyse van gebruikers is momenteel alleen beschikbaar voor Azure Active Directory-gebruikers. 

## <a name="enable-user-analytics"></a>Analyse van gebruikers inschakelen

1. Om te kunnen analyseren van de gebruiker in Azure Sentinel, in de portal gaat u naar de **gebruiker analytics** pagina en klik op **inschakelen**. Hierdoor wordt informatie over de werkruimte verzonden naar Azure ATP.

1. Vervolgens gaat u naar Azure ATP. Onder **Security extensions** in de **Microsoft Azure Sentinel** en klik op de **+ plus** wilt toevoegen en selecteer vervolgens de werkruimte. 
1. Klik op **Verbinden**.

## <a name="investigate-a-user"></a>Een gebruiker onderzoeken

1. In het menu Azure Sentinel onder **gebruiker analytics**, bekijk de lijst met gebruikers op basis van hun prioriteit onderzoek. De score is gebaseerd op Azure Sentinel waarschuwingen en meldingen van Microsoft 365.

2. Zoeken naar een gebruiker die u wilt onderzoeken. Klik op de gebruiker om te gaan naar de pagina van de gebruiker. Bekijk de activiteiten en waarschuwingen, van de gebruiker na verloop van tijd, in de tijdlijn geplaatst. Hier ziet u alle activiteiten van Microsoft 365 en Azure Sentinel. U kunt ook de gebruikerspagina bereiken door het inzoomen op de gebruikers vanuit een aanvraag.
      
    ![Gebruikers](./media/user-analytics/user-investigation.png)

 
3. Om dit te goed werken, die u hebt op de juiste wijze kan instellen de [aangepaste waarschuwingsregel](tutorial-detect-threats.md) om toe te wijzen de juiste gebruikers-id moet de **account** entiteit.

    - Voor Windows-gebeurtenissen toewijzen **Account** naar de **beveiligings-id**
    - Azure AD-gegevens (die kan worden gevonden in veel Logboeken, met inbegrip van Azure-activiteit) of Office 365-gegevens, wijzen de **Account** eigenschap in op de **GUID**, of de **User Principal Name**. 

   ![Query’s uitvoeren](./media/user-analytics/query-window.png)



Bijvoorbeeld: 
> [!NOTE]
> In de activiteitenlogboeken van Azure-activiteit bevat de entiteit van de oproepende functie de UPN.

1. Deze query zoekt voor het maken van een afwijkende aantal resources of implementaties uit te schakelen in Azure-activiteitenlogboek.

        AzureActivity      
        | where TimeGenerated >= startofday(ago(7d))        
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"        
        | where ActivityStatus == "Succeeded"        
        | make-series dResourceCount=dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(startofday(ago(7d)), now(), 1d) by Caller        
        | extend (RSquare,Slope,Variance,RVariance,Interception,LineFit)=series_fit_line(dResourceCount)        
        | where Slope > 0.2        
        | join kind=leftsemi (        
        // Last day's activity is anomalous        
        AzureActivity        
        | where TimeGenerated >= startofday(ago(1d))        
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"        
        | where ActivityStatus == "Succeeded"        
        | make-series dResourceCount=dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(startofday(ago(1d)), now(), 1d) by Caller        
        | extend (RSquare,Slope,Variance,RVariance,Interception,LineFit)=series_fit_line(dResourceCount)        
        | where Slope >0.2        
        ) on Caller        
        | extend AccountCustomEntity = Caller
 
2. In de aangepaste waarschuwingsregel, wijst u de **Account** eigenschap **aanroeper**.
   
   ![Query’s uitvoeren](./media/user-analytics/query-window.png)

3. De gebruiker in het venster van het onderzoek gebruiker onderzoeken. Zie voor advies over het onderzoeken van gebruikers, [zelfstudie: Een gebruiker onderzoeken](https://docs.microsoft.com/azure-advanced-threat-protection/investigate-a-user).



## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u verbinding maken met uw provider Bedreigingsinformatie Sentinel van Azure. Zie de volgende artikelen voor meer informatie over Azure Sentinel.

- Als u wilt aan de slag met Azure Sentinel, moet u een abonnement op Microsoft Azure. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/free/).
- Meer informatie over het [onboarding uw gegevens naar Azure Sentinel](quickstart-onboard.md), en [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
