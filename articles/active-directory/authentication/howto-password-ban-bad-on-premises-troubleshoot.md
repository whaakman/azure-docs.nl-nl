---
title: Probleemoplossing en logboekregistratie in Azure AD-wachtwoord protection preview
description: Inzicht in Azure AD-wachtwoordbeveiliging voorbeelden van logboekregistratie en het oplossen van algemene problemen
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: e5b3dc1bfa7c7890be83529e863907ec056f188f
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295657"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Voorbeeld: Azure AD wachtwoord beveiliging bewaking, rapportage en probleemoplossing

|     |
| --- |
| Azure AD-wachtwoordbeveiliging en de lijst met aangepaste verboden wachtwoorden zijn openbare preview-functies van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksvoorwaarden voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Zijn essentiële taken na de implementatie van Azure AD-wachtwoordbeveiliging bewaking en rapportage. In dit artikel biedt informatie om u te helpen dat u inzicht hebt waarin de gegevens voor elke service logboekbestanden en rapporteren over het gebruik van Azure AD-wachtwoordbeveiliging.

## <a name="on-premises-logs-and-events"></a>On-premises logboeken en gebeurtenissen

### <a name="dc-agent-service"></a>DC-agent-service

Op elke domeincontroller wordt de DC-agentsoftware service de resultaten van de validaties wachtwoord (en een andere status) naar een lokale gebeurtenislogboek: \Applications en Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin

Gebeurtenissen worden vastgelegd door de verschillende DC agent-onderdelen met behulp van de volgende bereiken:

|Onderdeel |Gebeurtenis-ID-bereik|
| --- | --- |
|DC-Agent wachtwoord filter-dll| 10000 19999|
|Hostproces van de DC-agent-service| 20000 29999|
|Beleid validatielogica voor DC agent-service| 30000 39999|

Voor het uitvoeren van een validatie geslaagd wachtwoord er wordt doorgaans één gebeurtenis vastgelegd van de DC-agent wachtwoord filter-dll. Voor een mislukte validatie-bewerking wachtwoord, er zijn doorgaans twee gebeurtenissen die zijn vastgelegd, een van de DC-agent-service en een van de DC-Agent wachtwoord filter-dll.

Afzonderlijke gebeurtenissen voor het vastleggen van deze situaties worden vastgelegd, gebaseerd op de volgende factoren:

* Hiermee wordt aangegeven of een bepaald wachtwoord wordt ingesteld of gewijzigd.
* Hiermee wordt aangegeven of validatie van een bepaald wachtwoord doorgegeven of is mislukt.
* Hiermee wordt aangegeven of validatie is mislukt als gevolg van de Microsoft globaal beleid tegenover organisatorisch beleid.
* Hiermee wordt aangegeven of modus alleen controleren is momenteel in- of uitschakelen voor het huidige wachtwoordbeleid.

De belangrijkste wachtwoord-validatie-gerelateerde gebeurtenissen zijn als volgt:

|   |Wachtwoord wijzigen |Wachtwoord instellen|
| --- | :---: | :---: |
|Geslaagd |10014 |10015|
|Mislukt (is niet geslaagd klant wachtwoordbeleid)| 10016, 30002| 10017, 30003|
|Mislukt (Microsoft-wachtwoordbeleid niet geslaagd)| 10016, 30004| 10017, 30005|
|Alleen controle op te geven (zou niet hebben klant wachtwoordbeleid)| 10024, 30008| 10025, 30007|
|Alleen controle op te geven (zou niet hebben Microsoft wachtwoordbeleid)| 10024, 30010| 10025, 30009|

> [!TIP]
> Binnenkomende wachtwoorden worden gevalideerd met de lijst met Microsoft globale wachtwoord eerst; Als dat mislukt, wordt er is geen verdere verwerking uitgevoerd. Dit is hetzelfde gedrag uitgevoerd op wachtwoordwijzigingen in Azure.

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Voorbeeldbericht gebeurtenislogboek voor gebeurtenis-ID 10014 geslaagde wachtwoord instellen

Het gewijzigde wachtwoord voor de opgegeven gebruiker is gevalideerd als compatibel met het huidige wachtwoord van Azure-beleid.

 Gebruikersnaam: BPL_02885102771 FullName:

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Voorbeeldbericht gebeurtenislogboek voor gebeurtenis-ID 10017 en 30003 mislukt wachtwoord instellen

