---
title: Zelf-hostende integratieruntime maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van zelf-hostende integratieruntime in Azure Data Factory, waarmee data factory's voor toegang tot gegevensarchieven in een particulier netwerk.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 705f2ce674a31d7dda4d87d893078a2ade26e327
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42443387"
---
# <a name="how-to-create-and-configure-self-hosted-integration-runtime"></a>Over het maken en configureren van zelfgehoste Cloudintegratieruntime
De Integration Runtime (IR) is de rekeninfrastructuur die door Azure Data Factory gebruikt om u te bieden mogelijkheden voor gegevensintegratie in verschillende netwerkomgevingen. Zie voor meer informatie over IR [overzicht van Integration Runtime](concepts-integration-runtime.md).

Een zelf-hostende integratieruntime is in staat van het uitvoeren van kopieeractiviteit tussen cloudgegevens in een-winkels en een gegevensarchief in een particulier netwerk en het verzenden van transformatieactiviteiten op basis van rekenresources in een on-premises of Azure Virtual Network. Zelf-hostende integration runtime moet installeren op een on-premises computer of een virtuele machine in een privénetwerk.  

Dit document beschrijft hoe u kunt maken en configureren van zelfgehoste IR.

## <a name="high-level-steps-to-install-self-hosted-ir"></a>De stappen op hoog niveau voor het installeren van de zelf-hostende IR
1. Een zelf-hostende integratieruntime maken. U kunt ADF UI gebruiken voor het maken van de zelf-hostende IR Hier volgt een voorbeeld van PowerShell:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
2. Download en installeer de zelf-hostende integratieruntime (op de lokale computer).
3. Verificatiesleutel ophalen en zelf-hostende integratieruntime registreren met de sleutel. Hier volgt een voorbeeld van PowerShell:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime.  
    ```

## <a name="setting-up-self-hosted-ir-on-azure-vm-using-azure-resource-manager-template-automatation"></a>Instellen van de zelf-hostende IR op virtuele Azure-machine met behulp van Azure Resource Manager-sjabloon (automatation)
U kunt zelf-hostende IR setup over het gebruik van een Azure-VM automatiseren [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vms-with-selfhost-integration-runtime). Dit biedt een eenvoudige manier om een volledig functionerende zelfgehoste IR in Azure VNet met hoge Avalaibility en schaalbaarheid functie (zolang u het aantal knooppunten aan 2 of hoger worden ingesteld).

## <a name="command-flow-and-data-flow"></a>Opdracht stroom en de gegevensstroom
Wanneer u de gegevens tussen on-premises en cloud verplaatst, de activiteit maakt gebruik van een zelf-hostende integratieruntime de gegevens van on-premises gegevensbron overdraagt naar de cloud en vice versa mogelijk.

Hier volgt een gegevensstroom op hoog niveau voor een samenvatting van de stappen voor het exemplaar met de zelf-hostende IR:

![Overzicht](media\create-self-hosted-integration-runtime\high-level-overview.png)

1. Data-ontwikkelaar Hiermee maakt u een zelf-hostende integratieruntime binnen een Azure data factory met behulp van een PowerShell-cmdlet. De Azure-portal biedt momenteel geen ondersteuning voor deze functie.
2. Data-ontwikkelaar maakt een gekoppelde service voor een on-premises gegevensarchief door het exemplaar van de zelf-hostende integration runtime die moet worden gebruikt verbinding maken met de opgeslagen gegevens op te geven. Als onderdeel van het instellen van de gekoppelde service, data-ontwikkelaar maakt gebruik van de toepassing ' Referentiebeheer ' (op dit moment niet ondersteund) voor het instellen van verificatietypen en -referenties. Het dialoogvenster voor gebruikersreferenties manager toepassing communiceert met het gegevensarchief voor het testen van verbinding en de zelf-hostende integratieruntime referenties op te slaan.
   - Zelf-hostende integration runtime-knooppunt versleutelt de referenties met behulp van Windows Data Protection Application Programming Interface (DPAPI) en lokaal opgeslagen. Als meerdere knooppunten voor hoge beschikbaarheid zijn ingesteld, worden de referenties verder gesynchroniseerd op andere knooppunten. Elk knooppunt worden versleuteld met behulp van DPAPI en lokaal opgeslagen. Synchronisatie van referenties is transparant voor de ontwikkelaar van de gegevens en wordt verwerkt door de zelf-hostende IR    
   - Data Factory-service communiceert met de zelf-hostende integratieruntime voor planning en beheer van taken via **besturingskanaal** die gebruikmaakt van een gedeelde Azure service bus-wachtrij. Wanneer de activiteitstaak van een worden uitgevoerd moet, wordt in Data Factory de aanvraag, samen met een referentie-informatie wachtrijen (als referenties niet al in de zelf-hostende integratieruntime opgeslagen zijn). De taak zelf-hostende integratieruntime begint nadat het opvragen van configuratiegegevens bij de wachtrij.
   - Zelf-hostende integratieruntime worden gegevens gekopieerd van een on-premises gegevensopslag naar een opslag in de cloud, of vice versa, afhankelijk van hoe de copy-activiteit is geconfigureerd in de pijplijn. Voor deze stap worden de zelf-hostende integratieruntime rechtstreeks communiceert met cloud-gebaseerde services zoals Azure Blob Storage via een beveiligd kanaal voor (HTTPS).

## <a name="considerations-for-using-self-hosted-ir"></a>Overwegingen voor het gebruik van zelf-hostende IR

- Een enkele zelf-hostende integratieruntime kan worden gebruikt voor meerdere on-premises gegevensbronnen. Echter, een **één zelf-hostende integratieruntime is gekoppeld aan slechts één Azure data factory** en kan niet worden gedeeld met een andere data factory.
- U kunt hebben **slechts één exemplaar van de zelf-hostende integratieruntime** op één computer geïnstalleerd. Stel dat u hebt twee data factory's die nodig hebt voor toegang tot on-premises gegevensbronnen, moet u zelf-hostende integratieruntime installeren op twee on-premises computers. Met andere woorden, is een zelf-hostende integratieruntime gekoppeld aan een specifieke data factory
- De **zelf-hostende integratieruntime niet hoeft te worden op dezelfde computer als de gegevensbron**. Echter, dat u hoeft zelf-hostende integratieruntime dichter bij de gegevensbron verkort de termijn voor de zelf-hostende integratieruntime verbinding maken met de gegevensbron. Het is raadzaam dat u installeert de zelf-hostende integratieruntime op een computer die verschilt van de hosts on-premises gegevensbron. Wanneer de zelf-hostende integration runtime en de gegevensbron zich op verschillende computers bevinden, kunnen de zelf-hostende integratieruntime niet concurreren voor resources met de gegevensbron.
- U kunt hebben **meerdere zelf-hostende integratieruntimes op verschillende computers verbinding maken met dezelfde on-premises gegevensbron**. Bijvoorbeeld, u hebt twee zelf-hostende integratieruntime voor de twee data factory's maar dezelfde on-premises gegevensbron is geregistreerd bij de gegevensfactory.
- Als u al een gateway is geïnstalleerd op uw computer voor een **Power BI** scenario, installeert een **afzonderlijke zelf-hostende integratieruntime voor Azure Data Factory** op een andere computer.
- Zelf-hostende integratieruntime moet worden gebruikt voor de ondersteuning voor integratie van gegevens binnen Azure Virtual Network.
- Uw gegevensbron behandelen als een on-premises gegevensbron (die zich achter een firewall bevindt), zelfs wanneer u gebruikt **ExpressRoute**. Gebruik de zelf-hostende integratieruntime om verbinding tussen de service en de gegevensbron te maken.
- U kunt de zelf-hostende integratieruntime moet gebruiken, zelfs als het gegevensarchief is in de cloud op een **Azure IaaS-virtuele machine**.
- Taken mislukken in een zelfgehoste Integration Runtime is geïnstalleerd op een Windows-Server op welke FIPS-compatibele versleuteling is ingeschakeld. U kunt dit probleem omzeilen, FIPS-compatibele versleuteling op de server uitschakelen Als u wilt uitschakelen FIPS-compatibele versleuteling, wijzigt u de volgende registerwaarde van 1 (ingeschakeld) op 0 (uitgeschakeld): `HKLM\System\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy\Enabled`.

## <a name="prerequisites"></a>Vereisten

- De ondersteunde **besturingssysteem** versies zijn Windows 7 Service Pack 1, Windows 8.1, Windows 10, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016. Installatie van de zelf-hostende integratieruntime op een **domeincontroller wordt niet ondersteund**.
- **.NET framework 4.6.1 of hoger** is vereist. Als u zelf-hostende integratieruntime op een computer met Windows 7 installeert, installeert u .NET Framework 4.6.1 of hoger. Zie [.NET Framework-systeemvereisten](/dotnet/framework/get-started/system-requirements) voor meer informatie.
- De aanbevolen **configuratie** voor de zelf-hostende integratieruntime machine ten minste 2 GHz, 4 cores, 8 GB RAM-geheugen en 80 GB-schijf is.
- Als de hostmachine in de slaapstand, reageert de zelf-hostende integratieruntime niet op aanvragen voor gegevens. Daarom een juiste energiebeheerschema op de computer configureren voor de installatie van de zelf-hostende integratieruntime. Als de machine is geconfigureerd voor de slaapstand, wordt een bericht gevraagd om de zelf-hostende integration runtime-installatie.
- U moet een beheerder op de computer installeren en configureren van de zelf-hostende integratieruntime is.
- Als u uitvoeringen van activiteit kopiëren gebeuren op een specifieke frequentie, volgt het Resourcegebruik (CPU, geheugen) op de computer ook hetzelfde patroon met piek- en niet-actieve tijden. Gebruik van resources is ook afhankelijk sterk de hoeveelheid gegevens die wordt verplaatst. Wanneer meerdere kopie-taken uitgevoerd worden, ziet u Resourcegebruik tijdens piektijden omhoog.

## <a name="installation-best-practices"></a>Best practices voor installatie
Zelf-hostende integratieruntime kan worden geïnstalleerd door het downloaden van een MSI-installatiepakket van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Zie [gegevens verplaatsen tussen on-premises en cloud artikel](tutorial-hybrid-copy-powershell.md) voor stapsgewijze instructies.

- Energiebeheerschema op de hostcomputer voor de zelf-hostende integratieruntime zo configureren dat de machine sluimerstand niet. Als de hostmachine in de slaapstand, schakelt de zelf-hostende integratieruntime offline.
- Back-up van de referenties die zijn gekoppeld aan de zelf-hostende integratieruntime regelmatig.

## <a name="install-and-register-self-hosted-ir-from-download-center"></a>Installeren en registreren zelfgehoste IR uit het Downloadcentrum

1. Navigeer naar [downloadpagina van Microsoft Integration Runtime](https://www.microsoft.com/download/details.aspx?id=39717).
2. Klik op **downloaden**, selecteer de juiste versie (**32-bits** vs. **64-bits**), en klikt u op **volgende**.
3. Voer de **MSI** rechtstreeks of sla deze op uw harde schijf en uitvoeren.
4. Op de **Welkom** weergeeft, schakelt een **taal** klikt u op **volgende**.
5. **Accepteer** de gebruiksrechtovereenkomst en klik op **volgende**.
6. Selecteer **map** de zelf-hostende integratieruntime installeren en op **volgende**.
7. Op de **gereed voor installatie** pagina, klikt u op **installeren**.
8. Klik op **voltooien** om installatie te voltooien.
9. Ophalen van de verificatiesleutel met behulp van Azure PowerShell. PowerShell-voorbeeld voor het ophalen van de verificatiesleutel:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```
11. Op de **registreert Integration Runtime (zelf-hostend)** pagina van Microsoft Integration Runtime Configuration Manager die worden uitgevoerd op uw computer, voer de volgende stappen uit:
    1. Plak de **verificatiesleutel** in het tekstvak.
    2. Klik desgewenst op **Show verificatiesleutel** de belangrijkste tekst wilt zien.
    3. Klik op **registreren**.


