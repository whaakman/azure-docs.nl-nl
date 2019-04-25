---
title: Een geïmplementeerde StorSimple-apparaat oplossen | Microsoft Docs
description: Beschrijft hoe u kunt vaststellen en oplossen van fouten die optreden op een StorSimple-apparaat dat momenteel is geïmplementeerd en operationele.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: ''
ms.assetid: ea5d89ae-e379-423f-b68b-53785941d9d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: 8ad3f09bf46caf426b2008b583ebd2ff78522462
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60302514"
---
# <a name="troubleshoot-an-operational-storsimple-device"></a>Een operationele StorSimple-apparaat oplossen
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. De StorSimple-apparaatbeheerfuncties worden automatisch volgens schema verplaatst naar de nieuwe Azure Portal. U ontvangt hierover een e-mailbericht en een portalmelding. Dit document wordt binnenkort ook ingetrokken. Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: Verplaatsen naar Azure portal](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Overzicht
In dit artikel vindt u nuttige richtlijnen voor probleemoplossing voor het oplossen van problemen met configuratie voor dat u tegenkomen kunt wanneer uw StorSimple-apparaat geïmplementeerd en operationele is. Hierin worden veelvoorkomende problemen, mogelijke oorzaken en aanbevolen stappen voor het oplossen van problemen die u tegenkomen kunt wanneer u Microsoft Azure StorSimple uitvoert. Deze informatie is van toepassing op zowel de on-premises StorSimple fysieke apparaat en het virtuele StorSimple-apparaat.

Aan het einde van dit artikel vindt u een lijst met foutcodes die u tijdens het Microsoft Azure StorSimple tegenkomen kunt, evenals stappen kunt u uitvoeren om de fouten oplossen. 

## <a name="setup-wizard-process-for-operational-devices"></a>Setup-wizard het proces voor actieve apparaten
U de wizard setup gebruiken ([Invoke-hcssetupwizard uit][1]) om te controleren van de apparaatconfiguratie en zo nodig actie ondernemen.

Wanneer u de wizard setup op een eerder geconfigureerde en operationele apparaat uitvoert, wordt het proces dat plaatsvindt verschilt. U kunt alleen de volgende items wijzigen:

* IP-adres, subnetmasker en gateway
* Primaire DNS-server
* Primaire NTP-server
* Optionele configuratie van uw webproxy

De wizard setup voert de bewerkingen die betrekking hebben op de verzameling en apparaat-wachtwoordregistratie.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Fouten die tijdens de volgende uitvoeringen van de wizard setup optreden
De volgende tabel beschrijft de fouten die optreden tijdens het uitvoeren van de wizard setup op een operationele apparaat, de mogelijke oorzaken voor de fouten en aanbevolen acties op te lossen. 

| Nee. | Foutbericht of een voorwaarde | Mogelijke oorzaken | Aanbevolen actie |
|:--- |:--- |:--- |:--- |
| 1 |Fout 350032: Dit apparaat is al gedeactiveerd. |U kunt deze fout wordt weergegeven als u de wizard setup uitvoeren op een apparaat dat is gedeactiveerd. |[Neem contact op met Microsoft Ondersteuning](storsimple-contact-microsoft-support.md) voor volgende stappen. Een gedeactiveerd apparaat kan niet worden geplaatst in de service. Fabrieksinstellingen terugzetten is mogelijk vereist voordat het apparaat kan opnieuw worden geactiveerd. |
| 2 |Invoke-hcssetupwizard uit: ERROR_INVALID_FUNCTION (uitzondering van HRESULT: 0x80070001) |Het bijwerken van de DNS-server is mislukt. DNS-instellingen zijn algemene instellingen en worden toegepast op alle ingeschakelde netwerkinterfaces. |Schakel de interface en de DNS-instellingen opnieuw toepassen. Omdat deze instellingen zijn van toepassing, kan dit het netwerk voor andere netwerkinterfaces ingeschakelde verstoren. |
| 3 |Het apparaat wordt weergegeven in de serviceportal voor StorSimple Manager online te zijn, maar wanneer u de minimale installatie voltooid en de configuratie op te slaan, de bewerking is mislukt. |Tijdens de eerste configuratie, is de webproxy niet geconfigureerd, zelfs als er een werkelijke proxyserver voldaan is. |Gebruik de [cmdlet Test-HcsmConnection] [ 2] te vinden van de fout. [Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md) als u niet om het probleem te verhelpen. |
| 4 |Invoke-hcssetupwizard uit: Waarde valt niet binnen het verwachte bereik. |Deze fout wordt veroorzaakt door een onjuiste subnetmasker. Mogelijke oorzaken zijn: <ul><li> Het subnetmasker is ontbreekt of is leeg.</li><li>Het Ipv6-voorvoegselindeling is onjuist.</li><li>De interface is ingeschakeld voor de cloud, maar de gateway is ontbreekt of is onjuist.</li></ul>Houd er rekening mee dat de DATA 0 automatisch ingeschakeld voor de cloud is als geconfigureerd via de wizard setup. |Subnet 0.0.0.0 of 256.256.256.256 gebruiken om te bepalen van het probleem, en zoek vervolgens naar de uitvoer. Geef de juiste waarden voor het subnetmasker, gateway en Ipv6-voorvoegsel, indien nodig. |

## <a name="error-codes"></a>Foutcodes
Fouten worden weergegeven in numerieke volgorde.

| Foutnummer | Fouttekst of beschrijving | Aanbevolen gebruikersactie |
|:--- |:--- |:--- |
| 10502 |Er is een fout opgetreden tijdens het openen van uw storage-account. |Wacht een paar minuten en probeer het vervolgens opnieuw. Als u de fout zich blijft voordoen, neemt u contact opnemen met Microsoft ondersteuning voor de volgende stappen. |
| 40017 |De back-upbewerking is mislukt omdat er een volume dat is opgegeven in de back-upbeleid is niet gevonden op het apparaat. |Probeer de back-up bewerking, als de fout zich blijft voordoen, neem contact op met Microsoft Support. voor de volgende stappen. |
| 40018 |De back-upbewerking is mislukt omdat geen van de volumes die zijn opgegeven in de back-upbeleid zijn gevonden op het apparaat. |Probeer de back-up bewerking, als de fout zich blijft voordoen, neem contact op met Microsoft Support. voor de volgende stappen. |
| 390061 |Het systeem is bezet of niet beschikbaar. |Wacht een paar minuten en probeer het vervolgens opnieuw. Als u de fout zich blijft voordoen, neemt u contact opnemen met Microsoft ondersteuning voor de volgende stappen. |
| 390143 |Er is een fout opgetreden met foutcode 390143. (Onbekende fout.) |Als de fout zich blijft voordoen, kunt u contact op met Microsoft Support voor de volgende stappen. |

## <a name="next-steps"></a>Volgende stappen
Als u niet het probleem op te lossen [Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md) voor hulp. 

[1]: https://technet.microsoft.com/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/%5Clibrary/Dn715782(v=WPS.630).aspx
