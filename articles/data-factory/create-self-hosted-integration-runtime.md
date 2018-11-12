---
title: Een zelf-hostende integratieruntime maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van een zelf-hostende integratieruntime in Azure Data Factory, waarmee data factory's voor toegang tot gegevensarchieven in een particulier netwerk.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: abnarain
ms.openlocfilehash: cae81bd2b856ae0fb4a648c03cbec1f87f222902
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51038465"
---
# <a name="create-and-configure-a-self-hosted-integration-runtime"></a>Maken en configureren van een zelf-hostende integratieruntime
De integratieruntime (IR) is de rekeninfrastructuur die Azure Data Factory gebruikt zodat de mogelijkheden van de integratie van gegevens in verschillende netwerkomgevingen. Zie voor meer informatie over IR [overzicht van Integration runtime](concepts-integration-runtime.md).

Een zelf-hostende integratieruntime kan worden uitgevoerd kopieeractiviteiten tussen een gegevensarchief voor de cloud en een gegevensarchief in een particulier netwerk en transformatieactiviteiten op basis van de compute-resources in een on-premises netwerk of een Azure-netwerk kan worden uitgevoerd. De installatie van een zelf-hostende integratieruntime moet op een on-premises computer of op een virtuele machine (VM) in een privénetwerk.  

Dit document wordt beschreven hoe u kunt maken en configureren van een zelf-hostende IR

## <a name="high-level-steps-to-install-a-self-hosted-ir"></a>De stappen op hoog niveau voor het installeren van een zelf-hostende IR
1. Een zelf-hostende Integration Runtime maken. Voor deze taak kunt u de Azure Data Factory-UI. Hier volgt een voorbeeld van PowerShell:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
  