## <a name="high-availability-and-scalability"></a>Hoge beschikbaarheid en schaalbaarheid
Een zelfgehoste Integration Runtime kan worden gekoppeld aan meerdere on-premises computers. Deze machines worden knooppunten genoemd. U kunt maximaal vier knooppunten die zijn gekoppeld aan een zelfgehoste Cloudintegratieruntime hebben. De voordelen van meerdere knooppunten (on-premises machines met de gateway is geïnstalleerd) voor een logische gateway zijn:
1. Hogere beschikbaarheid van zelfgehoste Integration Runtime, zodat dit niet meer het enige storingspunt in uw Big Data-oplossing of cloud gegevensintegratie met Azure Data Factory, ervoor te zorgen dat bedrijfscontinuïteit met maximaal 4 knooppunten.
2. Verbeterde prestaties en de doorvoer tijdens de verplaatsing van gegevens tussen on-premises en cloud gegevensarchieven. Meer informatie opvragen over [prestaties vergelijkingen](copy-activity-performance.md).

U kunt meerdere knooppunten koppelen gewoon de zelfgehoste Cloudintegratieruntime door software te installeren vanuit de [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=39717) en door deze te registreren door een van de verificatiesleutels verkregen uit Nieuwe AzureRmDataFactoryV2IntegrationRuntimeKey cmdlet zoals beschreven in de [zelfstudie](tutorial-hybrid-copy-powershell.md)

