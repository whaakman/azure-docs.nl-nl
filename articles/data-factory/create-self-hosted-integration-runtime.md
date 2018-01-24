---
title: Host zichzelf integratie runtime maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van de host zichzelf integratie runtime in Azure Data Factory, waardoor de data factory voor toegang tot gegevensarchieven in een particulier netwerk.
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 92f773d3bbabe763d342366f0d56a77621829487
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="how-to-create-and-configure-self-hosted-integration-runtime"></a>Het maken en configureren van Self-hosted integratie Runtime
De integratie Runtime (IR) is de beheerinfrastructuur gebruikt door Azure Data Factory om te bieden mogelijkheden voor de integratie in verschillende netwerkomgevingen. Zie voor meer informatie over IR [integratie Runtime overzicht](concepts-integration-runtime.md).

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).

Een host zichzelf integratie-runtime is kopie activiteiten uitgevoerd tussen een cloudgegevens opslaat en een gegevensopslag in particulier netwerk en het verzenden van de transformatie activiteiten tegen rekenresources in een on-premises of Azure Virtual Network. Runtime-host zichzelf integratiebehoeften installeren op een lokale computer of op een virtuele machine in een particulier netwerk.  

Dit document wordt geïntroduceerd, kunt u maken en configureren van Self-hosted IR

