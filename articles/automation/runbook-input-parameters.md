---
title: Invoerparameters voor runbook
description: Invoerparameters voor Runbook verhoogt de flexibiliteit van runbooks met zodat u kunt gegevens doorgeven aan een runbook wanneer deze wordt gestart. Dit artikel beschrijft de verschillende scenario's waarin invoerparameters worden gebruikt in runbooks.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 969e0c2582ce8f72592059fbf1d58e3ebe9faa5d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58117197"
---
# <a name="runbook-input-parameters"></a>Invoerparameters voor runbook

Invoerparameters voor Runbook verhoogt de flexibiliteit van runbooks met zodat u kunt gegevens geeft aan wanneer deze wordt gestart. De parameters kan de runbook-acties is dit bedoeld voor specifieke scenario's en omgevingen. In dit artikel helpen u bij verschillende scenario's waarin invoerparameters worden gebruikt in runbooks.

## <a name="configure-input-parameters"></a>Invoerparameters configureren

Invoerparameters kunnen worden geconfigureerd in PowerShell, PowerShell-werkstroom, Python en grafische runbooks. Een runbook kan meerdere parameters met verschillende gegevenstypen, of hebben geen parameters helemaal. Invoerparameters kunnen verplicht of optioneel, en u kunt een standaardwaarde opgeven voor optionele parameters hebben. U toewijzen waarden aan de invoerparameters voor een runbook wanneer u deze via een van de beschikbare methoden starten. Deze methoden omvatten een runbook starten vanuit de Azure-portal, een webservice of PowerShell. U kunt ook een als een onderliggend runbook dat inline wordt aangeroepen in een ander runbook starten.

## <a name="configure-input-parameters-in-powershell-runbooks"></a>Invoerparameters die zijn opgegeven in de PowerShell-runbooks configureren

PowerShell- en PowerShell Workflow-runbooks in Azure Automation ondersteunen invoerparameters die zijn gedefinieerd door de volgende kenmerken:  

| **Eigenschap** | **Beschrijving** |
|:--- |:--- |
| Type |Vereist. Het gegevenstype voor de parameterwaarde verwacht. Elk type .NET is geldig. |
| Name |Vereist. De naam van de parameter. Dit moet uniek zijn binnen het runbook en kunnen bevatten alleen letters, cijfers of onderstrepingstekens. Deze moet beginnen met een letter. |
| Verplicht |Optioneel. Hiermee geeft u op of moet een waarde worden opgegeven voor de parameter. Als u dit instellen op  **\$waar**, en vervolgens moet een waarde worden opgegeven wanneer het runbook wordt gestart. Als u dit instellen op  **\$false**, en vervolgens een waarde optioneel is. |
| Standaardwaarde |Optioneel. Hiermee geeft u een waarde die wordt gebruikt voor de parameter als een waarde niet wordt doorgegeven wanneer het runbook wordt gestart. Een standaardwaarde voor elke parameter kan worden ingesteld en wordt automatisch de parameter optioneel maken, ongeacht de verplichte instelling. |

Windows PowerShell ondersteunt meer kenmerken van de invoerparameters die zijn opgegeven dan die hier worden vermeld, zoals validatie, aliassen en de parameter wordt ingesteld. Op dit moment ondersteunt Azure Automation echter alleen de voorgaande invoerparameters.

De parameterdefinitie van een in PowerShell Workflow-runbooks heeft de volgende algemene notatie, waarbij meerdere parameters worden gescheiden door komma's.

```powershell
Param
(
  [Parameter (Mandatory= $true/$false)]
  [Type] $Name1 = <Default value>,

  [Parameter (Mandatory= $true/$false)]
  [Type] $Name2 = <Default value>
)
```

> [!NOTE]
> Wanneer u parameters, als u geen opgeeft definieert het **verplichte** kenmerk, wordt standaard de parameter wordt beschouwd als optioneel. Ook als u een standaardwaarde voor een parameter in PowerShell Workflow-runbooks instelt, wordt dit beschouwd door PowerShell als een optionele parameter, ongeacht de **verplichte** waarde van het kenmerk.

Bijvoorbeeld: we gaan de invoerparameters voor een PowerShell Workflow-runbook die uitvoer meer informatie over virtuele machines, één virtuele machine of alle virtuele machines binnen een resourcegroep configureren. Dit runbook heeft twee parameters zoals weergegeven in de volgende schermafbeelding: de naam van de virtuele machine en de naam van de resourcegroep.

