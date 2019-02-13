---
title: On-premises Azure AD-wachtwoord Protection agent versiegeschiedenis van release
description: Versie van documenten en het gedrag wijzigingsoverzicht
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8897651c963b0036bc2ac3d8cdb06a52d6f52ba1
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188032"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Preview:  Versiegeschiedenis van Azure AD wachtwoord Protection agent

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="12650"></a>1.2.65.0

Uitgebracht op: 2/1/2019

Wijzigingen in:

* DC-agent en proxy-service worden nu ondersteund op de Server Core. Vereisten voor het besturingssysteem Mininimum zijn ongewijzigd ten opzichte van vóór: Windows Server 2012 voor DC-agents en Windows Server 2012 R2 voor proxy's.
* De Register-AzureADPasswordProtectionProxy en registreer AzureADPasswordProtectionForest cmdlets bieden nu ondersteuning voor Azure verificatiemodi op basis van apparaat-code.
* De cmdlet Get-AzureADPasswordProtectionDCAgent negeert vervormde en/of ongeldige serviceaansluitpunten. Hiermee de bug opgelost waarbij domeincontrollers zou soms weergegeven meerdere keren in de uitvoer.
* De cmdlet Get-AzureADPasswordProtectionSummaryReport negeert vervormde en/of ongeldige serviceaansluitpunten. Hiermee de bug opgelost waarbij domeincontrollers zou soms weergegeven meerdere keren in de uitvoer.
* De Proxy powershell-module is nu geregistreerd van % ProgramFiles%\WindowsPowerShell\Modules. Van de machine PSModulePath omgevingsvariabele is niet meer gewijzigd.
* Een nieuwe Get-AzureADPasswordProtectionProxy cmdlet is toegevoegd om u te helpen bij het detecteren van geregistreerde proxy's in een forest of domein.
* De DC-agent maakt gebruik van een nieuwe map in de sysvol-share voor het repliceren van beleid voor wachtwoorden en andere bestanden.

   Oude locatie van map:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Nieuwe locatie:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   (Deze wijziging is aangebracht om te voorkomen dat waarschuwingen fout-positieve "zwevende groepsbeleidsobject").

   > [!NOTE]
   > Er is geen migratie of delen van gegevens wordt uitgevoerd tussen de oude map en de nieuwe map. Oudere versies van de DC-agent wordt echter ook doorgaan met de oude locatie totdat bijgewerkt naar deze versie of hoger. Nadat u alle DC-agents worden uitgevoerd op versie 1.2.65.0 of hoger, de oude sysvol-map handmatig kan worden verwijderd.

* De DC-agent en proxy-service wordt nu detecteren en vervormde kopieën van hun respectieve serviceverbindingspunten verwijderen.
* Elke agent DC wordt periodiek vervormde en verouderde serviceverbindingspunten in het domein, voor beide DC-agent en -proxy serviceverbindingspunten verwijderd. Beide DC-agent en -proxy serviceverbindingspunten worden beschouwd als verlopen zijn als de heartbeat-tijdstempel ouder dan zeven dagen is.
* De DC-agent wordt nu vernieuwen van het forest-certificaat naar behoefte.
* De Proxy-service wordt nu de proxy-certificaat vernieuwen, indien nodig.
* Updates voor de algoritme voor gegevensvalidatie gebruikt wachtwoord: de lijst met uitgesloten wachtwoorden globale en de lijst met uitgesloten wachtwoorden van klantspecifieke (indien geconfigureerd) voordat u wachtwoord validaties worden gecombineerd. Een opgegeven wachtwoord kan nu worden afgewezen (mislukt of alleen controle) als deze tokens in de globale en klantspecifieke lijst bevat. De event log-documentatie is bijgewerkt om te weerspiegelen. Raadpleeg [Monitor Azure AD-wachtwoordbeveiliging](howto-password-ban-bad-on-premises-monitor.md).
* Prestaties en robuustheid oplossingen
* Dankzij de verbeterde logboekregistratie

