---
title: 'Azure AD Connect-synchronisatie: Operationele taken en overwegingen | Microsoft Docs'
description: Dit onderwerp beschrijft operationele taken voor het Azure AD Connect-synchronisatie en hoe u voorbereidt voor dit onderdeel uitgevoerd.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: c4dc5ae107cc8babbd425edd6c5de428e130fc3a
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467533"
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect-synchronisatie: Operationele taken en afwegingen
Het doel van dit onderwerp is om te beschrijven van operationele taken voor Azure AD Connect-synchronisatie.

## <a name="staging-mode"></a>Faseringsmodus
De faseringsmodus kan worden gebruikt voor verschillende scenario's, met inbegrip van:

* Hoge beschikbaarheid.
* Testen en implementeren van nieuwe wijzigingen in de configuratie.
* Introduceren een nieuwe server en de oude uit bedrijf nemen.

Met een server in de faseringsmodus bevindt, kunt u wijzigingen aanbrengt aan de configuratie en de wijzigingen bekijken voordat u de server actief maken. U kunt er ook volledige import en een volledige synchronisatie om te verifiëren dat alle wijzigingen worden verwacht, voordat u deze wijzigingen in uw productieomgeving aanbrengt uit te voeren.

Tijdens de installatie, kunt u de server zich in **faseringsmodus**. Deze actie wordt de server actief is voor het importeren en synchroniseren, maar kan niet worden geëxporteerd worden uitgevoerd. Een server in de faseringsmodus wordt niet uitgevoerd Wachtwoordsynchronisatie of het terugschrijven van wachtwoorden, zelfs als u deze functies geselecteerd tijdens de installatie. Wanneer u de faseringsmodus uitschakelt, de server is, wordt het exporteren wordt gestart, kunt Wachtwoordsynchronisatie en kunt het terugschrijven van wachtwoorden.

> [!NOTE]
> Stel dat u hebt een Azure AD Connect met synchronisatie van wachtwoord-hash-functie is ingeschakeld. Wanneer u de faseringsmodus bevindt, de server stopt synchroniseren wachtwoord wordt gewijzigd van inschakelt on-premises AD. Wanneer u de faseringsmodus uitschakelt, hervat de server synchroniseren van wachtwoordwijzigingen vanuit waar het laatste afgebroken. Als de server is voor een lange periode links in de faseringsmodus bevindt, duurt het even voor de server om te synchroniseren van alle wachtwoordwijzigingen die tijdens de periode zich heeft voorgedaan.
>
>

U kunt nog steeds een export afdwingen met behulp van de synchronization servicemanager.

Een server in de faseringsmodus bevindt, blijft het ontvangen van wijzigingen van Active Directory en Azure AD. Deze heeft altijd een kopie van de meest recente wijzigingen en kan zeer snel nemen over de verantwoordelijkheden van een andere server. Als u configuratiewijzigingen in de primaire server aanbrengt, is uw verantwoordelijkheid de dezelfde wijzigingen aanbrengen in de server in de faseringsmodus bevindt.

De faseringsmodus is voor mensen met kennis van de oudere synchronisatie-technologieën, verschillende omdat de server een eigen SQL-database heeft. Deze architectuur kunt de modus server met tijdelijke bestanden zich bevinden in een ander datacenter.

### <a name="verify-the-configuration-of-a-server"></a>Controleer de configuratie van een server
Als u wilt toepassen met deze methode, de volgende stappen uit:

1. [Voorbereiden](#prepare)
2. [Configuratie](#configuration)
3. [Importeren en synchroniseren](#import-and-synchronize)
4. [Controleer of](#verify)
5. [Active switchserver](#switch-active-server)

#### <a name="prepare"></a>Voorbereiden
1. Azure AD Connect installeert, selecteert u **faseringsmodus**, en hef de selectie van **synchronisatie starten** op de laatste pagina in de installatiewizard. In deze modus kunt u de synchronisatie-engine handmatig uitvoeren.
   ![ReadyToConfigure](./media/how-to-connect-sync-operations/readytoconfigure.png)
2. Meld u af/sign in en uit het menu start selecteren **Synchronization Service**.

#### <a name="configuration"></a>Configuratie
Als u aangepaste wijzigingen hebt aangebracht in de primaire server en u wilt vergelijken van de configuratie met de staging-server, gebruikt u [documentatie voor Azure AD Connect-configuratie](https://github.com/Microsoft/AADConnectConfigDocumenter).

#### <a name="import-and-synchronize"></a>Importeren en synchroniseren
1. Selecteer **Connectors**, en selecteert u de Connector met het type **Active Directory Domain Services**. Klik op **uitvoeren**, selecteer **volledige import**, en **OK**. Voer deze stappen uit voor alle Connectors van dit type.
2. Selecteer de Connector met het type **Azure Active Directory (Microsoft)**. Klik op **uitvoeren**, selecteer **volledige import**, en **OK**.
3. Zorg ervoor dat het tabblad Connectors is nog steeds is geselecteerd. Voor elke Connector met het type **Active Directory Domain Services**, klikt u op **uitvoeren**, selecteer **Deltasynchronisatie**, en **OK**.
4. Selecteer de Connector met het type **Azure Active Directory (Microsoft)**. Klik op **uitvoeren**, selecteer **Deltasynchronisatie**, en **OK**.

U hebt nu gefaseerd uitvoer wordt gewijzigd in Azure AD en on-premises AD (als u hybride implementatie voor Exchange). De volgende stappen kunt u controleren wat bijna wordt gewijzigd voordat u de uitvoer naar de mappen daadwerkelijk wordt gestart.

#### <a name="verify"></a>Verifiëren
1. Start een opdrachtprompt en Ga naar `%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Uitvoeren: `csexport "Name of Connector" %temp%\export.xml /f:x` De naam van de Connector kan worden gevonden in Synchronization Service. Er is een naam die lijkt op 'contoso.com – AAD' voor Azure AD.
3. Uitvoeren: `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv` U hebt een bestand in % temp % met de naam export.csv die kunnen worden onderzocht in Microsoft Excel. Dit bestand bevat alle wijzigingen die moeten worden geëxporteerd.
4. Noodzakelijke wijzigingen aanbrengen in de gegevens of configuratie en voer deze stappen opnieuw (importeren en synchroniseren en controleren) totdat de wijzigingen die moeten worden geëxporteerd naar verwachting.

**Inzicht krijgen in het bestand export.csv** grootste deel van het bestand is geen uitleg. Sommige afkortingen te begrijpen van de inhoud:
* OMODT – wijziging objecttype. Geeft aan of de bewerking op het objectniveau van een een toevoegen, bijwerken of verwijderen is.
* AMODT – wijziging kenmerktype. Geeft aan of de bewerking op het kenmerkniveau van een een toevoegen, bijwerken of verwijderen is.

**Algemene id's ophalen** het export.csv-bestand bevat alle wijzigingen die moeten worden geëxporteerd. Elke rij overeenkomt met een wijziging voor een object in het connectorgebied en het object wordt geïdentificeerd door de DN-kenmerk. Het kenmerk DN-naam is een unieke id die is toegewezen aan een object in het connectorgebied. Wanneer u over veel rijen en/of wijzigingen in de export.csv te analyseren, kan het lastig zijn om te achterhalen welke dat de wijzigingen zijn objecten voor op basis van de DN-naam kenmerk alleen. Gebruik ter vereenvoudiging van het proces voor het analyseren van de wijzigingen de csanalyzer.ps1 PowerShell-script. Het script worden algemene id's (bijvoorbeeld, displayName, userPrincipalName) van de objecten opgehaald. Het script gebruiken:
1. Kopieer het PowerShell-script uit de sectie [CSAnalyzer](#appendix-csanalyzer) naar een bestand met de naam `csanalyzer.ps1`.
2. Open een PowerShell-venster en blader naar de map waarin u het PowerShell-script hebt gemaakt.
3. Uitvoeren: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.
4. U hebt nu een bestand met de naam **processedusers1.csv** die kunnen worden onderzocht in Microsoft Excel. Houd er rekening mee dat het bestand een toewijzing van het kenmerk DN-naam naar algemene id's (bijvoorbeeld displayName en userPrincipalName bevat). Deze omvatten momenteel niet de werkelijke kenmerkwijzigingen die moeten worden geëxporteerd.

#### <a name="switch-active-server"></a>Active switchserver
1. Op de momenteel actieve server uitschakelen (FIM-DirSync/Azure AD Sync) van de server, zodat deze niet worden geëxporteerd naar Azure AD of stel deze in de faseringsmodus bevindt (Azure AD Connect).
2. Voer de installatiewizard op de server in **faseringsmodus** en uit te schakelen **faseringsmodus**.
   ![ReadyToConfigure](./media/how-to-connect-sync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Herstel na noodgeval
Onderdeel van het implementatieontwerp van de is het plannen voor wat u moet doen als er zich een noodgeval voordoet waarbij u de synchronisatieserver verliest. Er zijn verschillende modellen moet worden gebruikt en welke computer te gebruiken is afhankelijk van diverse factoren, waaronder:

* Wat is de tolerantie voor worden geen wijzigingen kunnen aanbrengen op objecten in Azure AD tijdens de downtime?
* Als u synchronisatie van wachtwoord, de gebruikers accepteert dat ze het oude wachtwoord in Azure AD gebruiken moeten als ze deze on-premises wijzigen?
* Hebt u een afhankelijkheid op realtime bewerkingen, zoals het terugschrijven van wachtwoorden?

Afhankelijk van de antwoorden op deze vragen en beleid van uw organisatie, kan een van de volgende strategieën worden geïmplementeerd:

* Opnieuw opbouwen wanneer dat nodig is.
* Een spare stand-by-server, ook wel **faseringsmodus**.
* Gebruik virtuele machines.

Als u de ingebouwde SQL Express-database niet gebruikt, wordt Lees ook de [hoge beschikbaarheid van SQL](#sql-high-availability) sectie.

### <a name="rebuild-when-needed"></a>Opnieuw opbouwen wanneer dat nodig is
Een levensvatbare strategie bestaat uit het plannen van een server opnieuw opbouwen wanneer dat nodig is. Normaal gesproken installeert het synchronisatie-engine en voert u die de eerste import en synchronisatie kunnen worden uitgevoerd binnen een paar uur. Als er een vervangende server beschikbaar is, is het mogelijk om te gebruiken tijdelijk een domeincontroller voor het hosten van de synchronisatie-engine.

De synchronisatie-engine-server worden niet opgeslagen voor elke status over de objecten, zodat de database kan opnieuw worden gemaakt van de gegevens in Active Directory en Azure AD. De **sourceAnchor** kenmerk wordt gebruikt om lid van de objecten uit de on-premises en de cloud. Als u de server met bestaande objecten on-premises en in de cloud opnieuw maken, klikt u vervolgens de synchronisatie-engine komt overeen met die objecten samen nogmaals op opnieuw. De zaken die u wilt vastleggen en opslaan zijn wijzigingen aangebracht in de server, zoals filteren en synchronisatieregels in de configuratie. Deze aangepaste configuraties moeten opnieuw worden toegepast voordat u begint met het synchroniseren.

### <a name="have-a-spare-standby-server---staging-mode"></a>Een vervangende stand-by-server - faseringsmodus
Als u een complexere omgeving hebt, wordt klikt u vervolgens met een of meer stand-by-servers aanbevolen. Tijdens de installatie, kunt u een server in **faseringsmodus**.

Zie voor meer informatie, [faseringsmodus](#staging-mode).

### <a name="use-virtual-machines"></a>Gebruik virtuele machines
Een veelvoorkomende en ondersteunde methode is om uit te voeren van de synchronisatie-engine in een virtuele machine. Als de host een probleem heeft, kan de installatiekopie met de synchronisatie-engine-server kan worden gemigreerd naar een andere server.

### <a name="sql-high-availability"></a>Hoge beschikbaarheid van SQL
Als u de SQL Server Express die wordt geleverd met Azure AD Connect niet gebruikt, moet klikt u vervolgens hoge beschikbaarheid voor SQL Server ook worden overwogen. De hoge beschikbaarheidsoplossingen die ondersteund zijn SQL-clustering en AOA (Always On Availability Groups). Niet-ondersteunde oplossingen omvatten spiegelen.

Ondersteuning voor SQL AOA is toegevoegd aan Azure AD Connect versie 1.1.524.0 en hoger. Voordat u Azure AD Connect installeert, moet u SQL AOA inschakelen. Tijdens de installatie van detecteert Azure AD Connect of de opgegeven SQL-exemplaar voor SQL AOA is ingeschakeld of niet. Als SQL AOA is ingeschakeld, wordt Azure AD Connect verder zoekt uit als SQL AOA is geconfigureerd voor gebruik van de replicatie van synchrone of asynchrone replicatie. Bij het instellen van de beschikbaarheidsgroep-Listener, is het raadzaam dat u de eigenschap RegisterAllProvidersIP ingesteld op 0. Dit is omdat Azure AD Connect momenteel SQL Native Client gebruikt verbinding maken met SQL en SQL Native Client biedt geen ondersteuning voor het gebruik van de eigenschap MultiSubNetFailover.

## <a name="appendix-csanalyzer"></a>Bijlage CSAnalyzer
Zie de sectie [controleren](#verify) over het gebruik van dit script.

```
Param(
    [Parameter(Mandatory=$true, HelpMessage="Must be a file generated using csexport 'Name of Connector' export.xml /f:x)")]
    [string]$xmltoimport="%temp%\exportedStage1a.xml",
    [Parameter(Mandatory=$false, HelpMessage="Maximum number of users per output file")][int]$batchsize=1000,
    [Parameter(Mandatory=$false, HelpMessage="Show console output")][bool]$showOutput=$false
)

#LINQ isn't loaded automatically, so force it
[Reflection.Assembly]::Load("System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089") | Out-Null

[int]$count=1
[int]$outputfilecount=1
[array]$objOutputUsers=@()

#XML must be generated using "csexport "Name of Connector" export.xml /f:x"
write-host "Importing XML" -ForegroundColor Yellow

#XmlReader.Create won't properly resolve the file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader to deal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create the object placeholder
    #adding them up here means we can enforce consistency
    $objOutputUser=New-Object psobject
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name ID -Value ""
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name Type -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name DN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name operation -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name UPN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name displayName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name sourceAnchor -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name alias -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name primarySMTP -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name onPremisesSamAccountName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name mail -Value ""

    $user = [System.Xml.Linq.XElement]::ReadFrom($reader)
    if ($showOutput) {Write-Host Found an exported object... -ForegroundColor Green}

    #object id
    $outID=$user.Attribute('id').Value
    if ($showOutput) {Write-Host ID: $outID}
    $objOutputUser.ID=$outID

    #object type
    $outType=$user.Attribute('object-type').Value
    if ($showOutput) {Write-Host Type: $outType}
    $objOutputUser.Type=$outType

    #dn
    $outDN= $user.Element('unapplied-export').Element('delta').Attribute('dn').Value
    if ($showOutput) {Write-Host DN: $outDN}
    $objOutputUser.DN=$outDN

    #operation
    $outOperation= $user.Element('unapplied-export').Element('delta').Attribute('operation').Value
    if ($showOutput) {Write-Host Operation: $outOperation}
    $objOutputUser.operation=$outOperation

    #now that we have the basics, go get the details

    foreach ($attr in $user.Element('unapplied-export-hologram').Element('entry').Elements("attr"))
    {
        $attrvalue=$attr.Attribute('name').Value
        $internalvalue= $attr.Element('value').Value

        switch ($attrvalue)
        {
            "userPrincipalName"
            {
                if ($showOutput) {Write-Host UPN: $internalvalue}
                $objOutputUser.UPN=$internalvalue
            }
            "displayName"
            {
                if ($showOutput) {Write-Host displayName: $internalvalue}
                $objOutputUser.displayName=$internalvalue
            }
            "sourceAnchor"
            {
                if ($showOutput) {Write-Host sourceAnchor: $internalvalue}
                $objOutputUser.sourceAnchor=$internalvalue
            }
            "alias"
            {
                if ($showOutput) {Write-Host alias: $internalvalue}
                $objOutputUser.alias=$internalvalue
            }
            "proxyAddresses"
            {
                if ($showOutput) {Write-Host primarySMTP: ($internalvalue -replace "SMTP:","")}
                $objOutputUser.primarySMTP=$internalvalue -replace "SMTP:",""
            }
        }
    }

    $objOutputUsers += $objOutputUser

    Write-Progress -activity "Processing ${xmltoimport} in batches of ${batchsize}" -status "Batch ${outputfilecount}: " -percentComplete (($objOutputUsers.Count / $batchsize) * 100)

    #every so often, dump the processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit the maximum users processed without completion... -ForegroundColor Yellow

        #export the collection of users as a CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment the output file counter
        $outputfilecount+=1

        #reset the collection and the user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need to bail out of the loop if no more users to process
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need to write out any users that didn't get picked up in a batch of 1000
#export the collection of users as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**  

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)  
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)  