![Automation PowerShell-werkstroom](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

In deze parameterdefinitie van de, de parameters  **\$VMName** en  **\$resourceGroupName** zijn eenvoudige parameters van het typetekenreeks. PowerShell- en PowerShell Workflow-runbooks ondersteunen echter alle eenvoudige typen en complexe typen, zoals **object** of **PSCredential** voor invoerparameters die zijn opgegeven.

Als uw runbook een invoerparameter van het object type heeft, gebruikt u een PowerShell-hash-tabel met (naam, waarde) paren om door te geven in een waarde. Bijvoorbeeld, als u de volgende parameter in een runbook hebt:

```powershell
[Parameter (Mandatory = $true)]
[object] $FullName
```

U kunt vervolgens de volgende waarde doorgeven aan de parameter:

```powershell
@{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}
```

> [!NOTE]
> Als u geen waarde doorgeven aan een optionele `[String]` typeparameter waarvoor een _standaardwaarde_ van `\$null`, wordt de waarde van de parameter een _lege tekenreeks_, **niet** `\$null`.

## <a name="configure-input-parameters-in-graphical-runbooks"></a>Invoerparameters die zijn opgegeven in grafische runbooks configureren

Naar [configureren van een grafisch runbook](automation-first-runbook-graphical.md) met invoerparameters die zijn opgegeven, gaan we een grafisch runbook maken die meer informatie over virtuele machines, ofwel levert een enkele virtuele machine of alle virtuele machines binnen een resourcegroep. Configureren van een runbook bestaat uit twee belangrijke activiteiten, zoals hieronder wordt beschreven.

[**Runbooks verifiëren met een Azure uitvoeren als-account** ](automation-sec-configure-azure-runas-account.md) voor verificatie met Azure.

[**Get-AzureRmVm** ](/powershell/module/azurerm.compute/get-azurermvm) om op te halen van de eigenschappen van een virtuele machine.

U kunt de [ **Write-Output** ](/powershell/module/microsoft.powershell.utility/write-output) activiteit om uit te voeren van de namen van virtuele machines. De activiteit **Get-AzureRmVm** twee parameters accepteert de **virtuele-machinenaam** en de **groepsnaam voor accountresources**. Omdat deze parameters verschillende waarden telkens wanneer die u het runbook start vereist kunnen, kunt u invoerparameters toevoegen aan uw runbook. Hier volgen de stappen voor het invoerparameters toevoegen:

1. Selecteer het grafisch runbook vanuit de **Runbooks** blade en klik vervolgens op [ **bewerken** ](automation-graphical-authoring-intro.md) deze.
2. Klik in de runbookeditor op **invoer en uitvoer** openen de **invoer en uitvoer** blade.

   ![Automatisering van een grafisch runbook](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)

3. De **invoer en uitvoer** blade geeft een lijst van de invoerparameters die zijn gedefinieerd voor het runbook. Op deze blade kunt u een nieuwe invoerparameter toevoegen of bewerken van de configuratie van een bestaande invoerparameter. Als u wilt toevoegen van een nieuwe parameter voor het runbook, klikt u op **invoer toevoegen** openen de **runbookinvoerparameter** blade. Daar kunt u de volgende parameters:

   | **Eigenschap** | **Beschrijving** |
   |:--- |:--- |
   | Name |Vereist. De naam van de parameter. Dit moet uniek zijn binnen het runbook en kunnen bevatten alleen letters, cijfers of onderstrepingstekens. Deze moet beginnen met een letter. |
   | Description |Optioneel. Beschrijving over het doel van de invoerparameter. |
   | Type |Optioneel. Het gegevenstype dat wordt verwacht voor de waarde van parameter. Ondersteunde parametertypen zijn **tekenreeks**, **Int32**, **Int64**, **decimaal**, **Booleaanse**,  **Datum-/**, en **Object**. Als een gegevenstype dat niet is geselecteerd, wordt standaard **tekenreeks**. |
   | Verplicht |Optioneel. Hiermee geeft u op of moet een waarde worden opgegeven voor de parameter. Als u ervoor kiest **Ja**, en vervolgens moet een waarde worden opgegeven wanneer het runbook wordt gestart. Als u ervoor kiest **geen**, en vervolgens een waarde niet vereist is als het runbook wordt gestart en kan een standaardwaarde worden ingesteld. |
   | Standaardwaarde |Optioneel. Hiermee geeft u een waarde die wordt gebruikt voor de parameter als een waarde niet wordt doorgegeven wanneer het runbook wordt gestart. Een standaardwaarde kan worden ingesteld voor een parameter die is niet verplicht. Als u wilt een standaardwaarde instelt, kiest u **aangepaste**. Deze waarde wordt gebruikt, tenzij een andere waarde is opgegeven als het runbook wordt gestart. Kies **geen** als u niet wilt bieden een standaardwaarde. |

    ![Nieuwe invoer toevoegen](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. Maak twee parameters met de volgende eigenschappen die worden gebruikt door de **Get-AzureRmVm** activiteit:

   * **Parameter1:**
     * Naam van de-VMName
     * Type - String
     * Verplichte - Nee
   * **Parameter2:**
     * Name - resourceGroupName
     * Type - String
     * Verplichte - Nee
     * Standaardwaarde: aangepaste
     * Aangepaste standaardwaarde - \<naam van de resourcegroep met de virtuele machines\>

5. Nadat u de parameters toevoegt, klikt u op **OK**. U kunt nu deze bekijken in de **invoer en uitvoer pagina**. Klik op **OK** opnieuw, en klik vervolgens op **opslaan** en **publiceren** uw runbook.

## <a name="configure-input-parameters-in-python-runbooks"></a>Invoerparameters die zijn opgegeven in Python runbooks configureren

In tegenstelling tot PowerShell, PowerShell-werkstroom en grafische runbooks hebben Python-runbooks geen benoemde parameters.
Alle ingevoerde parameters worden geparseerd als een matrix met waarden van het argument.
U toegang tot de matrix door het importeren van de `sys` module in uw Python-script en klik vervolgens met behulp de `sys.argv` matrix.
Het is belangrijk te weten dat het eerste element van de matrix, `sys.argv[0]`, is de naam van het script, zodat de eerste parameter van de werkelijke invoer is `sys.argv[1]`.

Zie voor een voorbeeld van hoe u invoerparameters die zijn opgegeven in een Python-runbook [Mijn eerste Python-runbook in Azure Automation](automation-first-runbook-textual-python2.md).

## <a name="assign-values-to-input-parameters-in-runbooks"></a>Waarden voor het invoeren van parameters in runbooks toewijzen

U kunt doorgeven waarden op voor de invoerparameters in runbooks in de volgende scenario's:

### <a name="start-a-runbook-and-assign-parameters"></a>Een runbook te starten en parameters toe te wijzen

Een runbook kan veel verschillende manieren worden gestart: via de Azure portal, met een webhook, met PowerShell-cmdlets, met de REST API of met de SDK. Hieronder wordt besproken hoe verschillende methoden voor een runbook starten en parameters toe te wijzen.

#### <a name="start-a-published-runbook-by-using-the-azure-portal-and-assign-parameters"></a>Een gepubliceerd runbook start met behulp van Azure portal en parameters toewijzen

Wanneer u [het runbook starten](start-runbooks.md#start-a-runbook-with-the-azure-portal), wordt de **Runbook starten** blade wordt geopend en u kunt waarden opgeven voor de parameters die u hebt gemaakt.

![Start met behulp van de portal](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

In het label onder het invoervak ziet u de kenmerken die zijn ingesteld voor de parameter. Kenmerken zijn verplicht of optioneel, type en de standaardwaarde. In de help-ballon naast de parameternaam van de ziet u alle belangrijke informatie die u nodig hebt om beslissingen te nemen over invoer parameterwaarden. Deze informatie omvat of een parameter verplicht of optioneel is. Dit omvat ook het type en standaardwaarde (indien aanwezig) en andere nuttige notities.

> [!NOTE]
> Ondersteuning voor parameters van type String **leeg** tekenreekswaarden.  Invoeren **[EmptyString]** box wordt in de invoerparameter een lege tekenreeks doorgegeven aan de parameter. Ook geen ondersteuning voor parameters van het type tekenreeks **Null** waarden worden doorgegeven. Als u niet een willekeurige waarde aan de queryreeks-parameter doorgeven, klikt u vervolgens interpreteert PowerShell die waarde als null.

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a>Een gepubliceerd runbook start met behulp van PowerShell-cmdlets en parameters toewijzen

* **Azure Resource Manager-cmdlets:** U kunt een Automation-runbook dat is gemaakt in een resourcegroep gemaakt met behulp van starten [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook).
  
  **Voorbeeld:**

  ```powershell
  $params = @{"VMName"="WSVMClassic";"resourceGroupeName"="WSVMClassicSG"}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName "TestAutomation" -Name "Get-AzureVMGraphical" –ResourceGroupName $resourceGroupName -Parameters $params
  ```

* **Cmdlets voor Azure classic deployment model:** U kunt een automation-runbook dat is gemaakt in een standaard-resourcegroep met behulp van starten [Start AzureAutomationRunbook](/powershell/module/servicemanagement/azure/start-azureautomationrunbook).
  
  **Voorbeeld:**

  ```powershell
  $params = @{"VMName"="WSVMClassic"; "ServiceName"="WSVMClassicSG"}
  
  Start-AzureAutomationRunbook -AutomationAccountName "TestAutomation" -Name "Get-AzureVMGraphical" -Parameters $params
  ```

> [!NOTE]
> Wanneer u een runbook start met behulp van PowerShell-cmdlets, een standaardparameter **MicrosoftApplicationManagementStartedBy** wordt gemaakt met de waarde **PowerShell**. U vindt deze parameter in de **taakgegevens** pagina.  

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a>Een runbook start met behulp van een SDK en parameters toe te wijzen

* **Azure Resource Manager-methode:** U kunt een runbook start met behulp van de SDK van een programmeertaal. Hieronder ziet u een C#-codefragment voor het starten van een runbook in uw Automation-account. U vindt de code aan onze [GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).  

  ```csharp
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
  ```

* **Klassieke Azure-implementatie model methode:** U kunt een runbook start met behulp van de SDK van een programmeertaal. Hieronder ziet u een C#-codefragment voor het starten van een runbook in uw Automation-account. U vindt de code aan onze [GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).

  ```csharp
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
  ```

  Voor deze methode start, het maken van een woordenlijst voor het opslaan van de runbookparameters, **VMName** en **resourceGroupName**, en de bijbehorende waarden. Start het runbook. Hieronder vindt u de C#-codefragment voor het aanroepen van de methode die hierboven gedefinieerd.

  ```csharp
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call the StartRunbook method with parameters
  StartRunbook("Get-AzureVMGraphical", RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-the-rest-api-and-assign-parameters"></a>Een runbook start met behulp van de REST-API en parameters toe te wijzen

Een runbook-taak kan worden gemaakt en gestart met de REST-API van Azure Automation met behulp van de **plaatsen** methode met de volgende aanvraag-URI: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Automation/automationAccounts/{automationAccountName}/jobs/{jobName}?api-version=2017-05-15-preview`


In de aanvraag-URI, vervangt u de volgende parameters:

* **subscriptionId:** Uw Azure-abonnement-ID.  
* **resourceGroupName:** De naam van de resourcegroep voor het Automation-Account.
* **automationAccountName:** De naam van uw automation-account dat wordt gehost binnen de opgegeven cloudservice.  
* **jobName:** De GUID voor de taak. GUID's in PowerShell kunnen worden gemaakt met behulp van de **[GUID]::NewGuid(). ToString()** opdracht.

Gebruik de hoofdtekst van de aanvraag om de parameters doorgeven aan de runbooktaak. Het duurt voordat de volgende twee eigenschappen die is opgegeven in JSON-indeling:

* **Runbooknaam:** Vereist. De naam van het runbook voor de taak wordt gestart.  
* **Runbook-parameters:** Optioneel. Een woordenlijst met de lijst met parameters in (naam, waarde)-indeling waarin de naam moet van het type tekenreeks en waarde is geldige JSON-waarde.

Als u wilt beginnen de **Get-AzureVMTextual** runbook dat eerder is gemaakt met **VMName** en **resourceGroupName** als parameters, gebruik de volgende JSON-indeling voor de hoofdtekst van de aanvraag.

   ```json
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WindowsVM",
         "resourceGroupName":"ContosoSales"}
        }
    }
   ```

HTTP-statuscode 201 wordt geretourneerd als de taak is gemaakt. Zie het artikel over hoe u voor meer informatie over antwoordheaders en de antwoordtekst [een runbook-taak maken met behulp van de REST-API.](/rest/api/automation/job/create)

### <a name="test-a-runbook-and-assign-parameters"></a>Een runbook testen en parameters toe te wijzen

Wanneer u [de conceptversie van uw runbook testen](automation-testing-runbook.md) met de optie test de **testen** pagina wordt geopend en kunt u waarden voor de parameters die u hebt gemaakt.

![Testen en parameters toe te wijzen](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a>Een planning aan een runbook koppelen en parameters toe te wijzen

U kunt [een planning koppelen](automation-schedules.md) aan uw runbook zodat het runbook wordt gestart op een bepaald tijdstip. U toewijzen invoerparameters die zijn opgegeven bij het maken van de planning en het runbook maakt gebruik van deze waarden wanneer deze wordt gestart door de planning. U kunt het schema niet opslaan, totdat alle verplichte parameterwaarden zijn opgegeven.

![Plannen en parameters toe te wijzen](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>Een webhook voor een runbook maken en toewijzen van parameters

U kunt maken een [webhook](automation-webhooks.md) voor uw runbook en invoerparameters voor runbook configureren. U kunt de webhook niet opslaan, totdat alle verplichte parameterwaarden zijn opgegeven.

![Webhook maken en toewijzen van parameters](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

Wanneer het uitvoeren van een runbook met behulp van een webhook, de vooraf gedefinieerde invoerparameter **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** wordt verzonden, samen met de invoerparameters die u hebt gedefinieerd. U kunt klikken om uit te breiden de **WebhookData** parameter voor meer informatie.

![WebhookData-parameter](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="pass-a-json-object-to-a-runbook"></a>Een JSON-object doorgeven aan een runbook

Kan het nuttig zijn voor het opslaan van gegevens die u wilt doorgeven aan een runbook in een JSON-bestand.
U kunt bijvoorbeeld een JSON-bestand met alle van de parameters die u wilt doorgeven aan een runbook maken. Om dit te doen, moet u de JSON converteren naar een tekenreeks en converteert de tekenreeks naar een PowerShell-object voordat deze wordt doorgegeven aan het runbook.

In dit voorbeeld hebt u een PowerShell-script aanroept [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) naar een PowerShell-runbook, de inhoud van de JSON wordt doorgegeven aan het runbook te starten.
De PowerShell-runbook start een Azure-VM ophalen van de parameters voor de virtuele machine uit de JSON die is doorgegeven.

### <a name="create-the-json-file"></a>Het JSON-bestand maken

Typ de volgende test in een tekstbestand en sla het bestand als `test.json` ergens op de lokale computer.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

### <a name="create-the-runbook"></a>Het runbook te maken

Maak een nieuwe PowerShell-runbook met de naam 'Test-Json' in Azure Automation.
Zie voor meer informatie over het maken van een nieuwe PowerShell-runbook, [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md).

Voor het accepteren van de JSON-gegevens, moet het runbook uitvoeren van een object als invoerparameter.

Het runbook kan vervolgens gebruiken voor de eigenschappen die zijn gedefinieerd in de JSON.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
```

Opslaan en publiceren van dit runbook in uw Automation-account.

### <a name="call-the-runbook-from-powershell"></a>Het runbook aanroepen vanuit PowerShell

U kunt nu het runbook aanroepen vanuit uw lokale computer met behulp van Azure PowerShell.
Voer de volgende PowerShell-opdrachten:

1. Aanmelden bij Azure:

   ```powershell
   Connect-AzureRmAccount
   ```

   U wordt gevraagd uw Azure-referenties in te voeren.

   > [!IMPORTANT]
   > **Add-AzureRmAccount** is nu een alias voor **Connect-AzureRMAccount**. Wanneer uw bibliotheek zoeken items, als u niet ziet **Connect-AzureRMAccount**, kunt u **Add-AzureRmAccount**, of u kunt uw modules bijwerken in uw Automation-Account.

1. De inhoud van het JSON-bestand ophalen en deze converteren naar een tekenreeks:

   ```powershell
   $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
   ```

   `JsonPath` is het pad waar u het JSON-bestand hebt opgeslagen.

1. Converteren van de inhoud van de tekenreeks van `$json` naar een PowerShell-object:

   ```powershell
   $JsonParams = @{"json"=$json}
   ```

1. Maken van een hashtabel voor de parameters voor `Start-AzureRmAutomationRunbook`:

   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```

   U ziet u de waarde van de instelt `Parameters` naar het PowerShell-object dat de waarden van het JSON-bestand bevat.
1. Het runbook starten

   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de verschillende manieren om een runbook te starten, [een runbook starten](automation-starting-a-runbook.md).
* Als u wilt een tekstuele runbook bewerken, verwijzen naar [tekstrunbooks bewerken](automation-edit-textual-runbook.md).
* Als u wilt een grafisch runbook bewerken, verwijzen naar [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).