> [!WARNING]
> Beperkte functionaliteit: de DC-agent-service in deze release (1.2.65.0) wordt stoppen met het verwerken van aanvragen voor wachtwoord validatie vanaf 1 September-2019.  DC-agentservices in eerdere releases (Zie hieronder) wordt niet meer verwerkt vanaf 1 juli-2019. De DC-agent-service in alle versies wordt 10021 gebeurtenissen vastleggen in het gebeurtenislogboek van de beheerder in de twee maanden aanloop deze deadlines. Alle beperkingen van de termijn wordt verwijderd in de komende GA-versie. De Proxy-agent-service is niet gedurende beperkte tijd in een willekeurige versie, maar nog steeds moet worden bijgewerkt naar de meest recente versie om te profiteren van alle volgende oplossingen voor problemen en andere verbeteringen.

## <a name="12250"></a>1.2.25.0

Uitgebracht op: 11/01/2018

Opgelost:

* DC-agent en proxy-service moet niet meer mislukken vanwege certificaatfouten vertrouwen.
* DC-agent en proxy-service beschikken over de aanvullende oplossingen voor FIPS-compatibele computers.
* Proxy-service wordt nu werken alleen goed in een TLS 1.2 alleen-lezen-netwerkomgeving.
* Secundaire prestaties en robuustheid oplossingen
* Dankzij de verbeterde logboekregistratie

Wijzigingen in:

* De vereiste minimumversie van OS-niveau voor de Proxy-service is nu Windows Server 2012 R2. De minimaal vereiste besturingssysteemniveau hebben voor de DC-agent-service blijft op Windows Server 2012.
* De Proxy-service is het nu .NET versie 4.6.2 vereist.
* Het wachtwoord validatie-algoritme maakt gebruik van een uitgebreide teken normalisatie-tabel. Dit kan leiden tot wachtwoorden worden geweigerd en die zijn aanvaard in eerdere versies.

## <a name="12100"></a>1.2.10.0

Uitgebracht op: 8/17/2018

Opgelost:

* Register-AzureADPasswordProtectionProxy en registreer AzureADPasswordProtectionForest bieden nu ondersteuning voor meervoudige verificatie
* Register-AzureADPasswordProtectionProxy vereist een WS2012 of een latere versie domeincontroller in het domein om te voorkomen dat versleutelingsfouten.
* DC-agentservice is betrouwbaarder over het aanvragen van een nieuw wachtwoordbeleid van Azure bij het opstarten.
* DC-agent-service wordt van Azure om het uur indien nodig een nieuw wachtwoordbeleid aanvragen, maar wordt nu doen op een willekeurig geselecteerde begintijd.
* DC-agent-service wordt niet langer een onbepaalde vertraging veroorzaken in nieuwe DC-aankondiging wanneer geïnstalleerd op een server vóór de promotie als een replica.
* DC-agent-service wordt nu in acht neemt de 'Wachtwoordbeveiliging op Windows Server Active Directory inschakelen' configuratie-instelling
* Beide DC-agent en -proxy installatieprogramma's ondersteunen nu in-place upgrade bij het upgraden naar toekomstige versies.

> [!WARNING]
> In-place upgrade van versie 1.1.10.3 wordt niet ondersteund en zal leiden tot een fout bij de installatie. Op een upgrade uitvoeren naar versie 1.2.10 of hoger, u moet eerst volledig verwijderen van de DC-agent en -proxy-service-software, en vervolgens de nieuwe versie installeert helemaal. Registratie van de Azure AD-wachtwoordbeveiliging Proxy-service is vereist.  Het is niet vereist voor het forest opnieuw te registreren.

> [!NOTE]
> In-place upgrades van de DC-agentsoftware moet opnieuw worden opgestart.

* DC-agent en proxy-service bieden nu ondersteuning voor die worden uitgevoerd op een server die is geconfigureerd voor het alleen gebruik compatibele FIPS-algoritmen.
* Secundaire prestaties en robuustheid oplossingen
* Dankzij de verbeterde logboekregistratie

## <a name="11103"></a>1.1.10.3

Uitgebracht op: 6/15/2018

Eerste openbare preview-versie

## <a name="next-steps"></a>Volgende stappen

[Azure AD-wachtwoord beveiliging implementeren](howto-password-ban-bad-on-premises-deploy.md)
