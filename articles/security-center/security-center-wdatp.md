---
title: Windows Defender Advanced Threat Protection met Azure Security Center
description: Dit document beschrijft de integratie tussen Azure Security Center en Windows Defender Advanced Threat Protection.
services: security-center
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2018
ms.author: barclayn
ms.openlocfilehash: b73bf3e542801e58fbda6232eb063305785ce643
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53536267"
---
# <a name="windows-defender-advanced-threat-protection-with-azure-security-center"></a>Windows Defender Advanced Threat Protection met Azure Security Center

Azure Security Center is het aanbod voor Cloud Workload Protection platformen uitbreiden door te integreren met [Windows Defender Advanced Threat Protection](https://www.microsoft.com/en-us/WindowsForBusiness/windows-atp) (ATP).
Deze wijziging heeft uitgebreide mogelijkheden voor eindpunt detectie en reactie (EDR). Met Windows Defender ATP-integratie, kunt u afwijkingen herkennen. U kunt ook detecteren en reageren op geavanceerde aanvallen op de servereindpunten bewaakt door Azure Security Center.

Azure Security Center-klanten kunnen nu gebruikmaken van functies van Windows Defender ATP:

- **Volgende generatie post inbreuk detectie sensoren**: Windows Defender ATP sensoren voor Windows-servers verzamelen een groot scala aan gedragsalgoritmen signalen.

- **Detectie van Analytics gebaseerde, met behulp van de cloud post inbreuk**: Windows Defender ATP aangepast snel veranderende bedreigingen. Het maakt gebruik van geavanceerde analyses en big data. Windows Defender ATP wordt versterkt door de kracht van de Intelligent Security Graph met signalen voor Windows, Azure en Office voor het detecteren van onbekende bedreigingen. Het biedt praktische waarschuwingen en kunt u snel reageren.

- **Bedreigingsinformatie**: Windows Defender ATP geeft aanvaller hulpprogramma's, technieken en procedures. Wanneer wordt deze gedetecteerd, worden waarschuwingen gegenereerd. Maakt gebruik van gegevens die zijn gegenereerd door Microsoft threat jagers en beveiligingsteams, aangevuld door intelligence geleverd door partners.

Deze mogelijkheden zijn nu beschikbaar in Azure Security Center:

- **Automatische onboarding**: De Windows Defender ATP-sensor is automatisch ingeschakeld voor Windows-servers die toegevoegd aan Azure Security Center zijn.

- **Enkel glazen**: De Azure Security Center-console bevat Windows Defender ATP-waarschuwingen.

- **Gedetailleerde machine onderzoek**: Azure Security Center-klanten hebben toegang tot Windows Defender ATP-console op een gedetailleerd onderzoek uitvoert om het bereik van een schending bloot.

![Azure Security Center, een lijst met waarschuwingen en algemene informatie over elke waarschuwing weergeven](media/security-center-wdatp/image1.png)

U kunt [de waarschuwing onderzoeken](security-center-investigation.md) in Azure Security Center:

![Het dashboard van de waarschuwing verkennen in Azure Security Center](media/security-center-wdatp/image2.png)

U kunt de waarschuwing verder onderzoeken door draaien op Windows Defender ATP. U ziet er als u meer informatie, zoals de processtructuur van de waarschuwing en de incidenten grafiek. U ziet ook een gedetailleerde op machine tijdlijn waarin elke gedrag voor een historische periode van maximaal zes maanden.

![Windows Defender ATP-pagina met gedetailleerde informatie over een waarschuwing](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Platformondersteuning

Deze functie biedt ondersteuning voor detectie op Windows Server 2012 R2 en Windows Server 2016.

Alleen servers in voor het serviceniveau standard-abonnementen worden ondersteund.

## <a name="onboarding-servers-to-security-center"></a>Onboarding-servers met Security Center 

Security Center onboarden-servers, klikt u op **gaat u naar Azure Security Center onboarden servers** van de voorbereiding van Windows Defender ATP-server.

1. In de **Onboarding** blade selecteert of maakt u een werkruimte waarin gegevens worden opgeslagen. <br>
2. Als u al uw werkruimten niet ziet, kan het worden vanwege een gebrek aan machtigingen, zorg ervoor dat uw werkruimte is ingesteld op Azure Security Standard-laag. Zie voor meer informatie [een upgrade uitvoert naar Standard van Security Center-prijscategorie voor verbeterde beveiliging](security-center-pricing.md).
    
3.  Selecteer **servers toevoegen** voor de instructies voor het installeren van de Microsoft Monitoring Agent. 

4.  Na de onboarding, kunt u de machines onder bewaken **reken- en apps**.

  ![Onboarding-computers](media/security-center-wdatp/onboard-computers.png)


## <a name="enable-windows-defender-atp-integration"></a>Windows Defender ATP-integratie inschakelen

Als u wilt weergeven als Windows Defender ATP-integratie is ingeschakeld, selecteert u **Security center** > **beveiligingsbeleid** > **abonnement**  >  **Instellingen bewerken**.

  ![Azure Security Center-beleidsbeheer](media/security-center-wdatp/policy-management.png)

Hier ziet u de integraties momenteel ingeschakeld.

  ![Azure Security Center Threat detection instellingenpagina met Windows Defender ATP-integratie ingeschakeld](media/security-center-wdatp/enable-integrations.png)

- Als u al toegevoegd de servers naar Azure Security Center standard-laag hebt, hoeft u geen verdere actie te ondernemen. Azure Security Center wordt automatisch onboarding de servers aan Windows Defender ATP. Dit kan tot 24 uur duren.

- Als u nooit onboarding de servers naar Azure Security Center standard-laag, onboarding hebt deze naar Azure Security Center als normaal.

- Als u op de servers met Windows Defender ATP onboarding hebt:
  - Raadpleeg de documentatie voor hulp op [hoe u server-machines niet meer vrijgeven](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Onboarding deze servers naar Azure Security Center.

## <a name="access-to-the-windows-defender-atp-portal"></a>Toegang tot de Windows Defender ATP-portal

Volg de instructies in [gebruikerstoegang toewijzen aan de portal](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/assign-portal-access-windows-defender-advanced-threat-protection).

## <a name="set-the-firewall-configuration"></a>De firewallconfiguratie instellen

Als u hebt een proxy of firewall die door anonieme verkeer wordt geblokkeerd als een Windows Defender ATP-sensor verbinding vanaf de systeemcontext gemaakt wordt, ervoor zorgen dat anonieme verkeer is toegestaan. Volg de instructies in [toegang tot Windows Defender ATP-service-URL's in de proxyserver](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-proxy-internet-windows-defender-advanced-threat-protection#enable-access-to-windows-defender-atp-service-urls-in-the-proxy-server).

## <a name="test-the-feature"></a>De functie testen

Voor het genereren van een goedaardig Windows Defender ATP-Testwaarschuwing:

1. Extern bureaublad gebruiken voor toegang tot een virtuele machine met Windows Server 2012 R2 of een virtuele machine met Windows Server 2016.  Open een opdrachtpromptvenster.

2. Kopieer en voer de volgende opdracht bij de prompt. De opdrachtprompt-venster wordt automatisch gesloten.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-WDATP-test\\invoice.exe'); Start-Process 'C:\\test-WDATP-test\\invoice.exe'
    ```

   ![Een opdrachtpromptvenster met de bovenstaande opdracht](media/security-center-wdatp/image4.jpeg)

3. Als de opdracht geslaagd is, ziet u een nieuwe waarschuwing op het Azure Security Center-dashboard en de Windows Defender ATP-portal. Deze waarschuwing kan worden weergegeven in een paar minuten duren.

4. Als u wilt controleren van de waarschuwing in Security Center, gaat u naar **beveiligingswaarschuwingen** >  **verdachte Powershell-opdrachtregel**.

5. Selecteer de koppeling om naar de Windows Defender ATP-portal te gaan in het venster onderzoek.

## <a name="next-steps"></a>Volgende stappen

- [Beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md): Informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
- [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md): Leer hoe aanbevelingen u helpen bij uw Azure-resources te beveiligen.
- [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): Meer informatie over het controleren van de status van uw Azure-resources.
