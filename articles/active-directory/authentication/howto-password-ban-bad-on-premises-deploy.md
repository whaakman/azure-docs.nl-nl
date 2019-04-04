---
title: Wachtwoordbeveiliging in Azure AD - Azure Active Directory implementeren
description: Azure AD-wachtwoordbeveiliging voor het blokkeren van onjuiste wachtwoorden on-premises implementeren
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
ms.openlocfilehash: f1c24ec49652cfe9105aa66fd1d5e26c81afcd14
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904624"
---
# <a name="deploy-azure-ad-password-protection"></a>Wachtwoordbeveiliging in Azure AD implementeren

Nu dat u begrijpt [hoe af te dwingen van Azure AD wachtwoordbeveiliging voor Windows Server Active Directory](concept-password-ban-bad-on-premises.md), de volgende stap is het plannen en uitvoeren van uw implementatie.

## <a name="deployment-strategy"></a>Implementatiestrategie

Het is raadzaam dat u implementaties in de controlemodus start. De controlemodus is de eerste standaardinstelling, waarbij wachtwoorden kunnen doorgaan moet worden ingesteld. Wachtwoorden die zouden worden geblokkeerd worden vastgelegd in het gebeurtenislogboek. Nadat u de proxy-servers en de DC-Agents in de controlemodus implementeert, moet u controleren dat de impact die het wachtwoordbeleid voor gebruikers en de omgeving hebben wanneer het beleid wordt afgedwongen.

Tijdens de fase audit vinden veel organisaties dat:

* Ze moeten voor het verbeteren van de bestaande operationele processen voor het gebruik van meer veilige wachtwoorden.
* Gebruikers gebruiken vaak niet-beveiligde wachtwoorden.
* Ze moeten gebruikers informeren over toekomstige wijzigingen in de afdwinging van apparaatbeveiliging, mogelijk invloed op deze en hoe beter beveiligde wachtwoorden moeten kiezen.

Nadat de functie voor een redelijke termijn in de controlemodus gestart is, kunt u de configuratie van overschakelen *controleren* naar *afdwingen* om te vereisen meer veilige wachtwoorden. Gerichte bewaking gedurende deze tijd is een goed idee.

## <a name="deployment-requirements"></a>Vereisten voor de implementatie

