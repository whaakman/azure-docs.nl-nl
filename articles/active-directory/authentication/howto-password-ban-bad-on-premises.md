---
title: Azure AD-wachtwoord beveiliging preview implementeren
description: De evaluatieversie van Azure AD wachtwoord beveiliging voor het onjuiste wachtwoorden lokale bEen implementeren
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 6fda373f832d6e24d1252587a19c88b0f464dda6
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232313"
---
# <a name="preview-deploy-azure-ad-password-protection"></a>Voorbeeld: Azure AD-wachtwoordbeveiliging implementeren

|     |
| --- |
| Azure AD-wachtwoordbeveiliging en de lijst met aangepaste verboden wachtwoorden zijn openbare preview-functies van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksvoorwaarden voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Nu dat we een goed begrip van hebben [hoe af te dwingen van Azure AD-wachtwoordbeveiliging voor Windows Server Active Directory](concept-password-ban-bad-on-premises.md), de volgende stap is het plannen en uitvoeren van de implementatie.

## <a name="deployment-strategy"></a>Implementatiestrategie

Microsoft raadt aan dat alle implementatie in de controlemodus worden gestart. De controlemodus is de eerste standaardinstelling waarbij wachtwoorden kunnen worden voortgezet moet worden ingesteld en die wordt geblokkeerd bij het maken van vermeldingen in het gebeurtenislogboek. Zodra de proxyserver (s) en DC-agents zijn volledig geïmplementeerd in de controlemodus, regular bewaking moet worden gedaan om te bepalen welke impact wachtwoordbeleid afdwingen zou hebben voor gebruikers en de omgeving als het beleid is afgedwongen.

Tijdens de fase audit vinden veel organisaties:

* Ze moeten naar een bestaande operationele processen voor het gebruik van veiligere wachtwoorden verbeteren.
* Gebruikers worden vertrouwd te maken met niet-beveiligde wachtwoorden regelmatig kiezen
* Ze moeten informeren over gebruikers over toekomstige wijzigingen in de beveiliging afdwingen, de impact mogelijk hebben op de systemen en ze beter te begrijpen hoe ze beter beveiligde wachtwoorden kunnen kiezen.

Zodra de functie actief is geweest in de controlemodus voor een redelijke tijd, de configuratie voor uitvoering kan worden gespiegeld van **Audit** naar **afdwingen** waarbij vereist meer veilige wachtwoorden. Gerichte bewaking gedurende deze tijd is een goed idee.

## <a name="known-limitation"></a>Bekende beperking

Er is een bekende beperking in de preview-versie van de Azure AD-wachtwoord beveiliging proxy. Gebruik van accounts van pachterbeheerders waarvoor MFA wordt niet ondersteund. Een toekomstige update van de Azure AD-wachtwoord beveiliging proxy ondersteunt proxyregistratie met administrator-accounts die MFA is vereist.

## <a name="single-forest-deployment"></a>Implementatie met één forest

De evaluatieversie van Azure AD-wachtwoordbeveiliging wordt geïmplementeerd met de proxy-service op maximaal twee servers en de DC-agent-service kan incrementeel worden toegepast op alle domeincontrollers in het Active Directory-forest.