> [!NOTE]
> U hoeft geen nieuwe zelfgehoste Integration Runtime maken voor het koppelen van elk knooppunt. U kunt de zelf-hostende integratieruntime op een andere computer installeren en registreren met behulp van de dezelfde verificatiesleutel. 

> [!NOTE]
> Voordat u een ander knooppunt voor toevoegt **hoge beschikbaarheid en schaalbaarheid**, zorg ervoor dat **externe toegang tot intranet** optie **ingeschakeld** op 1 knooppunt (Microsoft Integration Runtime Configuration Manager -> Instellingen -> extern toegang tot intranet). 

### <a name="scale-considerations"></a>Overwegingen voor schalen

#### <a name="scale-out"></a>Uitschalen

Wanneer de **er weinig beschikbaar geheugen op de zelf-hostende IR is** en de **CPU-gebruik is hoog**, een nieuw knooppunt toevoegen helpt u scale-out van de belasting tussen computers. Als activiteiten zijn mislukken vanwege een time-out of zelf-hostende IR-knooppunt offline, is het handig als u een knooppunt aan de gateway toevoegt.

#### <a name="scale-up"></a>Omhoog schalen

Als het beschikbare geheugen en CPU niet goed worden gebruikt, maar de uitvoering van gelijktijdige taken de limiet is bereikt, moet u omhoog schalen door het aantal gelijktijdige taken die kunnen worden uitgevoerd op een knooppunt. U kunt ook omhoog schalen wanneer activiteiten zijn time-outs opgetreden omdat de zelf-hostende IR is overbelast. Zoals weergegeven in de volgende afbeelding, kunt u de maximale capaciteit voor een knooppunt verhogen.  