2. [Download](https://www.microsoft.com/download/details.aspx?id=39717) en de zelf-hostende integratieruntime installeren op een lokale computer.

3. Haal de verificatiesleutel en de zelf-hostende integratieruntime registreren met de sleutel. Hier volgt een voorbeeld van PowerShell:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime.  
    ```

## <a name="setting-up-a-self-hosted-ir-on-an-azure-vm-by-using-an-azure-resource-manager-template-automation"></a>Instellen van een zelf-hostende IR op een Azure-VM met behulp van een Azure Resource Manager-sjabloon (automatisering)
U kunt zelf-hostende IR-installatie op een Azure-machine automatiseren met behulp van [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vms-with-selfhost-integration-runtime). Deze sjabloon biedt een eenvoudige manier om een volledig werkende zelf-hostende IR in een Azure-netwerk met hoge beschikbaarheid en schaalbaarheid functies (zolang u het aantal knooppunten op 2 of hoger hebt ingesteld).

## <a name="command-flow-and-data-flow"></a>Opdracht stroom en de gegevensstroom
Wanneer u gegevens verplaatsen tussen on-premises en de cloud, de activiteit maakt gebruik van een zelf-hostende integratieruntime om over te dragen van de gegevens uit een on-premises gegevensbron naar de cloud en vice versa.

Hier volgt een gegevensstroom op hoog niveau voor een samenvatting van de stappen voor het kopiëren van met een zelf-hostende IR:

![Overzicht](media\create-self-hosted-integration-runtime\high-level-overview.png)

1. De ontwikkelaar van gegevens maakt een zelf-hostende integratieruntime binnen een Azure data factory met behulp van een PowerShell-cmdlet. De Azure-portal biedt momenteel geen ondersteuning voor deze functie.
2. Een gekoppelde service voor een on-premises gegevensarchief maakt de ontwikkelaar van gegevens door het exemplaar van de zelf-hostende integration runtime die moet worden gebruikt verbinding maken met de opgeslagen gegevens op te geven. De ontwikkelaar van de gegevens gebruikt als onderdeel van het instellen van de gekoppelde service, de toepassing Referentiebeheer (momenteel niet ondersteund) voor het instellen van verificatietypen en -referenties. De toepassing Referentiebeheer communiceert met het gegevensarchief om de verbinding en de zelf-hostende integratieruntime voor het opslaan van referenties te testen.
3. De zelf-hostende integration runtime-knooppunt versleutelt de referenties met behulp van Windows Data Protection Application Programming Interface (DPAPI) en de referenties die lokaal wordt opgeslagen. Als meerdere knooppunten voor hoge beschikbaarheid zijn ingesteld, worden de referenties verder gesynchroniseerd op andere knooppunten. Elk knooppunt versleutelt de referenties met behulp van DPAPI en slaat deze lokaal. Synchronisatie van referenties is transparant voor de ontwikkelaar van de gegevens en wordt verwerkt door de zelf-hostende IR    
4. De Data Factory-service communiceert met de zelf-hostende integratieruntime voor planning en het beheer van taken via een *besturingskanaal* die gebruikmaakt van een gedeelde Azure Service Bus-wachtrij. Wanneer de activiteitstaak van een worden uitgevoerd moet, wordt in Data Factory de aanvraag, samen met een referentie-informatie wachtrijen (als referenties niet al in de zelf-hostende integratieruntime opgeslagen zijn). De zelf-hostende integratieruntime is serversysteemstatus van de taak na polling van de wachtrij.
5. De zelf-hostende integratieruntime worden gegevens gekopieerd van een on-premises gegevensopslag naar een opslag in de cloud, of vice versa, afhankelijk van hoe de copy-activiteit is geconfigureerd in de pijplijn. Voor deze stap communiceert de zelf-hostende integratieruntime rechtstreeks met cloud-gebaseerde services zoals Azure Blob-opslag via een beveiligde (HTTPS)-kanaal.

## <a name="considerations-for-using-a-self-hosted-ir"></a>Overwegingen voor het gebruik van een zelf-hostende IR

- Een enkele zelf-hostende integratieruntime kan worden gebruikt voor meerdere on-premises gegevensbronnen. Een enkele zelf-hostende integratieruntime kan worden gedeeld met een andere data factory binnen dezelfde Azure Active Directory-tenant. Zie voor meer informatie, [delen van een zelf-hostende integratieruntime](#sharing-the-self-hosted-integration-runtime-with-multiple-data-factories).
- U kunt slechts één exemplaar van een zelf-hostende integratieruntime op één computer geïnstalleerd hebben. Hebt u twee data factory's die nodig voor toegang tot on-premises gegevensbronnen, moet u de zelf-hostende integratieruntime installeren op twee on-premises computers. Met andere woorden, is een zelf-hostende integratieruntime gekoppeld aan een specifieke data factory.
- De zelf-hostende integratieruntime hoeft niet te worden op dezelfde computer als de gegevensbron. Echter verkort met de zelf-hostende integratieruntime dichter bij de gegevensbron de termijn voor de zelf-hostende integratieruntime verbinding maken met de gegevensbron. Het is raadzaam dat u installeert de zelf-hostende integratieruntime op een computer die verschilt van de hosts on-premises gegevensbron. Wanneer de zelf-hostende integration runtime en de gegevensbron zich op verschillende computers bevinden, kunnen de zelf-hostende integratieruntime niet concurreren voor resources met de gegevensbron.
- U kunt meerdere zelf-hostende integratieruntimes hebben op verschillende computers die verbinding met dezelfde on-premises gegevensbron maken. Bijvoorbeeld, u mogelijk twee zelf-hostende integratieruntimes die u beschikbaar twee data factory's maakt, maar dezelfde on-premises gegevensbron is geregistreerd bij de gegevensfactory.
- Als u al een gateway is geïnstalleerd op uw computer voor het bieden van een Power BI-scenario, installeert u een afzonderlijke zelf-hostende integratieruntime voor Azure Data Factory op een andere computer.
- De zelf-hostende integratieruntime moet worden gebruikt voor de ondersteuning voor integratie van gegevens binnen een virtueel Azure-netwerk.
- Uw gegevensbron behandelen als een on-premises gegevensbron die zich achter een firewall, zelfs wanneer u Azure ExpressRoute gebruiken. Gebruik de zelf-hostende integratieruntime om verbinding tussen de service en de gegevensbron te maken.
- Zelfs als het gegevensarchief in de cloud op een virtuele machine van Azure IaaS is, moet u de zelf-hostende integratieruntime gebruiken.
- Taken mislukken in een zelf-hostende integratieruntime die geïnstalleerd op een Windows-server op welke FIPS-compatibele versleuteling is ingeschakeld. U lost dit probleem FIPS-compatibele versleuteling op de server uit te schakelen. Als u wilt uitschakelen FIPS-compatibele versleuteling, wijzigt u de volgende registerwaarde van 1 (ingeschakeld) op 0 (uitgeschakeld): `HKLM\System\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy\Enabled`.

## <a name="prerequisites"></a>Vereisten

- De ondersteunde besturingssysteemversies zijn Windows 7 Service Pack 1, Windows 8.1, Windows 10, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016. Installatie van de zelf-hostende integratieruntime op een domeincontroller wordt niet ondersteund.
- .NET framework 4.6.1 of hoger is vereist. Als u de zelf-hostende integratieruntime op een computer met Windows 7 installeert, installeert u .NET Framework 4.6.1 of hoger. Zie [.NET Framework-systeemvereisten](/dotnet/framework/get-started/system-requirements) voor meer informatie.
- De aanbevolen configuratie voor de zelf-hostende integration runtime-machine is ten minste 2 GHz, vier kernen, 8 GB aan RAM-geheugen en een schijf van 80GB.
- Als de hostmachine in de slaapstand, reageert de zelf-hostende integratieruntime niet op aanvragen voor gegevens. Een juiste energiebeheerschema configureren op de computer voordat u de zelf-hostende integratieruntime installeren. Als de machine is geconfigureerd voor de slaapstand, wordt een bericht gevraagd om de zelf-hostende integration runtime-installatie.
- U moet een beheerder op de computer installeren en configureren van de zelf-hostende integratieruntime is.
- Uitvoeringen van activiteit kopiëren gebeuren op een specifieke frequentie. Resourcegebruik (CPU, geheugen) op de machine volgt hetzelfde patroon met piek- en niet-actieve tijden. Gebruik van resources is ook afhankelijk sterk de hoeveelheid gegevens die wordt verplaatst. Wanneer meerdere kopie-taken uitgevoerd worden, ziet u Resourcegebruik tijdens piektijden omhoog.

## <a name="installation-best-practices"></a>Best practices voor installatie
U kunt de zelf-hostende integratieruntime installeren met het downloaden van een MSI-installatiepakket van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Zie [gegevens verplaatsen tussen on-premises en cloud artikel](tutorial-hybrid-copy-powershell.md) voor stapsgewijze instructies.

- Een energiebeheerschema op de hostcomputer voor de zelf-hostende integratieruntime zo configureren dat de machine sluimerstand niet. Als de hostmachine in de slaapstand, wordt de zelf-hostende integratieruntime offline gaat.
- Back-up van de referenties die zijn gekoppeld aan de zelf-hostende integratieruntime regelmatig.

## <a name="install-and-register-self-hosted-ir-from-the-download-center"></a>Installeren en zelf-hostende IR in het Downloadcentrum registreren

1. Ga naar de [downloadpagina van Microsoft integration runtime](https://www.microsoft.com/download/details.aspx?id=39717).
2. Selecteer **downloaden**, selecteer de juiste versie (**32-bits** of **64-bits**), en selecteer **volgende**.
3. Het MSI-bestand rechtstreeks uitvoeren of sla deze op uw harde schijf en voer deze uit.
4. Op de **Welkom** pagina, selecteer een taal en selecteer **volgende**.
5. Accepteer de licentievoorwaarden voor Microsoft-Software en selecteer **volgende**.
6. Selecteer **map** de zelf-hostende integratieruntime installeren en selecteer **volgende**.
7. Op de **gereed voor installatie** weergeeft, schakelt **installeren**.
8. Klik op **voltooien** om installatie te voltooien.
9. De verificatiesleutel ophalen met behulp van Azure PowerShell. Hier volgt een PowerShell-voorbeeld voor het ophalen van de verificatiesleutel:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```
11. Op de **registreert Integration Runtime (zelf-hostend)** pagina van Microsoft Integration Runtime Configuration Manager die worden uitgevoerd op uw computer, voert de volgende stappen uit:

    a. De verificatiesleutel in het tekstgebied te plakken.

    b. Selecteer desgewenst **Show verificatiesleutel** de belangrijkste tekst wilt zien.

    c. Selecteer **Registreren**.


## <a name="high-availability-and-scalability"></a>Hoge beschikbaarheid en schaalbaarheid
Een zelf-hostende integratieruntime kan worden gekoppeld aan meerdere on-premises computers. Deze machines worden knooppunten genoemd. U kunt maximaal vier knooppunten die zijn gekoppeld aan een zelf-hostende integratieruntime hebben. De voordelen van meerdere knooppunten (on-premises machines met een gateway is geïnstalleerd) voor een logische gateway zijn:
* Hogere beschikbaarheid van de zelf-hostende integratieruntime zodat het niet meer het enige storingspunt in uw big data-oplossing of cloud gegevensintegratie met Azure Data Factory, ervoor te zorgen dat bedrijfscontinuïteit met maximaal vier knooppunten.
* Verbeterde prestaties en de doorvoer tijdens de verplaatsing van gegevens tussen on-premises en cloud gegevensarchieven. Meer informatie opvragen over [prestaties vergelijkingen](copy-activity-performance.md).

U kunt meerdere knooppunten koppelen door het installeren van de zelf-hostende integration runtime-software van de [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=39717). Vervolgens, registreren met behulp van een van de verificatiesleutels verkregen uit de **New-AzureRmDataFactoryV2IntegrationRuntimeKey** cmdlet, zoals beschreven in de [zelfstudie](tutorial-hybrid-copy-powershell.md).

> [!NOTE]
> U hoeft te maken van nieuwe zelf-hostende integratieruntime voor het koppelen van elk knooppunt. U kunt de zelf-hostende integratieruntime op een andere computer installeren en registreren met behulp van de dezelfde verificatiesleutel. 

> [!NOTE]
> Voordat u een ander knooppunt voor hoge beschikbaarheid en schaalbaarheid toevoegt, zorg ervoor dat de **extern toegang tot intranet** optie is ingeschakeld op het eerste knooppunt (**Microsoft Integration Runtime Configuration Manager**  >  **Instellingen** > **extern toegang tot intranet**). 

### <a name="scale-considerations"></a>Overwegingen voor schalen

#### <a name="scale-out"></a>Uitschalen

Wanneer het beschikbare geheugen op de zelf-hostende IR laag is en het CPU-gebruik hoog is, kunt een nieuw knooppunt toevoegen scale-out van de belasting tussen computers. Als activiteiten mislukken omdat ze een opgetreden time-out bent of omdat het zelf-hostende IR-knooppunt offline is, is het handig als u een knooppunt aan de gateway toevoegt.

#### <a name="scale-up"></a>Omhoog schalen

Als het beschikbare geheugen en CPU niet goed worden gebruikt, maar de uitvoering van gelijktijdige taken de limiet is bereikt, moet u omhoog schalen door het aantal gelijktijdige taken die kunnen worden uitgevoerd op een knooppunt. U kunt ook omhoog schalen wanneer activiteiten zijn time-outs opgetreden omdat de zelf-hostende IR is overbelast. Zoals weergegeven in de volgende afbeelding, kunt u de maximale capaciteit voor een knooppunt verhogen:  

![Steeds meer gelijktijdige taken die kunnen worden uitgevoerd op een knooppunt](media\create-self-hosted-integration-runtime\scale-up-self-hosted-IR.png)

### <a name="tlsssl-certificate-requirements"></a>Vereisten voor TLS/SSL-certificaat

Hier volgen de vereisten voor het TLS/SSL-certificaat dat wordt gebruikt voor het beveiligen van communicatie tussen de integration runtime-knooppunten:

- Het certificaat moet een openbaar vertrouwde X509 v3-certificaat. Het is raadzaam dat u gebruik van certificaten die zijn uitgegeven door een openbare (partners) certificeringsinstantie (CA).
- Elk knooppunt voor integration runtime moet dit certificaat vertrouwen.
- SAN Subject Alternative Name ()-certificaten wordt niet aanbevolen omdat alleen het laatste item SAN wordt gebruikt en alle andere worden genegeerd vanwege de huidige beperkingen. Als u een SAN-certificaat hebt waarvan de SAN's zijn bijvoorbeeld **node1.domain.contoso.com** en **node2.domain.contoso.com**, kunt u dit certificaat alleen op een computer met FQDN-naam  **Node2.domain.contoso.com**.
- Het certificaat ondersteunt sleutelgrootte ondersteund door Windows Server 2012 R2 voor SSL-certificaten.
- Certificaten met CNG-sleutels worden niet ondersteund.  

## <a name="sharing-the-self-hosted-integration-runtime-with-multiple-data-factories"></a>Delen van de zelf-hostende integratieruntime met meerdere data factory 's

U kunt een bestaande zelf-hostende integration runtime-infrastructuur, die u hebt al ingesteld in een data factory opnieuw gebruiken. Hiermee kunt u maken een *gekoppelde zelf-hostende integratieruntime* in een andere data factory door te verwijzen naar een bestaande zelf-hostende IR (gedeeld).

Als u wilt delen een zelf-hostende integratieruntime met behulp van PowerShell, Zie [een gedeelde zelf-hostende integratieruntime maken in Azure Data Factory met PowerShell](create-shared-self-hosted-integration-runtime-powershell.md).

Bekijk de volgende video voor een 12 minuten durende inleiding en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Hybrid-data-movement-across-multiple-Azure-Data-Factories/player]