## <a name="high-level-steps-to-install-self-hosted-ir"></a>De stappen op hoog niveau voor het installeren van de host zichzelf IR
1.  Maak een host zichzelf integratie-runtime. Hier volgt een voorbeeld van PowerShell:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
2.  Download en installeer de runtime host zichzelf-integratie (op de lokale computer).
3.  Ophalen van de verificatiesleutel en host zichzelf integratie runtime registreren met de sleutel. Hier volgt een voorbeeld van PowerShell:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime.  
    ```

## <a name="command-flow-and-data-flow"></a>Opdracht stroom en de gegevensstroom
Als u de gegevens tussen on-premises en cloud, de activiteit maakt gebruik van een host zichzelf integratie runtime overzetten van de gegevens van on-premises gegevensbron naar de cloud en vice versa.

Hier volgt een gegevensstroom op hoog niveau voor de samenvatting van stappen voor het kopiëren van met automatische gehoste IR:

![Overzicht](media\create-self-hosted-integration-runtime\high-level-overview.png)

1. Gegevens developer maakt een host zichzelf integratie runtime binnen een Azure data factory met een PowerShell-cmdlet. De Azure-portal biedt momenteel geen ondersteuning voor deze functie.
2. Een gekoppelde service voor een on-premises gegevensopslag maakt gegevens developer door te geven van de host zichzelf integratie runtime-instantie die het verbinding maken met gegevensarchieven moet gebruiken. Als onderdeel van het instellen van de gekoppelde service gegevens developer maakt gebruik van de toepassing ' Referentiebeheer ' (momenteel niet ondersteund) voor het instellen van verificatietypen en referenties. Het dialoogvenster voor gebruikersreferenties manager toepassing communiceert met de gegevensopslag voor het testen van verbinding en de host zichzelf integratie runtime opslaan van referenties.
4.  Host zichzelf integratie runtime knooppunt versleutelt de referenties met behulp van Windows Data Protection Application Programming Interface (DPAPI) en lokaal opgeslagen. Als u meerdere knooppunten voor hoge beschikbaarheid zijn ingesteld, worden de referenties verder gesynchroniseerd op andere knooppunten. Elk knooppunt codeert met behulp van DPAPI en lokaal opgeslagen. Synchronisatie van referentie is transparant voor de ontwikkelaar van de gegevens en wordt verwerkt door de host zichzelf IR    
5.  Data Factory-service communiceert met de host zichzelf integratie-runtime voor de planning & beheer van taken via **besturingskanaal** die gebruikmaakt van een gedeelde Azure service bus-wachtrij. Wanneer de taak van een activiteit worden uitgevoerd moet, wachtrijen Data Factory de aanvraag samen met de referenties (als referenties zijn niet op de host zichzelf integratie-runtime zijn opgeslagen). De taak zelf gehoste integratie runtime automatisch serversysteemstatus na polling van de wachtrij.
6.  Host zichzelf integratie runtime kopieert gegevens van een winkel lokale naar een cloudopslag of omgekeerd, afhankelijk van hoe de kopieerbewerking is geconfigureerd in de pijplijn gegevens. Voor deze stap worden de runtime host zichzelf integratie rechtstreeks communiceert met de opslag op basis van een cloud-services zoals Azure Blob Storage via een beveiligd kanaal voor (HTTPS).

## <a name="considerations-for-using-self-hosted-ir"></a>Overwegingen voor het gebruik van zelf gehoste IR

- Een runtime die één host zichzelf integratie kan worden gebruikt voor meerdere on-premises-gegevensbronnen. Echter, een **één host zichzelf integratie runtime is gekoppeld aan slechts één Azure data factory** en kan niet worden gedeeld met een andere data factory.
- U kunt hebben **slechts één exemplaar van de host zichzelf integratie runtime** geïnstalleerd op een enkele computer. Stel dat u hebt twee gegevensfactory's die toegang moeten krijgen tot on-premises gegevensbronnen, moet u zelf gehoste integratie runtime op twee on-premises computers installeren. Met andere woorden, is een runtime-integratie host zichzelf gekoppeld aan een specifieke gegevensfactory
- De **host zichzelf integratie runtime niet hoeft te worden op dezelfde computer als de gegevensbron**. Echter, integratie runtime gelet zelf de hosting dichter met de gegevensbron minder tijd voor de host zichzelf integratie runtime verbinding maken met de gegevensbron. Het is raadzaam dat u installeert de runtime host zichzelf integratie op een computer waarop wijkt af van de versie die hosts on-premises-gegevensbron. Wanneer de host zichzelf integratie runtime en de gegevensbron zich op verschillende computers bevinden, de host zichzelf integratie-runtime niet zou concurreren voor bronnen met de gegevensbron.
- U kunt hebben **meerdere host zichzelf integratie runtimes op verschillende computers verbinding maken met dezelfde lokale gegevensbron**. Bijvoorbeeld wellicht hebt u twee host zichzelf integratie-runtime voor de twee data Factory, maar dezelfde lokale gegevensbron is geregistreerd bij de gegevensfactory.
- Als u al een gateway geïnstalleerd op uw computer voor een **Power BI** scenario installeert een **afzonderlijke host zichzelf integratie-runtime voor de Azure Data Factory** op een andere computer.
- Host zichzelf integratie runtime moet worden gebruikt voor de ondersteuning van de integratie van de gegevens in Azure Virtual Network.
- Uw gegevensbron behandelen als een lokale gegevensbron (die zich achter een firewall) zelfs als u werkt met **ExpressRoute**. De runtime host zichzelf integratie gebruiken om verbinding tussen de service en de gegevensbron te maken.
- U moet de host zichzelf integratie-runtime gebruiken, zelfs als het gegevensarchief is in de cloud op een **Azure IaaS virtuele machine**.

## <a name="prerequisites"></a>Vereisten

- De ondersteunde **besturingssysteem** versies zijn Windows 7 Service Pack 1, Windows 8.1, Windows 10, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016. Installatie van de runtime host zichzelf integratie op een **domeincontroller wordt niet ondersteund**.
- **.NET framework 4.6.1 of hoger** is vereist. Als u zelf gehoste integratie runtime op een computer met Windows 7 installeert, installeert u .NET Framework 4.6.1 of hoger. Zie [.NET Framework-systeemvereisten](/dotnet/framework/get-started/system-requirements) voor meer informatie.
- De aanbevolen **configuratie** voor de runtime host zichzelf integratie machine ten minste 2 GHz, 4 kernen, 8 GB RAM-geheugen en schijf 80 GB is.
- Als de hostmachine in de slaapstand, reageert de host zichzelf integratie-runtime niet op gegevensaanvragen. Een juiste energiebeheerschema op de computer daarom configureren voordat u de runtime host zichzelf integratie installeert. Als de machine is geconfigureerd voor de sluimerstand, vraagt de host zichzelf integratie runtime-installatie een bericht.
- U moet een beheerder op de computer installeren en configureren van de host zichzelf integratie-runtime is.
- Als kopie activiteiten bij uitvoering op een specifieke frequentie gebeuren, volgt het brongebruik (CPU, geheugen) op de machine ook hetzelfde patroon met piek- en niet-actieve tijd. Resourcegebruik ook afhankelijk van geheugenlatentie de hoeveelheid gegevens worden verplaatst. Als meerdere kopie taken uitgevoerd worden, ziet u Resourcegebruik tijdens piektijden omhoog gaan.

## <a name="installation-best-practices"></a>Best practices voor installatie
Host zichzelf integratie runtime kan worden geïnstalleerd door het downloaden van een MSI-installatiepakket van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Zie [gegevens verplaatsen tussen on-premises en cloud artikel](tutorial-hybrid-copy-powershell.md) voor stapsgewijze instructies.

- Energiebeheerschema op de hostcomputer voor de runtime host zichzelf integratie zo configureren dat de machine sluimerstand niet. Als de hostmachine in de slaapstand, wordt de host zichzelf integratie runtime offline.
- Back-up van de referenties die zijn gekoppeld aan de host zichzelf integratie runtime regelmatig.

## <a name="install-and-register-self-hosted-ir-from-download-center"></a>Installeren en registreren Self-hosted IR van Downloadcentrum

1. Navigeer naar [downloadpagina van Microsoft-integratie Runtime](https://www.microsoft.com/download/details.aspx?id=39717).
2. Klik op **downloaden**, selecteer de juiste versie (**32-bits** vs. **64-bits**), en klik op **volgende**.
3. Voer de **MSI** rechtstreeks of sla het op uw harde schijf en uitvoeren.
4. Op de **Welkom** pagina een **taal** klikt u op **volgende**.
5. **Accepteer** de gebruiksrechtovereenkomst en klik op **volgende**.
6. Selecteer **map** installeren van de runtime host zichzelf integratie en klikt u op **volgende**.
7. Op de **gereed voor installatie** pagina, klikt u op **installeren**.
8. Klik op **voltooien** om installatie te voltooien.
9. Ophalen van de verificatiesleutel met Azure PowerShell. PowerShell-voorbeeld voor het ophalen van de verificatiesleutel:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```
11. Op de **registreren integratie Runtime (zelf gehost)** pagina van Microsoft integratie Runtime Configuration Manager op uw computer, de volgende stappen uitvoeren:
    1. Plak de **verificatiesleutel** in het tekstvak.
    2. Klik desgewenst op **weergeven verificatiesleutel** om te zien van de belangrijkste tekst.
    3. Klik op **registreren**.


