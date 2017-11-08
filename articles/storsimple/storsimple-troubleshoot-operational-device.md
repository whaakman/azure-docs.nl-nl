---
title: "Een geïmplementeerde StorSimple-apparaat oplossen | Microsoft Docs"
description: "Beschrijft hoe opsporen en oplossen van fouten die optreden op een StorSimple-apparaat dat momenteel is geïmplementeerd en operationele."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ea5d89ae-e379-423f-b68b-53785941d9d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: 4639e749cfaa6b38fa6b91ddebfc17924c1d5bd6
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="troubleshoot-an-operational-storsimple-device"></a>Een operationele StorSimple-apparaat oplossen
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Overzicht
Dit artikel bevat nuttige richtlijnen voor probleemoplossing voor het oplossen van problemen met de configuratie die u tegenkomen kunt nadat uw StorSimple-apparaat geïmplementeerd en operationele is. Hierin worden veelvoorkomende problemen, mogelijke oorzaken en aanbevolen stappen voor het oplossen van problemen die u ervaren kunt wanneer u Microsoft Azure StorSimple uitvoert. Deze informatie is van toepassing op StorSimple on-premises fysieke apparaat en het virtuele StorSimple-apparaat.

Aan het einde van dit artikel vindt u een lijst met foutcodes die u tijdens de bewerking van de Microsoft Azure StorSimple tegenkomen kunt, evenals stappen kunt u nemen de fouten op te lossen. 

## <a name="setup-wizard-process-for-operational-devices"></a>Wizard configuratieproces voor operationele apparaten
Gebruik van de installatiewizard ([Invoke-HcsSetupWizard][1]) om te controleren van de apparaatconfiguratie en los de problemen indien nodig.

Wanneer u de wizard setup op een eerder geconfigureerde en operationele apparaat uitvoert, wordt de processtroom verschilt. U kunt alleen de volgende vermeldingen wijzigen:

* IP-adres, subnetmasker en gateway
* Primaire DNS-server
* Primaire NTP-server
* Optionele webproxyconfiguratie

De installatiewizard heeft de bewerkingen die betrekking hebben op de verzameling en apparaat-wachtwoordregistratie niet uitvoeren.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Fouten die tijdens de daaropvolgende wordt uitgevoerd van de wizard setup optreden
De volgende tabel beschrijft de fouten die optreden tijdens het uitvoeren van de wizard setup op een apparaat operationele, mogelijke oorzaken van de fouten en de aanbevolen acties deze op te lossen. 

| Nee. | Foutbericht of voorwaarde | Mogelijke oorzaken | Aanbevolen actie |
|:--- |:--- |:--- |:--- |
| 1 |Fout 350032: Dit apparaat is al uitgeschakeld. |U kunt deze fout wordt weergegeven als u de wizard setup uitvoeren op een apparaat dat wordt gedeactiveerd. |[Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md) voor volgende stappen. Een gedeactiveerde apparaat kan niet worden geplaatst in de service. Fabrieksinstellingen mogelijk zijn vereist voordat het apparaat kan opnieuw worden geactiveerd. |
| 2 |Invoke-HcsSetupWizard: ERROR_INVALID_FUNCTION (uitzondering van HRESULT: 0x80070001) |Het bijwerken van de DNS-server is mislukt. DNS-instellingen zijn algemene instellingen en worden toegepast op alle ingeschakelde netwerkinterfaces. |Schakel de interface en de DNS-instellingen opnieuw toepassen. Omdat deze instellingen globale zijn, kan dit het netwerk naar andere ingeschakelde interfaces verstoren. |
| 3 |Het apparaat wordt weergegeven in de StorSimple Manager-serviceportal online te zijn, maar wanneer u de minimale instelling voltooid en de configuratie op te slaan, mislukt de bewerking. |Tijdens de eerste configuratie is de webproxy niet geconfigureerd, zelfs als er een werkelijke proxyserver aanwezig is. |Gebruik de [cmdlet Test-HcsmConnection] [ 2] de fout te vinden. [Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md) als u niet om het probleem te verhelpen. |
| 4 |Invoke-HcsSetupWizard: Waarde ligt niet binnen het verwachte bereik. |Een onjuist subnetmasker produceert deze fout. Mogelijke oorzaken zijn: <ul><li> Het subnetmasker is ontbreekt of is leeg.</li><li>De Ipv6-voorvoegselindeling is onjuist.</li><li>De interface is ingeschakeld voor de cloud, maar de gateway is ontbreekt of is onjuist.</li></ul>Houd er rekening mee dat de DATA 0 automatisch ingeschakeld voor de cloud is als geconfigureerd via de wizard setup. |Subnet 0.0.0.0 of 256.256.256.256 gebruiken om te bepalen van het probleem, en zoek vervolgens naar de uitvoer. Geef de juiste waarden voor het subnetmasker, gateway en Ipv6-voorvoegsel, indien nodig. |

## <a name="error-codes"></a>Foutcodes
Fouten worden in de juiste volgorde weergegeven.

| Foutnummer | Fouttekst of beschrijving | Aanbevolen in te grijpen |
|:--- |:--- |:--- |
| 10502 |Er is een fout opgetreden tijdens het openen van uw opslagaccount. |Wacht een paar minuten en probeer het opnieuw. Als de fout zich blijft voordoen, neemt u contact opnemen met Microsoft ondersteuning voor de volgende stappen. |
| 40017 |De back-upbewerking is mislukt omdat een volume dat is opgegeven in de back-upbeleid niet op het apparaat gevonden is. |Probeer de back-up-bewerking, als de fout zich blijft voordoen, neem contact op met Microsoft Support. voor de volgende stappen. |
| 40018 |De back-upbewerking is mislukt omdat geen van de volumes die zijn opgegeven in het back-upbeleid op het apparaat is gevonden. |Probeer de back-up-bewerking, als de fout zich blijft voordoen, neem contact op met Microsoft Support. voor de volgende stappen. |
| 390061 |Het systeem is bezet of niet beschikbaar. |Wacht een paar minuten en probeer het opnieuw. Als de fout zich blijft voordoen, neemt u contact opnemen met Microsoft ondersteuning voor de volgende stappen. |
| 390143 |Er is een fout opgetreden met foutcode 390143. (Onbekende fout.) |Als de fout zich blijft voordoen, neem contact met Microsoft Support voor de volgende stappen. |

## <a name="next-steps"></a>Volgende stappen
Als u het probleem op te lossen niet [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) voor hulp. 

[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
