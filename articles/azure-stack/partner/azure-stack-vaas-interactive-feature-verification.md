---
title: Functie voor interactieve verificatie testen in Azure Stack-validatie als een Service | Microsoft Docs
description: Informatie over het maken van interactieve functie verificatietests uitvoeren voor Azure Stack met validatie uit als een Service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/07/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 01/07/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 20a48e67a24763af7bcce9e8831e2a1d1846d094
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594301"
---
# <a name="interactive-feature-verification-testing"></a>Functie voor interactieve verificatie testen  

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

U kunt de functie voor interactieve verificatie testen framework gebruiken om aan te vragen van tests voor uw systeem. Wanneer u een test aanvraagt, Microsoft het framework gebruikt om voor te bereiden van tests die handmatige interactieve stappen moeten worden uitgevoerd. Microsoft kan het framework gebruiken om te keten van verschillende zelfstandige geautomatiseerde tests.

Dit artikel wordt een eenvoudig scenario handmatige beschreven. De test controleert of het vervangen van een schijf in Azure Stack. Het framework verzamelt logboeken met diagnostische gegevens bij elke stap. Als u deze vinden, kunt u problemen met fouten opsporen. Het framework ook kunt het delen van logboeken die wordt geproduceerd door andere hulpprogramma's of processen, en kunt u feedback geven over het scenario.

> [!Important]  
> In dit artikel wordt verwezen naar de stappen voor het uitvoeren van schijf-ID. Dit is gewoon een demonstratie, zoals resultaten die zijn verzameld van de werkstroom testronde mag niet worden gebruikt voor verificatie van de nieuwe oplossing.

## <a name="overview-of-interactive-testing"></a>Overzicht van interactieve testen

Een test voor de nieuwe schijf is een veelvoorkomend scenario. In dit voorbeeld heeft de test zeven stappen:

1. Maak een nieuwe **testronde** werkstroom
1. Selecteer de **schijf-ID-Test**
1. Voert u de handmatige stap wanneer u hierom wordt gevraagd
1. Controleer het resultaat van het scenario
1. Het testresultaat naar Microsoft verzenden

## <a name="create-a-new-test-pass"></a>Maak een nieuwe testronde

Als u een bestaande test doorgeven beschikbaar hebt, volgt u de aanwijzingen voor het [plannen van een test](azure-stack-vaas-schedule-test-pass.md).

## <a name="schedule-the-test"></a>Plannen van de test

1. Selecteer **schijf-ID Test**.

    > [!Note]  
    > De versie van de test wordt verhoogd wanneer er verbeteringen in de test kunnen worden aangebracht. De hoogste versie moet altijd worden gebruikt, tenzij anders wordt door Microsoft wordt aangegeven.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image4.png)

1. Geef de gebruikersnaam van de domein-beheerder en het wachtwoord door te selecteren **bewerken**.

1. Selecteer de juiste test tot uitvoering van agent/DVM gestart van de test op.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image5.png)

1. Selecteer **indienen** om de test te starten.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image6.png)

1. Toegang tot de gebruikersinterface voor de interactieve test van de agent in de vorige stap hebt geselecteerd.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image8.png)

1. Ga als volgt de **documentatie** en **validatie** koppelingen naar instructies van Microsoft voor het uitvoeren van dit scenario te bekijken.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image9.png)

1. Selecteer **Volgende**.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image10.png)

1. Volg de instructies in de controle vooraf script uit te voeren.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image11.png)

1. Zodra de controle vooraf script is voltooid, de handmatige (nieuwe schijf)-scenario uitvoeren volgens de **documentatie** en **validatie** koppeling in de **informatie**tabblad.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image12.png)

    > [!Important]  
    > Sluit het dialoogvenster niet tijdens het uitvoeren van de handmatige scenario.

1. Wanneer u klaar bent met het uitvoeren van het scenario handmatige, volg de instructies de post-controle-script uit te voeren.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image13.png)

1. Selecteer op de voltooiing van de handmatige (nieuwe schijf)-scenario, **volgende**.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image14.png)

    > [!Important]  
    > Als u het venster sluit, wordt de test wordt gestopt voordat deze wordt uitgevoerd.

1. Feedback geven voor de test-ervaring. Deze vragen helpt Microsoft bij het beoordelen van de kwaliteit van succes snelheid en release van het scenario.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image15.png)

1. Koppel alle logboekbestanden die u wilt verzenden naar Microsoft.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image16.png)

1. Accepteer de gebruiksrechtovereenkomst voor het verzenden van de feedback.

1. Selecteer **indienen** de resultaten naar Microsoft verzenden.

## <a name="next-steps"></a>Volgende stappen

- [Controleren en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md)