![](media\create-self-hosted-integration-runtime\scale-up-self-hosted-IR.png)

### <a name="tlsssl-certificate-requirements"></a>Vereisten voor TLS/SSL-certificaat

Hier volgen de vereisten voor het TLS/SSL-certificaat dat wordt gebruikt voor het beveiligen van communicatie tussen de integration runtime-knooppunten:

- Het certificaat moet een openbaar vertrouwde X509 v3-certificaat. U wordt aangeraden dat u gebruik van certificaten die zijn uitgegeven door een openbare (derde) certificeringsinstantie (CA).
- Elk knooppunt voor integration runtime moet dit certificaat vertrouwen.
- Jokertekens certificaten worden ondersteund. Als uw FQDN-naam is **node1.domain.contoso.com**, kunt u ***. domain.contoso.com** als onderwerpnaam van het certificaat.
- SAN-certificaten worden niet aanbevolen, omdat alleen het laatste item van de alternatieve namen voor onderwerpen worden gebruikt en alle andere worden genegeerd vanwege een beperking. Bijvoorbeeld u hebt een SAN-certificaat waarvan SAN zijn **node1.domain.contoso.com** en **node2.domain.contoso.com**, kunt u dit certificaat alleen gebruiken op de machine waarvan FQDN **node2.domain.contoso.com**.
- Ondersteunt sleutelgrootte ondersteund door Windows Server 2012 R2 voor SSL-certificaten.
- Het certificaat met CNG sleutels worden niet ondersteund.  