## <a name="high-availability-and-scalability"></a>Hoge beschikbaarheid en schaalbaarheid
Een Self-hosted integratie Runtime kan worden gekoppeld aan meerdere on-premises machines. Deze machines worden knooppunten genoemd. U kunt maximaal vier knooppunten die zijn gekoppeld aan een Self-hosted integratie Runtime hebben. De voordelen van meerdere knooppunten (op lokale computers met de gateway is geïnstalleerd) voor een logische gateway zijn:
1. Hogere beschikbaarheid van Self-hosted integratie Runtime zodat deze niet langer de storingspunt in uw Big Data oplossing of cloud gegevensintegratie met Azure Data Factory gezorgd continuïteit met maximaal 4 knooppunten.
2. Verbeterde prestaties en de doorvoer tijdens de verplaatsing van gegevens tussen on-premises en cloud gegevensarchieven. Meer informatie opvragen over [prestaties vergelijkingen](copy-activity-performance.md).

U kunt meerdere knooppunten koppelen gewoon de Self-hosted integratie Runtime door software te installeren vanuit de [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=39717) en door het registreren van deze door een van de verificatiesleutels verkregen van Nieuwe AzureRmDataFactoryV2IntegrationRuntimeKey cmdlet zoals beschreven in de [zelfstudie](tutorial-hybrid-copy-powershell.md)

> [!NOTE]
> U hoeft niet te maken van nieuwe Self-hosted integratie Runtime voor het koppelen van elk knooppunt. U kunt zelf gehoste integratie runtime op een andere computer installeren en registreren met behulp van de dezelfde verificatiesleutel. 

> [!NOTE]
> Voordat u een ander knooppunt voor toevoegt **hoge beschikbaarheid en schaalbaarheid**, zorg ervoor dat **extern toegang tot intranet** optie is **ingeschakeld** op de 1e knooppunt (Microsoft Integratie Runtime Configuration Manager -> Instellingen -> extern toegang tot intranet). 

### <a name="tlsssl-certificate-requirements"></a>Vereisten voor TLS/SSL-certificaten
Hier volgen de vereisten voor het TLS/SSL-certificaat dat wordt gebruikt voor het beveiligen van communicatie tussen integratie runtime knooppunten:

- Het certificaat moet een openbaar vertrouwde X509 v3-certificaat. Het is raadzaam om gebruik te maken van certificaten die zijn uitgegeven door een openbare (derde) certificeringsinstantie (CA).
- Elk knooppunt van de runtime integratie moet dit certificaat vertrouwen.
- Met wild card certificaten worden ondersteund. Als uw FQDN-naam **node1.domain.contoso.com**, kunt u ***. domain.contoso.com** als de onderwerpnaam van het certificaat.
- SAN-certificaten worden niet aanbevolen omdat alleen het laatste item in de alternatieve onderwerpnamen wordt gebruikt en alle andere worden genegeerd vanwege een beperking van de huidige. Bijvoorbeeld u hebt een SAN-certificaat waarvan SAN zijn **node1.domain.contoso.com** en **node2.domain.contoso.com**, kunt u dit certificaat alleen gebruiken op de machine waarvan FQDN **node2.domain.contoso.com**.
- Ondersteunt sleutelgrootte voor SSL-certificaten wordt ondersteund door Windows Server 2012 R2.
- Certificaten met CNG sleutels worden niet ondersteund. Doesrted DoesDoes ondersteuning geen voor certificaten met CNG-sleutels.

## <a name="system-tray-icons-notifications"></a>Pictogrammen in systeemvak / meldingen
Als u de cursor op het systeem/Meldingsbericht voor een pictogram systeemvak plaatst, kunt u gegevens over de status van de host zichzelf integratie runtime vinden.

![Systeemvak systeemmeldingen](media\create-self-hosted-integration-runtime\system-tray-notifications.png)

## <a name="ports-and-firewall"></a>Poorten en firewall
Er zijn twee firewalls, moet u rekening houden: **bedrijfsfirewall** uitgevoerd op de centrale router van de organisatie en **Windows firewall** geconfigureerd als een daemon op de lokale computer waar de host zichzelf integratie runtime is geïnstalleerd.

![Firewall](media\create-self-hosted-integration-runtime\firewall.png)

Op **bedrijfsfirewall** niveau, moet u de volgende domeinen en uitgaande poorten:

Domeinnamen | Poorten | Beschrijving
------------ | ----- | ------------
*.servicebus.windows.net | 443, 80 | Gebruikt voor communicatie met Data Movement Service back-end
*.core.windows.net | 443 | Gebruikt voor de tijdelijke kopie met behulp van Azure-Blob (indien geconfigureerd)
*.frontend.clouddatahub.net | 443 | Gebruikt voor communicatie met Data Movement Service back-end

Op **Windows firewall** niveau (machine niveau), zijn normaal gesproken deze uitgaande poorten ingeschakeld. Als dat niet zo is, kunt u de domeinen en dienovereenkomstig op zelf de hosting-poorten configureren integratie runtime-machine.

> [!NOTE]
> Op basis van de bron / put u wellicht aanvullende domeinen van goedgekeurde IP-adressen en uitgaande poorten in uw zakelijke/Windows firewall.
>
> Voor sommige Databases van de Cloud (bijvoorbeeld: Azure SQL Database, Azure Data Lake, enz.), mogelijk moet u geaccepteerde IP-adres van de host zichzelf integratie runtime machine op de firewallconfiguratie.

### <a name="copy-data-from-a-source-to-a-sink"></a>Gegevens kopiëren van een bron naar een sink
Zorg ervoor dat de firewallregels correct zijn ingeschakeld op de firewall van het bedrijf, Windows firewall op de machine van de runtime host zichzelf integratie en opslaan van de gegevens zelf. Inschakelen van deze regels kunt de host zichzelf integratie runtime verbinding maken met zowel bron en sink is. Schakel regels voor elke gegevensopslag die bij de kopieerbewerking betrokken is.

Bijvoorbeeld: voor het kopiëren van een **on-premises gegevens opslaan op een Azure SQL Database-sink of een Azure SQL Data Warehouse sink**, moet u de volgende stappen uitvoeren:

- Uitgaand verkeer toestaan **TCP** -communicatie op poort **1433** voor Windows firewall- en firewall van het bedrijf.
- Configureer de firewall-instellingen van Azure SQL-server om de IP-adres van de host zichzelf integratie runtime-machine toevoegen aan de lijst met toegestane IP-adressen.

> [!NOTE]
> Als uw firewall kan geen uitgaande poort 1433, host zichzelf integratie runtime kan niet rechtstreeks toegang hebben tot Azure SQL. U kunt in dit geval [kopie gefaseerde](copy-activity-performance.md) naar Azure SQL Database / SQL Azure DW. In dit scenario zou u alleen HTTPS (poort 443) nodig voor de verplaatsing van gegevens.