* Alle domeincontrollers die aan de DC-Agent service voor Windows Server 2012 moet worden uitgevoerd in Azure AD wachtwoord protection is geïnstalleerd of hoger. Deze vereiste betekent niet dat het Active Directory-domein of forest ook op Windows Server 2012-domein of forest functionele niveau zijn moet. Zoals vermeld in [ontwerpprincipes](concept-password-ban-bad-on-premises.md#design-principles), is er geen minimale DFL of FFL vereist voor een van beide de DC-agent of proxy software om uit te voeren.
* Alle machines die aan de DC-agent-service geïnstalleerd moeten .NET 4.5 geïnstalleerd hebben.
* Alle machines die aan de proxy service voor Windows Server 2012 R2 moet worden uitgevoerd in Azure AD wachtwoord protection is geïnstalleerd of hoger.
* Alle computers waarop de Azure AD-wachtwoord beveiliging Proxy-service wordt geïnstalleerd, moeten .NET 4.7 geïnstalleerd hebben.
  .NET 4.7 moet al zijn geïnstalleerd op een volledig bijgewerkte Windows-Server. Als dit niet het geval is, downloaden en uitvoeren van het installatieprogramma gevonden op [het .NET Framework 4.7 offline-installatieprogramma voor Windows](https://support.microsoft.com/en-us/help/3186497/the-net-framework-4-7-offline-installer-for-windows).
* Alle machines, met inbegrip van domeincontrollers die aan Azure AD wachtwoord beveiliging onderdelen worden geïnstalleerd moet hebben als de universele C-Runtime geïnstalleerd. U kunt de runtime ophalen door ervoor te zorgen dat u beschikt over alle updates van Windows Update. Of u deze in een specifieke update-pakket kunt ophalen. Zie voor meer informatie, [Update voor universele C Runtime in Windows](https://support.microsoft.com/help/2999226/update-for-uniersal-c-runtime-in-windows).
* Verbinding met het netwerk moet bestaan tussen ten minste één domeincontroller in elk domein en ten minste één server die als host fungeert de proxy-service voor beveiliging met een wachtwoord. Deze connectiviteit moet de domeincontroller voor toegang tot RPC-eindpunt-mapper-poort 135 en de RPC-server-poort op de proxy-service toestaan. De RPC-server-poort is standaard een dynamische RPC-poort, maar deze kan worden geconfigureerd voor [gebruik van een statische poort](#static).
* Alle machines die als host de proxy-service fungeren moeten netwerktoegang hebben tot de volgende eindpunten:

    |**Eindpunt**|**Doel**|
    | --- | --- |
    |`https://login.microsoftonline.com`|Verificatieaanvragen|
    |`https://enterpriseregistration.windows.net`|Functionaliteit van Azure AD wachtwoord beveiliging|

* Alle machines die als host de proxy-service voor beveiliging met een wachtwoord fungeren moeten worden geconfigureerd voor het toestaan van uitgaande TLS 1.2 HTTP-verkeer.
* Een globale beheerdersaccount om u te registreren van de proxy-service voor bescherming van wachtwoord en het forest met Azure AD.
* Een account met beheerdersbevoegdheden voor Active Directory-domein in het forest-hoofddomein het Windows Server Active Directory-forest registreren bij Azure AD.
* Een Active Directory-domein waarop de software voor de DC-Agent-service moet Distributed File System Replication (DFSR) gebruiken voor sysvol-replicatie.
* De Key Distribution-Service moet zijn ingeschakeld op alle domeincontrollers in het domein waarop Windows Server 2012 wordt uitgevoerd. Deze service is standaard ingeschakeld via handmatige trigger start.

## <a name="single-forest-deployment"></a>Implementatie met één forest

Het volgende diagram toont hoe de basisonderdelen van Azure AD-wachtwoordbeveiliging samenwerken in een on-premises Active Directory-omgeving.

![Hoe Azure AD wachtwoord beveiliging onderdelen samenwerken.](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

Het is een goed idee te bekijken hoe de software werkt voordat u deze implementeert. Zie [conceptueel overzicht van Azure AD-wachtwoordbeveiliging](concept-password-ban-bad-on-premises.md).

### <a name="download-the-software"></a>De software downloaden

Er zijn twee vereist installatieprogramma's voor beveiliging van Azure AD-wachtwoord. Ze zijn beschikbaar vanaf de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071).

### <a name="install-and-configure-the-proxy-service-for-password-protection"></a>Installeer en configureer de proxy-service voor beveiliging met een wachtwoord

1. Kies een of meer servers als host voor de proxy-service voor beveiliging met een wachtwoord.
   * Elke deze service biedt alleen wachtwoordbeleid voor één forest. De hostmachine moet worden toegevoegd aan een domein in het forest. Basis- en onderliggende domeinen worden beide ondersteund. U moet verbinding met het netwerk tussen ten minste één domeincontroller in elk domein van het forest en het wachtwoord beveiliging-machine.
   * U kunt de proxy-service uitvoeren op een domeincontroller voor het testen. Maar die domeincontroller vervolgens verbinding met internet, dit kan een beveiligingsprobleem is vereist. U wordt aangeraden deze configuratie alleen voor testdoeleinden.
   * U wordt aangeraden ten minste twee proxyservers voor redundantie. Zie [hoge beschikbaarheid](howto-password-ban-bad-on-premises-deploy.md#high-availability).

1. Installeer de Azure AD wachtwoord beveiliging Proxy service met de `AzureADPasswordProtectionProxySetup.exe` software-installatieprogramma.
   * De software-installatie vereist niet opnieuw worden opgestart. De software-installatie kan worden geautomatiseerd met behulp van de standaardprocedures van het MSI-bestand, bijvoorbeeld:

      `AzureADPasswordProtectionProxySetup.exe /quiet`

      > [!NOTE]
      > De service Windows Firewall moet worden uitgevoerd voordat u het pakket AzureADPasswordProtectionProxySetup.msi om te voorkomen dat een installatiefout installeert. Als Windows Firewall is geconfigureerd voor het niet wordt uitgevoerd, wordt de tijdelijke oplossing is tijdelijk inschakelen en uitvoeren van de Firewall-service tijdens de installatie. De proxysoftware heeft geen specifieke afhankelijkheid van Windows Firewall na de installatie. Als u een firewall van derden gebruikt, moet deze nog steeds worden geconfigureerd om te voldoen aan de vereisten voor de implementatie. Hieronder vallen de inkomende toegang tot poort 135 en de proxy RPC-server-poort toe te staan. Zie [implementatievereisten](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements).

1. Open een PowerShell-venster als beheerder.
   * De proxy-wachtwoord virussoftware bevat een nieuwe PowerShell-module *AzureADPasswordProtection*. De volgende stappen uit verschillende cmdlets uitvoeren vanuit deze PowerShell-module. Als volgt te werk om de nieuwe module te importeren:

      ```powershell
      Import-Module AzureADPasswordProtection
      ```

   * Als u wilt controleren of de service wordt uitgevoerd, gebruikt u de volgende PowerShell-opdracht:

      `Get-Service AzureADPasswordProtectionProxy | fl`.

     Het resultaat moet geven een **Status** "Uitvoeren".

1. Registreer de proxy.
   * Nadat stap 3 is voltooid, wordt de proxy-service wordt uitgevoerd op de machine. Maar de service heeft nog geen voor de vereiste referenties om te communiceren met Azure AD. Registratie bij Azure AD is vereist:

     `Register-AzureADPasswordProtectionProxy`

     Deze cmdlet vereist referenties van de globale beheerder voor uw Azure-tenant. U moet ook on-premises Active Directory-domein administrator-bevoegdheden in het forest-hoofddomein. Nadat u deze opdracht is geslaagd nadat voor een proxy-service, extra aanroepen van deze slaagt maar niet nodig zijn.

      De `Register-AzureADPasswordProtectionProxy` cmdlet ondersteunt de volgende drie verificatiemodi.

     * Modus voor interactieve verificatie:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > In deze modus werkt niet op Server Core-besturingssystemen. In plaats daarvan gebruikt u een van de volgende verificatiemodi. In deze modus kan ook mislukken als verbeterde beveiliging van Internet Explorer is ingeschakeld. De oplossing is om te schakelen dat de configuratie, de proxy te registreren en vervolgens weer inschakelen.

     * Apparaat-code-verificatiemodus:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
        ```

        U kunt vervolgens authenticatie voltooien door de weergegeven instructies te volgen op een ander apparaat.

     * Op de achtergrond (op basis van wachtwoorden)-verificatiemodus:

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionProxy -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > In deze modus mislukt als de Azure multi-factor Authentication is vereist. In dat geval, gebruik een van de vorige twee verificatiemodi.

       U hoeft op dit moment te geven de *- ForestCredential* parameter, die is gereserveerd voor toekomstige functionaliteit.

   De registratie van de proxy-service voor beveiliging met een wachtwoord is slechts één keer nodig in de levensduur van de service. Daarna wordt de proxy-service automatisch eventuele andere benodigde onderhoud uitvoeren.

   > [!TIP]
   > Er is mogelijk een merkbare vertraging optreden voordat de eerste keer is dat deze cmdlet wordt uitgevoerd voor een specifieke Azure-tenant is voltooid. Tenzij een fout wordt gerapporteerd, u hoeft deze vertraging.

1. Registreer het forest.
   * U moet het initialiseren van de on-premises Active Directory-forest met de benodigde gegevens om te communiceren met Azure met behulp van de `Register-AzureADPasswordProtectionForest` PowerShell-cmdlet. De cmdlet vereist referenties van de globale beheerder voor uw Azure-tenant. Het is ook vereist voor on-premises Active Directory-domein administrator-bevoegdheden in het forest-hoofddomein. Deze stap wordt één keer per forest uitgevoerd.

      De `Register-AzureADPasswordProtectionForest` cmdlet ondersteunt de volgende drie verificatiemodi.

     * Modus voor interactieve verificatie:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > In deze modus werkt niet op Server Core-besturingssystemen. Gebruik in plaats daarvan een van de volgende twee verificatiemodi. In deze modus kan ook mislukken als verbeterde beveiliging van Internet Explorer is ingeschakeld. De oplossing is om te schakelen dat de configuratie, de proxy te registreren en vervolgens weer inschakelen.  

     * Apparaat-code-verificatiemodus:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
        ```

        U kunt vervolgens authenticatie voltooien door de weergegeven instructies te volgen op een ander apparaat.

     * Op de achtergrond (op basis van wachtwoorden)-verificatiemodus:

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > In deze modus mislukt als de Azure multi-factor Authentication is vereist. In dat geval, gebruik een van de vorige twee verificatiemodi.

       Deze voorbeelden slaagt alleen als de momenteel aangemelde gebruiker ook een beheerder van Active Directory-domein voor het hoofddomein. Als dit niet het geval is, kunt u alternatieve domeinreferenties via opgeven de *- ForestCredential* parameter.

   > [!NOTE]
   > Als meerdere proxy-servers zijn geïnstalleerd in uw omgeving, maakt het niet uit welke proxyserver die u gebruikt voor het registreren van het forest.
   >
   > [!TIP]
   > Er is mogelijk een merkbare vertraging optreden voordat de eerste keer is dat deze cmdlet wordt uitgevoerd voor een specifieke Azure-tenant is voltooid. Tenzij een fout wordt gerapporteerd, u hoeft deze vertraging.

   De registratie van het Active Directory-forest is slechts één keer nodig in de levensduur van het forest. Daarna wordt de Domain Controller Agents in het forest automatisch eventuele andere benodigde onderhoud uitvoeren. Na `Register-AzureADPasswordProtectionForest` wordt uitgevoerd is voor een forest, extra aanroepen van de cmdlet slaagt, maar zijn niet nodig.

   Voor `Register-AzureADPasswordProtectionForest` slagen als ten minste één domeincontroller met WindowsServer 2012 of later moet beschikbaar zijn in het domein van de proxyserver. Maar de DC-agentsoftware hoeft te worden geïnstalleerd op een domeincontroller voordat u deze stap.

1. Configureer de proxy-service voor wachtwoordbeveiliging om te communiceren via een HTTP-proxy.

   Als uw omgeving het gebruik van een specifieke HTTP-proxy om te communiceren met Azure vereist, moet u deze methode gebruikt: Maak een *AzureADPasswordProtectionProxy.exe.config* bestand in de map %ProgramFiles%\Azure AD-wachtwoord beveiliging Proxy\Service. Zijn onder andere de volgende inhoud:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   Als uw HTTP-proxy verificatie is vereist, voegt u toe de *useDefaultCredentials* tag:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true" useDefaultCredentials="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   Vervang in beide gevallen `http://yourhttpproxy.com:8080` met het adres en poort van uw specifieke HTTP-proxyserver.

   Als uw HTTP-proxy is geconfigureerd voor ons een autorisatiebeleid, moet u toegang verlenen aan het Active Directory-computeraccount van de computer die als host fungeert voor de proxy-service voor beveiliging met een wachtwoord.

   Het is raadzaam dat u stopt en de proxyservice opnieuw starten nadat u maakt of bijwerkt de *AzureADPasswordProtectionProxy.exe.config* bestand.

   De proxy-service biedt geen ondersteuning voor het gebruik van specifieke referenties voor het verbinden met een HTTP-proxy.

1. Optioneel: Configureer de proxy-service voor wachtwoordbeveiliging om op een specifieke poort te luisteren.
   * De DC-agentsoftware hiervoor op de domeincontrollers maakt gebruik van RPC via TCP om te communiceren met de proxy-service. Standaard luistert de proxy-service op alle beschikbare dynamische RPC-eindpunt. Maar u kunt de service om te luisteren naar een specifieke TCP-poort, configureren, indien dit noodzakelijk is vanwege de netwerktopologie of firewall-vereisten in uw omgeving.
      * <a id="static" /></a>Gebruik voor het configureren van de service moet worden uitgevoerd in een statische poort de `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet.

         ```powershell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > U moet stoppen en opnieuw starten van de service voor deze wijzigingen worden doorgevoerd.

      * Gebruik dezelfde procedure voor het configureren van de service moet worden uitgevoerd in een dynamische poort, maar ingesteld *StaticPort* op nul:

         ```powershell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > U moet stoppen en opnieuw starten van de service voor deze wijzigingen worden doorgevoerd.

   > [!NOTE]
   > De proxy-service voor beveiliging met een wachtwoord vereist een handmatig opnieuw opstarten na elke wijziging in de poortconfiguratie. Maar u hoeft niet aan de software voor de DC-Agent-service op domeincontrollers opnieuw worden opgestart nadat u deze wijzigingen in de configuratie.

   * Om te vragen voor de huidige configuratie van de service, gebruikt u de `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet:

      ```powershell
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0
      ```

### <a name="install-the-dc-agent-service"></a>De DC-Agent-service installeren

   Installeer de DC-agentservice voor beveiliging met een wachtwoord met behulp van de `AzureADPasswordProtectionDCAgentSetup.msi` pakket.

   De installatie van software- of uitschrijven-installatie, moet worden opgestart. Dit komt doordat de wachtwoordfilter-DLL-bestanden alleen worden geladen of is verwijderd door een opnieuw opstarten.

   U kunt de DC-Agent-service installeren op een computer die nog niet een domeincontroller. In dit geval wordt de service gestart en uitgevoerd maar inactief blijft totdat de computer wordt gepromoveerd tot een domeincontroller.

   U kunt de software-installatie automatiseren met behulp van standaard MSI-procedures. Bijvoorbeeld:

   `msiexec.exe /i AzureADPasswordProtectionDCAgentSetup.msi /quiet /qn`

   > [!WARNING]
   > De msiexec voorbeeldopdracht hier zorgt ervoor dat onmiddellijk opnieuw opstarten. Als u wilt voorkomen, gebruikt u de `/norestart` vlag.

De installatie is voltooid nadat de software DC-Agent is geïnstalleerd op een domeincontroller, en die computer opnieuw wordt opgestart. Er is geen andere configuratie vereist of mogelijk.

## <a name="multiple-forest-deployments"></a>Implementaties met meerdere forests

Er zijn geen aanvullende vereisten voor het implementeren van Azure AD-wachtwoordbeveiliging in verschillende forests. Elk forest is onafhankelijk van elkaar geconfigureerd zoals beschreven in de sectie 'implementatie met één forest'. Elk wachtwoord beveiliging proxy kan alleen domeincontrollers van het forest dat deze wordt gekoppeld aan ondersteunen. De wachtwoord-protection-software in een forest is geen informatie over wachtwoord-protection-software die geïmplementeerd in andere forests, ongeacht de Active Directory-vertrouwensrelaties.

## <a name="read-only-domain-controllers"></a>Alleen-lezen domeincontrollers

Wachtwoord wijzigingen/sets zijn niet verwerkt en opgeslagen op alleen-lezen domeincontrollers (RODC's). Ze worden doorgestuurd naar beschrijfbare domeincontrollers. U moet dus niet de DC-Agent-software installeren op de RODC's.

## <a name="high-availability"></a>Hoge beschikbaarheid

De beschikbaarheid van de belangrijkste aandachtspunt voor beveiliging met een wachtwoord is de beschikbaarheid van proxy-servers als de domeincontrollers in een forest probeert te downloaden van nieuwe beleidsregels of andere gegevens van Azure. Elke DC-Agent maakt gebruik van een eenvoudige round-robin-stijl-algoritme bij het bepalen van welke proxyserver om aan te roepen. De Agent wordt overgeslagen proxy-servers die worden niet meer reageert. Twee proxyservers voor meest volledig verbonden Active Directory-implementaties die in orde replicatie van de status van zowel directory en sysvol-map hebt, is voldoende om beschikbaarheid te garanderen. Dit resulteert in tijdig downloaden van het nieuwe beleid en andere gegevens. Maar u kunt extra proxy-servers implementeren.

Het ontwerp van de DC-agentsoftware vermindert de gebruikelijke problemen die gekoppeld aan de hoge beschikbaarheid zijn. De DC-Agent onderhoudt een lokale cache van het meest recent gedownloade wachtwoordbeleid. Zelfs als alle proxy-servers niet meer beschikbaar zijn geregistreerd, wordt de DC-agenten blijven om af te dwingen van hun in de cache wachtwoordbeleid. Een redelijke updatefrequentie voor wachtwoordbeleid in een grote implementatie is meestal *dagen*, niet uur of minder. Dus korte uitval van de proxy-servers niet aanzienlijk van invloed zijn op Azure AD-wachtwoordbeveiliging.

## <a name="next-steps"></a>Volgende stappen

Installatie van de services die u nodig hebt voor de beveiliging van Azure AD-wachtwoord op uw on-premises servers, [uitvoeren na de installatie, configuratie en verzamelen gegevens](howto-password-ban-bad-on-premises-operations.md) om uit te voeren van uw implementatie.

[Conceptueel overzicht van Azure AD-wachtwoordbeveiliging](concept-password-ban-bad-on-premises.md)