## <a name="sharing-the-self-hosted-integration-runtime-ir-with-multiple-data-factories"></a>De zelf-hostende Integration Runtime (IR) delen met meerdere data factory 's

U kunt een bestaande zelf-hostende integration runtime-infrastructuur dat u al ingesteld in een data factory hebt mogelijk opnieuw gebruiken. Hiermee kunt u maken een **gekoppelde zelf-hostende integratieruntime** in een andere data factory door te verwijzen naar een reeds bestaande zelf-hostende IR (gedeeld).

#### <a name="terminologies"></a>**Terminologie**

- **Gedeelde IR** : de oorspronkelijke zelf-hostende IR die wordt uitgevoerd op een fysieke infrastructuur.  
- **Gekoppelde IR** – de IR die verwijst naar een andere gedeelde IR. Dit is een logische IR en maakt gebruik van de infrastructuur van een andere zelf-hostende IR (gedeeld).

#### <a name="high-level-steps-for-creating-a-linked-self-hosted-ir"></a>Hoog niveau stappen voor het maken van een gekoppelde zelf-hostende IR

In de zelf-hostende IR moet worden gedeeld

1. Machtiging verlenen aan de Data Factory in die u wilt maken van de gekoppelde IR. 

   ![](media\create-self-hosted-integration-runtime\grant-permissions-IR-sharing.png)

2. Houd er rekening mee de **Resource-ID** van de zelf-hostende IR moet worden gedeeld.

   ![](media\create-self-hosted-integration-runtime\4_ResourceID_self-hostedIR.png)

In de Data Factory waaraan de machtigingen zijn verleend,

3. Maak een nieuwe zelfgehoste IR (gekoppeld) en voer de bovenstaande **Resource-ID**

   ![](media\create-self-hosted-integration-runtime\6_create-linkedIR_2.png)

   ![](media\create-self-hosted-integration-runtime\6_create-linkedIR_3.png)

#### <a name="known-limitations-of-self-hosted-ir-sharing"></a>Bekende beperkingen van het delen van zelf-hostende IR

1. Aantal gekoppelde IR dat kan worden gemaakt onder één zelf-hostende IR is **20**. Neem contact op met ondersteuning als u meer nodig hebt. 

2. De data factory waarin gekoppelde IR wordt gemaakt ze beschikken over een MSI-bestand ([beheerde service-identiteit](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)). Standaard de data factory's die zijn gemaakt in de Ibiza-portal of PowerShell-cmdlets hebben MSI impliciet gemaakt. Echter, in sommige gevallen wanneer data factory is gemaakt met behulp van een Azure Resource Manager-sjabloon of SDK gebruikt, de '**identiteit**" **eigenschap moet worden ingesteld** expliciet om te controleren of Azure Resource Manager maakt een data factory een MSI-bestand bevat. 

