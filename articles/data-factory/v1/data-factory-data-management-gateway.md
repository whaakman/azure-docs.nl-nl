---
title: Data Management Gateway voor Data Factory | Microsoft Docs
description: Instellen van een gegevensgateway om gegevens te verplaatsen tussen on-premises en de cloud. Gebruik Data Management Gateway in Azure Data Factory om uw gegevens te verplaatsen.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 6bb37008b6a8b37e575dcf83dffcf57ab08996fd
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57539991"
---
# <a name="data-management-gateway"></a>Gegevensbeheergateway
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [zelf-hostende integratieruntime in](../create-self-hosted-integration-runtime.md).

> [!NOTE]
> Data Management Gateway is nu is rebranded als zelfgehoste Cloudintegratieruntime.

De Data management gateway is een clientagent die u in uw on-premises omgeving installeren moet kopiëren van gegevens tussen cloud en on-premises gegevensarchieven. De on-premises gegevens opgeslagen die worden ondersteund door Data Factory worden vermeld in de [ondersteunde gegevensbronnen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) sectie.

In dit artikel is een aanvulling op de procedures in de [gegevens verplaatsen tussen on-premises en cloud gegevensarchieven](data-factory-move-data-between-onprem-and-cloud.md) artikel. In het scenario maakt u een pijplijn die de gateway wordt gebruikt om gegevens te verplaatsen van een on-premises SQL Server-database naar een Azure-blob. Dit artikel bevat gedetailleerde informatie over de data management gateway.

U kunt een data management gateway uitschalen door meerdere on-premises computers koppelen aan de gateway. U kunt de schaal omhoog door het aantal gegevens verkeer taken die gelijktijdig kunnen worden uitgevoerd op een knooppunt. Deze functie is ook beschikbaar voor een logische gateway met een enkel knooppunt. Zie [gegevensbeheergateway schalen in Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) artikel voor meer informatie.

> [!NOTE]
> Gateway ondersteunt momenteel alleen de kopieeractiviteit en de opgeslagen procedure-activiteit in Data Factory. Het is niet mogelijk om te gebruiken van de gateway van een aangepaste activiteit voor toegang tot on-premises gegevensbronnen.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Overzicht
### <a name="capabilities-of-data-management-gateway"></a>Mogelijkheden van data management gateway
Data management gateway biedt de volgende mogelijkheden:

* Model voor on-premises gegevensbronnen en gegevensbronnen binnen de dezelfde data factory in de cloud en verplaatsen van gegevens.
* Een enkel glazen voor bewaking en beheer met inzicht in de gatewaystatus van de van de Data Factory-pagina hebben.
* Toegang tot on-premises gegevensbronnen veilig beheren.
  * Er zijn geen wijzigingen vereist voor de firewall van het bedrijf. Gateway maakt alleen uitgaande HTTP-gebaseerde verbindingen met het openbare internet.
  * Versleutelen referenties voor uw on-premises gegevensarchieven met uw certificaat.
* Gegevens efficiënt verplaatsen - gegevens worden overgedragen parallel tegen onregelmatige netwerkproblemen met automatisch logica voor opnieuw proberen.

### <a name="command-flow-and-data-flow"></a>Opdracht stroom en de gegevensstroom
Wanneer u een kopieeractiviteit om gegevens tussen on-premises en cloud te kopiëren, de activiteit maakt gebruik van een gateway voor gegevensoverdracht van on-premises gegevensbron naar de cloud en vice versa.

