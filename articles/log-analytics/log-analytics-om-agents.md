---
title: Maak verbinding met Operations Manager met Log Analytics | Microsoft Docs
description: Voor het onderhouden van uw bestaande investeringen in System Center Operations Manager en de uitgebreide mogelijkheden met Log Analytics gebruiken, kunt u de Operations Manager integreren met de OMS-werkruimte.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2017
ms.author: magoedte
ms.openlocfilehash: 6db47c7baa0a345a32d26d56e843acd0204ae50b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="connect-operations-manager-to-log-analytics"></a>Maak verbinding met Operations Manager met Log Analytics
Voor het onderhouden van uw bestaande investeringen in System Center Operations Manager en de uitgebreide mogelijkheden met Log Analytics gebruiken, kunt u de Operations Manager integreren met de OMS-werkruimte.  Hiermee kunt dat u gebruikmaken van de mogelijkheden van OMS terwijl u Operations Manager gebruiken voor:

* Doorgaan met het bewaken van de status van uw IT-services met Operations Manager
* Integratie met uw ITSM oplossingen die incidenten en oplossen van problemen management ondersteunen onderhouden
* De levenscyclus van de agents die zijn geïmplementeerd op de on-premises en openbare cloud IaaS virtuele machines die u met Operations Manager bewaken beheren

Integratie met System Center Operations Manager meerwaarde biedt voor uw strategie voor service-bewerkingen met behulp van de snelheid en efficiëntie van OMS in verzamelen, opslaan en analyseren van gegevens uit Operations Manager.  OMS helpt correleren van en werken aan de problemen van problemen te identificeren en op te halen terugkeerpatronen ter ondersteuning van uw bestaande beheerproces van het probleem.   De flexibiliteit van de zoekmachine om te onderzoeken, prestaties, gebeurtenis- en waarschuwingsgegevens met uitgebreide dashboards en rapportagemogelijkheden voor het weergeven van deze gegevens op allerlei zinvolle manieren demonstreert de sterkte die OMS brengt complimenting Operations Manager.

De agents die rapporteren aan de beheergroep van Operations Manager verzamelt gegevens van uw servers op basis van de gegevensbronnen Log Analytics en u hebt ingeschakeld in uw abonnement OMS-oplossingen.  Afhankelijk van de oplossing die u hebt ingeschakeld, gegevens van deze oplossingen zijn ofwel verzonden rechtstreeks vanuit een Operations Manager-beheerserver naar de OMS-webservice, of vanwege de hoeveelheid gegevens die worden verzameld op het systeem door agents beheerde rechtstreeks vanuit de agent worden verzonden met OMS-webservice. De beheerserver stuurt de OMS-gegevens rechtstreeks naar de webservice OMS; Deze worden nooit naar de OperationsManager of OperationsManagerDW-database geschreven.  Wanneer een beheerserver verliest de verbinding met de OMS-webservice, slaat de gegevens lokaal tot communicatie opnieuw tot stand gebracht met OMS is.  Als de beheerserver offline vanwege gepland onderhoud of niet-geplande uitval is, hervat een andere beheerserver in de beheergroep connectiviteit met OMS.  

Het volgende diagram ziet u de verbinding tussen de beheerservers en agents in een System Center Operations Manager-beheergroep en OMS, met inbegrip van de richting en poorten.   