### <a name="terminology"></a>Terminologie

- **Gedeelde IR**: de oorspronkelijke zelf-hostende IR die wordt uitgevoerd op een fysieke infrastructuur.  
- **Gekoppelde IR**: de IR die verwijst naar een andere gedeelde IR. Dit is een logische IR en maakt gebruik van de infrastructuur van een andere zelf-hostende IR (gedeeld).

### <a name="high-level-steps-for-creating-a-linked-self-hosted-ir"></a>De stappen op hoog niveau voor het maken van een gekoppelde zelf-hostende IR

1. In de zelf-hostende IR moet worden gedeeld, toekennen aan de data factory waarin u wilt maken van de gekoppelde IR. 

   ![Knop voor het verlenen van machtiging op het tabblad delen](media\create-self-hosted-integration-runtime\grant-permissions-IR-sharing.png)

   ![Selecties voor het toewijzen van machtigingen](media\create-self-hosted-integration-runtime\3_rbac_permissions.png)

2. Houd er rekening mee de resource-ID van de zelf-hostende IR moet worden gedeeld.

   ![Locatie van de resource-ID](media\create-self-hosted-integration-runtime\4_ResourceID_self-hostedIR.png)

3. In de data factory waaraan de machtigingen zijn verleend, maken van een nieuwe zelf-hostende IR (gekoppeld) en voer de resource-ID.

   ![Knop voor het maken van een gekoppelde zelf-hostende integratieruntime](media\create-self-hosted-integration-runtime\6_create-linkedIR_2.png)

   ![Selectievakjes voor de naam en resourcegroep-ID](media\create-self-hosted-integration-runtime\6_create-linkedIR_3.png)