10017:

Het wachtwoord opnieuw instellen voor de opgegeven gebruiker is afgewezen omdat deze niet aan het huidige wachtwoord van Azure-beleid voldoet. Raadpleeg het gebeurtenislogboek van gecorreleerde bericht voor meer informatie.

 Gebruikersnaam: BPL_03283841185 FullName:

30003:

Het wachtwoord opnieuw instellen voor de opgegeven gebruiker is afgewezen omdat deze komt overeen met ten minste één van de tokens in de lijst per tenant verboden wachtwoord van het huidige wachtwoord van Azure-beleid.

 Gebruikersnaam: BPL_03283841185 FullName:

Sommige andere belangrijke gebeurtenislogboekberichten rekening houden met zijn:

#### <a name="sample-event-log-message-for-event-id-30001"></a>Voorbeeldbericht gebeurtenislogboek voor gebeurtenis-ID 30001

Het wachtwoord voor de opgegeven gebruiker is geaccepteerd, omdat een Azure-wachtwoordbeleid nog niet beschikbaar is

Gebruikersnaam: <user> FullName: <user>

Dit wordt mogelijk veroorzaakt door een of meer van de volgende redenen: % n

1. Het forest is nog niet geregistreerd bij Azure.

   Stappen voor het oplossen: een beheerder moet het forest met de cmdlet Register-AzureADPasswordProtectionForest registreren.

2. Een Azure AD-wachtwoordbeveiliging Proxy is nog niet beschikbaar is op ten minste één computer in het huidige forest.

   Stappen voor het oplossen: een beheerder moet installeren en registreren van een proxy die met de cmdlet Register-AzureADPasswordProtectionProxy.

3. Deze domeincontroller heeft geen netwerkverbindingen met een Proxy-exemplaren van Azure AD wachtwoord beveiliging.

   Stappen voor het oplossen: Zorg ervoor dat de verbinding met het netwerk bestaat met ten minste één Azure AD-wachtwoord beveiliging Proxy exemplaar.

4. Deze domeincontroller heeft geen connectiviteit met andere domeincontrollers in het domein.

   Stappen voor het oplossen: Zorg ervoor dat er een netwerkverbinding aanwezig aan het domein.

#### <a name="sample-event-log-message-for-event-id-30006"></a>Voorbeeldbericht gebeurtenislogboek voor gebeurtenis-ID 30006

De service wordt nu het volgende Azure wachtwoordbeleid afdwingen.

 AuditOnly: 1

 Datum van globaal beleid: 2018-05-15T00:00:00.000000000Z

 Tenant-beleid datum: 2018-06-10T20:15:24.432457600Z

 Afdwingen van beleid voor tenant: 1

#### <a name="dc-agent-log-locations"></a>DC-Agent logboeklocaties

De DC-agent-service ook een logboek operationele gebeurtenissen in verband met het volgende logboekbestand: \Applications en Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational

De DC-agent-service kan zich ook aanmelden uitgebreide foutopsporing niveau traceringsgebeurtenissen naar het volgende logboek: \Applications en Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace

> [!WARNING]
> Het traceerlogboek is standaard uitgeschakeld. Wanneer dit is ingeschakeld, wordt dit logboek ontvangt van een groot aantal gebeurtenissen en mogelijk van invloed op prestaties van domeincontrollers. Daarom dit logboek verbeterde moet alleen worden ingeschakeld wanneer een probleem nadere analyse nodig en vervolgens alleen voor een minimale hoeveelheid tijd.

### <a name="azure-ad-password-protection-proxy-service"></a>Azure AD wachtwoord beveiliging proxy-service

De wachtwoordbeveiliging Proxy-service verzendt een minimale set van gebeurtenissen in het volgende gebeurtenislogboek: \Applications en Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational

De wachtwoordbeveiliging Proxy-service kan zich ook aanmelden uitgebreide foutopsporing niveau traceringsgebeurtenissen in de volgende logboek: \Applications en Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace

> [!WARNING]
> Het traceerlogboek is standaard uitgeschakeld. Wanneer dit is ingeschakeld, dit logboek ontvangt een groot aantal gebeurtenissen en dit kan invloed hebben op prestaties van de proxyhost. Daarom dit logboek moet alleen worden ingeschakeld wanneer een probleem nadere analyse nodig en vervolgens alleen voor een minimale hoeveelheid tijd.

### <a name="dc-agent-discovery"></a>Detectie van DC-Agent

De `Get-AzureADPasswordProtectionDCAgent` cmdlet kan worden gebruikt om algemene informatie over de verschillende DC-agents uitgevoerd in een domein of forest weer te geven. Deze informatie wordt opgehaald uit de serviceConnectionPoint objecten geregistreerd door de actieve DC-agent (s). Een voorbeeld van uitvoer van deze cmdlet is als volgt:

```
PS C:\> Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

De verschillende eigenschappen worden bijgewerkt door elke domeincontroller agent-service op een benadering uurbasis. De gegevens zijn nog steeds onderworpen aan replicatievertraging van Active Directory.

Het bereik van de cmdlet-query kan worden beïnvloed met behulp van de parameters Forest of -domein.

### <a name="emergency-remediation"></a>Noodgevallen herstel

Als een vervelend situatie waarbij de DC-agentservice problemen veroorzaakt optreedt, kan de DC-agent-service worden onmiddellijk afgesloten. De DC-agent wachtwoord filter-dll roept de service niet actief en waarschuwingsgebeurtenissen (10012, 10013) wordt aangemeld, maar alle binnenkomende wachtwoorden gedurende die tijd worden geaccepteerd. De DC-agent-service kan vervolgens ook worden geconfigureerd via de Windows-Service Control Manager met een opstarttype "Uitgeschakeld" indien nodig.

### <a name="performance-monitoring"></a>Prestatiebewaking

De DC-agent-service-software wordt geïnstalleerd voor een prestatiemeteritem-object met de naam **Azure AD-wachtwoordbeveiliging**. De volgende prestatiemeteritems zijn beschikbaar:

|Tellernaam Perf | Beschrijving|
| --- | --- |
|Wachtwoorden die zijn verwerkt |Deze teller geeft het totale aantal verwerkte wachtwoorden (goedgekeurd of geweigerd) sinds de laatste keer opnieuw opstarten.|
|Wachtwoorden geaccepteerd |Deze teller geeft het totale aantal wachtwoorden die zijn aanvaard sinds de laatste keer opnieuw opstarten.|
|Wachtwoorden geweigerd |Deze teller geeft het totale aantal wachtwoorden dat sinds de laatste keer opnieuw opstarten is geweigerd.|
|Wachtwoord filter aanvragen in voortgang |Deze teller geeft het aantal aanvragen voor wachtwoord filteren uitgevoerd.|
|Aanvragen voor piek wachtwoord filteren |Deze teller geeft het maximum aantal gelijktijdige wachtwoord filter aanvragen sinds de laatste keer opnieuw opstarten.|
|Wachtwoord filter aanvraag fouten |Deze teller geeft het totale aantal aanvragen van de wachtwoord-filter dat is mislukt vanwege een fout sinds de laatste keer opnieuw opstarten. Fouten kunnen optreden wanneer de agent-service van Azure AD wachtwoord beveiliging DC niet wordt uitgevoerd.|
|Wachtwoord filter aanvragen per seconde |Deze teller geeft de snelheid waarmee wachtwoorden worden verwerkt.|
|Verwerkingstijd wachtwoord filter aanvraag |Deze teller geeft de gemiddelde tijd die nodig is om een wachtwoord filteraanvraag te verwerken.|
|Wachtwoord filter aanvraag verwerking piektijden |Deze teller geeft het piek wachtwoord filter verwerken van aanvragen tijd sinds de laatste keer opnieuw opstarten.|
|Wachtwoorden worden geaccepteerd als gevolg van de controlemodus |Deze teller geeft het totale aantal wachtwoorden die zouden normaal geweigerd, maar zijn geaccepteerd omdat het wachtwoordbeleid is geconfigureerd om te worden in de controlemodus (sinds de laatste keer opnieuw opstarten).|

## <a name="directory-services-repair-mode"></a>Directory Services Repair Mode.

Als de domeincontroller wordt opgestart naar Directory Services Repair Mode, detecteert de DC-agentservice dit leidt tot alle wachtwoordvalidatie of activiteiten worden uitgeschakeld, ongeacht de configuratie van de momenteel actieve beleid afdwingen.

## <a name="domain-controller-demotion"></a>Degradatie van domeincontrollers

Als u een domeincontroller waarop de software van DC-agent nog steeds wordt ondersteund. Beheerders moeten zich op de hoogte echter dat de DC-agentsoftware blijft actief en blijft het afdwingen van beleid voor de huidige tijdens de degradatie procedure. Het nieuwe lokale beheerderswachtwoord (opgegeven als onderdeel van het degraderen) wordt zoals elk ander wachtwoord gevalideerd. Microsoft raadt aan dat veilige wachtwoorden voor lokale Administrator-accounts worden gekozen als onderdeel van een DC degradatie procedure; de validatie van het nieuwe lokale beheerderswachtwoord op de DC-agentsoftware mogelijk echter verstoren aan vooraf bestaande degradatie operationele procedures.
Zodra de degradatie is voltooid en de domeincontroller opnieuw is opgestart en wordt opnieuw uitgevoerd als een normale lidserver, wordt de DC-agentsoftware in een passieve modus uitgevoerd. Deze kan vervolgens worden verwijderd op elk gewenst moment.

## <a name="removal"></a>Verwijdering

Als wordt besloten de openbare preview-software en het opruimen van alle gerelateerde status verwijderen van de domeinen en forests, kan deze taak worden uitgevoerd met behulp van de volgende stappen uit:

> [!IMPORTANT]
> Het is belangrijk dat u deze stappen in volgorde uitvoert. Als een willekeurig exemplaar van de wachtwoordbeveiliging Proxy-service wordt wordt uitgevoerd regelmatig opnieuw maken van het serviceConnectionPoint object evenals periodiek de sysvol-status opnieuw te maken.

1. Verwijder de wachtwoordbeveiliging proxysoftware van alle machines. Deze stap komt **niet** vereisen een opnieuw opstarten.
2. Verwijder de DC-agentsoftware van alle domeincontrollers. Deze stap **vereist** opnieuw worden opgestart.
3. Verwijder handmatig alle proxy serviceverbindingspunten in elke domeinnaamgevingscontext. De locatie van deze objecten kan worden gedetecteerd met de volgende Active Directory Powershell-opdracht:
   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{EBEFB703-6113-413D-9167-9F8DD4D24468}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -eq $keywords }
   ```

   Voer het sterretje niet weglaten (' * ') aan het einde van de variabele waarde $keywords.

   Het resulterende-object gevonden de `Get-ADObject` opdracht kan worden doorgesluisd naar `Remove-ADObject`, of handmatig verwijderd. 

