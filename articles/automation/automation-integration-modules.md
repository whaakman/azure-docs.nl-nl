---
title: Een Azure Automation-integratiemodule maken
description: Zelfstudie die u helpt bij het maken, testen en bij het voorbeeldgebruik van integratiemodules in Azure Automation.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 579be1f240f5661503caea16f7a7e42ee31a2fdd
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55495595"
---
# <a name="azure-automation-integration-modules"></a>Azure Automation-integratiemodules
PowerShell is de basistechnologie achter Azure Automation. Omdat Azure Automation is gebouwd op PowerShell, zijn de PowerShell-modules bepalend voor de uitbreidbaarheid van Azure Automation. In dit artikel helpen we u de details van het gebruik van Azure Automation van PowerShell-modules, aangeduid als 'Integratiemodules' en aanbevolen procedures voor het maken van uw eigen PowerShell-modules om ervoor te zorgen dat ze werken als integratiemodules binnen Azure Automation. 

## <a name="what-is-a-powershell-module"></a>Wat is een PowerShell-module?
Een PowerShell-module is een groep PowerShell-cmdlets, zoals **Get-Date** of **Copy-Item**, die kunnen worden gebruikt vanuit de PowerShell-console, scripts, werkstromen, runbooks en PowerShell DSC-resources, zoals WindowsFeature of File, die kunnen worden gebruikt vanuit PowerShell DSC-configuraties. Alle functionaliteit van PowerShell is zichtbaar via cmdlets en DSC-resources, en elke cmdlet/DSC-resource wordt ondersteund door een PowerShell-module. Veel van deze modules worden bij PowerShell zelf geleverd. De cmdlet **Get-Date** maakt bijvoorbeeld deel uit van de PowerShell-module Microsoft.PowerShell.Utility en de cmdlet **Copy-Item** maakt deel uit van de PowerShell-module Microsoft.PowerShell.Management en de Package DSC-resource maakt deel uit van de PowerShell-module PSDesiredStateConfiguration. Deze modules worden geleverd met PowerShell. Maar veel PowerShell-modules worden niet als onderdeel van PowerShell geleverd en worden in plaats daarvan gedistribueerd met producten van Microsoft of van derden zoals System Center 2012 Configuration Manager of door de enorme PowerShell-community op locaties zoals PowerShell Gallery. De modules zijn nuttig omdat ze complexe taken eenvoudiger maken via ingekapselde functionaliteit.  Meer informatie over [PowerShell-modules vindt u op MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx). 

## <a name="what-is-an-azure-automation-integration-module"></a>Wat is een Azure Automation-integratiemodule?
Er is een integratiemodule niet anders dan een PowerShell-module. Het is gewoon een PowerShell-module die desgewenst één extra bestand bevat: een metagegevensbestand waarin een Azure Automation-verbindingstype wordt opgegeven dat moet worden gebruikt met de cmdlets van de module in runbooks. Optioneel bestand of niet, deze PowerShell-modules kunnen in Azure Automation worden geïmporteerd om de bijbehorende cmdlets beschikbaar te maken voor gebruik in runbooks en de bijbehorende DSC-resources beschikbaar te maken voor gebruik in DSC-configuraties. Achter de schermen worden deze modules in Azure Automation opgeslagen, en bij de uitvoeringstijd van runbooktaken en DSC-compilatietaken worden ze in de Azure Automation-sandboxes geladen waar runbooks worden uitgevoerd en DSC-configuraties worden gecompileerd. Eventuele DSC-resources in modules worden ook automatisch op de Automation DSC-pull-server geplaatst, zodat ze kunnen worden opgehaald door machines waarmee wordt geprobeerd DSC-configuraties toe te passen.  

Een aantal Azure PowerShell-modules worden gebruiksklaar in Azure Automation geleverd zodat u meteen aan de slag kunt gaan met het automatiseren van Azure-beheer, maar u kunt PowerShell-modules importeren voor elk systeem, elke service of elk hulpprogramma waarmee u wilt integreren. 

> [!NOTE]
> Bepaalde modules worden geleverd als 'algemene modules' in de Automation-service. Deze algemene modules zijn beschikbaar wanneer u een automation-account maakt, en wij ze soms werken, waarbij de automatisch nieuwe versies van uw automation-account. Als u niet dat ze automatisch worden bijgewerkt wilt, kunt u altijd importeren dezelfde module zelf en die heeft voorrang op de algemene module-versie van die module die we bij de service meeleveren. 

