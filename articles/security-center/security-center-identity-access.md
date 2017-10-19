---
title: Identiteit en toegang bewaken in Azure Security Center | Microsoft Docs
description: Lees hoe u de functies voor identiteit en toegang in Azure Security Center gebruikt om problemen met de toegang en identiteit van uw gebruikers te bewaken.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: yurid
ms.openlocfilehash: 842045fbcb5b4f661cc006a207f4087370a7b4ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-identity-and-access-in-azure-security-center"></a>Identiteit en toegang bewaken in Azure Security Center
In dit artikel leest u hoe u Azure Security Center kunt gebruiken om de identiteit en toegangsactiviteiten van gebruikers te bewaken.

## <a name="why-monitor-identity-and-access"></a>Waarom moeten identiteit en toegang worden bewaakt?
Identiteit moet de controlelaag voor uw onderneming zijn. Het beveiligen van uw identiteit moet daarom de hoogste prioriteit hebben. Eerder maakten organisaties gebruik van een perimeternetwerk, en vormden dit netwerk een van de belangrijkste verdedigingslinies. Tegenwoordig bevinden zich steeds meer gegevens en apps in de cloud en is identiteit het nieuwe perimeternetwerk.

Door identiteitsactiviteiten te bewaken, kunt u proactieve maatregelen treffen voordat een incident plaatsvindt of reactieve maatregelen nemen om een aanvalspoging te stoppen. Het dashboard Identity & Access biedt een overzicht van uw identiteitstoestand, met onder andere deze gegevens:

* Het aantal mislukte aanmeldingspogingen. 
* De gebruikersaccounts die zijn gebruikt tijdens deze pogingen.
* Accounts die zijn vergrendeld.
* Accounts met gewijzigde of opnieuw ingestelde wachtwoorden. 
* Het huidige aantal accounts dat is aangemeld.

## <a name="monitor-identity-and-access-activities"></a>Identiteits- en toegangsactiviteiten bewaken
Als u de huidige activiteiten met betrekking tot identiteit en toegang wilt zien, moet u het dashboard **Identity & Access** openen.

1. Open het dashboard van **Security Center**.

2. Selecteer in het linkerdeelvenster, onder **Preventie**, de optie **Identity & Access**. De werkruimteselector verschijnt als u over meerdere werkruimten beschikt.

    ![Werkruimteselector](./media/security-center-identity-access\security-center-identity-access-fig1.png)

    > [!NOTE]
    > Als in de kolom helemaal rechts de tekst **UPGRADE ABONNEMENT** staat, is deze werkruimte gekoppeld aan het gratis abonnement. Upgrade naar de Standard-versie om deze functie te gebruiken. Als in de kolom helemaal rechts **VEREIST UPDATE** staat, moet u [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bijwerken om deze functie te gebruiken. Lees Prijzen van Security Center voor meer informatie over de prijzen. 
    > 
3. Als er meer dan één werkruimte is om te onderzoeken, bepaalt u de prioriteit van het onderzoek via de kolom **FAILED LOGONS**. Hier ziet u het huidige aantal mislukte aanmeldingspogingen in deze werkruimte. Selecteer de werkruimte die u wilt gebruiken. Het dashboard **Identity & Access** wordt weergegeven.

    ![Identity & Access](./media/security-center-identity-access\security-center-identity-access-fig2.png)

4. U kunt de informatie in dit dashboard onmiddellijk gebruiken om mogelijk verdachte activiteiten vast te stellen. Het dashboard is onderverdeeld in drie hoofdgebieden:

    a. **Identiteitspostuur**. Een overzicht van de aan identiteit gerelateerde activiteiten die in deze werkruimte plaatsvinden.

    b. **Mislukte aanmeldingen**. Hiermee kunt u snel de hoofdoorzaak van mislukte aanmeldingspogingen vaststellen. Toont een lijst met de tien accounts met het meeste aantal mislukte aanmeldingspogingen.

    c. **Aanmeldingen in de loop van de tijd**. Hiermee kunt u snel het aantal aanmeldingen identificeren gedurende een bepaalde periode. U ziet een lijst van de computeraccounts met de meeste aanmeldingspogingen.
    
Ongeacht de tegel die u selecteert, is het dashboard dat wordt weergegeven altijd gebaseerd op de query van Zoeken in logboeken. Het enige verschil is het type query en het resultaat. U kunt nog steeds een item selecteren, bijvoorbeeld een computer, om de belangrijkste gegevens te bekijken. 

## <a name="see-also"></a>Zie ook
In dit artikel hebt u kunnen lezen hoe u identiteit en toegang in Security Center bewaakt. Zie de volgende artikelen voor meer informatie over Security Center:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center. 
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Vind antwoorden op veelgestelde vragen over het gebruik van Security Center.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.

