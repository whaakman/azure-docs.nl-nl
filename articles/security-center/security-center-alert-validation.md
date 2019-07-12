---
title: Ontvang een waarschuwing validatie (EICAR test-bestand) in Azure Security Center | Microsoft Docs
description: In dit document leest u hoe u de beveiligingswaarschuwingen in Azure Security Center valideert.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: rkarlin
ms.openlocfilehash: f65b4b74a1a91fa081bd9c0d8146d055cebb0de6
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626306"
---
# <a name="alert-validation-eicar-test-file-in-azure-security-center"></a>Waarschuwingen valideren (EICAR test-bestand) in Azure Security Center
In dit document leest u hoe u kunt controleren of uw systeem op de juiste manier is geconfigureerd voor waarschuwingen van Azure Security Center.

## <a name="what-are-security-alerts"></a>Wat zijn beveiligingswaarschuwingen?
Meldingen zijn de berichten die Security Center worden gegenereerd wanneer bedreigingen voor uw resources zijn gedetecteerd. Dit bepaalt de volgorde en geeft een lijst van de waarschuwingen samen met de informatie die nodig zijn voor u het probleem snel te onderzoeken. Security Center bevat ook aanbevelingen voor het herstellen van een aanval.
Zie voor meer informatie, [beveiligingswaarschuwingen in Azure Security Center](security-center-alerts-overview.md) en [beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md)

## <a name="alert-validation"></a>Waarschuwingen valideren

* [Windows](#validate-windows)
* [Linux](#validate-linux)

## Validatie van op Windows-VM <a name="validate-windows"></a>

Volg deze stappen als de Security Center agent is geïnstalleerd op uw computer, van de computer waar u de aangevallen resource van de waarschuwing wilt:

1. Kopieer een uitvoerbaar bestand (bijvoorbeeld **calc.exe**) op het bureaublad van de computer, of een andere map van uw gemak en wijzig de naam als **ASC_AlertTest_662jfi039N.exe**.
1. Open de opdrachtprompt en voer dit bestand met het ongeldige argument (alleen een valse argumentnaam), zoals: ```ASC_AlertTest_662jfi039N.exe -foo```
1. Wacht 5 tot 10 minuten en open Security Center. Een waarschuwing die vergelijkbaar is met de [voorbeeld](#alert-validate) hieronder moet worden weergegeven:

> [!NOTE]
> Bij het beoordelen van deze Testwaarschuwing voor Windows, zorg ervoor dat het veld **argumenten controle ingeschakeld** is **waar**. Als het **false**, moet u de controle van opdrachtregelargumenten inschakelen. Als u wilt inschakelen, gebruikt u de volgende opdrachtregel:
>
>```reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"```

## Validatie van op Linux-VM <a name="validate-linux"></a>

Volg deze stappen als de Security Center agent is geïnstalleerd op uw computer, van de computer waar u de aangevallen resource van de waarschuwing wilt:
1. Kopieer een uitvoerbaar bestand op een geschikte locatie en wijzig de naam **. / asc_alerttest_662jfi039n**, bijvoorbeeld:

    ```cp /bin/echo ./asc_alerttest_662jfi039n```

1. Open de opdrachtprompt en voer dit bestand:

    ```./asc_alerttest_662jfi039n testing eicar pipe```

1. Wacht 5 tot 10 minuten en open Security Center. Een waarschuwing die vergelijkbaar is met de [voorbeeld](#alert-validate) hieronder moet worden weergegeven:

### Voorbeeld van waarschuwing <a name="alert-validate"></a>

![Voorbeeld van de waarschuwingen valideren](./media/security-center-alert-validation/security-center-alert-validation-fig2.png) 

## <a name="see-also"></a>Zie ook
In dit artikel hebben we aandacht besteed aan het valideren van waarschuwingen. Raadpleeg de volgende artikelen als u meer over dit onderwerp wilt weten:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.