### <a name="monitoring"></a>Bewaking 

- **Gedeelde IR**

  ![Selecties voor het zoeken naar een gedeelde integratieruntime](media\create-self-hosted-integration-runtime\Contoso-shared-IR.png)

  ![Tabblad voor het controleren](media\create-self-hosted-integration-runtime\contoso-shared-ir-monitoring.png)

- **Gekoppelde IR**

  ![Selecties voor het vinden van een gekoppelde integratieruntime](media\create-self-hosted-integration-runtime\Contoso-linked-ir.png)

  ![Tabblad voor het controleren](media\create-self-hosted-integration-runtime\Contoso-linked-ir-monitoring.png)

### <a name="known-limitations-of-self-hosted-ir-sharing"></a>Bekende beperkingen van het delen van zelf-hostende IR

* De data factory waarin een gekoppelde IR wordt gemaakt moet hebben een [MSI](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview). Standaard de data factory's die zijn gemaakt in Azure portal of PowerShell-cmdlets hebben een MSI-bestand impliciet gemaakt. Maar wanneer een data factory is gemaakt via een Azure Resource Manager-sjabloon of SDK gebruikt, de **identiteit** eigenschap expliciet moet worden ingesteld om ervoor te zorgen dat een gegevensfactory met een MSI-bestand wordt gemaakt door Azure Resource Manager. 