## <a name="proxy-server-considerations"></a>Overwegingen voor proxy-server
Als uw zakelijke netwerkomgeving een proxyserver gebruikt voor toegang tot het internet, configureert u zelf gehoste integratie-runtime voor het gebruik van de juiste proxy-instellingen. U kunt de proxy instellen tijdens de registratie van de eerste fase.

![Geef de proxy](media\create-self-hosted-integration-runtime\specify-proxy.png)

De proxy-server host zichzelf integratie runtime gebruikt om verbinding met de cloudservice. Klik op koppeling wijzigen tijdens de eerste configuratie. Ziet u het dialoogvenster proxy-instelling.

![Set-proxy](media\create-self-hosted-integration-runtime\set-http-proxy.png)

Er zijn drie opties:

- **Gebruik geen proxy**: runtime host zichzelf integratie expliciet gebruikt geen elke proxy verbinding maken met cloudservices.
- **Gebruik system proxy**: integratie runtime maakt gebruik van de proxy-instelling die is geconfigureerd op diahost.exe.config en diawp.exe.config zelf wordt gehost. Als geen proxy is geconfigureerd in diahost.exe.config en diawp.exe.config, verbinding host zichzelf integratie runtime met cloudservice rechtstreeks zonder gebruik te maken via proxy.
- **Aangepaste proxy gebruikt**: de HTTP-proxy-instellingen voor de host zichzelf integratie runtime, in plaats van configuraties in diahost.exe.config en diawp.exe.config configureren. Adres en poort zijn vereist. Gebruikersnaam en wachtwoord zijn optioneel, afhankelijk van uw proxyserver verificatie-instelling. Alle instellingen zijn versleuteld met Windows DPAPI op de host zichzelf integratie-runtime en lokaal opgeslagen op de machine.

De integratie-runtime-Host-Service wordt automatisch opnieuw opgestart nadat u de bijgewerkte proxy-instellingen opslaan.

Nadat de host zichzelf integratie runtime is geregistreerd, als u wilt weergeven of bijwerken van proxy-instellingen, moet u integratie Runtime Configuration Manager gebruiken.

1.  Start **Microsoft integratie Runtime Configuration Manager**.
2.  Overschakelen naar de **instellingen** tabblad.
3.  Klik op **wijziging** koppelen **HTTP-Proxy** sectie starten de **Set HTTP-Proxy** dialoogvenster.
4.  Nadat u op de **volgende** knop klikt, ziet u een waarschuwingsdialoogvenster waarin wordt gevraagd uw toestemming voor het opslaan van de proxy-instellingen en de integratie Runtime-Host-Service opnieuw starten.

U kunt weergeven en bijwerken van HTTP-proxy met behulp van Configuration Manager-hulpprogramma.

![Weergave-proxy](media\create-self-hosted-integration-runtime\view-proxy.png)

> [!NOTE]
> Als u een proxyserver met NTLM-verificatie hebt ingesteld, integratie runtime Host Service wordt uitgevoerd onder het domeinaccount. Als u het wachtwoord voor het domeinaccount later wijzigt, moet u configuratie-instellingen voor de service bijwerken en dienovereenkomstig aan het programma opnieuw. Vanwege deze vereiste raden we dat u een specifiek domeinaccount gebruiken voor toegang tot de proxyserver die vereist niet dat u het wachtwoord regelmatig bijwerken.

### <a name="configure-proxy-server-settings"></a>Proxyserverinstellingen configureren

Als u selecteert **system proxy gebruiken** instellen voor de HTTP-proxy, host zichzelf integratie runtime maakt gebruik van de proxy-instelling in diahost.exe.config en diawp.exe.config. Als er geen proxyserver is opgegeven in diahost.exe.config en diawp.exe.config, verbinding host zichzelf integratie runtime met cloudservice rechtstreeks zonder gebruik te maken via proxy. De volgende procedure bevat instructies voor het bijwerken van het bestand diahost.exe.config.

