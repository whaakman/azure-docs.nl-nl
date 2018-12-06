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
ms.date: 11/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: dc0c6a37e11574cef857a7af773a9d90bea57357
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52972062"
---
# <a name="interactive-feature-verification-testing"></a>Functie voor interactieve verificatie testen  

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

U kunt de functie voor interactieve verificatie testen framework gebruiken om aan te vragen van tests voor uw systeem. Wanneer u een test aanvraagt, Microsoft het framework gebruikt om voor te bereiden van tests die handmatige interactieve stappen moeten worden uitgevoerd. Microsoft kan het framework gebruiken om te keten van verschillende zelfstandige geautomatiseerde tests.

Dit artikel wordt een eenvoudig scenario handmatige beschreven. De test controleert of het vervangen van een schijf in Azure Stack. Het framework verzamelt logboeken met diagnostische gegevens bij elke stap. Als u deze vinden, kunt u problemen met fouten opsporen. Het framework ook kunt het delen van logboeken die wordt geproduceerd door andere hulpprogramma's of processen, en kunt u feedback geven over het scenario.

## <a name="overview-of-a-test-pass"></a>Overzicht van een testronde

Een test voor de nieuwe schijf is een veelvoorkomend scenario. In dit voorbeeld heeft de test zeven stappen:

1.  Maak een nieuwe **testronde** werkstroom.
2.  Selecteer de **schijf-ID Test**.
3.  De test te starten.
4.  Kies de acties die in het scenario voor interactieve verificatie.
5.  Controleer het resultaat van het scenario.
6.  Het testresultaat naar Microsoft verzonden.
7.  Controleer de status in de portal VaaS.

## <a name="create-a-new-test-pass"></a>Maak een nieuwe testronde

1.  Navigeer naar de [validatie van Azure Stack-portal](https://www.azurestackvalidation.com) en meld u aan.

2.  Een nieuwe oplossing maken of kies een bestaande resourcegroep.

3.  Selecteer **Start** op de **testronde** tegel.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image1.png)

4.  Voer een naam voor de **testronde** werkstroom.

5.  Voer de omgeving en de algemene testparameters door de instructies in de [werkstroom algemene parameters voor Azure Stack-validatie als een Service](azure-stack-vaas-parameters.md) artikel.

6.  De verbindingsreeks van diagnostische gegevens voldoen aan de opgegeven indeling in de [Webtestparameters](azure-stack-vaas-parameters.md#test-parameters) sectie de [algemene werkstroomparameters](azure-stack-vaas-parameters.md) artikel.

7.  Voer de vereiste parameters voor de test.

8.  Selecteer de agent om uit te voeren van de interactieve test uitvoeren.

> [!Note]  
> Gebruiker met beheerdersrechten en het wachtwoord moeten worden opgegeven voor schijf-id functie voor interactieve verificatie testen.

![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image2.png)

## <a name="select-the-test"></a>Selecteer de test

1.  Selecteer **schijf-ID Test\<versie >**.

    > [!Note]  
    > De versie van de test wordt verhoogd wanneer er verbeteringen in de test kunnen worden aangebracht. De hoogste versie moet altijd worden gebruikt, tenzij anders wordt door Microsoft wordt aangegeven.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image4.png)

2.  Geef de gebruiker met beheerdersrechten en het wachtwoord door te selecteren **bewerken**.

3.  Selecteer de juiste test tot uitvoering van agent/DVM gestart van de test op.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image5.png)

4.  Selecteer **indienen** om de test te starten.

![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image6.png)

## <a name="start-the-test"></a>De test te starten

De schijf-ID Test aanwijzingen geven op de computer waarop de agent VaaS uitvoert. Meestal is dit de DVM of de Jumpbox voor het Azure Stack-exemplaar.

![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image8.png)

## <a name="choose-the-actions"></a>Kies de acties

1.  Ga als volgt de **documentatie** en **validatie** koppelingen naar instructies van Microsoft voor het uitvoeren van dit scenario te bekijken.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image9.png)

2.  Selecteer **Volgende**.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image10.png)

3.  Volg de instructies in de controle vooraf script uit te voeren.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image11.png)

4.  Zodra de controle vooraf script is voltooid, de handmatige scenario (nieuwe schijf) worden uitgevoerd volgens de **documentatie** en **validatie** koppeling in de **informatie**tabblad.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image12.png)

5.  Sluit het dialoogvenster niet tijdens het uitvoeren van de handmatige scenario.

6.  Wanneer u klaar bent met het uitvoeren van het scenario handmatige, volg de instructies de post-controle-script uit te voeren.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image13.png)

7.  Selecteer op de voltooiing van de handmatige (nieuwe schijf)-scenario, **volgende**.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image14.png)

> [!Important]  
> Als u het venster sluit, wordt de test wordt gestopt voordat deze wordt uitgevoerd.

## <a name="check-the-status"></a>De status controleren

1.  Wanneer de test voltooid is, wordt u gevraagd om feedback te geven.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image15.png)

2.  Deze vragen helpt Microsoft bij het beoordelen van de kwaliteit van succes snelheid en release van het scenario.

## <a name="send-the-test-result"></a>Het testresultaat verzenden

1.  Koppel alle logboekbestanden die u wilt verzenden naar Microsoft.

    ![Alternatieve tekst](media/azure-stack-vaas-interactive-feature-verification/image16.png)

2.  Accepteer de gebruiksrechtovereenkomst voor het verzenden van de feedback.

3.  Selecteer **indienen** de resultaten naar Microsoft verzenden.

## <a name="next-steps"></a>Volgende stappen

- [Controleren en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md)
