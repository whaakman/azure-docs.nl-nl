---
title: Modules in Azure Automation beheren
description: In dit artikel wordt beschreven hoe u voor het beheren van modules in Azure Automation
services: automation
ms.service: automation
ms.subservice: shared-resources
author: georgewallace
ms.author: gwallace
ms.date: 03/13/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fa7f5d3fb38eb1dbca51dec9b73dca3c998436aa
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57909110"
---
# <a name="manage-modules-in-azure-automation"></a>Modules in Azure Automation beheren

Azure Automation biedt de mogelijkheid om te importeren van de PowerShell-modules in uw Automation-Account moet worden gebruikt door de runbooks op basis van PowerShell. Deze modules zijn aangepaste modules die u hebt gemaakt, vanuit de PowerShell Gallery, of de azurerm-module en Az-modules voor Azure.

## <a name="import-modules"></a>Modules importeren

Er zijn meerdere manieren dat u een module in uw Automation-Account importeren kunt. De volgende secties tonen de verschillende manieren om een module te importeren.

> [!NOTE]
> Het maximale pad van een bestand in een module moet worden gebruikt in Azure Automation is 140 tekens. Elk pad over 140 tekens niet mogelijk om te worden geïmporteerd in de PowerShell-sessie met `Import-Module`.

### <a name="powershell"></a>PowerShell

U kunt de [New-AzureRmAutomationModule](/powershell/module/azurerm.automation/new-azurermautomationmodule) importeren van een module in uw Automation-Account. De cmdlet voert een url aan een module-zip-pakket.

