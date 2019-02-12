---
title: Een Azure Automation-integratiemodule maken
description: Zelfstudie die u helpt bij het maken, testen en bij het voorbeeldgebruik van integratiemodules in Azure Automation.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 01/24/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9122cf5cc908d578d8b781c6fdc49d7b04b0ab58
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990339"
---
# <a name="azure-automation-integration-modules"></a>Azure Automation-integratiemodules

PowerShell is de basistechnologie achter Azure Automation. Omdat Azure Automation is gebouwd op PowerShell, zijn de PowerShell-modules bepalend voor de uitbreidbaarheid van Azure Automation. In dit artikel leert u de details van het gebruik van Azure Automation van PowerShell-modules, aangeduid als integratiemodules. Ook leert u aanbevolen procedures voor het maken van uw eigen PowerShell-modules om ervoor te zorgen dat ze werken als integratiemodules binnen Azure Automation. 

## <a name="what-is-a-powershell-module"></a>Wat is een PowerShell-module?

Een PowerShell-module is een groep PowerShell-cmdlets, zoals **Get-Date** of **Copy-Item**, die kan worden gebruikt vanuit:

* De PowerShell-console
* scripts
* Werkstromen
* runbooks
* DSC-resources

Alle functionaliteit van PowerShell is zichtbaar via cmdlets en DSC-resources. Elke cmdlet en DSC-resource wordt ondersteund door een PowerShell-module, veel van die worden geleverd met PowerShell zelf. Bijvoorbeeld, de **Get-Date** cmdlet maakt deel uit van de `Microsoft.PowerShell.Utility` PowerShell-module en **Copy-Item** cmdlet maakt deel uit van de `Microsoft.PowerShell.Management` PowerShell-module en de Package DSC-resource maakt deel uit van de PowerShell-module PSDesiredStateConfiguration. Deze modules worden geleverd met PowerShell. Veel PowerShell-modules niet geleverd als onderdeel van PowerShell. Deze modules worden gedistribueerd met producten van eerste of van derden of op locaties zoals PowerShell Gallery. De modules zijn nuttig omdat ze complexe taken eenvoudiger maken via ingekapselde functionaliteit.  Meer informatie over [PowerShell-modules vindt u op MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx). 

## <a name="what-is-an-azure-automation-integration-module"></a>Wat is een Azure Automation-integratiemodule?

Er is een integratiemodule niet anders dan een PowerShell-module. Het is gewoon een PowerShell-module die desgewenst één extra bestand bevat: een metagegevensbestand waarin een Azure Automation-verbindingstype wordt opgegeven dat moet worden gebruikt met de cmdlets van de module in runbooks. PowerShell-modules kunnen worden geïmporteerd in Azure Automation om hun cmdlets beschikbaar voor gebruik in runbooks en hun DSC-resources beschikbaar maken voor gebruik in DSC-configuraties. Azure Automation deze modules worden opgeslagen en bij van runbooktaken en DSC-compilatie uitvoeringstijd, worden ze in de Azure Automation-sandboxes geladen waar runbooks worden uitgevoerd en DSC-configuraties compileren achter de schermen. Een DSC-resources in modules worden ook automatisch op de Automation DSC pull-server geplaatst. Ze kunnen worden opgehaald door machines wanneer deze DSC-configuraties van toepassing zijn.  

We een aantal Azure PowerShell-modules worden gebruiksklaar in Azure Automation geleverd. Maar u kunt PowerShell-modules voor uw systeem, service of hulpprogramma waarmee u integreren wilt met importeren.

> [!NOTE]
> Bepaalde modules worden geleverd als **algemene modules** in de Automation-service. Deze algemene modules zijn beschikbaar wanneer u een automation-account maakt, en wij ze soms werken, waarbij de automatisch nieuwe versies van uw automation-account. Als u niet dat ze automatisch worden bijgewerkt wilt, kunt u altijd importeren dezelfde module zelf en die heeft voorrang op de algemene module-versie van die module die we bij de service meeleveren.

De indeling waarin u een integratiemodulepakket importeert, is een gecomprimeerd bestand met dezelfde naam als de module en met een ZIP-extensie. Het bevat de Windows PowerShell-module en ondersteunende bestanden, inclusief een manifestbestand (.psd1) als de module die een heeft.

Als de module een Azure Automation-verbindingstype moet bevatten, moet deze ook een bestand bevatten met de naam `<ModuleName>-Automation.json`, waarin de eigenschappen van het verbindingstype zijn opgegeven. Dit is een json-bestand, dat wordt geplaatst in de modulemap van uw gecomprimeerde ZIP-bestand. Dit bestand bevat de velden van een **verbinding** die is vereist voor het verbinding maken met het systeem of de service de module wordt aangegeven. Deze configuratie het geval is een verbindingstype gemaakt in Azure Automation. Met dit veld kunt u de veldnamen en typen instellen en opgeven of de velden moeten worden versleuteld en/of optioneel moeten zijn, voor het verbindingstype van de module. Het volgende voorbeeld wordt een sjabloon in de json-indeling:

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

