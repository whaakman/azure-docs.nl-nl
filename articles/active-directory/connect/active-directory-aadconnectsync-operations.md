---
title: 'Azure AD Connect-synchronisatie: operationele taken en overwegingen | Microsoft Docs'
description: Dit onderwerp beschrijft operationele taken voor Azure AD Connect-synchronisatie en voorbereiden voor de werking van dit onderdeel.
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: b7583a1556bb1113f349a78890768451e39c6878
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect-synchronisatie: operationele taken en afweging
Het doel van dit onderwerp is om operationele taken voor Azure AD Connect-synchronisatie te beschrijven.

## <a name="staging-mode"></a>Faseringsmodus
De faseringsmodus kan worden gebruikt voor verschillende scenario's, waaronder:

* Hoge beschikbaarheid.
* Testen en implementeren van nieuwe wijzigingen in de configuratie.
* Introduceert een nieuwe server en de oude uit bedrijf nemen.

U kunt wijzigingen aanbrengen in de configuratie en de wijzigingen bekijken voordat u de server actief maken met een server in de faseringsmodus. Ook kunt u volledige import en een volledige synchronisatie om te verifiëren dat alle wijzigingen worden verwacht, voordat u deze wijzigingen in uw productieomgeving aanbrengt uitvoeren.

Tijdens de installatie, kunt u de server zich in **faseringsmodus**. Hiermee wordt de server actief is voor de import en synchronisatie, maar eventuele uitvoer kan niet worden uitgevoerd. Een server in de faseringsmodus is Wachtwoordsynchronisatie of wachtwoord terugschrijven niet actief, zelfs als u deze functies ingeschakeld tijdens de installatie. Wanneer u de faseringsmodus uitschakelt, de server is, wordt het exporteren wordt gestart, schakelt Wachtwoordsynchronisatie en wachtwoord terugschrijven maakt.

U kunt nog steeds exporteren van een afdwingen met behulp van synchronization servicemanager.

Een server in de faseringsmodus blijft ontvangen van wijzigingen van Active Directory en Azure AD. Heeft altijd een kopie van de meest recente wijzigingen en kunnen zeer snel uitvoeren via de verantwoordelijkheden van een andere server. Als u configuratiewijzigingen in de primaire server aanbrengt, is uw verantwoordelijkheid om dezelfde wijzigingen aanbrengen op de server in de faseringsmodus.

De faseringsmodus is voor mensen met kennis van de oudere synchronisatie-technologieën, anders omdat de server een eigen SQL-database heeft. Deze architectuur kan de modus server met tijdelijke bestanden zich bevinden in een ander datacenter.

### <a name="verify-the-configuration-of-a-server"></a>Controleer de configuratie van een server
Als u wilt toepassen op deze methode, de volgende stappen uit:

1. [Voorbereiden](#prepare)
2. [Configuratie](#configuration)
3. [Importeren en synchroniseren](#import-and-synchronize)
4. [Controleer of](#verify)
5. [Actieve switchserver](#switch-active-server)

#### <a name="prepare"></a>Voorbereiden
1. Azure AD Connect installeert, selecteert u **faseringsmodus**, en deselecteren **synchronisatie starten** op de laatste pagina van de installatiewizard. Deze modus kunt u de synchronisatie-engine handmatig uitvoeren.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Meld u af/aanmelding in en selecteer menu start in **synchronisatieservice**.

#### <a name="configuration"></a>Configuratie
Als u aangepaste wijzigingen hebt aangebracht in de primaire server en u wilt vergelijken van de configuratie met de staging-server, gebruikt u [documentatie voor Azure AD Connect-configuratie](https://github.com/Microsoft/AADConnectConfigDocumenter).

#### <a name="import-and-synchronize"></a>Importeren en synchroniseren
1. Selecteer **Connectors**, en selecteer de eerste Connector met het type **Active Directory Domain Services**. Klik op **uitvoeren**, selecteer **volledige import**, en **OK**. Voer deze stappen uit voor alle Connectors van dit type.
2. Selecteer de Connector met het type **Azure Active Directory (Microsoft)**. Klik op **uitvoeren**, selecteer **volledige import**, en **OK**.
3. Zorg ervoor dat het tabblad Connectors nog steeds is geselecteerd. Voor elke Connector met het type **Active Directory Domain Services**, klikt u op **uitvoeren**, selecteer **Deltasynchronisatie**, en **OK**.
4. Selecteer de Connector met het type **Azure Active Directory (Microsoft)**. Klik op **uitvoeren**, selecteer **Deltasynchronisatie**, en **OK**.

U hebt nu gefaseerde export wijzigingen naar Azure AD en on-premises AD (als u hybride implementatie voor Exchange). De volgende stappen kunnen u controleren wat bijna wordt gewijzigd voordat u de uitvoer naar de mappen daadwerkelijk wordt gestart.

#### <a name="verify"></a>Verifiëren
1. Start een opdrachtprompt en Ga naar`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Voer: `csexport "Name of Connector" %temp%\export.xml /f:x` de naam van de Connector vindt u in de synchronisatieservice. Een naam die vergelijkbaar is met 'contoso.com – AAD' voor Azure AD.
3. Kopieer het PowerShell-script uit de sectie [CSAnalyzer](#appendix-csanalyzer) naar een bestand met de naam `csanalyzer.ps1`.
4. Open een PowerShell-venster en blader naar de map waar u het PowerShell-script hebt gemaakt.
5. Voer: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.
6. U hebt nu een bestand met de naam **processedusers1.csv** die kan worden onderzocht in Microsoft Excel. Alle wijzigingen die zijn voorbereid om te worden geëxporteerd naar Azure AD zijn gevonden in dit bestand.
7. Noodzakelijke wijzigingen aanbrengen in de gegevens of configuratie en voer deze stappen opnieuw (importeren en synchroniseren en controleer of) totdat de wijzigingen die zijn geëxporteerd worden verwacht.

#### <a name="switch-active-server"></a>Actieve switchserver
1. Op de momenteel actieve server uitschakelen (FIM-DirSync/Azure AD Sync) van de server zodat deze niet worden geëxporteerd naar Azure AD of stel deze in de faseringsmodus (Azure AD Connect).
2. De installatiewizard uitvoeren op de server in **faseringsmodus** en uitschakelen **faseringsmodus**.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Herstel na noodgevallen
Onderdeel van het implementatieontwerp is het plannen voor wat te doen als er een ramp optreedt waarbij u de synchronisatieserver verliezen. Er zijn verschillende modellen moet gebruiken en welke een om te gebruiken is afhankelijk van verschillende factoren, waaronder:

* Wat is uw tolerantie voor niet dat deze wijzigingen kunnen aanbrengen op objecten in Azure AD tijdens de uitvaltijd?
* Als u synchronisatie van wachtwoorden, de gebruikers accepteert dat ze hebben het oude wachtwoord in Azure AD gebruiken als ze deze lokale wijzigen?
* Hebt u een afhankelijkheid op realtime bewerkingen, zoals wachtwoord terugschrijven?

Afhankelijk van de antwoorden op deze vragen en beleid van uw organisatie, kan een van de volgende strategieën worden geïmplementeerd:

* Opnieuw opbouwen wanneer deze nodig is.
* Een ongebruikte stand-by-server, ook wel **faseringsmodus**.
* Gebruik virtuele machines.

Als u niet de ingebouwde SQL Express-database gebruikt, wordt ook rekening met de [SQL maximale beschikbaarheid](#sql-high-availability) sectie.

### <a name="rebuild-when-needed"></a>Opnieuw opbouwen wanneer deze nodig is
Een strategie voor een praktische is voor het plannen van de server opnieuw opbouwen wanneer deze nodig is. Normaal gesproken installeert het synchronisatie-engine en voert u die de initiële importeren en de synchronisatie kunnen worden uitgevoerd binnen een paar uur. Als er niet een vervangende server beschikbaar is, is het mogelijk om tijdelijk met een domeincontroller voor het hosten van de synchronisatie-engine.

De synchronisatie-engine-server worden niet opgeslagen voor elke status over de objecten zodat de database kan worden gemaakt van de gegevens in Active Directory en Azure AD. De **sourceAnchor** kenmerk wordt gebruikt om lid van de objecten uit de on-premises en de cloud. Als u de server met bestaande objecten on-premises en de cloud opnieuw maken, klikt u vervolgens de synchronisatie-engine komt overeen met die objecten samen opnieuw op opnieuw installeren. De zaken die u wilt vastleggen en opslaan zijn de configuratiewijzigingen van de server, zoals filteren en synchronisatieregels. Deze aangepaste configuraties moeten opnieuw worden toegepast voordat u begint te synchroniseren.

### <a name="have-a-spare-standby-server---staging-mode"></a>Een ongebruikte stand-by-server - faseringsmodus
Als u een meer complexe omgeving hebt, wordt klikt u vervolgens met een of meer stand-by-servers aanbevolen. Tijdens de installatie, kunt u een server in **faseringsmodus**.

Zie voor meer informatie [faseringsmodus](#staging-mode).

### <a name="use-virtual-machines"></a>Gebruik virtuele machines
Een veelvoorkomende en ondersteunde methode is het uitvoeren van de synchronisatie-engine in een virtuele machine. Als de host een probleem heeft, kan de installatiekopie met de synchronisatie-engine-server kan worden gemigreerd naar een andere server.

### <a name="sql-high-availability"></a>Hoge beschikbaarheid van SQL
Als u niet de SQL Server Express die wordt geleverd met Azure AD Connect, moet vervolgens hoge beschikbaarheid voor SQL Server ook worden overwogen. De hoge beschikbaarheid-oplossingen die ondersteund zijn SQL-clustering en AOA (AlwaysOn-beschikbaarheidsgroepen). Niet-ondersteunde oplossingen bevatten mirroring.

Ondersteuning voor SQL AOA is toegevoegd aan Azure AD Connect in versie 1.1.524.0. Voordat u Azure AD Connect installeert, moet u SQL AOA inschakelen. Tijdens de installatie van detecteert Azure AD Connect of de opgegeven SQL-exemplaar is ingeschakeld voor SQL AOA. Als SQL AOA is ingeschakeld, wordt Azure AD Connect meer cijfers uit als SQL AOA is geconfigureerd voor gebruik van replicatie van synchrone of asynchrone replicatie. Bij het instellen van de beschikbaarheidsgroep-Listener, is het raadzaam dat u de eigenschap RegisterAllProvidersIP ingesteld op 0. Dit is omdat Azure AD Connect momenteel SQL Native Client verbinding maakt met SQL en SQL Native Client biedt geen ondersteuning voor het gebruik van de MultiSubNetFailover-eigenschap.

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
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
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

        #export the collection of users as as CSV
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
#export the collection of users as as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**  

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)  
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)  
