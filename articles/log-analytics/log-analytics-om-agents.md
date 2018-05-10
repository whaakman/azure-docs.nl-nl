---
title: Maak verbinding met Operations Manager met Log Analytics | Microsoft Docs
description: Voor het onderhouden van uw bestaande investeringen in System Center Operations Manager en de uitgebreide mogelijkheden met Log Analytics gebruiken, kunt u de Operations Manager integreren met uw werkruimte.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: magoedte
ms.openlocfilehash: 84eabef06b4d2ad71e6d9a947a77589f9159e030
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="connect-operations-manager-to-log-analytics"></a>Maak verbinding met Operations Manager met Log Analytics
Voor het onderhouden van uw bestaande investeringen in System Center Operations Manager en de uitgebreide mogelijkheden met Log Analytics gebruiken, kunt u de Operations Manager integreren met uw werkruimte voor logboekanalyse.  Hiermee kunt dat u gebruikmaken van de mogelijkheden van logboekanalyse terwijl u Operations Manager gebruiken voor:

* De status van uw IT-services met Operations Manager
* Integratie met uw ITSM oplossingen die incidenten en oplossen van problemen management ondersteunen onderhouden
* De levenscyclus van de agents die zijn geïmplementeerd op de on-premises en openbare cloud IaaS virtuele machines die u met Operations Manager bewaken beheren

Integratie met System Center Operations Manager meerwaarde biedt voor uw strategie voor service-bewerkingen met behulp van de snelheid en efficiëntie van logboekanalyse in verzamelen, opslaan en analyseren van gegevens uit Operations Manager.  Log Analytics helpt correleer en werken aan de problemen van problemen te identificeren en op te halen terugkeerpatronen ter ondersteuning van uw bestaande beheerproces van het probleem.  De flexibiliteit van de zoekmachine om te onderzoeken, prestaties, gebeurtenis en een waarschuwing gegevens, met uitgebreide dashboards en rapportagemogelijkheden voor het weergeven van deze gegevens op allerlei zinvolle manieren demonstreert de sterkte logboekanalyse brengt complimenting Operations Manager.

De agents die rapporteren aan de beheergroep van Operations Manager verzamelt gegevens van uw servers op basis van de Log Analytics-gegevensbronnen en oplossingen die u hebt ingeschakeld in uw werkruimte.  Afhankelijk van de oplossingen die zijn ingeschakeld, hun gegevens worden ofwel verzonden rechtstreeks vanuit een Operations Manager-beheerserver met de service of vanwege de hoeveelheid gegevens die worden verzameld op het systeem agent worden beheerd met logboekanalyse rechtstreeks vanuit de agent zijn verzonden. De beheerserver stuurt de gegevens rechtstreeks naar de service; Deze worden nooit naar de operationele of datawarehouse-database geschreven.  Wanneer een beheerserver connectiviteit met Log Analytics verliest, slaat de gegevens lokaal tot communicatie opnieuw tot stand gebracht met logboekanalyse is.  Als de beheerserver offline vanwege gepland onderhoud of niet-geplande uitval is, hervat een andere beheerserver in de beheergroep verbonden met logboekanalyse.  

Het volgende diagram toont de verbinding tussen de beheerservers en agents in een System Center Operations Manager-beheergroep en Log Analytics, met inbegrip van de richting en poorten.   