![OMS-operations-manager-integratie-diagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

Als de beleidsregels van uw IT-beveiliging niet toestaan computers in uw netwerk verbinding maken met Internet dat, kunnen beheerservers worden geconfigureerd voor verbinding met de OMS-Gateway voor configuratie-informatie te ontvangen en verzenden van verzamelde gegevens afhankelijk van de oplossing die u hebt ingeschakeld.  Zie voor meer informatie en stapsgewijze instructies voor het configureren van uw Operations Manager-beheergroep om te communiceren via een OMS-Gateway met de OMS-service [computers koppelen aan OMS met behulp van de Gateway OMS](log-analytics-oms-gateway.md).  

## <a name="system-requirements"></a>Systeemvereisten
Voordat u begint, lees de volgende informatie om te controleren of u voldoet aan vereisten.

* OMS biedt alleen ondersteuning voor Operations Manager 2016, UR6 van Operations Manager 2012 SP1 en hoger, en UR2 van Operations Manager 2012 R2 en hoger.  Proxyondersteuning is toegevoegd aan Operations Manager 2012 SP1 UR7 en Operations Manager 2012 R2 UR3.
* Alle Operations Manager-agents moeten voldoen aan de vereisten voor minimale ondersteuning. Zorg ervoor dat agents de minimale bijgewerkt zijn, anders verkeer voor Windows-agent mislukken en veel fouten u het gebeurtenislogboek van Operations Manager vult mogelijk.
* Een OMS-abonnement.  Bekijk voor meer informatie [aan de slag met logboekanalyse](log-analytics-get-started.md).

### <a name="network"></a>Netwerk
Gegevens van de onderstaande lijst de vereist voor de Operations Manager-agent, beheerservers en Operations-console om te communiceren met OMS proxy- en firewall-configuratie-informatie.  Verkeer van elk onderdeel is uitgaand vanaf het netwerk naar de OMS-service.     

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


## <a name="connecting-operations-manager-to-oms"></a>Operations Manager met OMS koppelen
Voer de volgende reeks stappen voor het configureren van uw Operations Manager-beheergroep verbinding maken met een van uw werkruimten OMS.

1. Selecteer in de Operations Manager-console de **beheer** werkruimte.
2. Vouw het knooppunt Operations Management Suite en klik op **verbinding**.
3. Klik op de **registreren bij Operations Management Suite** koppeling.
4. Op de **Wizard Operations Management Suite voorbereiden: verificatie** pagina, voer het e-mailadres of telefoonnummer en het wachtwoord van het beheerdersaccount dat is gekoppeld aan uw abonnement OMS en klikt u op  **Meld u aan**.
5. Wanneer u bent geverifieerd, op de **Wizard Operations Management Suite voorbereiden: werkruimte selecteren** pagina u wordt gevraagd de OMS-werkruimte selecteren.  Als u meer dan één werkruimte hebt, selecteert u de werkruimte die u wilt registreren bij de Operations Manager-beheergroep van de vervolgkeuzelijst en klik vervolgens op **volgende**.
   
   > [!NOTE]
   > Operations Manager ondersteunt slechts één OMS-werkruimte tegelijk. De verbinding en de computers die zijn geregistreerd met OMS met de vorige werkruimte worden verwijderd van OMS.
   > 
   > 
6. Op de **Wizard Operations Management Suite voorbereiden: Samenvatting** pagina, bevestig de instellingen en als ze juist zijn, klikt u op **maken**.
7. Op de **Wizard Operations Management Suite voorbereiden: voltooien** pagina, klikt u op **sluiten**.

### <a name="add-agent-managed-computers"></a>Door agents beheerde computers toevoegen
Na het configureren van integratie met de OMS-werkruimte, dit alleen verbinding maakt met een OMS, geen gegevens verzameld van de agents die rapporteren aan de beheergroep. Dit zal niet worden uitgevoerd totdat nadat u hebt geconfigureerd welke specifieke door agents beheerde computers worden gegevens verzameld voor logboekanalyse. U kunt de computerobjecten afzonderlijk selecteren of u een groep met objecten voor Windows-computer kunt selecteren. U kunt een groep met exemplaren van een andere klasse, zoals logische schijven of SQL-databases niet selecteren.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Vouw het knooppunt Operations Management Suite en klik op **verbinding**.
3. Klik op de **een computergroep toevoegen** koppeling onder de acties kop aan de rechterkant van het deelvenster.
4. In de **Computer zoeken** in het dialoogvenster dat u kunt zoeken naar computers of groepen die worden bewaakt door Operations Manager. Selecteer computers of groepen voor Onboarding van OMS, klik op **toevoegen**, en klik vervolgens op **OK**.

U kunt weergeven, computers en groepen die zijn geconfigureerd voor het verzamelen van gegevens vanuit het knooppunt van de Computers die worden beheerd in Operations Management Suite in de **beheer** werkruimte van de Operations-console.  Hier kunt u toevoegen of verwijderen van computers en groepen indien nodig.

### <a name="configure-oms-proxy-settings-in-the-operations-console"></a>OMS proxy-instellingen configureren in de Operations-console
De volgende stappen uitvoeren als een interne proxyserver tussen de beheergroep en de OMS-webservice is.  Deze instellingen worden centraal beheerd uit de beheergroep en gedistribueerd naar door agent beheerde systemen die zijn opgenomen in het bereik voor het verzamelen van gegevens voor OMS.  Dit is nuttig voor wanneer bepaalde oplossingen overslaan van de beheerserver en gegevens rechtstreeks naar de webservice OMS verzenden.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Vouw Operations Management Suite en klik vervolgens op **verbindingen**.
3. Klik in de weergave OMS-verbinding op **Proxyserver configureren**.
4. Op **Wizard Operations Management Suite: proxyserver** pagina **een proxyserver gebruiken voor toegang tot de Operations Management Suite**, en typ vervolgens de URL met het poortnummer, bijvoorbeeld http:// corpproxy:80 en klik vervolgens op **voltooien**.

Als de proxyserver verificatie vereist, moet u de volgende stappen voor het configureren van referenties en instellingen die worden doorgegeven naar beheerde computers die rapporteert aan OMS in de beheergroep moet uitvoeren.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Selecteer onder **RunAs-configuratie** de optie **Profielen**.
3. Open het profiel **System Center Advisor RunAs-profielproxy**.
4. In de Wizard Run As-profiel, klik op toevoegen als u wilt uitvoeren als-account gebruiken. Kunt u een [Run As-account](https://technet.microsoft.com/library/hh321655.aspx) of een bestaand account gebruiken. Dit account moet over voldoende rechten beschikken voor het doorgeven van de proxyserver.
5. Kies het account voor het beheren van stelt **een geselecteerde klasse, groep of object**, klikt u op **selecteren...** en klik vervolgens op **groep...** openen van de **groep zoeken** vak.
6. Zoek en selecteer vervolgens **Microsoft System Center Advisor Monitoring Server-groep**.  Klik op **OK** na het selecteren van de groep te sluiten de **groep zoeken** vak.
7. Klik op **OK** sluiten de **een Run As-account toevoegen** vak.
8. Klik op **opslaan** naar de wizard hebt voltooid en sla de wijzigingen.

Nadat de verbinding is gemaakt en u configureren welke agents verzamelt en rapportgegevens op OMS, wordt in de beheergroep niet noodzakelijkerwijs in volgorde van de volgende configuratie toegepast:

* De Run As-Account **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** wordt gemaakt.  Het is gekoppeld aan het Run As-profiel **Microsoft System Center Advisor uitvoeren als-profiel Blob** en is gemaakt voor twee klassen - **Verzamelingserver** en **Operations Manager-beheergroep** .
* Twee connectors worden gemaakt.  De eerste heet **Microsoft.SystemCenter.Advisor.DataConnector** en wordt automatisch geconfigureerd met een abonnement dat u alle waarschuwingen gegenereerd op basis van exemplaren van alle klassen in de beheergroep met OMS Log Analytics stuurt. De tweede connector is **Advisor Connector**, die verantwoordelijk is voor communicatie met OMS-webservice en delen van gegevens.
* Agents en groepen die u hebt geselecteerd voor het verzamelen van gegevens in de beheergroep is toegevoegd aan de **Microsoft System Center Advisor Monitoring Server-groep**.

## <a name="management-pack-updates"></a>Management pack-updates
Nadat de configuratie is voltooid, de beheergroep van Operations Manager een verbinding maakt met de OMS-service.  De beheerserver wordt gesynchroniseerd met de webservice en informatie over de bijgewerkte configuratie ontvangen in de vorm van management packs voor de oplossingen die u hebt ingeschakeld en die zijn geïntegreerd met Operations Manager.   Operations Manager wordt gecontroleerd op updates van deze management packs en automatisch downloaden en importeert wanneer ze beschikbaar.  Er zijn twee regels in het bijzonder die dit gedrag beheren:

* **Microsoft.SystemCenter.Advisor.MPUpdate** -Updates van de basis OMS management packs. Standaard wordt elke 12 uur uitgevoerd.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** -oplossing management packs is ingeschakeld in uw werkruimte-Updates. Wordt standaard uitgevoerd om minuten vijf (5).

U kunt deze twee regels voor automatische downloads voorkomen door ze uit te schakelen, of wijzigen van de frequentie voor hoe vaak de beheerserver met OMS om te bepalen synchroniseert of een nieuw management pack beschikbaar en moet worden gedownload overschrijven.  Volg de stappen [het onderdrukken van een regel of Monitor](https://technet.microsoft.com/library/hh212869.aspx) wijzigen de **frequentie** parameter met een waarde in seconden voor het wijzigen van de synchronisatieplanning, of wijzig de **ingeschakeld**-parameter voor de regels uitschakelen.  Richt de onderdrukkingen op alle objecten van klasse Operations Manager-beheergroep.

Als u doorgaan na uw bestaande wijziging proces wilt voor het beheren van management Packs in uw productiebeheergroep, kunt u de regels uitschakelen en inschakelen tijdens bepaalde tijden wanneer updates zijn toegestaan. Als u een ontwikkelings- of QA-beheergroep in uw omgeving hebt en deze verbonden met Internet is, kunt u die beheergroep configureren met een OMS-werkruimte om dit scenario te ondersteunen.  Hiermee kunt u bekijken en evalueren van de iteratieve releases van de OMS-management packs voordat ze in uw productiebeheergroep zijn vrijgegeven.

## <a name="switch-an-operations-manager-group-to-a-new-oms-workspace"></a>Een Operations Manager-beheergroep overschakelen naar een nieuwe OMS-werkruimte
1. Aanmelden bij uw abonnement OMS en maken van een werkruimte in [Microsoft Operations Management Suite](http://oms.microsoft.com/).
2. Open de Operations Manager-console met een account dat lid is van de rol Administrators in Operations Manager en selecteer de **beheer** werkruimte.
3. Vouw Operations Management Suite en selecteer **verbindingen**.
4. Selecteer de **bewerking Management Suite opnieuw configureren** koppeling op de middelste-zijde van het deelvenster.
5. Ga als volgt de **Wizard Operations Management Suite voorbereiden** en voer de e-mailadres of telefoonnummer getal en het wachtwoord van de administrator-account dat is gekoppeld aan uw nieuwe OMS-werkruimte.
   
   > [!NOTE]
   > De **Wizard Operations Management Suite voorbereiden: werkruimte selecteren** pagina geeft de bestaande werkruimte die wordt gebruikt.
   > 
   > 

## <a name="validate-operations-manager-integration-with-oms"></a>Integratie van Operations Manager met OMS valideren
Er zijn een aantal verschillende manieren kunt u controleren of uw OMS aan Operations Manager-integratie is geslaagd.

### <a name="to-confirm-integration-from-the-oms-portal"></a>Om te bevestigen dat de integratie van OMS-portal
1. Klik in de OMS-portal op de **instellingen** tegel
2. Selecteer **verbonden gegevensbronnen**.
3. U ziet in de tabel onder de sectie System Center Operations Manager, de naam van de beheergroep worden weergegeven met het aantal agents en status wanneer gegevens voor het laatst is ontvangen.
   
   ![OMS-instellingen-connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)
4. Opmerking de **werkruimte-ID** waarde onder de aan de linkerkant van de pagina instellingen.  U te valideren op basis van uw beheergroep Operations Manager.  

### <a name="to-confirm-integration-from-the-operations-console"></a>Om te bevestigen dat de integratie van de Operations-console
1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Selecteer **Management Packs** en in de **zoekt u naar:** vak teksttype **Advisor** of **Intelligence**.
3. Afhankelijk van de oplossingen die u hebt ingeschakeld, ziet u een overeenkomstige management pack die worden vermeld in de zoekresultaten.  Bijvoorbeeld, als u de waarschuwing beheeroplossing hebt ingeschakeld, is het management pack Microsoft System Center Advisor waarschuwing Management in de lijst.
4. Van de **bewaking** bekijken, navigeert u naar de **Operations Management Suite\Health status** weergeven.  Selecteer een beheerserver onder de **status van beheerservers** deelvenster en in de **detailweergave** deelvenster Bevestig de waarde voor eigenschap **URI verificatieservice** komt overeen met de OMS-werkruimte-ID.
   
   ![OMS-opsmgr-mg-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-oms"></a>Integratie met OMS verwijderen
Wanneer u integratie tussen uw Operations Manager-beheergroep en de OMS-werkruimte niet meer nodig hebt, zijn er verschillende stappen nodig goed verwijderen van de verbinding en de configuratie in de beheergroep. De volgende procedure moet u de OMS-werkruimte bijwerken door de documentatie van uw beheergroep te verwijderen, verwijder de OMS-connectors en verwijder vervolgens de management packs ondersteunende OMS.   

Management packs voor de oplossingen u die zijn geïntegreerd met Operations Manager hebt ingeschakeld en de management packs vereist ter ondersteuning van integratie met de OMS-service kunnen niet eenvoudig worden verwijderd uit de beheergroep.  Dit komt omdat sommige van de OMS-management packs afhankelijk van andere gerelateerde management packs zijn.  Download het script voor het verwijderen van management packs met een afhankelijkheid van andere management packs [verwijderen van een management pack met afhankelijkheden](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) van TechNet Script Center.  

1. Open de opdrachtshell van Operations Manager met een account dat lid is van de rol Administrators in Operations Manager.
   
    > [!WARNING]
    > Controleer of u hebt geen aangepaste management packs die het woord Advisor of IntelligencePack in de naam voordat u doorgaat, anders de volgende stappen verwijderen uit de beheergroep.
    > 

2. Typ vanaf de opdrachtregel-shell`Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
3. Volgende type`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
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

In de toekomst als u opnieuw verbinding te maken van uw beheergroep met een OMS-werkruimte wilt, moet u opnieuw importeren de `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` management pack-bestand van de meest recente updatepakket zijn toegepast op uw beheergroep.  U vindt dit bestand in de `%ProgramFiles%\Microsoft System Center 2012` of de `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` map.

## <a name="next-steps"></a>Volgende stappen
Als u wilt toevoegen, functionaliteit en verzamelen van gegevens, Zie [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).