3. De zelf-hostende IR-versie moet gelijk zijn aan of groter zijn dan 3.8.xxxx.xx. Neem [download de nieuwste versie](https://www.microsoft.com/download/details.aspx?id=39717) van zelf-hostende IR

4. De data factory waarin gekoppelde IR wordt gemaakt ze beschikken over een MSI-bestand ([beheerde service-identiteit](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)). De data factory's die zijn gemaakt in de Ibiza-portal of PowerShell-cmdlets hebben standaard MSI ([beheerde service-identiteit](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)).
impliciet gemaakt, data factory's die zijn gemaakt met Azure Resource Manager (ARM)-sjabloon of SDK vereist echter "identiteitseigenschap" moet worden ingesteld om te controleren of een MSI-bestand wordt gemaakt.

5. ADF .net SDK die ondersteuning bieden voor deze functie is versie > = 1.1.0

6. Azure PowerShell die ondersteuning bieden voor deze functie is versie > = 6.6.0 (AzureRM.DataFactoryV2 > = 0.5.7)

  > [!NOTE]
  > Deze functie is alleen beschikbaar in Azure Data Factory versie 2 

## <a name="system-tray-icons-notifications"></a>Pictogrammen in het systeemvak / meldingen

Als u de cursor op het systeem/Meldingsbericht voor een pictogram lade plaatst, vindt u meer informatie over de status van de zelf-hostende integratieruntime.

![Systeemvak systeemmeldingen](media\create-self-hosted-integration-runtime\system-tray-notifications.png)

## <a name="ports-and-firewall"></a>Poorten en firewall
Er zijn twee firewalls moet u rekening houden: **bedrijfsfirewall** die worden uitgevoerd op de centrale-router van de organisatie, en **Windows firewall** geconfigureerd als een daemon op de lokale computer waar de zelf-hostende integratieruntime is geïnstalleerd.

![Firewall](media\create-self-hosted-integration-runtime\firewall.png)

Op **bedrijfsfirewall** niveau, moet u de volgende domeinen bevinden en uitgaande poorten:

Domeinnamen | Poorten | Beschrijving
------------ | ----- | ------------
*.servicebus.windows.net | 443, 80 | Gebruikt voor communicatie met back-end Data Movement Service
*.core.windows.net | 443 | Gebruikt voor tijdelijke kopiëren met behulp van Azure-Blob (indien geconfigureerd)
*.frontend.clouddatahub.net | 443 | Gebruikt voor communicatie met back-end Data Movement Service
download.microsoft.com | 443 | Gebruikt voor het downloaden van updates

Op **Windows firewall** niveau (machine niveau), deze uitgaande poorten normaal gesproken zijn ingeschakeld. Als u niet het geval is, kunt u de domeinen en poorten dienovereenkomstig op zelf-hostend configureren integration runtime-machine.

> [!NOTE]
> Op basis van de bron / sink, mogelijk moet u whitelist aanvullende domeinen en uitgaande poorten in uw zakelijke/Windows-firewall.
>
> Voor sommige Clouddatabases (bijvoorbeeld: Azure SQL Database, Azure Data Lake, enzovoort), moet u mogelijk geaccepteerde IP-adres van de zelf-hostende integration runtime-machine op de firewallconfiguratie.

### <a name="copy-data-from-a-source-to-a-sink"></a>Gegevens kopiëren van een bron naar een sink
Zorg ervoor dat de firewall-regels correct zijn ingeschakeld op de firewall van het bedrijf, Windows firewall op de zelf-hostende integration runtime-machine en de gegevens zelf opslaan. Inschakelen van deze regels kunt de zelf-hostende integratieruntime verbinding maken met zowel bron en sink is. Inschakelen van regels voor elke gegevensopslag die bij de kopieerbewerking betrokken is.

Bijvoorbeeld, om te kopiëren vanuit een **on-premises gegevens opslaan naar een Azure SQL Database-sink of een Azure SQL Data Warehouse sink**, de volgende stappen uit:

- Uitgaand verkeer toestaan **TCP** communicatie op poort **1433** voor zowel Windows firewall en firewall van het bedrijf.
- De firewallinstellingen van Azure SQL-server als u wilt het IP-adres van de zelf-hostende integration runtime-machine toevoegen aan de lijst met toegestane IP-adressen configureren.

> [!NOTE]
> Als de firewall geen uitgaande poort 1433 toestaat, zelf-hostende integratieruntime kan niet rechtstreeks toegang hebben tot Azure SQL. U kunt in dit geval [gefaseerd kopiëren](copy-activity-performance.md) met SQL Azure-Database / SQL Azure DW. In dit scenario zou u alleen HTTPS (poort 443) nodig voor de verplaatsing van gegevens.


## <a name="proxy-server-considerations"></a>Overwegingen voor de proxy-server
Als uw bedrijfsnetwerk bevindt een proxyserver gebruikt voor toegang tot het internet, configureert u zelf-hostende integratieruntime voor het gebruik van de juiste proxy-instellingen. U kunt de proxy instellen tijdens de registratie-fase.

![Geef-proxy](media\create-self-hosted-integration-runtime\specify-proxy.png)

Zelf-hostende integratieruntime maakt gebruik van de proxy-server verbinding maken met de cloudservice. Klik op koppeling wijzigen tijdens de eerste configuratie. Ziet u het dialoogvenster proxy-instelling.

![Set-proxy](media\create-self-hosted-integration-runtime\set-http-proxy.png)

Er zijn drie opties:

- **Gebruik geen proxy**: zelf-hostende integratieruntime de proxy niet expliciet gebruikt voor het verbinding maken met cloudservices.
- **Systeemproxy gebruiken**: zelf-hostende integration runtime maakt gebruik van de proxy-instellingen die is geconfigureerd in de diahost.exe.config en diawp.exe.config. Als er geen proxy is geconfigureerd in diahost.exe.config en diawp.exe.config, verbinding zelf-hostende integratieruntime met cloudservice rechtstreeks zonder tussenkomst van proxy.
- **Aangepaste proxy gebruikt**: de HTTP-proxy-instellingen met voor zelf-hostende integratieruntime, in plaats van configuraties in diahost.exe.config en diawp.exe.config configureren. Adres en poort zijn vereist. Gebruikersnaam en wachtwoord zijn optioneel, afhankelijk van de instelling van de verificatie van uw proxy. Alle instellingen zijn versleuteld met Windows DPAPI op de zelf-hostende integratieruntime en lokaal opgeslagen op de machine.

De hostservice van integratieruntime wordt automatisch opnieuw opgestart nadat u de bijgewerkte proxy-instellingen hebt opgeslagen.

Nadat de zelf-hostende integratieruntime is geregistreerd, als u wilt weergeven of bijwerken van proxy-instellingen, gebruikt u Integration Runtime Configuration Manager.

1. Start **Microsoft Integration Runtime Configuration Manager**.
   - Schakel over naar het tabblad **Instellingen**.
   - Klik op **wijzigen** koppelen **HTTP-Proxy** sectie starten de **HTTP-Proxy instellen** dialoogvenster.
   - Nadat u op de **volgende** knop klikt, ziet u een waarschuwingsdialoogvenster waarin wordt gevraagd uw toestemming voor het opslaan van de proxy-instellingen en start de hostservice van Integration Runtime opnieuw.

U kunt weergeven en bijwerken van HTTP-proxy met behulp van Configuration Manager-hulpprogramma.

![Weergave-proxy](media\create-self-hosted-integration-runtime\view-proxy.png)

> [!NOTE]
> Als u een proxyserver met NTLM-verificatie instelt, integratieruntime Host-Service wordt uitgevoerd onder het domeinaccount. Als u het wachtwoord voor het domeinaccount dat later wijzigt, moet u bijwerken van configuratie-instellingen voor de service en start deze dienovereenkomstig opnieuw. Vanwege deze vereiste is het raadzaam dat u een speciale domeinaccount gebruiken voor toegang tot de proxyserver die vereist niet dat u het wachtwoord regelmatig bijwerken.

### <a name="configure-proxy-server-settings"></a>Proxyserverinstellingen configureren

Als u selecteert **systeemproxy gebruiken** instellen voor de HTTP-proxy, zelf-hostende integratieruntime maakt gebruik van de proxy-instellingen in diahost.exe.config en diawp.exe.config. Als er geen proxy is opgegeven in de diahost.exe.config en diawp.exe.config, verbinding zelf-hostende integratieruntime met cloudservice rechtstreeks zonder tussenkomst van proxy. De volgende procedure bevat instructies voor het bijwerken van het bestand diahost.exe.config.

1. Maak een veilige kopie van C:\Program Files\Microsoft integratie Runtime\3.0\Shared\diahost.exe.config back-up van het oorspronkelijke bestand in Verkenner.
2. Start Notepad.exe uitvoert als beheerder, en opent u tekstbestand "C:\Program Files\Microsoft integratie Runtime\3.0\Shared\diahost.exe.config. U kunt de standaardtag voor system.net vinden zoals wordt weergegeven in de volgende code:

    ```xml
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```
    Vervolgens kunt u proxy-server-gegevens toevoegen, zoals wordt weergegeven in het volgende voorbeeld:

    ```xml
    <system.net>
        <defaultProxy enabled="true">
              <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Aanvullende eigenschappen zijn toegestaan in de proxy-tag om op te geven van de vereiste instellingen, zoals scriptLocation. Zie [proxy Element (netwerkinstellingen)](https://msdn.microsoft.com/library/sa91de1e.aspx) -syntaxis.

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Sla het configuratiebestand naar de oorspronkelijke locatie, start vervolgens de zelfgehoste Integration Runtime Host-service, die de wijzigingen neemt. De service opnieuw starten: Gebruik de applet services in het Configuratiescherm of in de **Integration Runtime Configuration Manager** > Klik op de **-Service stoppen** knop en klik vervolgens op de **starten Service**. Als de service niet wordt gestart, is het waarschijnlijk dat een onjuiste syntaxis van de XML-code is toegevoegd in het configuratiebestand van de toepassing die is bewerkt.

> [!IMPORTANT]
> Vergeet niet om zowel diahost.exe.config en diawp.exe.config te werken.

Naast deze punten moet u ook om te controleren of dat Microsoft Azure is in de lijst met toegestane adressen van uw bedrijf. De lijst met geldige Microsoft Azure IP-adressen kan worden gedownload vanaf de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Mogelijke problemen voor de firewall en proxy-server-problemen
Als u fouten die vergelijkbaar is met de volgende query optreden, is het waarschijnlijk veroorzaakt door onjuiste configuratie van de firewall of proxy-server, die zelf-hostende integratieruntime verbinding maken met Data Factory wordt geblokkeerd om zichzelf te verifiëren. Raadpleeg de vorige sectie om te controleren of uw firewall en proxy-server correct zijn geconfigureerd.

1. Als u probeert om de zelf-hostende integratieruntime te registreren, krijgt u de volgende fout: "is mislukt voor het registreren van dit knooppunt voor Integratieruntime. Controleer of de verificatiesleutel geldig is en de hostservice van Integration-Service wordt uitgevoerd op deze machine. "
   - Wanneer u Integration Runtime Configuration Manager opent, ziet u de status '**verbroken**'of'**verbinden**'. Bij het weergeven van Windows-gebeurtenislogboeken, onder 'Logboeken' > 'Toepassingen en Services Logs' > "Microsoft Integration Runtime", ziet u foutberichten, zoals de volgende fout:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="enable-remote-access-from-intranet"></a>Externe toegang via een Intranet inschakelen  
In geval als u **PowerShell** of **toepassing Referentiebeheer** voor het versleutelen van referenties vanaf een andere computer (in het netwerk) dan wanneer de zelf-hostende integratieruntime is geïnstalleerd, klikt u vervolgens u moet de **'Externe toegang via Intranet'** optie wordt ingeschakeld. Als u de **PowerShell** of **toepassing Referentiebeheer** voor het versleutelen van de referentie op dezelfde computer waarop de zelf-hostende integratieruntime is geïnstalleerd, klikt u vervolgens **' externe toegang via een Intranet'** kan niet worden ingeschakeld.

Externe toegang via Intranet moet **ingeschakeld** vóór het toevoegen van een ander knooppunt voor **hoge beschikbaarheid en schaalbaarheid**.  

Tijdens de zelf-hostende integration runtime installatie (en hoger v 3.3.xxxx.x), standaard, de zelf-hostende integration runtime-installatie schakelt de **'Externe toegang via Intranet'** op de zelf-hostende integration runtime-machine.

Als u een firewall van derden gebruikt, kunt u handmatig de poort 8060 (of de poort van de gebruiker die is geconfigureerd) openen. Als u tijdens de installatie van de zelf-hostende integration runtime firewall probleem ondervindt, kunt u proberen met behulp van de volgende opdracht voor het installeren van de zelf-hostende integratieruntime zonder dat de firewall configureren.

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
```
> [!NOTE]
> **Toepassing Referentiebeheer** is nog niet beschikbaar voor het versleutelen van de referenties in ADFv2. Deze ondersteuning zullen we later toevoegen.  

Als u niet de poort te openen 8060 op de zelf-hostende integration runtime-machine, gebruikt u mechanismen dan met behulp van de ** instelling referenties ** toepassing voor het configureren van de referenties van de gegevensopslag. U kunt bijvoorbeeld New-AzureRmDataFactoryV2LinkedServiceEncryptCredential PowerShell-cmdlet gebruiken. Zie de sectie over de instelling referenties en beveiliging op hoe de referenties voor het opslaan van gegevens kan worden ingesteld.


## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudies voor stapsgewijze instructies: [zelfstudie: on-premises gegevens kopiëren naar de cloud](tutorial-hybrid-copy-powershell.md).