![OMS-operations-manager-integratie-diagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

Als de beleidsregels van uw IT-beveiliging niet toestaan computers in uw netwerk verbinding maken met Internet dat, kunnen beheerservers worden geconfigureerd voor verbinding met de OMS-Gateway voor configuratie-informatie ontvangen en verzenden van verzamelde gegevens afhankelijk van de oplossingen die zijn ingeschakeld.  Zie voor meer informatie en stapsgewijze instructies voor het configureren van uw Operations Manager-beheergroep om te communiceren via een OMS-Gateway naar de service logboekanalyse [computers koppelen aan OMS met behulp van de Gateway OMS](log-analytics-oms-gateway.md).  

## <a name="system-requirements"></a>Systeemvereisten
Voordat u begint, lees de volgende informatie om te controleren of u voldoet aan vereisten.

* Log Analytics biedt alleen ondersteuning voor System Center Operations Manager 1801, Operations Manager 2016 Operations Manager 2012 SP1 UR6 en hoger, en UR2 van Operations Manager 2012 R2 en hoger.  Proxyondersteuning is toegevoegd aan Operations Manager 2012 SP1 UR7 en Operations Manager 2012 R2 UR3.
* Alle Operations Manager-agents moeten voldoen aan de vereisten voor minimale ondersteuning. Zorg ervoor dat agents de minimale bijgewerkt zijn, anders verkeer voor Windows-agent mislukken en veel fouten u het gebeurtenislogboek van Operations Manager vult mogelijk.
* Een werkruimte voor logboekanalyse.  Bekijk voor meer informatie [aan de slag met logboekanalyse](log-analytics-get-started.md).

### <a name="network"></a>Netwerk
Gegevens van de onderstaande lijst de vereist voor de Operations Manager-agent, beheerservers en Operations-console om te communiceren met logboekanalyse proxy- en firewall-configuratie-informatie.  Verkeer van elk onderdeel is uitgaand vanaf het netwerk naar de Log Analytics-service.     

|Resource | Poortnummer| HTTP-controle overslaan|  
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
|**Operations Manager-console met OMS**|||  
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

## <a name="connecting-operations-manager-to-log-analytics"></a>Operations Manager verbinden met Log Analytics
Voer de volgende reeks stappen voor het configureren van uw Operations Manager-beheergroep verbinding maken met een van uw Log Analytics-werkruimten.

Als dit de eerste keer dat de Operations Manager-beheergroep met een werkruimte voor logboekanalyse registreert en de beheerservers communiceren met de service via een proxy of Gateway OMS-server, de mogelijkheid moeten te geven van de configuratie van de proxy voor de beheergroep is niet beschikbaar in de Operations-console.  De beheergroep heeft met succes worden geregistreerd met de service voordat u deze optie is beschikbaar.  U moet de systeem-proxyconfiguratie met behulp van Netsh op het systeem uw uitgevoerd vanuit de Operations-console voor het configureren van integratie en alle beheerservers in de beheergroep bijwerken.  

1. Open een opdrachtprompt.
   a. Ga naar **Start** en het type **cmd**.
   b. Met de rechtermuisknop op **opdrachtprompt** en uitvoeren als beheerder ** selecteren.
2. Voer de volgende opdracht en druk op **Enter**:

    `netsh winhttp set proxy <proxy>:<port>`

Na het voltooien van de volgende stappen uit om te integreren met Log Analytics, kunt u de configuratie verwijderen door het uitvoeren van `netsh winhttp reset proxy` en gebruik vervolgens de **proxyserver configureren** optie in de Operations-console de proxy of OMS opgeven Gateway-server. 

1. Selecteer in de Operations Manager-console de **beheer** werkruimte.
2. Vouw het knooppunt Operations Management Suite en klik op **verbinding**.
3. Klik op de **registreren bij Operations Management Suite** koppeling.
4. Op de **Wizard Operations Management Suite voorbereiden: verificatie** pagina, voer het e-mailadres of telefoonnummer en het wachtwoord van het beheerdersaccount dat is gekoppeld aan uw abonnement OMS en klikt u op  **Meld u aan**.
5. Wanneer u bent geverifieerd, op de **Wizard Operations Management Suite voorbereiden: werkruimte selecteren** pagina u wordt gevraagd uw werkruimte voor logboekanalyse selecteren.  Als u meer dan één werkruimte hebt, selecteert u de werkruimte die u wilt registreren bij de Operations Manager-beheergroep van de vervolgkeuzelijst en klik vervolgens op **volgende**.
   
   > [!NOTE]
   > Operations Manager ondersteunt slechts één werkruimte voor logboekanalyse tegelijk. De verbinding en de computers die zijn geregistreerd met logboekanalyse met de vorige werkruimte worden verwijderd van logboekanalyse.
   > 
   > 
6. Op de **Wizard Operations Management Suite voorbereiden: Samenvatting** pagina, bevestig de instellingen en als ze juist zijn, klikt u op **maken**.
7. Op de **Wizard Operations Management Suite voorbereiden: voltooien** pagina, klikt u op **sluiten**.

### <a name="add-agent-managed-computers"></a>Door agents beheerde computers toevoegen
Na het configureren van integratie met uw werkruimte voor logboekanalyse, dit alleen een verbinding maakt met de service, geen gegevens verzameld van de agents die rapporteren aan de beheergroep. Dit zal niet worden uitgevoerd totdat nadat u hebt geconfigureerd welke specifieke door agents beheerde computers worden gegevens verzameld voor logboekanalyse. U kunt de computerobjecten afzonderlijk selecteren of u een groep met objecten voor Windows-computer kunt selecteren. U kunt een groep met exemplaren van een andere klasse, zoals logische schijven of SQL-databases niet selecteren.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Vouw het knooppunt Operations Management Suite en klik op **verbinding**.
3. Klik op de **een computergroep toevoegen** koppeling onder de acties kop aan de rechterkant van het deelvenster.
4. In de **Computer zoeken** in het dialoogvenster dat u kunt zoeken naar computers of groepen die worden bewaakt door Operations Manager. Selecteer computers of groepen voor Onboarding van logboekanalyse, klik op **toevoegen**, en klik vervolgens op **OK**.

U kunt weergeven, computers en groepen die zijn geconfigureerd voor het verzamelen van gegevens vanuit het knooppunt van de Computers die worden beheerd in Operations Management Suite in de **beheer** werkruimte van de Operations-console.  Hier kunt u toevoegen of verwijderen van computers en groepen indien nodig.

### <a name="configure-proxy-settings-in-the-operations-console"></a>Proxy-instellingen configureren in de Operations-console
De volgende stappen uitvoeren als een interne proxyserver tussen de beheergroep en Log Analytics-service is.  Deze instellingen worden centraal beheerd uit de beheergroep en gedistribueerd naar door agent beheerde systemen die zijn opgenomen in het bereik voor het verzamelen van gegevens voor logboekanalyse.  Dit is nuttig voor wanneer bepaalde oplossingen overslaan van de beheerserver en het verzenden van gegevens rechtstreeks naar de service.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Vouw Operations Management Suite en klik vervolgens op **verbindingen**.
3. Klik in de weergave OMS-verbinding op **Proxyserver configureren**.
4. Op **Wizard Operations Management Suite: proxyserver** pagina **een proxyserver gebruiken voor toegang tot de Operations Management Suite**, en typ vervolgens de URL met het poortnummer, bijvoorbeeld http://corpproxy:80 en klik vervolgens op **voltooien**.

Als de proxyserver verificatie vereist, moet u de volgende stappen voor het configureren van referenties en instellingen die worden doorgegeven naar beheerde computers die rapporteert aan OMS in de beheergroep moet uitvoeren.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Selecteer onder **RunAs-configuratie** de optie **Profielen**.
3. Open het profiel **System Center Advisor RunAs-profielproxy**.
4. In de Wizard Run As-profiel, klik op toevoegen als u wilt uitvoeren als-account gebruiken. Kunt u een [Run As-account](https://technet.microsoft.com/library/hh321655.aspx) of een bestaand account gebruiken. Dit account moet over voldoende rechten beschikken voor het doorgeven van de proxyserver.
5. Kies het account voor het beheren van stelt **een geselecteerde klasse, groep of object**, klikt u op **selecteren...** en klik vervolgens op **groep...** openen van de **groep zoeken** vak.
6. Zoek en selecteer vervolgens **Microsoft System Center Advisor Monitoring Server-groep**.  Klik op **OK** na het selecteren van de groep te sluiten de **groep zoeken** vak.
7. Klik op **OK** sluiten de **een Run As-account toevoegen** vak.
8. Klik op **opslaan** naar de wizard hebt voltooid en sla de wijzigingen.

Nadat de verbinding is gemaakt en u configureren welke agents wordt verzamelen en rapporteren van gegevens met Log Analytics, wordt in de beheergroep niet noodzakelijkerwijs in volgorde van de volgende configuratie toegepast:

* De Run As-Account **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** wordt gemaakt.  Het is gekoppeld aan het Run As-profiel **Microsoft System Center Advisor uitvoeren als-profiel Blob** en is gemaakt voor twee klassen - **Verzamelingserver** en **Operations Manager-beheergroep** .
* Twee connectors worden gemaakt.  De eerste heet **Microsoft.SystemCenter.Advisor.DataConnector** en wordt automatisch geconfigureerd met een abonnement dat u alle waarschuwingen gegenereerd op basis van exemplaren van alle klassen in de beheergroep met logboekanalyse stuurt. De tweede connector is **Advisor Connector**, die verantwoordelijk is voor communicatie met OMS-webservice en delen van gegevens.
* Agents en groepen die u hebt geselecteerd voor het verzamelen van gegevens in de beheergroep is toegevoegd aan de **Microsoft System Center Advisor Monitoring Server-groep**.

## <a name="management-pack-updates"></a>Management pack-updates
Nadat de configuratie is voltooid, stelt u de beheergroep van Operations Manager een verbinding met de Log Analytics-service.  De beheerserver wordt gesynchroniseerd met de webservice en informatie over de bijgewerkte configuratie ontvangen in de vorm van management packs voor de oplossingen die u hebt ingeschakeld en die zijn geïntegreerd met Operations Manager.   Operations Manager wordt gecontroleerd op updates van deze management packs en automatisch downloaden en importeert wanneer ze beschikbaar.  Er zijn twee regels in het bijzonder die dit gedrag beheren:

* **Microsoft.SystemCenter.Advisor.MPUpdate** -Updates van de basis Log Analytics management packs. Standaard wordt elke 12 uur uitgevoerd.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** -oplossing management packs is ingeschakeld in uw werkruimte-Updates. Wordt standaard uitgevoerd om minuten vijf (5).

U kunt deze twee regels voor automatische downloads voorkomen door ze uit te schakelen, of wijzigen van de frequentie voor hoe vaak de beheerserver met OMS om te bepalen synchroniseert of een nieuw management pack beschikbaar en moet worden gedownload overschrijven.  Volg de stappen [het onderdrukken van een regel of Monitor](https://technet.microsoft.com/library/hh212869.aspx) wijzigen de **frequentie** parameter met een waarde in seconden voor het wijzigen van de synchronisatieplanning, of wijzig de **ingeschakeld**-parameter voor de regels uitschakelen.  Richt de onderdrukkingen op alle objecten van klasse Operations Manager-beheergroep.

Als u doorgaan na uw bestaande wijziging proces wilt voor het beheren van management Packs in uw productiebeheergroep, kunt u de regels uitschakelen en inschakelen tijdens bepaalde tijden wanneer updates zijn toegestaan. Als u een ontwikkelings- of QA-beheergroep in uw omgeving hebt en deze verbonden met Internet is, kunt u de beheergroep met een werkruimte voor logboekanalyse voor de ondersteuning van dit scenario kunt configureren.  Hiermee kunt u bekijken en evalueren van de iteratieve releases van de Log Analytics management packs voordat ze in uw productiebeheergroep zijn vrijgegeven.

## <a name="switch-an-operations-manager-group-to-a-new-log-analytics-workspace"></a>Een Operations Manager-beheergroep overschakelen naar een nieuwe Log Analytics-werkruimte
1. Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com).
2. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **logboekanalyse** en maak vervolgens een werkruimte.  
3. Open de Operations Manager-console met een account dat lid is van de rol Administrators in Operations Manager en selecteer de **beheer** werkruimte.
4. Vouw Operations Management Suite en selecteer **verbindingen**.
5. Selecteer de **bewerking Management Suite opnieuw configureren** koppeling op de middelste-zijde van het deelvenster.
6. Ga als volgt de **Wizard Operations Management Suite voorbereiden** en voer de e-mailadres of telefoonnummer getal en het wachtwoord van de administrator-account dat is gekoppeld aan uw nieuwe werkruimte voor logboekanalyse.
   
   > [!NOTE]
   > De **Wizard Operations Management Suite voorbereiden: werkruimte selecteren** pagina geeft de bestaande werkruimte die wordt gebruikt.
   > 
   > 

## <a name="validate-operations-manager-integration-with-log-analytics"></a>Operations Manager-integratie met logboekanalyse valideren
Er zijn een aantal verschillende manieren kunt u controleren logboekanalyse tot integratie van Operations Manager is voltooid.

### <a name="to-confirm-integration-from-the-azure-portal"></a>Om te bevestigen dat de integratie van de Azure-portal
1. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer.
2. Selecteer de toepasselijke werkruimte in de lijst met Log Analytics-werkruimten.  
3. Selecteer **geavanceerde instellingen**, selecteer **verbonden bronnen**, en selecteer vervolgens **System Center**. 
4. U ziet in de tabel onder de sectie System Center Operations Manager, de naam van de beheergroep worden weergegeven met het aantal agents en status wanneer gegevens voor het laatst is ontvangen.
   
   ![OMS-instellingen-connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)

### <a name="to-confirm-integration-from-the-operations-console"></a>Om te bevestigen dat de integratie van de Operations-console
1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Selecteer **Management Packs** en in de **zoekt u naar:** vak teksttype **Advisor** of **Intelligence**.
3. Afhankelijk van de oplossingen die u hebt ingeschakeld, ziet u een overeenkomstige management pack die worden vermeld in de zoekresultaten.  Bijvoorbeeld, als u de waarschuwing beheeroplossing hebt ingeschakeld, is het management pack Microsoft System Center Advisor waarschuwing Management in de lijst.
4. Van de **bewaking** bekijken, navigeert u naar de **Operations Management Suite\Health status** weergeven.  Selecteer een beheerserver onder de **status van beheerservers** deelvenster en in de **detailweergave** deelvenster Bevestig de waarde voor eigenschap **URI verificatieservice** komt overeen met de Log Analytics werkruimte-ID.
   
   ![OMS-opsmgr-mg-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-log-analytics"></a>Integratie met logboekanalyse verwijderen
Wanneer u niet langer integratie tussen uw Operations Manager-beheergroep en de werkruimte voor logboekanalyse vereist, zijn er verschillende stappen nodig goed verwijderen van de verbinding en de configuratie in de beheergroep. De volgende procedure moet u uw werkruimte voor logboekanalyse bijwerken door de documentatie van uw beheergroep te verwijderen, de connectors voor logboekanalyse verwijderen en vervolgens verwijderen beheerpakketten ondersteunende integratie met de service.   

Management packs voor de oplossingen u die zijn geïntegreerd met Operations Manager hebt ingeschakeld en de management packs vereist ter ondersteuning van integratie met de Log Analytics-service kunnen niet eenvoudig worden verwijderd uit de beheergroep.  Dit komt omdat sommige van de Log Analytics management packs afhankelijk van andere gerelateerde management packs zijn.  Download het script voor het verwijderen van management packs met een afhankelijkheid van andere management packs [verwijderen van een management pack met afhankelijkheden](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) van TechNet Script Center.  

1. Open de opdrachtshell van Operations Manager met een account dat lid is van de rol Administrators in Operations Manager.
   
    > [!WARNING]
    > Controleer of u hebt geen aangepaste management packs die het woord Advisor of IntelligencePack in de naam voordat u doorgaat, anders de volgende stappen verwijderen uit de beheergroep.
    > 

2. Typ vanaf de opdrachtregel-shell `Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
3. Volgende type `Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
4. Het script gebruiken voor het verwijderen van management packs die een afhankelijkheid hebben van andere management packs van System Center Advisor resterende *RecursiveRemove.ps1* u eerder via het TechNet Script Center hebt gedownload.  
 
    > [!NOTE]
    > De Microsoft System Center Advisor of Microsoft System Center Advisor interne management packs niet verwijderen.  
    >  

5. Open de Operations Manager, Operations-console met een account dat lid is van de rol Administrators in Operations Manager.
6. Onder **beheer**, selecteer de **Management Packs** knooppunt en in de **zoekt u naar:** in het vak **Advisor** en controleer het volgende Management packs zijn nog steeds in de beheergroep geïmporteerd:
   
   * Microsoft System Center Advisor
   * Microsoft System Center Advisor interne
7. Klik in de OMS-portal op de tegel **Instellingen**.
8. Selecteer **verbonden gegevensbronnen**.
9. U ziet in de tabel onder de sectie System Center Operations Manager, de naam van de beheergroep die u wilt verwijderen uit de werkruimte.  Onder de kolom **laatste gegevens**, klikt u op **verwijderen**.  
   
    > [!NOTE]
    > De **verwijderen** koppeling wordt pas beschikbaar nadat het is 14 dagen als er geen activiteit gedetecteerd op basis van de verbonden beheergroep.  
    > 

10. Een venster weergegeven waarin u te bevestigen dat u wilt doorgaan met het verwijderen.  Klik op **Ja** om door te gaan. 

Als u wilt verwijderen van de twee connectors - Microsoft.SystemCenter.Advisor.DataConnector en Advisor Connector, sla het onderstaande PowerShell-script op uw computer en uitvoeren met behulp van de volgende voorbeelden:

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> De computer die u dit script uit als dit niet een beheerserver uitvoert, moet de Operations Manager-opdrachtshell afhankelijk van de versie van uw beheergroep zijn geïnstalleerd hebben.
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

In de toekomst als u opnieuw verbinding te maken voor een werkruimte voor logboekanalyse uw beheergroep wilt, moet u opnieuw importeren de `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` management pack-bestand.  Afhankelijk van de versie van System Center Operations Manager is geïmplementeerd in uw omgeving, kunt u dit bestand vinden in de volgende locatie:

* Op de bronmedia onder de `\ManagementPacks` map voor System Center 2016 - Operations Manager en hoger.
* Van de meest recente update rollup toegepast op uw beheergroep.  Voor Operations Manager 2012 de bronmap is` %ProgramFiles%\Microsoft System Center 2012\Operations Manager\Server\Management Packs for Update Rollups` en voor 2012 R2, bevindt zich in `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups`.

## <a name="next-steps"></a>Volgende stappen
Als u wilt toevoegen, functionaliteit en verzamelen van gegevens, Zie [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).


