---
title: Operations Manager verbinden met Log Analytics | Microsoft Docs
description: Als u uw bestaande investering in System Center Operations Manager wilt behouden en combineren met de uitgebreide mogelijkheden van Log Analytics, kunt u Operations Manager integreren met uw werkruimte.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/23/2018
ms.author: magoedte
ms.openlocfilehash: 722712d1e38efa3e1accc41abca5c9d88b3c1229
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58095922"
---
# <a name="connect-operations-manager-to-log-analytics"></a>Operations Manager verbinden met Log Analytics
Voor het onderhouden van uw bestaande investeringen in [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/key-concepts?view=sc-om-1807) en uitgebreide mogelijkheden met Log Analytics gebruiken, kunt u Operations Manager integreren met uw Log Analytics-werkruimte. Zo profiteert u van de mogelijkheden van Log Analytics, maar kunt u Operations Manager nog steeds gebruiken voor het volgende:

* Statuscontrole van uw IT-services met Operations Manager
* Onderhoud van de integratie met uw ITSM-beheeroplossingen voor incidenten en problemen
* Beheer van de levenscyclus van agents die lokaal zijn geïmplementeerd of op IaaS-VM's in de openbare cloud, en die u bewaakt met Operations Manager

Integratie met System Center Operations Manager levert meerwaarde op voor uw strategie voor servicebewerkingen. Dit is te danken aan de snelheid en efficiëntie van Log Analytics bij het verzamelen, opslaan en analyseren van gegevens van Operations Manager. Log Analytics helpt u de oorzaken van (terugkerende) problemen te correleren en identificeren, zodat u uw bestaande probleembeheerproces beter kunt ondersteunen. De zoekmachine voor het onderzoeken van prestatie-, gebeurtenis- en waarschuwingsgegevens is uiterst flexibel en bevat geavanceerde dashboards en uitgebreide rapportagemogelijkheden om deze gegevens op verschillende zinvolle manieren weer te geven. Dat is de kracht van Log Analytics in combinatie met Operations Manager.

De agents die rapporteren aan de beheergroep van Operations Manager, verzamelen gegevens van uw servers op basis van de gegevensbronnen en oplossingen van Log Analytics die u hebt ingeschakeld in uw werkruimte. Afhankelijk van de ingeschakelde oplossingen, worden de gegevens rechtstreeks verzonden vanaf een Operations Manager-beheerserver naar de service of, vanwege de grote hoeveelheid gegevens die wordt verzameld op de agent, rechtstreeks vanaf de agent naar Log Analytics. De beheerserver stuurt de gegevens rechtstreeks naar de service. Deze worden nooit naar de operationele database of datawarehouse-database geschreven. Als de verbinding tussen een beheerserver en Log Analytics wordt verbroken, worden de gegevens lokaal opgeslagen totdat de verbinding met Log Analytics hersteld is. Als de beheerserver offline is vanwege gepland onderhoud of niet-geplande uitval, neemt een andere beheerserver in de beheergroep de verbinding met Log Analytics over.  

Het volgende diagram toont de verbinding tussen de beheerservers en agents in een System Center Operations Manager-beheergroep en Log Analytics, met inbegrip van de richting en poorten.   

![oms-operations-manager-integration-diagram](./media/om-agents/oms-operations-manager-connection.png)

Als uw IT-beveiligingsbeleid niet computers in uw netwerk verbinding maken met Internet toestaat, kunnen servers voor het beheer worden geconfigureerd voor het verbinding maken met de Log Analytics-gateway naar configuratie-informatie ontvangen en verzenden van verzamelde gegevens, afhankelijk van de oplossingen ingeschakeld. Zie voor meer informatie en instructies over het configureren van uw Operations Manager-beheergroep om te communiceren via een Log Analytics-gateway naar de Log Analytics-service, [computers koppelen aan Log Analytics met behulp van de Log Analytics-gateway](../../azure-monitor/platform/gateway.md).  

## <a name="prerequisites"></a>Vereisten 
Voordat u begint, raadpleegt u de volgende vereisten.

