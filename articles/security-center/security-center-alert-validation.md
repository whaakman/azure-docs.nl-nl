---
title: Validatie van waarschuwingen in Azure Security Center | Microsoft Docs
description: In dit document leest u hoe u de beveiligingswaarschuwingen in Azure Security Center valideert.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: yurid
ms.openlocfilehash: d7aa8544f50b42bacfa1e1f16fdce468d8fc81ef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="alerts-validation-in-azure-security-center"></a>Validatie van waarschuwingen in Azure Security Center
In dit document leest u hoe u kunt controleren of uw systeem op de juiste manier is geconfigureerd voor waarschuwingen van Azure Security Center.

## <a name="what-are-security-alerts"></a>Wat zijn beveiligingswaarschuwingen?
In Security Center worden automatisch logboekgegevens verzameld, geanalyseerd en geïntegreerd van uw Azure-resources, het netwerk en verbonden partneroplossingen, zoals firewalls en oplossingen voor eindpuntbeveiliging, om bedreigingen te detecteren en u hiervoor te waarschuwen. Lees [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) voor meer informatie over beveiligingswaarschuwingen. Lees [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) voor meer informatie over de verschillende typen waarschuwingen.

## <a name="alert-validation"></a>Waarschuwingen valideren
Als de Security Center-agent is geïnstalleerd op uw computer, volgt u de onderstaande stappen op de computer waarvoor u een waarschuwing wilt ontvangen bij een aanval:

1. Kopieer een uitvoerbaar bestand (bijvoorbeeld calc.exe) naar het bureaublad van de computer of een andere map die eenvoudig toegankelijk is.
2. Wijzig de naam van dit bestand in **ASC_AlertTest_662jfi039N.exe**.
3. Open de opdrachtprompt en voer dit bestand uit met een zelf bedacht argument zoals *ASC_AlertTest_662jfi039N.exe - foo*
4. Wacht 5 tot 10 minuten en open Security Center. Hier moet u nu een waarschuwing zien zoals deze:

    ![Waarschuwingen valideren](./media/security-center-alert-validation/security-center-alert-validation-fig2.png)

Als u deze waarschuwing bekijkt, moet in het veld Controle van argumenten ingeschakeld 'waar' worden weergegeven. Als u de waarde 'onwaar' ziet, moet u de controle van opdrachtregelargumenten inschakelen. Dit kan met de volgende opdrachtregel:

*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*


> [!NOTE]
> Bekijk de video [Validatie van waarschuwingen in Azure Security Center](https://channel9.msdn.com/Blogs/Azure-Security-Videos/Alert-Validation-in-Azure-Security-Center) voor een demonstratie van deze functie. 

## <a name="see-also"></a>Zie ook
In dit artikel hebben we aandacht besteed aan het valideren van waarschuwingen. Raadpleeg de volgende artikelen als u meer over dit onderwerp wilt weten:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center. 
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.