Als u Service Management Automation hebt geïmplementeerd en integratiemodulepakketten voor uw automation-runbooks hebt gemaakt, deze sjabloon er waarschijnlijk vertrouwd voor u ziet.

## <a name="authoring-best-practices"></a>Aanbevolen procedures voor ontwerpen

Hoewel integratiemodules PowerShell-modules zijn, is er nog steeds een aantal zaken waar dat u moet denken bij het ontwerpen van een PowerShell-module voor gebruik in Azure Automation. Enkele aanbevelingen zijn handig voor het maken van de modules die ze goed werken in PowerShell Workflow.

1. Neem een samenvatting, beschrijving en Help-URI op voor elke cmdlet in de module. In PowerShell kunt u bepaalde Help-informatie voor cmdlets definiëren, zodat de gebruiker hulp ontvangt bij het gebruik van cmdlets met de cmdlet **Get-Help**. Bijvoorbeeld, als volgt het definiëren van een samenvatting en help-URI voor een PowerShell-module die is geschreven in een psm1-bestand. 
   
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

   Deze gegevens bieden ziet deze Help-informatie met behulp van de **Get-Help** cmdlet in de PowerShell-console. Deze informatie wordt ook weergegeven in Azure Automation.  Bijvoorbeeld wanneer u activiteiten invoegt tijdens het ontwerpen van een runbook. Te klikken op 'Gedetailleerde help weergeven', wordt de help-URI geopend in een ander tabblad van de webbrowser die u gebruikt voor toegang tot Azure Automation.

   ![Help bij integratiemodule](media/automation-integration-modules/automation-integration-module-activitydesc.png)