De indeling waarin u een integratiemodulepakket importeert, is een gecomprimeerd bestand met dezelfde naam als de module en met een ZIP-extensie. Het bevat de Windows PowerShell-module en ondersteunende bestanden, inclusief een manifestbestand (.psd1) als de module die een heeft.

Als de module een Azure Automation-verbindingstype moet bevatten, moet deze ook een bestand bevatten met de naam `<ModuleName>-Automation.json`, waarin de eigenschappen van het verbindingstype zijn opgegeven. Dit is een json-bestand geplaatst in de modulemap van uw gecomprimeerde ZIP-bestand en bevat de velden van een 'verbinding' die is vereist voor het verbinding maken met het systeem of de service de module wordt aangegeven. Dit het geval is een verbindingstype gemaakt in Azure Automation. Met dit veld kunt u de veldnamen en typen instellen en opgeven of de velden moeten worden versleuteld en/of optioneel moeten zijn, voor het verbindingstype van de module. Hier volgt een sjabloon in de JSON-bestandsindeling:

```json
{ 
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  false,
      "Name":  "ComputerName",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
   "TypeName":  "System.String"
   }],
   "ConnectionTypeName":  "DataProtectionManager",
   "IntegrationModuleName":  "DataProtectionManager"
}
```

Als u Service Management Automation hebt geïmplementeerd en integratiemodulepakketten voor uw automation-runbooks hebt gemaakt, dit er waarschijnlijk vertrouwd voor u ziet. 

## <a name="authoring-best-practices"></a>Aanbevolen procedures voor ontwerpen
Hoewel integratiemodules in wezen PowerShell-modules zijn, is er nog steeds een aantal zaken waar dat u moet denken bij het ontwerpen van een PowerShell-module, zodat u deze zo bruikbaar mogelijk in Azure Automation. Sommige hiervan zijn specifiek voor Azure Automation en sommige zijn nuttig om dit te uw modules ze goed werken in PowerShell Workflow, ongeacht of u Automation gebruikt. 

1. Neem een samenvatting, beschrijving en Help-URI op voor elke cmdlet in de module. In PowerShell kunt u bepaalde Help-informatie voor cmdlets definiëren, zodat de gebruiker hulp ontvangt bij het gebruik van cmdlets met de cmdlet **Get-Help**. Bijvoorbeeld, als volgt het definiëren van een samenvatting en help-URI voor een PowerShell-module die is geschreven in een psm1-bestand.<br>  
   
    ```powershell
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='https://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
    param(
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AccountSid,
   
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AuthToken,
   
       [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [Hashtable]
       $Connection
    )
   
    $cred = CreateTwilioCredential -Connection $Connection -AccountSid $AccountSid -AuthToken $AuthToken
   
    $uri = "$TWILIO_BASE_URL/Accounts/" + $cred.UserName + "/IncomingPhoneNumbers"
   
    $response = Invoke-RestMethod -Method Get -Uri $uri -Credential $cred
   
    $response.TwilioResponse.IncomingPhoneNumbers.IncomingPhoneNumber
    }
    ```
   <br> Als u deze informatie opgeeft, wordt deze Help-informatie niet alleen getoond met de cmdlet **Get-Help** in de PowerShell-console, maar wordt deze Help-functionaliteit ook weergegeven in Azure Automation.  Bijvoorbeeld wanneer u activiteiten invoegt tijdens het ontwerpen van een runbook. Te klikken op 'Gedetailleerde help weergeven', wordt de help-URI geopend in een ander tabblad van de webbrowser die u gebruikt voor toegang tot Azure Automation.<br>![Help bij integratiemodules](media/automation-integration-modules/automation-integration-module-activitydesc.png)