![Hoe Azure AD-wachtwoord beveiliging onderdelen samenwerken.](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="download-the-software"></a>De software downloaden

Er zijn twee vereiste installatieprogramma's voor Azure AD-wachtwoordbeveiliging die kan worden gedownload vanaf de [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=57071)

### <a name="install-and-configure-the-azure-ad-password-protection-proxy-service"></a>Installeren en configureren van de proxy-service van Azure AD wachtwoord beveiliging

1. Kies een of meer servers voor het hosten van de proxy-service van Azure AD wachtwoord beveiliging.
   * Elke dergelijke service alleen wachtwoordbeleid biedt voor één forest en de hostmachine moet zijn met het domein is gekoppeld aan een domein (en onderliggende worden beide evenveel ondersteund) in dat forest. Voor Azure AD wachtwoord beveiliging proxy-service om te voldoen aan de missie moet bestaan netwerkverbinding tussen ten minste één domeincontroller in elk domein van het forest en de Azure AD wachtwoord beveiliging Proxy hostmachine.
   * Als u wilt installeren en uitvoeren van de proxy-service van Azure AD wachtwoord beveiliging op een domeincontroller voor testdoeleinden wordt ondersteund, maar vervolgens verbinding met internet vereist.

   > [!NOTE]
   > De openbare preview ondersteunt maximaal twee (2) proxyservers per forest.

2. Installeer de software wachtwoord beleid Proxy-Service is met het AzureADPasswordProtectionProxy.msi MSI-pakket.
   * De software-installatie hoeft niet opnieuw worden opgestart. De software-installatie kan worden geautomatiseerd met behulp van de MSI standaardprocedures, bijvoorbeeld: `msiexec.exe /i AzureADPasswordProtectionProxy.msi /quiet /qn`

3. Open een PowerShell-venster als beheerder.
   * De Azure AD-wachtwoordbeveiliging Proxy-software bevat een nieuwe PowerShell-module met de naam AzureADPasswordProtection. De volgende stappen zijn gebaseerd op verschillende cmdlets van deze PowerShell-module worden uitgevoerd en wordt ervan uitgegaan dat u hebt een nieuwe PowerShell-venster wordt geopend en de nieuwe module als volgt zijn geïmporteerd:
      * `Import-Module AzureADPasswordProtection`

      > [!NOTE]
      > De hostmachine PSModulePath omgevingsvariabele Hiermee wijzigt u de van installatiesoftware. Om deze wijziging wilt doorvoeren, zodat de AzureADPasswordProtection powershell-module kan worden geïmporteerd en gebruikt, moet u wellicht een nieuwe PowerShell-consolevenster openen.

   * Controleer of de service wordt uitgevoerd met de volgende PowerShell-opdracht: `Get-Service AzureADPasswordProtectionProxy | fl`.
      * Het resultaat moet een resultaat produceren de **Status** een 'Actief' resultaat te retourneren.

4. De proxy registreren.
   * Nadat u stap 3 is voltooid de proxy-service van Azure AD wachtwoord beveiliging op de machine wordt uitgevoerd, maar nog niet de vereiste referenties om te communiceren met Azure AD. Registratie bij Azure AD is vereist om in te schakelen die mogelijkheid met behulp van de `Register-AzureADPasswordProtectionProxy` PowerShell-cmdlet. De cmdlet moet hoofdbeheerder zijn referenties voor uw Azure-tenant, evenals lokale Active Directory-domein administrator-bevoegdheden in het foresthoofddomein. Zodra deze is voltooid voor een opgegeven proxy-service, extra aanroepen van `Register-AzureADPasswordProtectionProxy` voortgezet, maar zijn niet nodig.
      * De cmdlet kan als volgt worden uitgevoerd:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionProxy -AzureCredential $tenantAdminCreds
         ```

         In het voorbeeld werkt alleen als de momenteel aangemelde gebruiker ook een beheerder van de Active Directory-domein voor het hoofddomein. Een alternatief is de benodigde domeinreferenties opgeven via de `-ForestCredential` parameter.

   > [!TIP]
   > Mogelijk zijn er een aanzienlijke vertraging (veel seconden) de eerste keer dat deze cmdlet wordt uitgevoerd voor een bepaalde Azure-tenant voordat de uitvoering door de cmdlet is voltooid. Tenzij er een fout wordt gerapporteerd. deze vertraging mogen niet worden beschouwd als verontrustend.

   > [!NOTE]
   > De registratie van de proxy-service van Azure AD wachtwoord beveiliging wordt verwacht een eenmalige stap in de levensduur van de service. De proxy-service voert automatisch andere benodigde maintainenance vanaf dit punt en hoger. Zodra deze is voltooid voor een opgegeven forest extra aanroepen van 'Register AzureADPasswordProtectionProxy' Doorgaan zou lukken, maar zijn niet nodig.

5. Registreer het forest.
   * De lokale Active Directory-forest moet worden geïnitialiseerd met de vereiste referenties om te communiceren met het gebruik van Azure de `Register-AzureADPasswordProtectionForest` Powershell-cmdlet. De cmdlet moet hoofdbeheerder zijn referenties voor uw Azure-tenant, evenals lokale Active Directory-domein administrator-bevoegdheden in het foresthoofddomein. Deze stap wordt één keer per forest uitgevoerd.
      * De cmdlet kan als volgt worden uitgevoerd:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $tenantAdminCreds
         ```

         In het voorbeeld werkt alleen als de momenteel aangemelde gebruiker ook een beheerder van de Active Directory-domein voor het hoofddomein. Een alternatief is de benodigde domeinreferenties opgeven via de parameter - ForestCredential.

         > [!NOTE]
         > Als meerdere proxyservers zijn geïnstalleerd in uw omgeving, maakt het niet uit welke specifieke proxyserver de bovenstaande bovenstaande procedure wordt uitgevoerd bij.

         > [!TIP]
         > Mogelijk zijn er een aanzienlijke vertraging (veel seconden) de eerste keer dat deze cmdlet wordt uitgevoerd voor een bepaalde Azure-tenant voordat de uitvoering door de cmdlet is voltooid. Tenzij er een fout wordt gerapporteerd. deze vertraging mogen niet worden beschouwd als verontrustend.

   > [!NOTE]
   > De registratie van het Active Directory-forest is een eenmalige stap verwacht in de levensduur van het forest. De domain controller agents in het forest met voert automatisch andere benodigde maintainenance vanaf dit punt en hoger. Zodra deze is voltooid voor een opgegeven forest is aanvullende aanroepen van `Register-AzureADPasswordProtectionForest` voortgezet, maar zijn niet nodig.

6. Optioneel: De Azure AD wachtwoord beveiliging proxy-service om te luisteren op een specifieke poort configureren.
   * RPC via TCP wordt gebruikt door de Azure AD-wachtwoordbeveiliging DC agentsoftware op de domeincontrollers om te communiceren met de proxy-service van Azure AD wachtwoord beveiliging. Standaard wordt de Azure AD-wachtwoordbeveiliging wachtwoord beleid Proxy-service luistert op alle beschikbare dynamische RPC-eindpunt. Indien nodig vanwege netwerktopologie of firewallvereisten, kan de service in plaats daarvan worden geconfigureerd om op een specifieke TCP-poort te luisteren.
      * Om de service worden uitgevoerd onder een statische poort configureren, gebruiken de `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet.
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > U moet stoppen en opnieuw starten van de service om deze wijzigingen van kracht te laten worden.

      * Voor het configureren van de service voor uitvoering van een dynamische poort gebruikt dezelfde procedure maar StaticPort opnieuw ingesteld op nul, als volgt te werk:
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > U moet stoppen en opnieuw starten van de service om deze wijzigingen van kracht te laten worden.

   > [!NOTE]
   > De proxy-service van Azure AD wachtwoord beveiliging vereist handmatig opnieuw worden gestart na elke wijziging in de poortconfiguratie. Het is niet opnieuw opstarten van DC-agentsoftware voor de service uitgevoerd op domeincontrollers nadat u deze wijzigingen in de configuratie van deze aard.

   * De huidige configuratie van de service worden opgevraagd met de `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet zoals het volgende voorbeeld:

      ```
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0 
      ```

### <a name="install-the-azure-ad-password-protection-dc-agent-service"></a>Installeer de agent-service van Azure AD wachtwoord beveiliging DC

* Installeer de Azure AD wachtwoord beveiliging DC agent service software met de `AzureADPasswordProtectionDCAgent.msi` MSI-pakket:
   * De software-installatie vereisen een opnieuw opstarten op installeren en verwijderen als gevolg van het besturingssysteemvereiste dat wachtwoord filter-dll's alleen worden geladen of verwijderd na het opnieuw opstarten.
   * Het wordt ondersteund voor het installeren van de DC-agent-service op een machine dat nog niet een domeincontroller. In dit geval de service wordt gestart en wordt uitgevoerd, maar wordt anders worden inactieve pas nadat de computer wordt gepromoveerd tot een domeincontroller.

   De software-installatie kan worden geautomatiseerd met behulp van de MSI standaardprocedures, bijvoorbeeld:  `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`

   > [!WARNING]
   > De opdracht msiexec leidt ertoe dat onmiddellijk opnieuw opstarten; Dit kan worden voorkomen door te geven de `/norestart` vlag.

Eenmaal geïnstalleerd op een domeincontroller en opnieuw opgestart, wordt de Azure AD-wachtwoordbeveiliging DC Agent software-installatie is voltooid. Er is geen andere configuratie vereist of mogelijk.

## <a name="multiple-forest-deployments"></a>Implementaties met meerdere forests

Er zijn geen extra vereisten voor het implementeren van Azure AD-wachtwoordbeveiliging voor meerdere forests. Elk forest is onafhankelijk geconfigureerd zoals beschreven in de sectie van de implementatie van één forest. Elke Azure AD-wachtwoordbeveiliging Proxy biedt slechts ondersteuning voor domeincontrollers in het forest dat deze is gekoppeld aan. De Azure AD-wachtwoord virussoftware in een opgegeven forest is niet op de hoogte van Azure AD wachtwoord beveiliging software geïmplementeerd in een ander forest ongeacht configuraties van Active Directory-vertrouwensrelatie.

## <a name="read-only-domain-controllers"></a>Alleen-lezen domeincontrollers

Wachtwoord changes\sets worden nooit verwerkt en opgeslagen op alleen-lezen domeincontrollers (RODC's); Deze worden in plaats daarvan doorgestuurd naar beschrijfbare domeincontrollers. Er is daarom niet nodig de DC-agent om software te installeren op de RODC's.

## <a name="high-availability"></a>Hoge beschikbaarheid

De belangrijkste factor met hoge beschikbaarheid van de Azure AD-wachtwoordbeveiliging is de beschikbaarheid van de proxy-servers als domeincontrollers in een forest probeert te downloaden van nieuwe beleidsregels of andere gegevens van Azure. De openbare preview ondersteunt maximaal twee proxyservers per forest. Elke agent DC gebruikt een eenvoudige round robin-stijl-algoritme wanneer u beslist welke proxyserver aanroep en slaat het via de proxy-servers die niet reageren.
De gebruikelijke problemen in verband met hoge beschikbaarheid worden door het ontwerp van de DC-agentsoftware verholpen. De DC-agent onderhoudt een lokale cache van het meest recent gedownloade wachtwoordbeleid. Zelfs als alle proxyservers voor een of andere reden niet meer beschikbaar zijn geregistreerd, blijven de DC-agents hun in de cache wachtwoordbeleid afdwingen. Een redelijke updatefrequentie wachtwoordbeleid in een grote implementatie is meestal op de volgorde van niet meer in uren dagen of minder.   Daarom opnieuw korte uitval van de proxyservers niet aanzienlijke gevolgen voor de werking van de functie voor Azure AD wachtwoordbeveiliging of de voordelen van de beveiliging.

## <a name="next-steps"></a>Volgende stappen

Nu dat u de services die vereist zijn voor beveiliging van Azure AD-wachtwoord op uw on-premises servers hebt geïnstalleerd, voltooi de [installeren na configuratie en de rapportage-informatie verzamelen](howto-password-ban-bad-on-premises-operations.md) om uw implementatie te vervolledigen.

[Conceptueel overzicht van Azure AD-wachtwoordbeveiliging](concept-password-ban-bad-on-premises.md)