Dit is de gegevensstroom op hoog niveau voor en samenvatting van de stappen voor het kopiëren met data gateway: ![Gegevensstroom met behulp van gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1. Data-ontwikkelaar maakt een gateway voor een Azure Data Factory met behulp van de [Azure-portal](https://portal.azure.com) of [PowerShell-Cmdlet](https://docs.microsoft.com/powershell/module/az.datafactory/).
2. Data-ontwikkelaar maakt een gekoppelde service voor een on-premises gegevensarchief door de gateway op te geven. Data-ontwikkelaar gebruikt als onderdeel van het instellen van de gekoppelde service, de instelling referenties toepassing verificatietypen en referenties op te geven. Het dialoogvenster referenties instellen-toepassing communiceert met het gegevensarchief voor het testen van verbinding en de gateway naar de referenties op te slaan.
3. Gateway versleutelt de referenties met het certificaat dat is gekoppeld aan de gateway (opgegeven door de ontwikkelaar van gegevens), voordat de referenties worden opgeslagen in de cloud.
4. Data Factory-service communiceert met de gateway voor planning en beheer van taken via een besturingskanaal die gebruikmaakt van een gedeelde Azure service bus-wachtrij. Wanneer een taak voor het kopiëren activiteit worden gestart moet, wachtrijen voor Data Factory de aanvraag, samen met referentie-informatie. De taak begint gateway nadat het opvragen van configuratiegegevens bij de wachtrij.
5. De gateway, ontsleutelt de referenties met hetzelfde certificaat en maakt vervolgens verbinding met het on-premises gegevensarchief met het juiste verificatietype en referenties.
6. De gateway worden gegevens gekopieerd van een on-premises gegevensopslag naar een opslag in de cloud, of vice versa, afhankelijk van hoe de Copy-activiteit is geconfigureerd in de pijplijn. Voor deze stap worden de gateway rechtstreeks communiceert met cloud-gebaseerde services zoals Azure Blob Storage via een beveiligd kanaal voor (HTTPS).

### <a name="considerations-for-using-gateway"></a>Overwegingen voor het gebruik van gateway
* Slechts één exemplaar van data management gateway kan worden gebruikt voor meerdere on-premises gegevensbronnen. Echter, **één gatewayexemplaar is gekoppeld aan slechts één Azure data factory** en kan niet worden gedeeld met een andere data factory.
* U kunt hebben **slechts één exemplaar van data management gateway** op één computer geïnstalleerd. Stel dat u hebt twee data factory's die nodig hebt voor toegang tot on-premises gegevensbronnen, moet u gateways installeren op twee on-premises computers. Met andere woorden, is een gateway gekoppeld aan een specifieke data factory
* De **gateway hoeft niet te worden op dezelfde computer als de gegevensbron**. Echter, gateway dichter naar de gegevensbron met verkort de termijn voor de gateway verbinding maken met de gegevensbron. Het is raadzaam dat u de gateway installeert op een computer die verschilt van de naam die als host fungeert voor on-premises gegevensbron. Wanneer de gateway en de gegevensbron zich op verschillende computers bevinden, kunnen de gateway niet concurreren voor resources met de gegevensbron.
* U kunt hebben **meerdere gateways op verschillende computers verbinding maken met dezelfde on-premises gegevensbron**. Bijvoorbeeld, u hebt twee gateways voor de twee data factory's maar dezelfde on-premises gegevensbron is geregistreerd bij de gegevensfactory.
* Als u al een gateway is geïnstalleerd op uw computer voor een **Power BI** scenario, installeert een **afzonderlijke gateway voor Azure Data Factory** op een andere computer.
* Gateway moet worden gebruikt, zelfs wanneer u **ExpressRoute**.
* Uw gegevensbron behandelen als een on-premises gegevensbron (die zich achter een firewall bevindt), zelfs wanneer u gebruikt **ExpressRoute**. De gateway gebruiken om verbinding tussen de service en de gegevensbron te maken.
* U moet **gebruik van de gateway** , zelfs als het gegevensarchief is in de cloud op een **Azure IaaS VM**.

## <a name="installation"></a>Installatie
### <a name="prerequisites"></a>Vereisten
* De ondersteunde **besturingssysteem** versies zijn Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Installatie van de data management gateway op een domeincontroller wordt momenteel niet ondersteund.
* .NET framework 4.5.1 of hoger is vereist. Als u gateway op een computer met Windows 7 installeert, installeert u .NET Framework 4.5 of hoger. Zie [.NET Framework-systeemvereisten](https://msdn.microsoft.com/library/8z6watww.aspx) voor meer informatie.
* De aanbevolen **configuratie** voor de gateway machine ten minste 2 GHz, 4 cores, 8 GB RAM-geheugen en 80 GB-schijf is.
* Als de hostmachine in de slaapstand, wordt de gateway niet reageert op gegevensaanvragen. Configureer daarom een geschikte **energiebeheerschema** op de computer voordat u de gateway installeert. Als de machine is geconfigureerd voor de slaapstand, wordt een bericht gevraagd om de installatie van de gateway.
* U moet een beheerder op de computer installeren en configureren van de data management gateway is. U kunt extra gebruikers toevoegen aan de **gegevensbeheergateway gebruikers** lokale Windows-groep. De leden van deze groep mogen gebruiken de **Data Management Gateway Configuratiebeheer** hulpprogramma om de gateway te configureren.

Als u uitvoeringen van activiteit kopiëren gebeuren op een specifieke frequentie, volgt het Resourcegebruik (CPU, geheugen) op de computer ook hetzelfde patroon met piek- en niet-actieve tijden. Gebruik van resources is ook afhankelijk sterk de hoeveelheid gegevens die wordt verplaatst. Wanneer meerdere kopie-taken uitgevoerd worden, ziet u Resourcegebruik tijdens piektijden omhoog.

### <a name="installation-options"></a>Opties voor de installatie
Data management gateway kan worden geïnstalleerd op de volgende manieren:

* Door het downloaden van een MSI-installatiepakket van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Het MSI-bestand kan ook worden gebruikt om te upgraden van bestaande data management gateway naar de nieuwste versie, met alle instellingen behouden.
* Door te klikken op **downloaden en installeren van de gegevensgateway** koppeling onder de handmatige installatie of **rechtstreeks op deze computer installeren** onder de snelle installatie. Zie [gegevens verplaatsen tussen on-premises en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies over het gebruik van snelle installatie. De handmatige stap gaat u naar het download center. De instructies voor het downloaden en installeren van de gateway via download center zijn in de volgende sectie.

### <a name="installation-best-practices"></a>Best practices voor installatie:
1. Energiebeheerschema op de hostcomputer voor de gateway zodanig configureren dat de machine sluimerstand niet. Als de hostmachine in de slaapstand, wordt de gateway niet reageert op gegevensaanvragen.
2. Back-up van het certificaat dat is gekoppeld aan de gateway.

### <a name="install-the-gateway-from-download-center"></a>De gateway van het Downloadcentrum installeren
1. Navigeer naar [downloadpagina van Microsoft Data Management Gateway](https://www.microsoft.com/download/details.aspx?id=39717).
2. Klik op **downloaden**, selecteer de juiste versie (**32-bits** vs. **64-bits**), en klikt u op **volgende**.
3. Voer de **MSI** rechtstreeks of sla deze op uw harde schijf en uitvoeren.
4. Op de **Welkom** weergeeft, schakelt een **taal** klikt u op **volgende**.
5. **Accepteer** de gebruiksrechtovereenkomst en klik op **volgende**.
6. Selecteer **map** installeren van de gateway en klikt u op **volgende**.
7. Op de **gereed voor installatie** pagina, klikt u op **installeren**.
8. Klik op **voltooien** om installatie te voltooien.
9. De sleutel ophalen uit de Azure portal. Zie de volgende sectie voor stapsgewijze instructies.
10. Op de **registreren gateway** pagina van **Data Management Gateway Configuratiebeheer** die worden uitgevoerd op uw computer, de volgende stappen uitvoeren:
    1. De sleutel in de tekst te plakken.
    2. Klik desgewenst op **Show gateway sleutel** de belangrijkste tekst wilt zien.
    3. Klik op **registreren**.

### <a name="register-gateway-using-key"></a>Gateway sleutel met registreren
#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Als u een logische gateway al hebt gemaakt in de portal
Een gateway maken in de portal en de sleutel van de **configureren** pagina, volg de stappen van de procedures in de [gegevens verplaatsen tussen on-premises en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Als u de logische gateway al hebt gemaakt in de portal
1. Navigeer in Azure portal naar de **Data Factory** pagina en klik op **gekoppelde Services** tegel.

    ![Pagina Data Factory](media/data-factory-data-management-gateway/data-factory-blade.png)
2. In de **gekoppelde Services** pagina, selecteert u de logische **gateway** in de portal hebt gemaakt.

    ![logische gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)
3. In de **gegevensgateway** pagina, klikt u op **downloaden en installeren van de gegevensgateway**.

    ![Download de koppeling in de portal](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)
4. In de **configureren** pagina, klikt u op **opnieuw sleutel**. Klik op Ja in het waarschuwingsbericht staan aangegeven na het lezen van deze zorgvuldig.

    ![Sleutel opnieuw maken](media/data-factory-data-management-gateway/recreate-key-button.png)
5. Klik op de knop kopiëren naast de sleutel. De sleutel wordt gekopieerd naar het Klembord.

    ![Sleutel kopiëren](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a>Pictogrammen in het systeemvak / meldingen
De volgende afbeelding ziet u enkele van de lade pictogrammen die worden weergegeven.

![pictogrammen in het systeemvak](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Als u de cursor op het systeem/Meldingsbericht voor een pictogram lade plaatst, ziet u meer informatie over de status van de gateway/update-bewerking in een pop-upvenster.

### <a name="ports-and-firewall"></a>Poorten en firewall
Er zijn twee firewalls moet u rekening houden: **bedrijfsfirewall** die worden uitgevoerd op de centrale-router van de organisatie, en **Windows firewall** als een daemon op de lokale computer waarop de gateway is geconfigureerd geïnstalleerd.

![firewalls](./media/data-factory-data-management-gateway/firewalls2.png)

Op het niveau van de firewall van het bedrijf, moet u de volgende domeinen bevinden en uitgaande poorten configureren:

| Domeinnamen | Poorten | Description |
| --- | --- | --- |
| *.servicebus.windows.net |443 |Gebruikt voor communicatie met back-end Data Movement Service |
| *.core.windows.net |443 |Gebruikt voor tijdelijke kopiëren met behulp van Azure-Blob (indien geconfigureerd)|
| *.frontend.clouddatahub.net |443 |Gebruikt voor communicatie met back-end Data Movement Service |
| *.servicebus.windows.net |9350-9354, 5671 |Optionele service bus relay via TCP wordt gebruikt door de Wizard kopiëren |

Deze uitgaande poorten zijn normaal gesproken op niveau van Windows firewall ingeschakeld. Als u niet het geval is, kunt u de domeinen en poorten dienovereenkomstig op gateway-apparaat.

> [!NOTE]
> 1. Op basis van de bron / sink, mogelijk moet u whitelist aanvullende domeinen en uitgaande poorten in uw zakelijke/Windows-firewall.
> 2. Voor sommige Clouddatabases (bijvoorbeeld: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access), enzovoort), moet u mogelijk geaccepteerde IP-adres van Gateway-apparaat op de firewallconfiguratie.
>
>

#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Gegevens kopiëren van een brongegevensarchief naar een sink-gegevensopslag
Zorg ervoor dat de firewall-regels correct zijn ingeschakeld op de firewall van het bedrijf, Windows firewall op de gatewaycomputer en opslaan van de gegevens zelf. Inschakelen van deze regels zorgt ervoor dat de gateway verbinding maken met zowel bron en sink is. Inschakelen van regels voor elke gegevensopslag die bij de kopieerbewerking betrokken is.

Bijvoorbeeld, voor het kopiëren van **een on-premises gegevensarchief naar een Azure SQL Database-sink of een Azure SQL Data Warehouse sink**, de volgende stappen uit:

* Uitgaand verkeer toestaan **TCP** communicatie op poort **1433** voor zowel Windows firewall en firewall van het bedrijf.
* De firewallinstellingen van Azure SQL-server om toe te voegen van de IP-adres van het gateway-apparaat aan de lijst met toegestane IP-adressen configureren.

> [!NOTE]
> Als de firewall geen uitgaande poort 1433 toestaat, Gateway kan niet rechtstreeks toegang hebben tot Azure SQL. U kunt in dit geval [gefaseerd kopiëren](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) met SQL Azure-Database / SQL Azure DW. In dit scenario zou u alleen HTTPS (poort 443) nodig voor de verplaatsing van gegevens.
>
>

### <a name="proxy-server-considerations"></a>Overwegingen voor de proxy-server
Als uw bedrijfsnetwerk bevindt een proxyserver gebruikt voor toegang tot het internet, configureert u de data management gateway voor het gebruik van de juiste proxy-instellingen. U kunt de proxy instellen tijdens de registratie-fase.

![Proxy instellen tijdens de registratie](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Gateway maakt gebruik van de proxy-server verbinding maken met de cloudservice. Klik op **wijziging** koppeling tijdens de eerste configuratie. U ziet de **proxy-instellingen** dialoogvenster.

![Set-proxy met behulp van Configuration manager](media/data-factory-data-management-gateway/SetProxySettings.png)

Er zijn drie opties:

* **Gebruik geen proxy**: Gateway niet expliciet gebruikt de proxy voor het verbinding maken met cloudservices.
* **Systeemproxy gebruiken**: Gateway maakt gebruik van de proxy-instellingen die in diahost.exe.config en diawp.exe.config is geconfigureerd. Als er geen proxy is geconfigureerd in diahost.exe.config en diawp.exe.config, wordt de status van gateway verbonden met cloudservice rechtstreeks zonder tussenkomst van proxy.
* **Aangepaste proxy gebruikt**: De HTTP-proxy instellen voor gebruik in plaats van configuraties in diahost.exe.config en diawp.exe.config-gateway configureren. Adres en poort zijn vereist. Gebruikersnaam en wachtwoord zijn optioneel, afhankelijk van de instelling van de verificatie van uw proxy. Alle instellingen zijn versleuteld met het referentiecertificaat van de gateway en lokaal opgeslagen op de gatewaycomputer host.

De data management gateway Host-Service wordt automatisch opnieuw opgestart nadat u de bijgewerkte proxy-instellingen hebt opgeslagen.

Nadat de gateway is geregistreerd, als u wilt weergeven of bijwerken van proxy-instellingen, gebruikt u Data Management Gateway Configuratiebeheer.

1. Start **Data Management Gateway Configuratiebeheer**.
2. Schakel over naar het tabblad **Instellingen**.
3. Klik op **wijzigen** koppelen **HTTP-Proxy** sectie starten de **HTTP-Proxy instellen** dialoogvenster.
4. Nadat u op de **volgende** knop klikt, ziet u een waarschuwingsdialoogvenster waarin wordt gevraagd uw toestemming voor het opslaan van de proxy-instellingen en de Gateway-Host-Service opnieuw starten.

U kunt weergeven en bijwerken van HTTP-proxy met behulp van Configuration Manager-hulpprogramma.

![Set-proxy met behulp van Configuration manager](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> Als u een proxyserver met NTLM-verificatie hebt ingesteld, is Gateway Host-Service wordt uitgevoerd onder het domeinaccount. Als u het wachtwoord voor het domeinaccount dat later wijzigt, moet u bijwerken van configuratie-instellingen voor de service en start deze dienovereenkomstig opnieuw. Vanwege deze vereiste is het raadzaam dat u een speciale domeinaccount gebruiken voor toegang tot de proxyserver die vereist niet dat u het wachtwoord regelmatig bijwerken.
>
>

### <a name="configure-proxy-server-settings"></a>Proxyserverinstellingen configureren
Als u selecteert **systeemproxy gebruiken** instellen voor de HTTP-proxy, gateway maakt gebruik van de proxy-instellingen in diahost.exe.config en diawp.exe.config. Als er geen proxy is opgegeven in de diahost.exe.config en diawp.exe.config, maakt gateway verbinding met de cloudservice rechtstreeks zonder tussenkomst van proxy. De volgende procedure bevat instructies voor het bijwerken van het bestand diahost.exe.config.

1. Maak een veilige kopie van C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config back-up van het oorspronkelijke bestand in Verkenner.
2. Start Notepad.exe uitvoert als beheerder, en opent u tekstbestand "C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. U kunt de standaardtag voor system.net vinden zoals wordt weergegeven in de volgende code:

    ```
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```

    Vervolgens kunt u proxy-server-gegevens toevoegen, zoals wordt weergegeven in het volgende voorbeeld:

    ```
    <system.net>
        <defaultProxy enabled="true">
            <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Aanvullende eigenschappen zijn toegestaan in de proxy-tag om op te geven van de vereiste instellingen, zoals scriptLocation. Raadpleeg [proxy Element (netwerkinstellingen)](https://msdn.microsoft.com/library/sa91de1e.aspx) op syntaxis.

    ```
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Sla het configuratiebestand naar de oorspronkelijke locatie, start vervolgens de Data Management Gateway-Host-service, die de wijzigingen neemt. De service opnieuw starten: Gebruik de applet services in het Configuratiescherm of in de **Data Management Gateway Configuratiebeheer** > klikt u op de **-Service stoppen** knop en klik vervolgens op de **starten Service**. Als de service niet wordt gestart, is het waarschijnlijk dat een onjuiste syntaxis van de XML-code is toegevoegd in het configuratiebestand van de toepassing die is bewerkt.

> [!IMPORTANT]
> Vergeet niet om bij te werken **beide** diahost.exe.config en diawp.exe.config.

Naast deze punten moet u ook om te controleren of dat Microsoft Azure is in de lijst met toegestane adressen van uw bedrijf. De lijst met geldige Microsoft Azure IP-adressen kan worden gedownload vanaf de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Mogelijke problemen voor de firewall en proxy-server-problemen
Als u fouten die vergelijkbaar is met de volgende query optreden, is het waarschijnlijk veroorzaakt door onjuiste configuratie van de firewall of proxy-server, waarmee de gateway verbinding maken met Data Factory worden geblokkeerd om zichzelf te verifiëren. Raadpleeg de vorige sectie om te controleren of uw firewall en proxy-server correct zijn geconfigureerd.

1. Als u probeert om de gateway te registreren, krijgt u de volgende fout: 'Kan niet registreren van de gateway sleutel. Voordat u de gateway sleutel opnieuw te registreren, bevestigt u dat de data management gateway een verbonden status heeft is en de Data Management Gateway Host-Service wordt gestart."
2. Wanneer u Configuration Manager opent, ziet u de status als "Niet-verbonden" of "Verbinding maken." Bij het weergeven van Windows-gebeurtenislogboeken, onder 'Logboeken' > 'Toepassingen en Services Logs' > 'Data Management Gateway', ziet u foutberichten, zoals de volgende fout: `Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Open poort 8050 voor referentieversleuteling.
De **instelling referenties** toepassing maakt gebruik van de binnenkomende poort **8050** relay referenties naar de gateway bij het instellen van een on-premises gekoppelde service in Azure portal. Tijdens de installatie van de gateway, standaard, de installatie van de gateway wordt deze geopend op de gatewaycomputer.

Als u een firewall van derden gebruikt, kunt u handmatig de poort 8050 openen. Als u firewall probleem tijdens de installatie van de gateway ondervindt, kunt u proberen de gateway te installeren zonder dat de firewall configureren met behulp van de volgende opdracht uit.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Als u niet de poort 8050 op de gatewaycomputer openen, gebruikt u mechanismen dan met behulp van de **instelling referenties** toepassing voor het configureren van de referenties van de gegevensopslag. U kunt bijvoorbeeld [New-AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) PowerShell-cmdlet. Zie de sectie over de instelling referenties en beveiliging op hoe de referenties voor het opslaan van gegevens kan worden ingesteld.

## <a name="update"></a>Update
Gegevensbeheergateway wordt standaard automatisch bijgewerkt wanneer een nieuwere versie van de gateway beschikbaar is. De gateway is niet bijgewerkt totdat alle geplande taken worden uitgevoerd. Er zijn geen taken meer worden verwerkt door de gateway, totdat de updatebewerking is voltooid. Als de update is mislukt, is gateway teruggedraaid naar de oude versie.

Ziet u de tijd van de geplande update in de volgende locaties:

* De eigenschappenpagina van de gateway in Azure portal.
* Startpagina van Data Management Gateway Configuratiebeheer
* Systeembericht lade melding.

Het tabblad Start van Data Management Gateway Configuratiebeheer wordt weergegeven voor het bijwerken van de planning en de laatste keer dat de gateway is geïnstalleerd/bijgewerkt.

![Updates plannen](media/data-factory-data-management-gateway/UpdateSection.png)

U kunt de update direct te installeren of wacht op de gateway kunnen automatisch worden bijgewerkt op het geplande tijdstip. Bijvoorbeeld, de volgende afbeelding ziet u de melding wordt weergegeven in de Gateway Configuration Manager samen met de knop bijwerken waarop u klikken kunt om het te installeren onmiddellijk.

![Update in DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Het meldingsbericht in het systeemvak eruit zoals in de volgende afbeelding:

![Systeemvak systeembericht](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

U ziet de status van update-bewerking (handmatig of automatisch) in het systeemvak. Als u Gateway Configuration Manager volgende keer start, ziet u een bericht op het systeemvak dat de gateway is bijgewerkt, samen met een koppeling naar [wat is er nieuw onderwerp](data-factory-gateway-release-notes.md).

### <a name="to-disableenable-auto-update-feature"></a>Aan de functie voor automatisch bijwerken inschakelen/uitschakelen
U kunt in-/ uitschakelen de functie voor automatisch bijwerken door de volgende stappen:

[Voor één knooppunt gateway]
1. Start Windows PowerShell op de gatewaycomputer.
2. Ga naar de map C:\Program Files\Microsoft integratie Runtime\3.0\PowerShellScript\.
3. Voer de volgende opdracht om het automatisch bijwerken inschakelen functie uit te schakelen.

    ```PowerShell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -off
    ```
4. Aan deze weer inschakelen:

    ```PowerShell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -on
    ```
[Voor de maximaal beschikbare en schaalbare gateway met meerdere knooppunten](data-factory-data-management-gateway-high-availability-scalability.md)
1. Start Windows PowerShell op de gatewaycomputer.
2. Ga naar de map C:\Program Files\Microsoft integratie Runtime\3.0\PowerShellScript\.
3. Voer de volgende opdracht om het automatisch bijwerken inschakelen functie uit te schakelen.

    Een extra AuthKey-parameter is vereist voor de gateway met hoge beschikbaarheid.
    ```PowerShell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -off -AuthKey <your auth key>
    ```
4. Aan deze weer inschakelen:

    ```PowerShell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -on -AuthKey <your auth key>
    ```

## <a name="configuration-manager"></a>Configuratiebeheer
Nadat u de gateway hebt geïnstalleerd, kunt u Data Management Gateway Configuration Manager in een van de volgende manieren starten:

1. In de **zoeken** venster, type **Data Management Gateway** voor toegang tot dit hulpprogramma.
2. Voer het uitvoerbare bestand **ConfigManager.exe** in de map: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### <a name="home-page"></a>Startpagina
De startpagina kunt u de volgende acties uitvoeren:

* De status van de gateway (verbonden met de cloudservice enz.).
* **Registreren** met behulp van een sleutel van de portal.
* **Stop** en start de **Data Management Gateway Host-service** op de gatewaycomputer.
* **Updates plannen** op een bepaald tijdstip van de dagen.
* De datum waarop de gateway is **laatst bijgewerkt**.

### <a name="settings-page"></a>De pagina Instellingen
De pagina instellingen kunt u de volgende acties uitvoeren:

* Weergeven, wijzigen en exporteren **certificaat** gebruikt door de gateway. Dit certificaat wordt gebruikt voor het versleutelen van de referenties van de gegevensbron.
* Wijziging **HTTPS-poort** voor het eindpunt. Een poort voor het instellen van referenties voor de gegevensbron van de gateway wordt geopend.
* **Status** van het eindpunt
* Weergave **SSL-certificaat** voor SSL-communicatie tussen de portal en de gateway wordt gebruikt voor het instellen van referenties voor gegevensbronnen.

### <a name="remote-access-from-intranet"></a>Externe toegang via intranet
Deze functionaliteit wordt in de toekomst worden ingeschakeld. In de toekomstige updates (v3.4 of hoger) ontvangt u inschakelen / uitschakelen van externe connectiviteit die vandaag plaatsvindt tijdens het gebruik van PowerShell of Referentiebeheer-toepassing voor het versleutelen van referenties met behulp van poort 8050 (Zie hierboven).

### <a name="diagnostics-page"></a>Pagina Diagnostische gegevens
De diagnostische pagina kunt u de volgende acties uitvoeren:

* Inschakelen van uitgebreide **logboekregistratie**, logboeken bekijken in Logboeken en logboeken naar Microsoft verzenden als er een fout opgetreden is.
* **Verbinding testen** met een gegevensbron.

### <a name="help-page"></a>Help-pagina
De Help-pagina bevat de volgende informatie:

* Korte beschrijving van de gateway
* Versienummer
* Koppelingen naar online-help, privacyverklaring en licentie-overeenkomst.

## <a name="monitor-gateway-in-the-portal"></a>Monitor-gateway in de portal
U kunt de near-real-time-momentopname van Resourcegebruik (CPU, geheugen, network(in/out), enzovoort) op een gateway-apparaat weergeven in de Azure-portal.

1. Navigeer naar de startpagina van uw data factory in Azure portal, en klikt u op **gekoppelde services** tegel.

    ![Startpagina van de gegevensfactory](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png)
2. Selecteer de **gateway** in de **gekoppelde services** pagina.

    ![Gekoppelde services-pagina](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In de **Gateway** pagina, ziet u het geheugen en CPU-gebruik van de gateway.

    ![CPU- en geheugengebruik van gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png)
4. Schakel **geavanceerde instellingen** voor meer informatie, zoals netwerkgebruik.
    
    ![Geavanceerde controle van gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

De volgende tabel bevat beschrijvingen van de kolommen in de **Gatewayknooppunten** lijst:

Bewaking van eigenschap | Description
:------------------ | :----------
Name | De naam van de logische-gateway en de knooppunten die zijn gekoppeld aan de gateway. Knooppunt is een on-premises Windows-computer waarop de gateway is geïnstalleerd. Zie voor meer informatie over meer dan één knooppunt (maximaal vier knooppunten) die in één logische gateway [Data Management Gateway - hoge beschikbaarheid en schaalbaarheid](data-factory-data-management-gateway-high-availability-scalability.md).
Status | De status van de logische-gateway en de gateway-knooppunten. Voorbeeld: Online/Offline/Limited/etc. Zie voor meer informatie over deze statussen [gatewaystatus](#gateway-status) sectie.
Versie | Toont de versie van de logische gateway en de gateway-knooppunt. De versie van de logische gateway wordt bepaald op basis van de versie van het merendeel van de knooppunten in de groep. Als er knooppunten met verschillende versies in de logische gateway-instellingen, alleen de knooppunten met het versienummer hetzelfde als de functie logische gateway correct zijn. Anderen in de beperkte modus zijn en moeten handmatig worden bijgewerkt (alleen als automatisch bijwerken is mislukt).
Beschikbaar geheugen | Beschikbaar geheugen op een gateway-knooppunt. Deze waarde is een momentopname van een bijna realtime.
CPU-gebruik | CPU-gebruik van een gateway-knooppunt. Deze waarde is een momentopname van een bijna realtime.
Netwerken (In/uit) | Het netwerkgebruik van een gateway-knooppunt. Deze waarde is een momentopname van een bijna realtime.
Gelijktijdige taken (actief / beperken) | Het aantal taken of taken die worden uitgevoerd op elk knooppunt. Deze waarde is een momentopname van een bijna realtime. Limiet geeft aan dat het maximum aantal gelijktijdige taken voor elk knooppunt. Deze waarde is gedefinieerd op basis van de machinegrootte. U kunt de limiet voor gelijktijdige taakuitvoering in geavanceerde scenario's waarbij CPU/geheugen/netwerk wordt benut, maar activiteiten zijn time-outs opschalen verhogen. Deze mogelijkheid is ook beschikbaar met één knooppunt gateway (zelfs als de functie voor schaalbaarheid en beschikbaarheid is niet ingeschakeld).
Rol | Er zijn twee soorten rollen in een gateway met meerdere knooppunten - functie voor berichtverzending en -werkrollen. Alle knooppunten zijn werknemers, wat betekent dat ze kunnen allemaal worden gebruikt voor het uitvoeren van taken. Er is slechts één functie voor berichtverzending knooppunt, die wordt gebruikt voor het ophalen van taken/taken uit cloudservices en ze verzenden naar verschillende worker-knooppunten (inclusief zelf).

In deze pagina ziet u enkele instellingen die meer geschikt wanneer er twee of meer knooppunten (scale-out scenario) in de gateway. Zie [Data Management Gateway - hoge beschikbaarheid en schaalbaarheid](data-factory-data-management-gateway-high-availability-scalability.md) voor meer informatie over het instellen van een gateway met meerdere knooppunten.

### <a name="gateway-status"></a>Gatewaystatus
De volgende tabel bevat de mogelijke statussen van een **gateway-knooppunt**:

Status  | Opmerkingen /-scenario 's
:------- | :------------------
Online | Knooppunt is verbonden met Data Factory-service.
Offline | Knooppunt is offline.
Upgraden | Het knooppunt wordt automatisch bijgewerkt.
Beperkt | Vanwege een probleem met de netwerkverbinding. Kan worden veroorzaakt door HTTP-poort 8050 probleem, service bus-connectiviteitsprobleem of synchronisatieprobleem in de referentie.
Inactief | Er is een knooppunt in een van de configuratie van andere knooppunten van de meeste andere configuratie.<br/><br/> Een knooppunt mag inactief zijn als er geen verbinding met andere knooppunten.

De volgende tabel bevat de mogelijke statussen van een **logische gateway**. Status van de gateway, is afhankelijk van de status van de gateway-knooppunten.

Status | Opmerkingen
:----- | :-------
Moet worden geregistreerd | Kan geen knooppunt is nog geregistreerd bij deze logische gateway
Online | Gateway-knooppunten zijn online
Offline | Kan geen knooppunt in de online status.
Beperkt | Niet alle knooppunten in deze gateway zijn in orde. Deze status is een waarschuwing dat een bepaald knooppunt is mogelijk niet beschikbaar. <br/><br/>Kan worden veroorzaakt door synchronisatieprobleem in de referentie op knooppunt verzender/werknemer.

## <a name="scale-up-gateway"></a>Gateway opschalen
U kunt configureren dat het aantal **gelijktijdige verkeer taken** die kunnen worden uitgevoerd op een knooppunt de mogelijkheid om gegevens te verplaatsen tussen on-premises en cloud kan worden uitgebreid gegevensarchieven.

Als het beschikbare geheugen en CPU niet goed worden gebruikt, maar de inactieve capaciteit 0 is, moet u omhoog schalen door het aantal gelijktijdige taken die kunnen worden uitgevoerd op een knooppunt. U kunt ook omhoog schalen wanneer activiteiten zijn time-outs opgetreden omdat de gateway overbelast is. In de geavanceerde instellingen van een gateway-knooppunt, kunt u de maximale capaciteit voor een knooppunt verhogen.

## <a name="troubleshooting-gateway-issues"></a>Het oplossen van problemen met gateway
Zie [oplossen van dergelijke problemen](data-factory-troubleshoot-gateway-issues.md) artikel voor informatie/tips voor het oplossen van problemen bij het gebruik van de data management gateway.

## <a name="move-gateway-from-one-machine-to-another"></a>Gateway van een machine te verplaatsen naar een andere
Deze sectie bevat stappen voor het verplaatsen van gateway-client van een machine naar een andere computer.

1. In de portal, gaat u naar de **Data Factory-startpagina**, en klik op de **gekoppelde Services** tegel.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Selecteer uw gateway in de **GEGEVENSGATEWAYS** sectie van de **gekoppelde Services** pagina.

    ![Gekoppelde Services-pagina met gateway geselecteerd](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In de **gegevensgateway** pagina, klikt u op **downloaden en installeren van de gegevensgateway**.

    ![Koppeling van de gateway downloaden](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In de **configureren** pagina, klikt u op **downloaden en installeren van de gegevensgateway**, en volg de instructies voor het installeren van de data gateway op de machine.

    ![De pagina configureren](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Houd de **Microsoft Data Management Gateway Configuratiebeheer** openen.

    ![Configuratiebeheer](./media/data-factory-data-management-gateway/ConfigurationManager.png)
6. In de **configureren** pagina in de portal, klikt u op **opnieuw sleutel** op de opdrachtbalk en klik op **Ja** voor het waarschuwingsbericht staan aangegeven. Klik op **kopieerknop** naast belangrijke tekst waarmee de sleutel naar het Klembord gekopieerd. De gateway op de oude machine niet meer werkt zoals snel u de sleutel opnieuw maken.

    ![Sleutel opnieuw maken](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Plak de **sleutel** in het tekstvak in de **Gateway registreren** pagina van de **Data Management Gateway Configuratiebeheer** op uw computer. (optioneel) Klik op **Show gateway sleutel** selectievakje in om te zien van de belangrijkste tekst.

    ![Sleutel kopiëren en registreren](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Klik op **registreren** de gateway registreren bij de cloudservice.
9. Op de **instellingen** tabblad **wijzigen** Selecteer hetzelfde certificaat dat is gebruikt met de oude gateway, voer de **wachtwoord**, en klikt u op **Voltooien**.

   ![Certificaat opgeven](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   U kunt een certificaat van de oude gateway exporteren door de volgende stappen: Data Management Gateway Configuration Manager op de oude machine starten, gaat u naar de **certificaat** tabblad **exporteren** knop en volg de instructies.
10. Na de registratie van de gateway is gelukt, ziet u de **registratie** ingesteld op **geregistreerde** en **Status** ingesteld op **gestart** op de startpagina van de Gateway Configuration Manager.

## <a name="encrypting-credentials"></a>Referenties versleutelen
Voor het versleutelen van referenties in de Data Factory-Editor, voer de volgende stappen uit:

1. Webbrowser starten op de **gatewaycomputer**, gaat u naar [Azure-portal](https://portal.azure.com). Zoeken naar uw data factory, indien nodig, opent u de gegevensfactory in de **DATA FACTORY** pagina en klik vervolgens op **maken en implementeren** Data Factory-Editor gestart.
2. Klik op een bestaande **gekoppelde service** in de structuurweergave wordt weergegeven om te bekijken van de JSON-definitie of een gekoppelde service waarvoor een data management gateway maken (bijvoorbeeld: SQL Server of Oracle).
3. In de JSON-editor voor de **gatewayName** eigenschap, voer de naam van de gateway.
4. Voer de naam van de server voor de **gegevensbron** eigenschap in de **connectionString**.
5. Voer de naam van de database voor de **Initial Catalog** eigenschap in de **connectionString**.
6. Klik op **versleutelen** knop op de opdrachtbalk waarmee wordt gestart van de Klik-zodra **Referentiebeheer** toepassing. U ziet de **instelling referenties** in het dialoogvenster.

    ![Dialoogvenster voor referenties instellen](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In de **instelling referenties** dialoogvenster vak, voer de volgende stappen uit:
   1. Selecteer **verificatie** dat u wilt dat de Data Factory-service gebruiken om te verbinden met de database.
   2. Voer de naam van de gebruiker die toegang tot de database heeft voor de **gebruikersnaam** instelling.
   3. Wachtwoord invoeren voor de gebruiker voor de **wachtwoord** instelling.
   4. Klik op **OK** voor het versleutelen van referenties en sluit het dialoogvenster.
8. U ziet een **encryptedCredential** eigenschap in de **connectionString** nu.

    ```JSON
    {
        "name": "SqlServerLinkedService",
        "properties": {
            "type": "OnPremisesSqlServer",
            "description": "",
            "typeProperties": {
                "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                "gatewayName": "adftutorialgateway"
            }
        }
    }
    ```
Als u toegang de portal van een virtuele machine die verschilt van het gateway-apparaat tot, moet u ervoor zorgen dat de toepassing Referentiebeheer verbinding met het gateway-apparaat maken kunt. Als de toepassing het gateway-apparaat niet bereiken kan, is het niet kunt u referenties instellen voor de gegevensbron en om verbinding met de gegevensbron te testen.

Wanneer u gebruikt de **instelling referenties** toepassing, de portal versleutelt de referenties met het certificaat dat is opgegeven in de **certificaat** tabblad van de **Gateway Configuration Manager**  op de gatewaycomputer.

Als u een op API gebaseerde aanpak voor het versleutelen van de referenties zoekt, kunt u de [New-AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) PowerShell-cmdlet voor het versleutelen van referenties. De cmdlet maakt gebruik van het certificaat dat de gateway is geconfigureerd om te gebruiken voor het versleutelen van de referenties. Toevoegen van versleutelde referenties voor de **EncryptedCredential** element van de **connectionString** in de JSON. U gebruikt de JSON met de [New-AzDataFactoryLinkedService](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactorylinkedservice) cmdlet of in de Data Factory-Editor.

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

Er is een meer aanpak voor het instellen van referenties met behulp van Data Factory-Editor. Als u een gekoppelde SQL Server-service maakt met behulp van de editor en u referenties als tekst zonder opmaak invoeren, kan de referenties zijn versleuteld met behulp van een certificaat dat is eigenaar van de Data Factory-service. Maakt geen gebruik van het certificaat dat de gateway is geconfigureerd voor gebruik. Deze aanpak is wellicht iets sneller in sommige gevallen, is het minder veilig. Daarom raden wij aan dat u deze aanpak alleen voor ontwikkeling/testen doeleinden volgt.

## <a name="powershell-cmdlets"></a>PowerShell-cmdlets
In deze sectie wordt beschreven hoe u maken en registreren van een gateway met behulp van Azure PowerShell-cmdlets.

1. Start **Azure PowerShell** in de beheerdersmodus.
2. Meld u aan bij uw Azure-account aan door de volgende opdracht uit en uw Azure-referenties in te voeren.

    ```PowerShell
    Connect-AzAccount
    ```
3. Gebruik de **New-AzDataFactoryGateway** te maken van een logische gateway als volgt:

    ```PowerShell
    $MyDMG = New-AzDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    **Van de voorbeeldopdracht en het resultaat**:

    ```
    PS C:\> $MyDMG = New-AzDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

    Name              : MyGateway
    Description       : gateway for walkthrough
    Version           :
    Status            : NeedRegistration
    VersionStatus     : None
    CreateTime        : 9/28/2014 10:58:22
    RegisterTime      :
    LastConnectTime   :
    ExpiryTime        :
    ProvisioningState : Succeeded
    Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI
    ```

1. Ga naar de map in Azure PowerShell: **C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**. Voer **RegisterGateway.ps1** die zijn gekoppeld aan de lokale variabele **$Key** zoals wordt weergegeven in de volgende opdracht uit. Met dit script registreert de clientagent geïnstalleerd op uw computer met de logische gateway die u eerder hebt gemaakt.

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    U kunt de gateway op een externe computer registreren met behulp van de parameter IsRegisterOnRemoteMachine. Voorbeeld:

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. U kunt de **Get-AzDataFactoryGateway** cmdlet om op te halen van de lijst met Gateways in uw gegevensfactory. Wanneer de **Status** bevat **online**, betekent dit dat uw gateway is gereed om te gebruiken.

    ```PowerShell        
    Get-AzDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
Kunt u een gateway met de **Remove-AzDataFactoryGateway** cmdlet, update en beschrijving voor het gebruik van een gateway de **Set AzDataFactoryGateway** cmdlets. Zie voor de syntaxis en andere informatie over deze cmdlets, Data Factory Cmdlet Reference.  

### <a name="list-gateways-using-powershell"></a>Lijst met gateways met behulp van PowerShell

```PowerShell
Get-AzDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a>Verwijderen van de gateway met behulp van PowerShell

```PowerShell
Remove-AzDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```

## <a name="next-steps"></a>Volgende stappen
* Zie [gegevens verplaatsen tussen on-premises en cloud gegevensarchieven](data-factory-move-data-between-onprem-and-cloud.md) artikel. In het scenario maakt u een pijplijn die de gateway wordt gebruikt om gegevens te verplaatsen van een on-premises SQL Server-database naar een Azure-blob.