1. Als de module op een extern systeem wordt uitgevoerd, geldt het volgende:

    a. Deze moet een metagegevensbestand voor de integratiemodule bevatten waarin de informatie wordt gedefinieerd die nodig is om verbinding te maken met dat externe systeem, dus het verbindingstype.  
    b. Elke cmdlet in de module moet een verbindingsobject (een exemplaar van dat verbindingstype) kunnen bevatten als een parameter.  

    Cmdlets in de module worden eenvoudiger te gebruiken in Azure Automation als u toestaat dat een object met de velden van het verbindingstype als een parameter aan de cmdlet wordt doorgegeven. Deze manier waarop gebruikers hoeft te parameters van de verbindingsasset toewijzen aan de bijbehorende parameters van de cmdlet telkens wanneer die ze een cmdlet aanroepen. Op basis van het bovenstaande runbookvoorbeeld wordt een Twilio-verbindingsasset genaamd CorpTwilio gebruikt voor toegang tot Twilio en voor het retourneren van alle telefoonnummers in het account.  Ziet u hoe hiermee de velden van de verbinding worden toegewezen aan de parameters van de cmdlet?<br>
   
    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Een eenvoudigere en betere manier om dit te doen is het rechtstreeks doorgeven van het verbindingsobject aan de cmdlet -
   
    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    U kunt dergelijk gedrag mogelijk maken voor uw cmdlets door deze toe te staan een verbindingsobject rechtstreeks als parameter te accepteren, in plaats van alleen verbindingsvelden voor parameters. Doorgaans wilt u een parameter die is ingesteld voor elk, zodat een gebruiker die niet met behulp van Azure Automation uw cmdlets aanroepen kan zonder een hash-tabel om te fungeren als het verbindingsobject maken. Parameterset **SpecifyConnectionFields** hieronder wordt gebruikt om de eigenschappen van het verbindingsveld één voor één door te geven. Met **UseConnectionObject** kunt u de verbinding rechtstreeks doorgeven. Zoals u kunt zien is voor de cmdlet Send-TwilioSMS in de [Twilio PowerShell-module](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) de doorgave op beide manieren mogelijk: 
   
    ```powershell
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='https://www.twilio.com/docs/api/rest/sending-sms')]
      param(
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AccountSid,
   
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AuthToken,
   
         [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [Hashtable]
         $Connection
   
       )
    }
    ```
   <br>