4. Verwijder handmatig alle verbindingspunten van DC-agent in elke domeinnaamgevingscontext. Mogelijk zijn er een deze objecten per domeincontroller in het forest, afhankelijk van hoe ver de openbare preview-software is geïmplementeerd. De locatie van dat object kan worden gedetecteerd met de volgende Active Directory Powershell-opdracht:

   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{B11BB10A-3E7D-4D37-A4C3-51DE9D0F77C9}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -eq $keywords }
   ```

   Het resulterende-object gevonden de `Get-ADObject` opdracht kan worden doorgesluisd naar `Remove-ADObject`, of handmatig verwijderd.

5. Verwijder handmatig de configuratiestatus van forest-niveau. De status van de forest-configuratie wordt bijgehouden in een container in de configuratienaamgevingscontext van Active Directory. Het kan worden gedetecteerd en verwijderd als volgt:

   ```
   $passwordProtectonConfigContainer = "CN=Azure AD password protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject $passwordProtectonConfigContainer
   ```

6. Handmatig verwijderen verwijder dan alle sysvol status door handmatig van de volgende map en alle bijbehorende inhoud:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Indien nodig, kan dit pad ook worden geopend lokaal op een bepaalde domeincontroller. de standaardlocatie is ongeveer het volgende pad:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Dit pad is anders als de sysvol-share is geconfigureerd in een niet-standaard-locatie.

## <a name="next-steps"></a>Volgende stappen

Zie het artikel voor meer informatie over de globale en aangepaste verboden wachtwoordenlijsten [bEen onjuiste wachtwoorden](concept-password-ban-bad.md)