```azurepowershell-interactive
New-AzureRmAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

### <a name="azure-portal"></a>Azure Portal

Navigeer in de Azure-portal naar uw Automation-Account en selecteer **Modules** onder **gedeelde bronnen**. Klik op **+ toevoegen van een module**. Selecteer een **.zip** -bestand met de module en klik op **Ok** om te beginnen met het importproces.

### <a name="powershell-gallery"></a>PowerShell Gallery

Modules van de PowerShell gallery kunnen ofwel worden geïmporteerd vanuit de [PowerShell Gallery](https://www.powershellgallery.com) rechtstreeks of via uw Automation-Account.

Als u wilt een module importeren vanuit de PowerShell Gallery, gaat u naar https://www.powershellgallery.com en zoek de module die u wilt importeren. Klik op **implementeren in Azure Automation** op de **Azure Automation** tabblad onder **installatieopties**. Deze actie opent u de Azure-portal. Op de **importeren** pagina, selecteert u uw Automation-Account en klikt u op **OK**.

![Import-module voor PowerShell Gallery](../media/modules/powershell-gallery.png)

U kunt ook modules importeren vanuit de PowerShell Gallery rechtstreeks vanuit uw Automation-Account. Selecteer in uw Automation-Account **Modules** onder **gedeelde bronnen**. Klik op de pagina modules **bladeren in galerie**. Hiermee opent u de **bladeren in galerie** pagina. U kunt deze pagina gebruiken om te zoeken naar de PowerShell Gallery voor een module. Selecteer de module die u wilt importeren en klik op **importeren**. Op de **importeren** pagina, klikt u op **OK** te starten tijdens het importeren.

![PowerShell Gallery importeren uit Azure portal](../media/modules/gallery-azure-portal.png)

## <a name="internal-cmdlets"></a>Interne-cmdlets

Hieronder volgt een overzicht van de cmdlets in de interne `Orchestrator.AssetManagement.Cmdlets` -module die is geïmporteerd in alle Automation-Account. Deze cmdlets zijn beschikbaar in uw runbooks en DSC-configuraties en kunt u werken met uw activa in uw Automation-Account. Bovendien de interne cmdlets kunt u geheimen ophalen van versleutelde **variabele** waarden, **referenties**, en versleutelde **verbinding** velden. De Azure PowerShell-cmdlets zijn niet meer ophalen van deze geheime gegevens. Deze cmdlets hebben het impliciet verbinding maken met Azure wanneer u ze niet nodig. Dit is nuttig voor scenario's waarbij u een verbinding, zoals uitvoeren als-Account dat u gebruiken wilt voor verificatie op Azure.

|Name|Description|
|---|---|
|Get-AutomationCertificate|`Get-AutomationCertificate [-Name] <string> [<CommonParameters>]`|
|Get-AutomationConnection|`Get-AutomationConnection [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]` |
|Get-AutomationPSCredential|`Get-AutomationPSCredential [-Name] <string> [<CommonParameters>]` |
|Get-AutomationVariable|`Get-AutomationVariable [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]`|
|Set-AutomationVariable|`Set-AutomationVariable [-Name] <string> -Value <Object> [<CommonParameters>]` |
|Start-AutomationRunbook|`Start-AutomationRunbook [-Name] <string> [-Parameters <IDictionary>] [-RunOn <string>] [-JobId <guid>] [<CommonParameters>]`|
|Wait-AutomationJob|`Wait-AutomationJob -Id <guid[]> [-TimeoutInMinutes <int>] [-DelayInSeconds <int>] [-OutputJobsTransitionedToRunning] [<CommonParameters>]`|

## <a name="add-a-connection-type-to-your-module"></a>Een verbindingstype aan uw module toevoegen

U kunt opgeven dat een aangepaste [verbindingstype](../automation-connections.md) voor gebruik in uw Automation-Account door een optioneel bestand toe te voegen aan uw module. Dit bestand is een metagegevensbestand waarin een Azure Automation-verbindingstype moet worden gebruikt met de cmdlets van de module in uw Automation-Account opgegeven. Hiervoor moet u eerst weten over het ontwerpen van een PowerShell-module. Zie voor meer informatie over het ontwerpen van module [over het schrijven van een PowerShell-Script-Module](/powershell/developer/module/how-to-write-a-powershell-script-module).

![Een aangepaste verbinding gebruiken in Azure portal](../media/modules/connection-create-new.png)

Als u wilt toevoegen van een Azure Automation-verbindingstype, moet uw module een bestand met de naam bevatten `<ModuleName>-Automation.json` dat de verbindingstype eigenschappen specificeert. Dit is een json-bestand, dat wordt geplaatst in de modulemap van uw gecomprimeerde ZIP-bestand. Dit bestand bevat de velden van een verbinding die is vereist voor het verbinding maken met het systeem of de service die de module wordt aangegeven. Deze configuratie het geval is een verbindingstype gemaakt in Azure Automation. Met behulp van dit bestand kunt u de veldnamen en instellen van het type, en of de velden moeten worden versleuteld of optioneel, voor het verbindingstype van de module. Het volgende voorbeeld wordt een sjabloon in de json-bestandsindeling die een eigenschap van de gebruikersnaam en wachtwoord definieert:

```json
{
   "ConnectionFields": [
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
   }
   ],
   "ConnectionTypeName":  "MyModuleConnection",
   "IntegrationModuleName":  "MyModule"
}
```

## <a name="module-best-practices"></a>Aanbevolen procedures van module

PowerShell-modules kunnen worden geïmporteerd in Azure Automation om hun cmdlets beschikbaar voor gebruik in runbooks en hun DSC-resources beschikbaar maken voor gebruik in DSC-configuraties. Azure Automation deze modules worden opgeslagen en bij van runbooktaken en DSC-compilatie uitvoeringstijd, worden ze in de Azure Automation-sandboxes geladen waar runbooks worden uitgevoerd en DSC-configuraties compileren achter de schermen. Een DSC-resources in modules worden ook automatisch op de Automation DSC pull-server geplaatst. Ze kunnen worden opgehaald door machines wanneer deze DSC-configuraties van toepassing zijn.

U wordt aangeraden u rekening houden met het volgende wanneer u een PowerShell-module voor gebruik in Azure Automation maken:

* Neem een samenvatting, beschrijving en Help-URI op voor elke cmdlet in de module. In PowerShell kunt u bepaalde Help-informatie voor cmdlets definiëren, zodat de gebruiker hulp ontvangt bij het gebruik van cmdlets met de cmdlet **Get-Help**. Het volgende voorbeeld laat zien hoe om te definiëren van een samenvatting en help-URI voor in een psm1-module-bestand:

  ```powershell
  <#
       .SYNOPSIS
        Gets a Contoso User account
  #>
  function Get-ContosoUser {
  [CmdletBinding](DefaultParameterSetName='UseConnectionObject', `
  HelpUri='https://www.contoso.com/docs/information')]
  [OutputType([String])]
  param(
     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $UserName,

     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $Password,

     [Parameter(ParameterSetName='ConnectionObject', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [Hashtable]
     $Connection
  )

  switch ($PSCmdlet.ParameterSetName) {
     "UserAccount" {
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $UserName, $Password
        Connect-Contoso -Credential $cred
     }
     "ConnectionObject" {
        Connect-Contoso -Connection $Connection
    }
  }
  }
  ```

  Deze gegevens bieden ziet deze Help-informatie met behulp van de **Get-Help** cmdlet in de PowerShell-console. Deze beschrijving wordt ook weergegeven in de Azure-portal.

  ![Help bij integratiemodule](../media/modules/module-activity-description.png)

* Als de module verbinding met een externe service maakt, moet deze bevatten een [verbindingstype](#add-a-connection-type-to-your-module). Elke cmdlet in de module moet een verbindingsobject (een exemplaar van dat verbindingstype) kunnen bevatten als een parameter. Hiermee kunnen gebruikers parameters van de verbindingsasset toewijzen aan de bijbehorende parameters van de cmdlet telkens wanneer die ze een cmdlet aanroepen. Op basis van bovenstaand voorbeeld runbook, wordt een voorbeeld van de Contoso-verbindingsasset genaamd ContosoConnection voor toegang tot resources van Contoso en als resultaat de gegevens van de externe service.

  In het volgende voorbeeld wordt de velden zijn toegewezen aan de gebruikersnaam en wachtwoord voor de eigenschappen van een `PSCredential` object en vervolgens doorgegeven aan de cmdlet.

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $contosoConnection.UserName, $contosoConnection.Password
  Connect-Contoso -Credential $cred
  }
  ```

  Een eenvoudigere en betere manier om dit gedrag is het verbindingsobject rechtstreeks doorgeven aan de cmdlet:

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  Connect-Contoso -Connection $contosoConnection
  }
  ```

  U kunt dergelijk gedrag in het voorgaande voorbeeld inschakelen voor uw cmdlets door ze te accepteren van een verbindingsobject rechtstreeks als parameter, in plaats van alleen Verbindingsvelden voor parameters. Doorgaans wilt u een parameter die is ingesteld voor elk, zodat een gebruiker die niet met behulp van Azure Automation uw cmdlets aanroepen kan zonder een hash-tabel om te fungeren als het verbindingsobject maken. De parameterset `UserAccount`, om door te geven van de verbinding eigenschappen van het veld wordt gebruikt. `ConnectionObject` Hiermee kunt u de verbinding rechtstreeks doorgeven.

* Definieer het uitvoertype voor alle cmdlets in de module. Als u een uitvoertype voor een cmdlet definieert, kan IntelliSense u tijdens het ontwerpen helpen bij het bepalen van de uitvoereigenschappen van de cmdlet, voor gebruik tijdens het ontwerpen. Het is vooral nuttig tijdens de Automation runbook grafisch ontwerpen, waarbij ontwerptijdkennis essentieel is voor een goede gebruikerservaring met uw module.

  Dit kan worden bereikt door toe te voegen `[OutputType([<MyOutputType>])]` waar MyOutputType is een ongeldig type. Zie voor meer informatie over OutputType [over functies OutputTypeAttribute](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute). De volgende code is een voorbeeld van het toevoegen van `OutputType` aan een cmdlet:

  ```powershell
  function Get-ContosoUser {
  [OutputType([String])]
  param(
     [string]
     $Parameter1
  )
  # <script location here>
  }
  ```

  ![Uitvoertype grafisch runbook](../media/modules/runbook-graphical-module-output-type.png)

  Dit gedrag is vergelijkbaar met de functionaliteit 'automatisch aanvullen' van de uitvoer van een cmdlet in PowerShell ISE zonder uit te voeren.

  ![POSH IntelliSense](../media/modules/automation-posh-ise-intellisense.png)

* Maak alle cmdlets in de module staatloos. Meerdere runbooktaken kunnen tegelijkertijd worden uitgevoerd in hetzelfde toepassingsdomein en hetzelfde proces en sandbox. Als er status die op deze niveaus worden gedeeld, kunnen elkaar van invloed op taken. Dit gedrag kan leiden tot onregelmatige en moeilijk om problemen te diagnosticeren.  Hier ziet u een voorbeeld van wat u niet moet doen.

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

* De module moet volledig zijn opgenomen in een pakket waarvoor xcopy kan. Azure Automation-modules worden gedistribueerd naar de Automation-sandboxes wanneer runbooks moeten worden uitgevoerd. De modules moeten werken onafhankelijk van de host die ze op worden uitgevoerd. U moet kunnen zippen vergroten en het verplaatsen van een module-pakket en het laten functioneren wanneer geïmporteerd in een andere host PowerShell-omgeving. In de volgorde voor die worden uitgevoerd, mag niet afhankelijk van de module van bestanden buiten de modulemap. Dit is de map die wordt gezipt wanneer de module wordt geïmporteerd in Azure Automation. De module moet ook niet afhankelijk zijn van unieke registerinstellingen op een host, zoals deze instellingen worden ingesteld wanneer een product is geïnstalleerd. Alle bestanden in de module moeten een pad minder dan 140 tekens bevatten. Alle paden over 140 tekens zorgt ervoor dat problemen met het importeren van uw runbook. Als deze aanbevolen procedure niet wordt gevolgd, is de module niet bruikbaar in Azure Automation.  

* Als u verwijst naar een [Az van Azure Powershell-modules](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) in uw module, zorg ervoor dat u niet zijn ook verwijzen naar `AzureRM`. De `Az` module kan niet worden gebruikt in combinatie met de `AzureRM` modules. `Az` wordt ondersteund in runbooks, maar worden niet standaard geïmporteerd. Voor meer informatie over de `Az` modules en overwegingen bij het rekening mee dat, Zie [Az-module-ondersteuning in Azure Automation](../az-modules.md).

## <a name="next-steps"></a>Volgende stappen

* Zie [Een Windows PowerShell-module schrijven](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx) voor meer informatie over het maken van PowerShell-modules.