* Log Analytics biedt alleen ondersteuning voor System Center Operations Manager 1807, Operations Manager 1801, Operations Manager 2016, Operations Manager 2012 SP1 UR6 of hoger en Operations Manager 2012 R2 UR2 of hoger. Proxyondersteuning is toegevoegd aan Operations Manager 2012 SP1 UR7 en Operations Manager 2012 R2 UR3.
* Alle Operations Manager-agents moeten voldoen aan de minimale vereisten voor ondersteuning. Zorg ervoor dat agents zijn aan de minimale update, anders Windows-agentcommunicatie kan mislukken en genereren van fouten in het gebeurtenislogboek van Operations Manager.
* Een Log Analytics-werkruimte. Raadpleeg voor meer informatie, [Log Analytics-werkruimte overzicht](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json).
* U verificatie op Azure met een account dat lid is van de [rol van inzender van Log Analytics](../../azure-monitor/platform/manage-access.md#manage-accounts-and-users).  

>[!NOTE]
>Recente wijzigingen in Azure API's wordt voorkomen dat klanten kunnen met succes integratie tussen de beheergroep en de Log Analytics voor het eerst configureren. Voor klanten die hun beheergroep al hebt geïntegreerd met de service, wordt u niet beïnvloed, tenzij u nodig hebt om uw bestaande verbinding opnieuw te configureren.  
>Een nieuw managementpack is vrijgegeven voor de volgende versies van Operations Manager:
>  
>* Download het managementpack uit voor System Center Operations Manager 1801 [hier](https://www.microsoft.com/download/details.aspx?id=57173)  
>* Voor System Center 2016 - Operations Manager, downloadt u het managementpack uit [hier](https://www.microsoft.com/download/details.aspx?id=57172)  
>* Voor System Center Operations Manager 2012 R2, downloadt u het managementpack uit [hier](https://www.microsoft.com/download/details.aspx?id=57171)  
>
>Dit management pack-update is niet van toepassing op System Center Operations Manager 1807, dit is een update-release van versie 1801 en niet een volledige build van het product.   

### <a name="network"></a>Netwerk
De onderstaande lijst bevat de vereiste proxy- en firewallconfiguratiegegevens voor de Operations Manager-agent, beheerservers en Operations-console voor communicatie met Log Analytics. Het verkeer van elk onderdeel is uitgaand vanaf het netwerk naar de Log Analytics-service.   

|Resource | Poortnummer| HTTPS-controle overslaan|  
|---------|------|-----------------------|  
|**Agent**|||  
|\*.ods.opinsights.azure.com| 443 |Ja|  
|\*.oms.opinsights.azure.com| 443|Ja|  
|\*.blob.core.windows.net| 443|Ja|  
|\*.azure-automation.net| 443|Ja|  
|**Beheerserver**|||  
|\*.service.opinsights.azure.com| 443||  
|\*.blob.core.windows.net| 443| Ja|  
|\*.ods.opinsights.azure.com| 443| Ja|  
|*.azure-automation.net | 443| Ja|  
|**Operations Manager-console naar Log Analytics**|||  
|service.systemcenteradvisor.com| 443||  
|\*.service.opinsights.azure.com| 443||  
|\*.live.com| 80 en 443||  
|\*.microsoft.com| 80 en 443||  
|\*.microsoftonline.com| 80 en 443||  
|\*.mms.microsoft.com| 80 en 443||  
|login.windows.net| 80 en 443||  
|portal.loganalytics.io| 80 en 443||
|api.loganalytics.io| 80 en 443||
|docs.loganalytics.io| 80 en 443||  

### <a name="tls-12-protocol"></a>TLS 1.2-protocol
Als u wilt controleren of de beveiliging van gegevens die onderweg zijn naar Log Analytics, sterk raden we u aan de groep agent en -beheerserver te gebruiken ten minste configureren Transport Layer Security (TLS) 1.2. Oudere versies van TLS/Secure Sockets Layer (SSL) kwetsbaar zijn gevonden en hoewel ze op dit moment nog steeds werken om toe te staan achterwaartse compatibiliteit, zijn ze onderling **niet aanbevolen**. Raadpleeg voor meer informatie, [verzenden van gegevens veilig gebruik TLS 1.2](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12). 

## <a name="connecting-operations-manager-to-log-analytics"></a>Operations Manager verbinden met Log Analytics
Voer de volgende stappen uit om uw Operations Manager-beheergroep te verbinden met een van uw Log Analytics-werkruimten.

De optie om op te geven van de configuratie van de proxy voor de beheergroep is tijdens de eerste registratie van uw Operations Manager-beheergroep met Log Analytics-werkruimte niet beschikbaar in de Operations-console.  Deze optie is pas beschikbaar als de beheergroep bij de service is geregistreerd.  Om dit te omzeilen, moet u bijwerken van de systeemconfiguratie van de proxyserver met behulp van Netsh op het systeem het uitvoeren van de Operations-console van het configureren van integratie en alle beheerservers in de beheergroep.  

1. Open een opdrachtprompt met verhoogde bevoegdheid.
   a. Ga naar **Start** en het type **cmd**.
   b. Met de rechtermuisknop op **opdrachtprompt** en selecteert u uitvoeren als beheerder **.
1. Voer de volgende opdracht in en druk op **Enter**:

    `netsh winhttp set proxy <proxy>:<port>`

Na het voltooien van de volgende stappen uit om te integreren in Log Analytics, kunt u de configuratie verwijderen door het uitvoeren van `netsh winhttp reset proxy` en gebruik vervolgens de **proxyserver configureren** optie in de Operations-console voor het opgeven van de proxy- of logboekpad Analytics-gatewayserver. 

1. Selecteer de werkruimte **Beheer** in de Operations Manager-console.
1. Vouw het knooppunt Operations Management Suite uit en klik op **Verbinding**.
1. Klik op de koppeling **Registreren bij Operations Management Suite**.
1. Op de **Wizard Operations Management Suite voorbereiden: Verificatie** pagina, voer het e-mailadres of telefoonnummer en het wachtwoord van de administrator-account dat is gekoppeld aan uw OMS-abonnement en op **aanmelden**.

   >[!NOTE]
   >OMS wordt nu aangeduid als Log Analytics.
   
1. Nadat u bent geverifieerd, op de **Wizard Operations Management Suite voorbereiden: Selecteer werkruimte** pagina, wordt u gevraagd om uw Azure-tenant, abonnement en Log Analytics-werkruimte te selecteren. Als u meerdere werkruimten hebt, selecteert u in de keuzelijst de werkruimte die u bij de Operations Manager-beheergroep wilt registreren, en klikt u op **Volgende**.
   
   > [!NOTE]
   > Operations Manager ondersteunt slechts één Log Analytics-werkruimte tegelijk. De verbinding en de computers die bij de vorige Log Analytics-werkruimte waren geregistreerd, worden verwijderd.
   > 
   > 
1. Op de **Wizard Operations Management Suite voorbereiden: Samenvatting** pagina, bevestig de instellingen en als ze juist zijn, klikt u op **maken**.
1. Op de **Wizard Operations Management Suite voorbereiden: Voltooien** pagina, klikt u op **sluiten**.

### <a name="add-agent-managed-computers"></a>Door een agent beheerde computers toevoegen
Na het configureren van integratie met uw Log Analytics-werkruimte, wordt alleen een verbinding tot stand met de service, er worden geen gegevens worden verzameld van de agents die rapporteren aan de beheergroep. Dit zal niet worden uitgevoerd tot nadat u hebt geconfigureerd welke specifieke door agents beheerde computers verzamelen van gegevens voor Log Analytics. U kunt de computerobjecten afzonderlijk selecteren of een groep met Windows-computerobjecten selecteren. U kunt geen groep met objecten van een andere klasse selecteren, zoals logische schijven of SQL-databases.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
1. Vouw het knooppunt Operations Management Suite uit en klik op **Verbinding**.
1. Klik op de koppeling **Computer/Groep toevoegen** onder de kop Acties rechts in het deelvenster.
1. In het dialoogvenster **Computer zoeken** kunt u zoeken naar computers of groepen die worden bewaakt door Operations Manager. Selecteer de computers of groepen die u wilt toevoegen aan Log Analytics, klik op **Toevoegen** en klik vervolgens op **OK**.

U kunt de computers en groepen die zijn geconfigureerd voor het verzamelen van gegevens bekijken in het knooppunt Beheerde computers onder Operations Management Suite in de werkruimte **Beheer** van de Operations-console. Hier kunt u zo nodig computers en groepen toevoegen of verwijderen.

### <a name="configure-proxy-settings-in-the-operations-console"></a>Proxy-instellingen configureren in de Operations-console
Voer de volgende stappen uit als een interne proxyserver tussen de beheergroep en de Log Analytics-service is. Deze instellingen worden centraal beheerd via de beheergroep en gedistribueerd naar door een agent beheerde systemen die zijn opgenomen in het bereik voor het verzamelen van gegevens voor Log Analytics.  Dit is nuttig voor oplossingen die de beheerserver overslaan en gegevens rechtstreeks naar de service verzenden.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
1. Vouw Operations Management Suite uit en klik op **Verbindingen**.
1. Klik in de weergave OMS-verbinding op **Proxyserver configureren**.
1. Op **Wizard Operations Management Suite: Proxyserver** weergeeft, schakelt **een proxyserver gebruiken voor toegang tot de Operations Management Suite**, en typ vervolgens de URL het poortnummer, bijvoorbeeld <http://corpproxy:80> en klik vervolgens op **voltooien** .

Als uw proxyserver verificatie is vereist, moet u de volgende stappen voor het configureren van referenties en instellingen die moet worden doorgegeven aan beheerde computers die in de beheergroep met Log Analytics-rapporten uitvoeren.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
1. Selecteer onder **RunAs-configuratie** de optie **Profielen**.
1. Open het profiel **System Center Advisor RunAs-profielproxy**.
1. Klik in de wizard Uitvoeren als-profiel op Toevoegen om een Uitvoeren als-account te gebruiken. U kunt een nieuw [Uitvoeren als-account](https://technet.microsoft.com/library/hh321655.aspx) maken of een bestaand account gebruiken. Dit account moet over voldoende rechten beschikken voor het doorgeven van de proxyserver.
1. Als u het account voor beheer wilt instellen, kiest u **Een geselecteerde klasse, groep of object**, klikt u op **Selecteren...** en klikt u vervolgens op **Groep...** om het vak **Groep zoeken** te openen.
1. Zoek en selecteer de **Microsoft System Center Advisor Monitoring Server-groep**. Klik op **OK** na het selecteren van de groep om het vak **Groep zoeken** te sluiten.
1. Klik op **OK** om het vak **Een Uitvoeren als-account toevoegen** te sluiten.
1. Klik op **Opslaan** om de wizard te voltooien en de wijzigingen op te slaan.

Nadat de verbinding is gemaakt en u de agents die gegevens verzamelen en verzenden naar Log Analytics hebt geconfigureerd, wordt de volgende configuratie toegepast in de beheergroep, niet noodzakelijkerwijs in deze volgorde:

* Het Uitvoeren als-account **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** wordt gemaakt. Het wordt gekoppeld aan het Uitvoeren als-profiel **Microsoft System Center Advisor Run As Profile Blob** voor twee klassen: **Collection Server** (server voor verzameling) en **Operations Manager Management Group** (Operations Manager-beheergroep).
* Er worden twee connectors gemaakt.  De eerste connector is **Microsoft.SystemCenter.Advisor.DataConnector**. Deze wordt automatisch geconfigureerd met een abonnement zodat alle waarschuwingen die door de klassen in de beheergroep worden gegenereerd, worden doorgestuurd naar Log Analytics. De tweede connector is **Advisor Connector**, die verantwoordelijk is voor communicatie met Log Analytics en het delen van gegevens.
* Agents en groepen die u hebt geselecteerd voor het verzamelen van gegevens in de beheergroep, worden toegevoegd aan de **Microsoft System Center Advisor Monitoring Server-groep**.

## <a name="management-pack-updates"></a>Management pack-updates
Nadat de configuratie is voltooid, wordt er een verbinding tot stand gebracht tussen de Operations Manager-beheergroep en de Log Analytics-service. De beheerserver wordt gesynchroniseerd met de webservice en ontvangt informatie over de bijgewerkte configuratie in de vorm van management packs voor de oplossingen die u hebt ingeschakeld en die zijn geïntegreerd met Operations Manager. Operations Manager wordt gecontroleerd op updates van deze management packs en automatisch downloaden en importeert deze als ze beschikbaar. Dit gedrag wordt voornamelijk bepaald door twee regels:

* **Microsoft.SystemCenter.Advisor.MPUpdate** - Hiermee worden de basis-management packs van Log Analytics bijgewerkt. Deze regel wordt standaard elke 12 uur uitgevoerd.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** - Hiermee worden de management packs van ingeschakelde oplossingen in uw werkruimte bijgewerkt. Deze regel wordt standaard elke vijf (5) minuten uitgevoerd.

Deze twee regels om te voorkomen van automatische download door ze uit te schakelen, of de frequentie voor hoe vaak de beheerserver wordt gesynchroniseerd met Log Analytics om te bepalen of een nieuw managementpack beschikbaar is en moet worden gedownload, kunt u negeren. Volg de stappen voor het [overschrijven van een regel of controle](https://technet.microsoft.com/library/hh212869.aspx) als u de parameter **Frequency** (Frequentie) wilt bijwerken met een waarde in seconden om het synchronisatieschema te wijzigen. Wijzig de parameter **Enabled** (Ingeschakeld) als u de regels wilt uitschakelen. Configureer de overschrijvingen voor alle objecten van de klasse Operations Manager Management Group.

Als u wilt doorgaan na uw bestaande wijzigingsproces voor een besturingselement voor het beheren van management Packs in uw productiebeheergroep, kunt u uitschakelen van de regels en ze inschakelen tijdens bepaalde uren waarin updates mogen. Als u een ontwikkelings- of QA-beheergroep in uw omgeving hebt en deze met internet verbonden is, kunt u die beheergroep configureren met een Log Analytics-werkruimte ter ondersteuning van dit scenario. Zo kunt u de iteratieve vrijgaven van de Log Analytics-management packs beoordelen en evalueren voordat u ze vrijgeeft in uw productiebeheergroep.

## <a name="switch-an-operations-manager-group-to-a-new-log-analytics-workspace"></a>Een Operations Manager-groep overschakelen naar een nieuwe Log Analytics-werkruimte
1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
1. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics** en maak vervolgens een werkruimte.  
1. Open de Operations Manager-console met een account dat lid is van de rol Administrators in Operations Manager en selecteer de werkruimte **Beheer**.
1. Log Analytics en selecteer **verbindingen**.
1. Selecteer de koppeling **Operations Management Suite opnieuw configureren** in het middelste gedeelte van het deelvenster.
1. Ga als volgt de **Wizard van Log Analytics voorbereiden** en voer de e-mailadres of telefoonnummer getal en het wachtwoord van de administrator-account dat is gekoppeld aan uw nieuwe Log Analytics-werkruimte.
   
   > [!NOTE]
   > De **Wizard Operations Management Suite voorbereiden: Selecteer werkruimte** pagina geeft de bestaande werkruimte die wordt gebruikt.
   > 
   > 

## <a name="validate-operations-manager-integration-with-log-analytics"></a>Integratie van Log Analytics met Operations Manager valideren
Er zijn meerdere manieren om te controleren of Log Analytics met succes is geïntegreerd met Operations Manager.

### <a name="to-confirm-integration-from-the-azure-portal"></a>Integratie controleren vanuit Azure Portal
1. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer.
1. Selecteer de toepasselijke werkruimte in de lijst met Log Analytics-werkruimten.  
1. Selecteer achtereenvolgens **Geavanceerde instellingen**, **Verbonden bronnen** en **System Center**. 
1. In de tabel onder de sectie System Center Operations Manager ziet u de naam van de beheergroep met het aantal agents en wanneer er voor het laatst gegevens zijn ontvangen.
   
   ![oms-settings-connectedsources](./media/om-agents/oms-settings-connectedsources.png)

### <a name="to-confirm-integration-from-the-operations-console"></a>Integratie controleren vanuit de Operations-console
1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
1. Selecteer **Management Packs** en typ in het tekstvak **Look for:** (Zoeken naar) **Advisor** of **Intelligence**.
1. Voor elke oplossing die u hebt ingeschakeld, ziet u een bijbehorend management pack in de lijst met zoekresultaten.  Als u bijvoorbeeld de oplossing Waarschuwingenbeheer hebt ingeschakeld, wordt het management pack Microsoft System Center Advisor Waarschuwingenbeheer weergegeven in de lijst.
1. Schakel over van de weergave **Monitoring** (Bewaking) naar de weergave **Operations Management Suite\Health State** (Status van Operations Management Suite).  Selecteer een beheerserver in het deelvenster **Management Server State** (Status van beheerserver) en controleer in het deelvenster **Detail View** (Detailweergave) of de waarde voor de eigenschap **Authentication service URI** (URI van verificatieservice) overeenkomt met de id van de Log Analytics-werkruimte.
   
   ![oms-opsmgr-mg-authsvcuri-property-ms](./media/om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-log-analytics"></a>Integratie met Log Analytics verwijderen
Als de integratie tussen uw Operations Manager-beheergroep en de Log Analytics-werkruimte niet meer nodig is, moet u verschillende stappen uitvoeren om de verbinding en de configuratie in de beheergroep correct te verwijderen. Met de volgende procedure werkt u de Log Analytics-werkruimte bij door de referentie van de beheergroep, de Log Analytics-connectors en de management packs voor integratie met de service te verwijderen.  

De management packs voor de oplossingen die u hebt ingeschakeld en die zijn geïntegreerd met Operations Manager en de management packs die zijn vereist voor integratie met de Log Analytics-service kunnen niet eenvoudig worden verwijderd uit de beheergroep. Dat komt omdat bepaalde Log Analytics-management packs afhankelijk zijn van andere gerelateerde management packs. Download het script [remove a management pack with dependencies](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) (een management pack met afhankelijkheden verwijderen) van TechNet Script Center als u management packs die afhankelijk zijn van andere management packs wilt verwijderen.  

1. Open de Operations Manager-opdrachtshell met een account dat lid is van de rol Administrators in Operations Manager.
   
    > [!WARNING]
    > Controleer voordat u doorgaat of er geen aangepaste management packs zijn met 'Advisor' of 'IntelligencePack' in de naam, anders worden ze uit de beheergroep verwijderd tijdens de volgende stappen.
    > 

1. Typ vanaf de opdrachtshell-prompt `Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
1. Typ vervolgens `Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
1. Als u ook alle resterende management packs die afhankelijk zijn van andere management packs van System Center Advisor wilt verwijderen, gebruikt u het script *RecursiveRemove.ps1* dat u eerder van TechNet Script Center hebt gedownload.  
 
    > [!NOTE]
    > De management packs voor Microsoft System Center Advisor of Microsoft System Center Advisor interne wordt niet automatisch verwijderd door de stap voor het verwijderen van de management packs voor Advisor met PowerShell.  Probeer niet om ze te verwijderen.  
    >  

1. Open de Operations-console van Operations Manager met een account dat lid is van de rol Administrators in Operations Manager.
1. Selecteer onder **Beheer** het knooppunt **Management Packs**, typ in het vak **Look for:** (Zoeken naar) **Advisor** en controleer of de volgende management packs nog zijn geïmporteerd in de beheergroep:
   
   * Microsoft System Center Advisor
   * Microsoft System Center Advisor Internal

1. Klik in de Azure-portal op de **instellingen** tegel.
1. Selecteer **verbonden bronnen**.
1. In de tabel onder de sectie System Center Operations Manager ziet u de naam van de beheergroep die u wilt verwijderen uit de werkruimte. Klik onder de kolom **Laatste gegevens** op **Verwijderen**.  
   
    > [!NOTE]
    > De koppeling **Verwijderen** is pas beschikbaar als er 14 dagen geen activiteit van de verbonden beheergroep is gedetecteerd.  
    > 

1. Er wordt een bevestigingsvenster weergegeven waarin u wordt gevraagd of u wilt doorgaan met het verwijderen.  Klik op **Ja** om door te gaan. 

Als u de twee connectors Microsoft.SystemCenter.Advisor.DataConnector en Advisor Connector wilt verwijderen, slaat u het onderstaande PowerShell-script op uw computer op en voert u het script uit met behulp van de volgende voorbeelden:

```
    .\OM2012_DeleteConnectors.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SystemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> Als de computer waarop u dit script uitvoert geen beheerserver is, moet mogelijk de Operations Manager-opdrachtshell erop zijn geïnstalleerd, afhankelijk van de versie van uw beheergroep.
> 
> 

```
    param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

In de toekomst als u van plan uw beheergroep die u wilt een Log Analytics-werkruimte opnieuw verbinding maken bent, moet u om opnieuw te importeren de `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` management pack-bestand. Afhankelijk van welke versie van System Center Operations Manager in uw omgeving is geïmplementeerd, bevindt dit bestand zich op de volgende locatie:

* Op het bronmedium in de map `\ManagementPacks` voor System Center 2016 - Operations Manager of hoger.
* Van het meest recente updatepakket dat op de beheergroep is toegepast. Voor Operations Manager 2012 is de bronmap ` %ProgramFiles%\Microsoft System Center 2012\Operations Manager\Server\Management Packs for Update Rollups` en voor 2012 R2 bevindt deze zich in `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups`.

## <a name="next-steps"></a>Volgende stappen
Zie [Log Analytics-oplossingen uit de galerie met oplossingen toevoegen](../../azure-monitor/insights/solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.


