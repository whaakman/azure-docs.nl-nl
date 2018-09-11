---
title: Windows Defender Advanced Threat Protection (ATP) met Azure Security Center (openbare preview) | Microsoft Docs
description: Dit document beschrijft de integratie tussen Azure Security Center en Windows Defender ATP.
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
ms.date: 09/06/2018
ms.author: barclayn
ms.openlocfilehash: f2cb4edd469d76f79e4134ca261bac5263bf3ce4
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296054"
---
# <a name="windows-defender-advanced-threat-protection-atp-with-azure-security-center-public-preview"></a>Windows Defender Advanced Threat Protection (ATP) met Azure Security Center (preview-versie)

Azure Security Center is het aanbod voor Cloud Workload Protection Platforms (CWPP) uitbreiden door te integreren met [Windows Defender ATP](https://www.microsoft.com/WindowsForBusiness/windows-atp).
Deze wijziging heeft uitgebreide mogelijkheden voor eindpunt detectie en reactie (EDR). Hiermee kunt u afwijkingen herkennen, detecteren en reageren op geavanceerde aanvallen op de servereindpunten bewaakt door ASC.

Azure Security Center-klanten kunnen nu gebruikmaken van Windows Defender ATP-functies:

- **Volgende generatie boeken schending detectie sensoren:** Windows Defender ATP-sensor voor Windows-Servers die worden verzameld van een groot scala aan gedragsalgoritmen signalen om in te schakelen van geavanceerde aanvallen detectie en onderzoek.

- **Detectie van inbreuk op Analytics gebaseerde, met behulp van de cloud-bericht:** Windows Defender ATP aangepast snel veranderende bedreigingen. Het maakt gebruik van geavanceerde analyses en big data. Het wordt versterkt door de kracht van de Intelligent Security Graph met signalen voor Windows, Azure en Office voor het detecteren van onbekende bedreigingen. Het biedt praktische waarschuwingen en kunt u snel reageren.

- **Bedreigingsinformatie**: Windows Defender ATP identificeert aanvaller hulpprogramma's, technieken en procedures en genereert waarschuwingen wanneer deze worden waargenomen. Het maakt gebruik van gegevens die worden gegenereerd door Microsoft jagers, beveiligingsteams, en wordt aangevuld door bedreigingsinformatie geleverd door partners.

Deze mogelijkheden zijn nu beschikbaar in Azure Security Center:

- Automatische onboarding - Windows Defender ATP-sensor is automatisch ingeschakeld op Windows-Servers die toegevoegd aan ASC zijn

- Enkel glazen in ASC - Windows Defender ATP-waarschuwingen zijn beschikbaar in de ASC-console

- Gedetailleerd onderzoek van de machine - ASC-klanten toegang tot Windows Defender ATP console gedetailleerd onderzoek uitvoert om bereik van schending ontdekken

![* Afbeelding 1 ziet de volledige afbeelding tijdens onderzoeken met inbegrip van waarschuwingen die zijn gegenereerd door ASC *](media/security-center-wdatp/image1.png)

U kunt de waarschuwing in Azure Security Center onderzoeken:

![Onderzoek in afbeelding 2 - Azure Security Center](media/security-center-wdatp/image2.png)

U kunt de waarschuwing verder onderzoeken door draaien op Windows Defender ATP. U ziet er aanvullende informatie zoals een waarschuwing processtructuur incident grafiek en een gedetailleerde op machine tijdlijn weergeven van alle van de problemen voor een historische periode van maximaal zes maanden.

![Afbeelding 3 onderzoek: Windows Defender ATP](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Platformondersteuning

Deze functie biedt ondersteuning voor detectie op Windows Server 2012 R2 en Windows Server 2016.

Alleen servers in abonnementen in de Standard-laag

## <a name="onboarding-instructions"></a>Instructies voor onboarding

- Als u al de servers aan de standaardlaag ASC - er is geen actie vereist, onboarding ASC automatisch onboarding wordt WDATP-servers.

- Als u nooit onboarding de ASC (Standard)-servers laag – vrijgeven aan ASC zoals gebruikelijk.

- Als u een onboarding uitgevoerd de servers via WDATP:
  - Raadpleeg de documentatie voor hulp op [hoe u server-machines niet meer vrijgeven](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Onboarding van ASC

## <a name="access-to-wdatp-portal"></a>Toegang tot WDATP-portal

Volg de instructies op [gebruikerstoegang toewijzen aan de portal](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/assign-portal-access-windows-defender-advanced-threat-protection)

## <a name="set-firewall-configuration"></a>Set firewall-configuratie

Als u hebt een proxy of firewall die door anonieme verkeer wordt geblokkeerd als Windows Defender ATP-sensor verbinding vanaf de systeemcontext gemaakt wordt, controleert u of anoniem verkeer is toegestaan; Volg de instructies [toegang tot Windows Defender ATP-service-URL's in de proxy-server inschakelen](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-proxy-internet-windows-defender-advanced-threat-protection#enable-access-to-windows-defender-atp-service-urls-in-the-proxy-server)

## <a name="how-can-i-test-the-feature"></a>Hoe kan ik de functie testen?

 De volgende stappen wordt een waarschuwing voor een goedaardig WDATP test gegenereerd.

1. Verbinding met een van de Windows Server VM's (2012R2 of 2016) in het abonnement en open een opdrachtpromptvenster

2. Na de prompt, kopiëren en voer de onderstaande opdracht uit. Houd er rekening mee dat het opdrachtpromptvenster automatisch wordt gesloten.

    **PowerShell.exe - NoExit - ExecutionPolicy Bypass - vensterstijl verborgen (New-Object System.Net.WebClient). DownloadFile ('http://127.0.0.1/1.exe', ' C:\\test-WDATP-test\\invoice.exe "); Start het proces ' C:\\test-WDATP-test\\invoice.exe' **

  ![afbeelding van de opdracht hierboven in een opdrachtpromptvenster](media/security-center-wdatp/image4.jpeg)

3. Als dit lukt, wordt een nieuwe waarschuwing wordt weergegeven in een paar minuten in ASC en Word ATP-portal.

4. Controleer de melding in Azure Security Center, gaat u naar **beveiligingswaarschuwingen -\> verdachte Powershell-opdrachtregel**

5. Van het onderzoek venster Klik op de koppeling om te leiden naar WDATP-portal

## <a name="next-steps"></a>Volgende stappen

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