1. Maak een veilige kopie van C:\Program Files\Microsoft integratie Runtime\3.0\Shared\diahost.exe.config back-up van het oorspronkelijke bestand in Verkenner.
2. Starten van Notepad.exe uitgevoerd als administrator en open tekstbestand "C:\Program Files\Microsoft integratie Runtime\3.0\Shared\diahost.exe.config. U kunt het standaardlabel voor system.net vinden zoals weergegeven in de volgende code:

    ```xml
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```
    Vervolgens kunt u proxy-servergegevens toevoegen, zoals wordt weergegeven in het volgende voorbeeld:

    ```xml
    <system.net>
        <defaultProxy enabled="true">
              <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Aanvullende eigenschappen zijn toegestaan in de tag proxy om op te geven van de vereiste instellingen zoals scriptLocation. Zie [proxy Element (netwerkinstellingen)](https://msdn.microsoft.com/library/sa91de1e.aspx) syntaxis.

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Sla het configuratiebestand in de oorspronkelijke locatie, start daarna de Self-hosted integratie Runtime-Host-service, die de wijzigingen opneemt. De service te starten: Gebruik de applet services in het Configuratiescherm, of vanuit de **integratie Runtime Configuration Manager** > Klik op de **Service stoppen** knop en klik vervolgens op de **starten Service**. Als de service niet wordt gestart, is het waarschijnlijk dat er een onjuiste syntaxis van de XML-label is toegevoegd in het toepassingsconfiguratiebestand die is bewerkt.

> [!IMPORTANT]
> Vergeet niet zowel diahost.exe.config en diawp.exe.config bij te werken.

Naast deze punten moet u ook om te controleren of Microsoft Azure in goedgekeurde lijst van uw bedrijf. De lijst met geldige Microsoft Azure-IP-adressen kan worden gedownload vanuit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Mogelijke problemen voor firewall en proxy server-problemen
Als u fouten overeen met de volgende procedures optreden, is het waarschijnlijk door onjuiste configuratie van de firewall of proxy-server, die zelf gehoste integratie runtime verbinding maken met de Data Factory blokkeert zichzelf verifiëren. Raadpleeg de vorige sectie om te controleren of uw firewall en proxy-server correct zijn geconfigureerd.

1.  Wanneer u probeert te registreren van de host zichzelf integratie-runtime, wordt de volgende fout: 'kan niet worden geregistreerd van dit knooppunt integratie Runtime! Controleer of de verificatiesleutel geldig is en de integratie-Host-Service wordt uitgevoerd op deze machine. "
2.  Wanneer u integratie Runtime Configuration Manager opent, ziet u de status als '**verbroken**"of"**verbinding maakt met**'. Tijdens het weergeven van Windows-gebeurtenislogboeken onder 'Logboeken' > 'Toepassingen en Services Logs' > 'Microsoft-integratie Runtime', ziet u foutberichten, zoals de volgende fout:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="enable-remote-access-from-intranet"></a>Externe toegang via Intranet inschakelen  
In geval als u **PowerShell** of **Referentiebeheer toepassing** om referenties te versleutelen vanaf een andere computer (in het netwerk) dan waar de host zichzelf integratie-runtime is geïnstalleerd, klikt u vervolgens u moet de **'Externe toegang via Intranet'** optie zijn ingeschakeld. Als u de **PowerShell** of **Referentiebeheer toepassing** voor het versleutelen van de referentie op dezelfde computer waarop de host zichzelf integratie-runtime is geïnstalleerd, klikt u vervolgens **' externe toegang via Intranet'** mogelijk niet ingeschakeld.

Externe toegang via Intranet moet **ingeschakeld** voordat u een ander knooppunt voor toevoegt **hoge beschikbaarheid en schaalbaarheid**.  

Tijdens de integratie van host zichzelf runtime installatie (of hoger v 3.3.xxxx.x), standaard de runtime-installatie zelf gehoste integratie schakelt de **'Externe toegang via Intranet'** op de host zichzelf integratie runtime-machine.

Als u een firewall van derden gebruikt, kunt u handmatig de 8060 (of de gebruiker geconfigureerde poort) openen. Als u in de firewallprobleem tijdens de installatie van de runtime host zichzelf integratie uitvoert, kunt u proberen met de volgende opdracht voor het installeren van de runtime host zichzelf integratie zonder het configureren van de firewall.

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
```
> [!NOTE]
> **Referentiebeheer toepassing** is nog niet beschikbaar voor het versleutelen van de referenties in ADFv2. We zullen deze ondersteuning later toevoegen.  

Als u niet de poort te openen 8060 op de host zichzelf integratie runtime-machine, gebruikgemaakt van mechanismen dan met behulp van de ** instelling referenties ** toepassing voor het configureren van referenties voor gegevensopslag. U kunt bijvoorbeeld New AzureRmDataFactoryV2LinkedServiceEncryptCredential PowerShell-cmdlet. Zie de sectie over de instelling referenties en beveiliging op hoe de referenties voor het opslaan van gegevens kan worden ingesteld.


## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudie voor stapsgewijze instructies: [zelfstudie: kopie on-premises gegevens in de cloud](tutorial-hybrid-copy-powershell.md).