* De Azure Data Factory .NET SDK die ondersteuning biedt voor deze functie is versie 1.1.0 of hoger.

* De Azure PowerShell-versie die ondersteuning biedt voor deze functie is 6.6.0 of hoger (AzureRM.DataFactoryV2, 0.5.7 of hoger).

* Als u wilt machtigen, moet de gebruiker de rol eigenaar of de overgenomen rol van eigenaar in de data factory waar de gedeelde IR bestaat. 

* Voor Active Directory [gastgebruikers](https://docs.microsoft.com/azure/active-directory/governance/manage-guest-access-with-access-reviews), de zoekfunctie van (alle data factory's vermeld met behulp van een zoekwoord) in de gebruikersinterface [werkt niet](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#SearchLimits). Maar, zolang de gastgebruiker de eigenaar van de data factory is, kunnen zij de IR zonder de zoekfunctie delen door direct het MSI-bestand van de data factory die waarmee de IR worden gedeeld moet de **machtiging toewijzen** in het tekstvak en selecteren **toevoegen** in de Azure Data Factory-UI. 

  > [!NOTE]
  > Deze functie is alleen beschikbaar in Azure Data Factory V2. 

## <a name="notification-area-icons-and-notifications"></a>Pictogrammen in het systeemvak en meldingen

Als u de cursor boven het pictogram of het bericht in het systeemvak klikt verplaatst, vindt u meer informatie over de status van de zelf-hostende integratieruntime.

![Meldingen in het systeemvak](media\create-self-hosted-integration-runtime\system-tray-notifications.png)

## <a name="ports-and-firewall"></a>Poorten en firewall
Er zijn twee firewalls om te overwegen: de *bedrijfsfirewall* die worden uitgevoerd op de centrale-router van de organisatie, en de *Windows firewall* geconfigureerd als een daemon op de lokale computer waar de zelf-hostende integratieruntime is geïnstalleerd.

![Firewall](media\create-self-hosted-integration-runtime\firewall.png)

Op de *bedrijfsfirewall* niveau, moet u de volgende domeinen bevinden en uitgaande poorten configureren:

Domeinnamen | Poorten | Beschrijving
------------ | ----- | ------------
*.servicebus.windows.net | 443 | Gebruikt voor communicatie met de back-end data movement service
*.core.windows.net | 443 | Gebruikt voor gefaseerd kopiëren via Azure Blob-opslag (indien geconfigureerd)
*.frontend.clouddatahub.net | 443 | Gebruikt voor communicatie met de back-end data movement service
download.microsoft.com | 443 | Gebruikt voor het downloaden van updates

Op de *Windows firewall* niveau (machine niveau), deze uitgaande poorten normaal gesproken zijn ingeschakeld. Als u niet het geval is, kunt u de domeinen en poorten dienovereenkomstig op een zelf-hostende integration runtime-machine.

> [!NOTE]
> Op basis van de bron- en sinks, u mogelijk moet whitelist aanvullende domeinen en uitgaande poorten in uw bedrijfsfirewall of Windows firewall.
>
> Voor sommige clouddatabases (bijvoorbeeld Azure SQL Database en Azure Data Lake) moet u mogelijk naar geaccepteerde IP-adressen van de zelf-hostende integration runtime-machines op de firewallconfiguratie.

### <a name="copy-data-from-a-source-to-a-sink"></a>Gegevens kopiëren van een bron naar een sink
Zorg ervoor dat de firewall-regels correct zijn ingeschakeld op de firewall van het bedrijf, de Windows firewall op de zelf-hostende integration runtime-machine en de gegevens zelf opslaan. Inschakelen van deze regels kunt de zelf-hostende integratieruntime verbinding maken met zowel bron en sink is. Inschakelen van regels voor elke gegevensopslag die bij de kopieerbewerking betrokken is.

Bijvoorbeeld, als u wilt kopiëren van een on-premises gegevensarchief naar een Azure SQL Database-sink of een Azure SQL Data Warehouse-sink, de volgende stappen uitvoeren:

1. Uitgaande TCP-communicatie op poort 1433 voor zowel Windows firewall en firewall van het bedrijf toestaan.
2. Configureer de firewall-instellingen van de Azure SQL database als u wilt het IP-adres van de zelf-hostende integration runtime-machine toevoegen aan de lijst met toegestane IP-adressen.

> [!NOTE]
> Als uw firewall niet toe de uitgaande poort 1433 dat staat, de zelf-hostende integratieruntime kan niet rechtstreeks toegang hebben tot de Azure SQL-database. In dit geval kunt u een [gefaseerd kopiëren](copy-activity-performance.md) met Azure SQL Database en Azure SQL Data Warehouse. In dit scenario zou u alleen HTTPS (poort 443) vereist voor de verplaatsing van gegevens.


## <a name="proxy-server-considerations"></a>Overwegingen voor de proxy-server
Als uw bedrijfsnetwerk bevindt een proxyserver gebruikt voor toegang tot het internet, configureert u de zelf-hostende integratieruntime voor het gebruik van de juiste proxy-instellingen. U kunt de proxy instellen tijdens de registratie-fase.

![Geef-proxy](media\create-self-hosted-integration-runtime\specify-proxy.png)

De zelf-hostende integratieruntime maakt gebruik van de proxy-server verbinding maken met de cloudservice. Selecteer **koppeling wijzigen** tijdens de eerste configuratie. Ziet u het dialoogvenster proxy-instellingen.

![Set-proxy](media\create-self-hosted-integration-runtime\set-http-proxy.png)

Er zijn drie opties:

- **Gebruik geen proxy**: de zelf-hostende integratieruntime de proxy niet expliciet gebruikt voor het verbinding maken met cloudservices.
- **Systeemproxy gebruiken**: de zelf-hostende integratieruntime maakt gebruik van de proxy-instellingen die is geconfigureerd in diahost.exe.config en diawp.exe.config. Als er geen proxy is geconfigureerd in diahost.exe.config en diawp.exe.config, maakt de zelf-hostende integratieruntime verbinding met de cloudservice rechtstreeks zonder tussenkomst van een proxy.
- **Aangepaste proxy gebruikt**: de HTTP-proxy-instellingen moet worden gebruikt voor de zelf-hostende integratieruntime, in plaats van configuraties in diahost.exe.config en diawp.exe.config configureren. **Adres** en **poort** zijn vereist. **Gebruikersnaam** en **wachtwoord** zijn optioneel, afhankelijk van de instelling van de verificatie van uw proxy. Alle instellingen zijn versleuteld met Windows DPAPI op de zelf-hostende integratieruntime en lokaal opgeslagen op de machine.

De hostservice van integratieruntime wordt automatisch opnieuw opgestart nadat u de bijgewerkte proxy-instellingen hebt opgeslagen.

Nadat de zelf-hostende integratieruntime is geregistreerd, als u wilt weergeven of bijwerken van proxy-instellingen, gebruikt u de Integration Runtime Configuration Manager.

1. Open **Microsoft Integration Runtime Configuration Manager**.
2. Schakel over naar het tabblad **Instellingen**.
3. Selecteer de **wijziging** herstelkoppeling in de **HTTP-Proxy** sectie om te openen de **HTTP-Proxy instellen** in het dialoogvenster.
4. Selecteer **Volgende**. U gaat vervolgens een waarschuwing wordt weergegeven waarin u wordt gevraagd om uw toestemming om op te slaan van de proxy-instellingen en start de hostservice van integratieruntime opnieuw.

U kunt weergeven en bijwerken van de HTTP-proxy met behulp van de Configuration Manager-hulpprogramma.

![Weergave-proxy](media\create-self-hosted-integration-runtime\view-proxy.png)

> [!NOTE]
> Als u een proxyserver met NTLM-verificatie instelt, wordt de hostservice van integratieruntime wordt uitgevoerd onder het domeinaccount. Als u het wachtwoord voor het domeinaccount dat later wijzigt, moet u bijwerken van de configuratie-instellingen voor de service en start deze dienovereenkomstig opnieuw. Vanwege deze vereiste is het raadzaam dat u een speciale domeinaccount gebruiken voor toegang tot de proxyserver die vereist niet dat u het wachtwoord regelmatig bijwerken.

### <a name="configure-proxy-server-settings"></a>Proxyserverinstellingen configureren

Als u selecteert de **systeemproxy gebruiken** instellen voor de HTTP-proxy, de zelf-hostende integratieruntime maakt gebruik van de proxy-instellingen in diahost.exe.config en diawp.exe.config. Als er geen proxy is opgegeven in de diahost.exe.config en diawp.exe.config, maakt de zelf-hostende integratieruntime verbinding met de cloudservice rechtstreeks zonder tussenkomst van proxy. De volgende procedure bevat instructies voor het bijwerken van het bestand diahost.exe.config:

1. Maak een veilige kopie van C:\Program Files\Microsoft integratie Runtime\3.0\Shared\diahost.exe.config back-up van het oorspronkelijke bestand in Verkenner.
2. Open Notepad.exe uitvoeren als beheerder en het tekstbestand C:\Program Files\Microsoft integratie Runtime\3.0\Shared\diahost.exe.config openen. De standaardtag voor system.net vinden, zoals wordt weergegeven in de volgende code:

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

    Aanvullende eigenschappen zijn toegestaan in de proxy-tag om op te geven van de vereiste instellingen, zoals `scriptLocation`. Zie [proxy Element (netwerkinstellingen)](https://msdn.microsoft.com/library/sa91de1e.aspx) -syntaxis.

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Sla het configuratiebestand op de oorspronkelijke locatie. Start de zelf-hostende integratieruntime Host-Service, de wijzigingen neemt vervolgens opnieuw. 

   Als u wilt de service opnieuw start, gebruik de applet services in het Configuratiescherm. Of selecteer van Integration Runtime Configuration Manager de **-Service stoppen** knop en selecteer vervolgens **Service starten**. 
   
   Als de service niet wordt gestart, is het waarschijnlijk dat een onjuiste syntaxis van de XML-code is toegevoegd in het configuratiebestand van de toepassing die is bewerkt.

> [!IMPORTANT]
> Vergeet niet om zowel diahost.exe.config en diawp.exe.config te werken.

U moet ook om ervoor te zorgen dat Microsoft Azure in de lijst met toegestane adressen van uw bedrijf is. U kunt downloaden de lijst met geldige Microsoft Azure IP-adressen van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Mogelijke problemen voor de firewall en proxy-server-problemen
Als u fouten die vergelijkbaar is met de volgende query optreden, is het waarschijnlijk veroorzaakt door onjuiste configuratie van de firewall of proxy-server, waarmee de zelf-hostende integratieruntime verbinding maken met Data Factory worden geblokkeerd om zichzelf te verifiëren. Om ervoor te zorgen dat uw firewall en proxy-server correct zijn geconfigureerd, raadpleegt u de vorige sectie.

* Als u probeert om de zelf-hostende integratieruntime te registreren, krijgt u de volgende fout: "is mislukt voor het registreren van dit knooppunt voor Integratieruntime. Controleer of de verificatiesleutel geldig is en de service voor integratie met Host-Service wordt uitgevoerd op deze machine."
* Wanneer u Integration Runtime Configuration Manager opent, ziet u de status van **verbroken** of **verbinden**. Wanneer u onder Windows-gebeurtenislogboeken, bekijkt **logboeken** > **logboeken toepassingen en Services** > **Microsoft Integration Runtime**, u ziet foutberichten zoals deze:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="enabling-remote-access-from-an-intranet"></a>Externe toegang via een intranet inschakelen  
Als u PowerShell of de toepassing Referentiebeheer gebruiken voor het versleutelen van de referenties van een andere computer (in het netwerk) dan wanneer de zelf-hostende integratieruntime is geïnstalleerd, kunt u inschakelen de **externe toegang via Intranet**optie. Als u PowerShell of de toepassing Referentiebeheer voor het versleutelen van de referenties op dezelfde computer waarop de zelf-hostende integratieruntime is geïnstalleerd, u kunt niet inschakelen **externe toegang via Intranet**.

U moet inschakelen **externe toegang via Intranet** voordat u een ander knooppunt voor hoge beschikbaarheid en schaalbaarheid kunt toevoegen.  

Tijdens de zelf-hostende integration runtime installatie (later versie 3.3.xxxx.x), standaard, de zelf-hostende integration runtime-installatie uitgeschakeld **externe toegang via Intranet** op de zelf-hostende integration runtime-machine.

Als u een firewall van derden gebruikt, kunt u handmatig openen poort 8060 (of de gebruiker geconfigureerde poort). Hebt u een firewall-probleem tijdens het instellen van de zelf-hostende integratieruntime, probeert u het gebruik van de volgende opdracht voor het installeren van de zelf-hostende integratieruntime zonder dat de firewall configureren:

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
```
> [!NOTE]
> De toepassing Referentiebeheer is nog niet beschikbaar voor het versleutelen van de referenties in Azure Data Factory V2.  

Als u ervoor geen open poort 8060 op de zelf-hostende integration runtime-machine kiest, kunt u methoden dan de instelling referenties toepassing gebruiken voor het configureren van de referenties van de gegevensopslag. Bijvoorbeeld, kunt u de **New-AzureRmDataFactoryV2LinkedServiceEncryptCredential** PowerShell-cmdlet.


## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudies voor stapsgewijze instructies: [zelfstudie: on-premises gegevens kopiëren naar de cloud](tutorial-hybrid-copy-powershell.md).
