---
title: Identiteit en toegang bewaken in Azure Security Center | Microsoft Docs
description: Met behulp van dit document kunt u gebruikmaken van identiteits- en toegangsmogelijkheden in Azure Security Center om problemen met de toegangsactiviteit en identiteit van uw gebruikers te bewaken.
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
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 2e351cd38e7baefc09fa36ceabffec92de22433b
ms.contentlocale: nl-nl
ms.lasthandoff: 09/14/2017

---
# <a name="monitoring-identity-and-access-in-azure-security-center"></a>Identiteit en toegang bewaken in Azure Security Center
Met behulp van dit document kunt u gebruikmaken van Azure Security Center om de identiteit en toegangsactiviteit van gebruikers te bewaken.

## <a name="why-monitor-identity-and-access"></a>Waarom moeten identiteit en toegang worden bewaakt?
Identiteit moet de controlelaag voor uw onderneming zijn. Het beveiligen van uw identiteit moet de hoogste prioriteit hebben. De omheining rond bedrijven vormde vroeger een van de primaire verdedigingslinies. Doordat gegevens en apps steeds vaker naar de cloud worden verplaatst, is identiteit tegenwoordig de nieuwe omheining.

Door uw identiteitsactiviteiten te bewaken, kunt u proactieve maatregelen treffen voordat een incident plaatsvindt of reactieve maatregelen nemen om een aanvalspoging te stoppen. Op het dashboard Identiteit en toegang vindt u een overzicht van uw identiteitsstatus, waaronder het aantal mislukte aanmeldingspogingen, het gebruikersaccount dat voor deze pogingen is gebruikt, accounts die zijn vergrendeld, accounts met een gewijzigd of opnieuw ingesteld wachtwoord en het aantal accounts dat op dat moment is aangemeld.

## <a name="how-to-monitor-identity-and-access-activities"></a>Hoe moeten de identiteits- en toegangsactiviteiten worden bewaakt?
Volg de stappen hieronder om een voorstelling te maken van de huidige activiteiten in verband met identiteit en toegang. Hiervoor moet u het dashboard **Identiteit & toegang** openen:

1.  Open het dashboard **Security Center**.
2.  Klik in het linkerdeelvenster, onder **Preventie**, op **Identiteit & toegang**. De werkruimteselector verschijnt als u over meerdere werkruimten beschikt.

    ![werkruimteselectie](./media/security-center-identity-access\security-center-identity-access-fig1.png)

    > [!NOTE]
    > Als in de laatste kolom **UPGRADE PLAN** wordt weergegeven, dan maakt deze werkruimte niet langer gebruik van het gratis abonnement en moet u een upgrade uitvoeren naar Standard om deze functie te gebruiken. Als er REQUIRES UPDATE wordt weergegeven, dan moet u [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bijwerken om deze functie te gebruiken. Lees de prijsopgave van het Azure Security Center voor meer informatie over de prijzen. 
    > 
3. Als er meer dan één werkruimte moet worden onderzocht, dan kunt u een voorkeur aangeven volgens de kolom **FAILED LOGONS**, waarin het huidige aantal mislukte aanmeldpogingen in deze werkruimte wordt weergegeven. Selecteer de werkruimte die u wilt gebruiken. Het dashboard **Identiteit & toegang** wordt weergegeven.

    ![identiteit en toegang](./media/security-center-identity-access\security-center-identity-access-fig2.png)

4. U kunt de informatie in dit dashboard onmiddellijk gebruiken om mogelijk verdachte activiteiten vast te stellen. Dit dashboard is onderverdeeld in drie belangrijke gebieden:
    * **Identiteitspostuur**: geeft een overzicht van de met identiteit gerelateerde activiteiten die in deze werkruimte plaatsvinden.
    * **Mislukte aanmeldingen**: hiermee kunt u snel de hoofdoorzaak identificeren van de mislukte aanmelding, en er wordt een lijst getoond met de belangrijkste tien accounts waarvoor de meeste mislukte aanmeldingen hebben plaatsgevonden.
    * **Verlopen aanmeldingen**: hiermee kunt u snel de hoeveelheid verlopen aanmeldingen bepalen en er wordt een lijst getoond met computeraccounts met de meeste mislukte aanmeldingen.
    
Ongeacht de geselecteerde tegel, is het dashboard dat wordt weergegeven, gebaseerd op de [zoekquery voor het logboek](https://docs.microsoft.com/azure/security-center/security-center-search). Het enige verschil is het type query en het resultaat. U kunt nog steeds een item selecteren, bijvoorbeeld een computer. Klik erop om de belangrijkste gegevens te bekijken. 

## <a name="see-also"></a>Zie ook
In dit document hebt u kunnen lezen hoe u identiteit en toegang in Azure Security Center bewaakt. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center. 
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.