1. Definieer het uitvoertype voor alle cmdlets in de module. Als u een uitvoertype voor een cmdlet definieert, kan IntelliSense u tijdens het ontwerpen helpen bij het bepalen van de uitvoereigenschappen van de cmdlet, voor gebruik tijdens het ontwerpen. Het is vooral nuttig tijdens het grafisch ontwerpen van een Automation-runbook, waarbij ontwerptijdkennis essentieel is voor een goede gebruikerservaring met uw module.<br><br> ![Uitvoertype grafisch runbook](media/automation-integration-modules/runbook-graphical-module-output-type.png)<br> Dit is vergelijkbaar met de functionaliteit 'automatisch aanvullen' van de uitvoer van een cmdlet in PowerShell ISE zonder deze te hoeven uitvoeren.<br><br> ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)<br>
1. Cmdlets in de module mogen geen complexe objecttypen voor parameters hebben. PowerShell Workflow verschilt van PowerShell in het volgende opzicht: complexe typen worden in gedeserialiseerde vorm opgeslagen. Primitieve typen blijven primitieven, maar complexe typen worden geconverteerd naar hun gedeserialiseerde versies, die in wezen Eigenschappenverzamelingen zijn. Als u bijvoorbeeld de cmdlet **Get-Process** hebt gebruikt in een runbook (of een PowerShell Workflow), wordt een object van het type [Deserialized.System.Diagnostic.Process] geretourneerd, niet van het verwachte type [System.Diagnostic.Process]. Dit type heeft dezelfde eigenschappen als het niet-gedeserialiseerde type, maar geen van de methoden van dat type. En als u deze wordt doorgegeven als parameter aan een cmdlet probeert, waarbij de cmdlet wordt een waarde [System.Diagnostic.Process] voor deze parameter wordt verwacht, ontvangt u de volgende fout: *Argumenttransformatie voor parameter 'process' kan niet worden verwerkt. Fout: "De waarde"System.Diagnostics.Process (CcmExec)"van het type"Deserialized.System.Diagnostics.Process"naar type"System.Diagnostics.Process"kan niet worden geconverteerd.*   Dit komt omdat de typen niet overeenkomen van het verwachte type [System.Diagnostic.Process] en het opgegeven type [Deserialized.System.Diagnostic.Process]. De manier om dit probleem op te lossen is ervoor te zorgen dat de cmdlets van uw module geen complexe typen voor parameters hebben. Dit is de verkeerde manier.
   
    ```powershell
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    En dit is de juiste manier, een primitieve nemen die intern kan worden gebruikt door de cmdlet om het complexe object op te halen en te gebruiken. Aangezien de cmdlets worden uitgevoerd in de context van PowerShell, en niet PowerShell Workflow, wordt het correcte type in de cmdlet $process [System.Diagnostic.Process].  
   
    ```powershell
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```
   <br>
   Verbindingsassets in runbooks zijn hashtabellen, die een complex type zijn, en toch lijken deze hashtabellen te kunnen worden doorgegeven in cmdlets voor hun - Connection-parameter, met zonder gecaste uitzondering. In technisch opzicht kunnen sommige PowerShell-typen goed worden gecast van hun geserialiseerde vorm naar hun gedeserialiseerde vorm en kunnen deze daarom worden doorgegeven in cmdlets voor parameters die het niet-gedeserialiseerde type accepteren. Hashtabel is daar een van. Het is mogelijk om gedefinieerde typen van een module-auteur te implementeren op een manier die deze ook correct kan deserialiseren, maar er zijn enkele compromissen nodig om te overwegen. Het type moet een standaardconstructor hebben, alle eigenschappen van het type moeten openbaar zijn en het type moet een PSTypeConverter hebben. Voor al gedefinieerde typen waarvan de module-auteur geen eigenaar, er is echter geen manier om te 'op te lossen', vandaar de aanbeveling om complexe typen voor parameters helemaal te voorkomen. Ontwerpen van een Runbook tip: Als voor een of andere reden die de cmdlets wilt nemen van een complex type parameter, of u iemand anders de module die is vereist van een complex typeparameter, de tijdelijke oplossing in PowerShell Workflow-runbooks en PowerShell Workflows in lokale PowerShell gebruikt, is het verpakken van de cmdlet die genereert het complexe type en de cmdlet waarmee het complexe type in dezelfde InlineScript-activiteit worden verbruikt. Omdat met InlineScript de inhoud wordt uitgevoerd als PowerShell in plaats van PowerShell Workflow, zou met de cmdlet waarmee het complexe type wordt gegenereerd, dat correcte type worden gemaakt, niet het gedeserialiseerde complexe type.
1. Maak alle cmdlets in de module staatloos. In PowerShell Workflow wordt elke cmdlet die in de werkstroom wordt aangeroepen in een andere sessie uitgevoerd. Dit betekent dat cmdlets die afhankelijk zijn van de sessiestatus gemaakt/gewijzigd door andere cmdlets in dezelfde module niet zullen werken in PowerShell Workflow-runbooks.  Hier ziet u een voorbeeld van wat u niet moet doen.
   
    ```powershell
    $globalNum = 0
    function Set-GlobalNum {
       param(
           [int] $num
       )
   
       $globalNum = $num
    }
    function Get-GlobalNumTimesTwo {
       $output = $globalNum * 2
   
       $output
    }
    ```
   <br>
1. De module moet volledig zijn opgenomen in een pakket waarvoor Xcopy kan worden gebruikt. Omdat Azure Automation-modules worden gedistribueerd naar de Automation-sandboxes wanneer runbooks moeten worden uitgevoerd, moeten ze onafhankelijk van de host werken waarop ze worden uitgevoerd. Dit betekent dat u kunnen zippen moet van het pakket module verplaatsen naar een andere host met de dezelfde of een nieuwere PowerShell-versie, en deze functioneren wanneer geïmporteerd in de PowerShell-omgeving van die host. Hiervoor mag de module niet afhankelijk zijn van bestanden buiten de modulemap (de map die wordt gezipt wanneer u in Azure Automation importeert), of van unieke registerinstellingen voor een host, zoals die die zijn ingesteld bij de installatie van een product. Als deze aanbevolen procedure niet wordt gevolgd, is de module niet bruikbaar in Azure Automation.  

## <a name="next-steps"></a>Volgende stappen

* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie [Een Windows PowerShell-module schrijven](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx) voor meer informatie over het maken van PowerShell-modules.