2. Als de module wordt uitgevoerd op een extern systeem:

   1. Deze moet een metagegevensbestand voor de integratiemodule bevatten waarin de informatie wordt gedefinieerd die nodig is om verbinding te maken met dat externe systeem, dus het verbindingstype.
   2. Elke cmdlet in de module moet een verbindingsobject (een exemplaar van dat verbindingstype) kunnen bevatten als een parameter.  

    Cmdlets in de module worden eenvoudiger te gebruiken in Azure Automation als u toestaat dat een object met de velden van het verbindingstype als een parameter aan de cmdlet wordt doorgegeven. Hiermee kunnen gebruikers parameters van de verbindingsasset toewijzen aan de bijbehorende parameters van de cmdlet telkens wanneer die ze een cmdlet aanroepen.

    Op basis van het bovenstaande runbookvoorbeeld wordt een Twilio-verbindingsasset genaamd CorpTwilio gebruikt voor toegang tot Twilio en voor het retourneren van alle telefoonnummers in het account.  U ziet hoe het de velden van de verbinding wordt toegewezen aan de parameters van de cmdlet?

    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'

      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Een eenvoudigere en betere manier om dit gedrag wordt het verbindingsobject rechtstreeks doorgegeven aan de cmdlet-

    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'

      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```

    U kunt dergelijk gedrag in het voorgaande voorbeeld inschakelen voor uw cmdlets door ze te accepteren van een verbindingsobject rechtstreeks als parameter, in plaats van alleen Verbindingsvelden voor parameters. Doorgaans wilt u een parameter die is ingesteld voor elk, zodat een gebruiker die niet met behulp van Azure Automation uw cmdlets aanroepen kan zonder een hash-tabel om te fungeren als het verbindingsobject maken. De parameterset **SpecifyConnectionFields**, wordt gebruikt om door te geven van de verbinding veldeigenschappen één voor één. Met **UseConnectionObject** kunt u de verbinding rechtstreeks doorgeven. Zoals u kunt zien is voor de cmdlet Send-TwilioSMS in de [Twilio PowerShell-module](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) de doorgave op beide manieren mogelijk:

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

3. Definieer het uitvoertype voor alle cmdlets in de module. Als u een uitvoertype voor een cmdlet definieert, kan IntelliSense u tijdens het ontwerpen helpen bij het bepalen van de uitvoereigenschappen van de cmdlet, voor gebruik tijdens het ontwerpen. Het is vooral nuttig tijdens de Automation runbook grafisch ontwerpen, waarbij ontwerptijdkennis essentieel is voor een goede gebruikerservaring met uw module.

   ![Uitvoertype grafisch runbook](media/automation-integration-modules/runbook-graphical-module-output-type.png)

   Dit gedrag is vergelijkbaar met de functionaliteit 'automatisch aanvullen' van de uitvoer van een cmdlet in PowerShell ISE zonder uit te voeren.

   ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)

4. Cmdlets in de module mogen geen complexe objecttypen voor parameters duren. PowerShell Workflow verschilt van PowerShell in het volgende opzicht: complexe typen worden in gedeserialiseerde vorm opgeslagen. Primitieve typen blijven primitieven, maar complexe typen converteren naar hun gedeserialiseerde versies, die in wezen Eigenschappenverzamelingen zijn. Als u bijvoorbeeld de cmdlet **Get-Process** hebt gebruikt in een runbook (of een PowerShell Workflow), wordt een object van het type [Deserialized.System.Diagnostic.Process] geretourneerd, niet van het verwachte type [System.Diagnostic.Process]. Dit type heeft dezelfde eigenschappen als het niet-gedeserialiseerde type, maar geen van de methoden van dat type. En als u deze wordt doorgegeven als parameter aan een cmdlet probeert, waarbij de cmdlet wordt een waarde [System.Diagnostic.Process] voor deze parameter wordt verwacht, ontvangt u de volgende fout: *Argumenttransformatie voor parameter 'process' kan niet worden verwerkt. Fout: "De waarde"System.Diagnostics.Process (CcmExec)"van het type"Deserialized.System.Diagnostics.Process"naar type"System.Diagnostics.Process"kan niet worden geconverteerd.*   Dit komt doordat er een type komt niet overeen tussen het verwachte type [System.Diagnostic.Process] en het opgegeven type [Deserialized.System.Diagnostic.Process is]. De manier om dit probleem is om te controleren of dat de cmdlets van uw module geen complexe typen voor parameters nemen. Dit is de verkeerde manier.

    ```powershell
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ```

    De juiste manier is om op te nemen in een primitieve nemen die intern door de cmdlet is gebruikt om het complexe object halen en te gebruiken. Aangezien de cmdlets worden uitgevoerd in de context van PowerShell, en niet PowerShell Workflow, wordt het correcte type in de cmdlet $process [System.Diagnostic.Process].  

    ```powershell
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```

   Verbindingsassets in runbooks zijn hashtabellen, die een complex type zijn, en toch lijken deze hashtabellen te kunnen worden doorgegeven in cmdlets voor hun-Connection-parameter, met zonder gecaste uitzondering. In technisch opzicht kunnen sommige PowerShell-typen goed worden gecast van hun geserialiseerde vorm naar hun gedeserialiseerde vorm en kunnen deze daarom worden doorgegeven in cmdlets voor parameters die het niet-gedeserialiseerde type accepteren. Hashtabel is daar een van. Het is mogelijk om gedefinieerde typen van een module-auteur te implementeren op een manier die deze ook correct kan deserialiseren, maar er zijn enkele compromissen nodig om te overwegen. Het type moet een standaardconstructor hebben, alle eigenschappen van het type moeten openbaar zijn en het type moet een PSTypeConverter hebben. Voor al gedefinieerde typen waarvan de module-auteur geen eigenaar, er is echter geen manier om te 'oplossen', vandaar de aanbeveling om complexe typen voor parameters helemaal te voorkomen. Ontwerpen van een Runbook tip: Als uw cmdlets moet een complex typeparameter nemen, wordt de tijdelijke oplossing in PowerShell-werkstromen, is het verpakken van de cmdlet waarmee het complexe type wordt gegenereerd en de cmdlet waarmee het complexe type in dezelfde InlineScript-activiteit worden verbruikt. Omdat met InlineScript de inhoud wordt uitgevoerd als PowerShell in plaats van PowerShell Workflow, zou met de cmdlet waarmee het complexe type wordt gegenereerd, dat correcte type worden gemaakt, niet het gedeserialiseerde complexe type.

5. Maak alle cmdlets in de module staatloos. In PowerShell Workflow wordt elke cmdlet die in de werkstroom wordt aangeroepen in een andere sessie uitgevoerd. Dit betekent dat cmdlets die afhankelijk van de sessiestatus gemaakt zijn / gewijzigd door andere cmdlets in dezelfde module niet werken in PowerShell Workflow-runbooks.  Hier ziet u een voorbeeld van wat u niet moet doen.
   
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

6. De module moet volledig zijn opgenomen in een pakket waarvoor Xcopy kan worden gebruikt. Azure Automation-modules zijn distributd naar de Automation-sandboxes wanneer runbooks moeten worden uitgevoerd. De modules moeten werken onafhankelijk van de host die ze op worden uitgevoerd. U moet kunnen zippen vergroten en het verplaatsen van een module-pakket en het laten functioneren wanneer geïmporteerd in een andere host PowerShell-omgeving. In de volgorde voor die worden uitgevoerd, mag niet afhankelijk van de module van bestanden buiten de modulemap. Dit is de map die wordt gezipt wanneer de module wordt geïmporteerd in Azure Automation. De module moet ook niet afhankelijk zijn van unieke registerinstellingen op een host, zoals deze instellingen worden ingesteld wanneer een product is geïnstalleerd. Als deze aanbevolen procedure niet wordt gevolgd, is de module niet bruikbaar in Azure Automation.  

7. Als u verwijst naar een [Az van Azure Powershell-modules](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) in uw module, zorg ervoor dat u niet zijn ook verwijzen naar `AzureRM`. De `Az` module kan niet worden gebruikt in combinatie met de `AzureRM` modules. `Az` wordt ondersteund in runbooks, maar worden niet standaard geïmporteerd. Voor meer informatie over de `Az` modules en overwegingen bij het rekening mee dat, Zie [Az-module-ondersteuning in Azure Automation](az-modules.md).

## <a name="next-steps"></a>Volgende stappen

* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie [Een Windows PowerShell-module schrijven](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx) voor meer informatie over het maken van PowerShell-modules.
