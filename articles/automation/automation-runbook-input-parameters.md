---
title: De invoerparameters van Runbook | Microsoft Docs
description: Invoerparameters van Runbook vergroot de flexibiliteit van runbooks doordat u gegevens moeten worden doorgegeven aan een runbook wanneer deze wordt gestart. In dit artikel beschrijft de verschillende scenario's waar invoerparameters worden gebruikt in runbooks.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 4d3dff2c-1f55-498d-9a0e-eee497e5bedb
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sngun
ms.openlocfilehash: 889d1ac1597bd88ae7455ac98bfdb34f4013e0de
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="runbook-input-parameters"></a>Invoerparameters voor runbook

Invoerparameters van Runbook vergroot de flexibiliteit van runbooks doordat u gegevens moeten worden doorgegeven aan wanneer deze wordt gestart. De parameters kunnen de acties van het runbook moet worden toegepast voor specifieke scenario's en omgevingen. In dit artikel begeleidt we u stapsgewijs door verschillende scenario's waar invoerparameters worden gebruikt in runbooks.

## <a name="configure-input-parameters"></a>Invoerparameters configureren

Invoerparameters kunnen worden geconfigureerd in PowerShell, PowerShell Workflow, Python en grafische runbooks. Een runbook kan meerdere parameters met verschillende gegevenstypen, of hebben geen parameters helemaal. Invoerparameters kunnen worden verplicht of optioneel en u een standaardwaarde opgeven voor optionele parameters kunt toewijzen. U kunt waarden toewijzen aan de invoerparameters voor een runbook bij het starten via een van de beschikbare methoden. Deze methoden omvatten een runbook starten vanuit de portal of een webservice. U kunt ook starten als een onderliggend runbook dat inline wordt aangeroepen in een ander runbook.

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a>De invoerparameters in PowerShell en PowerShell Workflow-runbooks configureren

PowerShell en [PowerShell Workflow-runbooks](automation-first-runbook-textual.md) in Azure Automation ondersteunen invoerparameters die via de volgende kenmerken worden gedefinieerd.  

| **Eigenschap** | **Beschrijving** |
|:--- |:--- |
| Type |Vereist. Het gegevenstype voor de waarde van parameter verwacht. Elk type .NET is ongeldig. |
| Naam |Vereist. De naam van de parameter. Dit moet uniek zijn binnen het runbook en kan bevatten alleen letters, cijfers of onderstrepingstekens bevatten. Moet beginnen met een letter. |
| Verplicht |Optioneel. Hiermee geeft u op of een waarde voor de parameter moet worden opgegeven. Als u dit instellen op **$true**, en vervolgens moet een waarde worden opgegeven wanneer het runbook wordt gestart. Als u dit instellen op **$false**, en vervolgens een waarde optioneel is. |
| Standaardwaarde |Optioneel.  Hiermee geeft u een waarde die voor de parameter worden gebruikt als een waarde niet is doorgegeven als het runbook wordt gestart. Een standaardwaarde zal kan worden ingesteld voor elke parameter en automatisch de parameter optioneel ongeacht de verplichte instelling. |

Windows PowerShell ondersteunt meer kenmerken van invoerparameters dan die hier worden vermeld, zoals validatie, aliassen en parametersets. Azure Automation ondersteunt momenteel echter alleen de invoerparameters die hierboven worden genoemd.

De parameterdefinitie van een in PowerShell Workflow-runbooks heeft de volgende algemene vorm waarin meerdere parameters zijn gescheiden door komma's.

   ```powershell
     Param
     (
         [Parameter (Mandatory= $true/$false)]
         [Type] Name1 = <Default value>,

         [Parameter (Mandatory= $true/$false)]
         [Type] Name2 = <Default value>
     )
   ```

> [!NOTE]
> Wanneer u parameters, als u geen opgeeft definieert het **verplichte** kenmerk, wordt standaard de parameter wordt beschouwd als optioneel. Ook als u een standaardwaarde voor een parameter in PowerShell Workflow-runbooks instelt, wordt deze verwerkt door PowerShell als een optionele parameter, ongeacht de **verplichte** kenmerkwaarde.
> 
> 

Als voorbeeld gaan we de invoerparameters configureren voor een PowerShell Workflow-runbook die uitvoer meer informatie over virtuele machines, één VM of alle virtuele machines binnen een resourcegroep. Dit runbook heeft twee parameters zoals aangegeven in de volgende schermafbeelding: de naam van de virtuele machine en de naam van de resourcegroep.

![Werkstroom voor automatisering van PowerShell](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

In de parameterdefinitie van deze, de parameters **$VMName** en **$resourceGroupName** eenvoudige parameters van het type tekenreeks zijn. PowerShell en PowerShell Workflow-runbooks ondersteunen echter alle eenvoudige typen en complexe typen, zoals **object** of **PSCredential** voor invoerparameters.

Als uw runbook een invoerparameter objecttype heeft, gebruikt u een PowerShell-hashtable met (naam, waarde) paren om door te geven in een waarde. Bijvoorbeeld, als u de volgende parameter in een runbook hebt:

     [Parameter (Mandatory = $true)]
     [object] $FullName

U kunt vervolgens de volgende waarde doorgeven aan de parameter:

    @{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}


## <a name="configure-input-parameters-in-graphical-runbooks"></a>De invoerparameters in grafische runbooks configureren

Naar [configureren van een grafisch runbook](automation-first-runbook-graphical.md) met invoerparameters we maken een grafisch runbook die meer informatie over virtuele machines, ofwel een enkele virtuele machine of alle virtuele machines binnen een resourcegroep. Configureren van een runbook bestaat uit twee belangrijkste activiteiten, zoals hieronder wordt beschreven.

[**Runbooks verifiëren met Azure uitvoeren als-account** ](automation-sec-configure-azure-runas-account.md) te verifiëren bij Azure.

[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) ophalen van de eigenschappen van een virtuele machines.

U kunt de [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) activiteit voor uitvoer van de namen van virtuele machines. De activiteit **Get-AzureRmVm** twee parameters accepteert de **virtuele-machinenaam** en de **Resourcegroepnaam**. Omdat deze parameters voor verschillende waarden telkens wanneer die u het runbook start vereist kunnen, kunt u invoerparameters toevoegen aan uw runbook. Hier volgen de stappen voor de invoerparameters toevoegen:

1. Selecteer het grafisch runbook vanuit de **Runbooks** blade en klik vervolgens op [ **bewerken** ](automation-graphical-authoring-intro.md) deze.
2. Klik in de runbookeditor op **invoer en uitvoer** openen de **invoer en uitvoer** blade.
   
    ![Grafisch runbook automatisering](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. De **invoer en uitvoer** blade geeft een lijst met invoerparameters die zijn gedefinieerd voor het runbook. Op deze blade kunt u een nieuwe invoerparameter toevoegen of bewerken van de configuratie van een bestaande invoerparameter. Klik op als u een nieuwe parameter voor het runbook **invoer toevoegen** openen de **runbookinvoerparameter** blade. Daar kunt u de volgende parameters:
   
   | **Eigenschap** | **Beschrijving** |
   |:--- |:--- |
   | Naam |Vereist.  De naam van de parameter. Dit moet uniek zijn binnen het runbook en kan bevatten alleen letters, cijfers of onderstrepingstekens bevatten. Moet beginnen met een letter. |
   | Beschrijving |Optioneel. Beschrijving van het doel van de invoerparameter. |
   | Type |Optioneel. Het gegevenstype dat voor de waarde van parameter wordt verwacht. De van de ondersteunde parametertypen zijn **tekenreeks**, **Int32**, **Int64**, **decimale**, **Booleaanse**, **DateTime**, en **Object**. Als een gegevenstype dat niet is ingeschakeld, wordt standaard **tekenreeks**. |
   | Verplicht |Optioneel. Hiermee geeft u op of een waarde voor de parameter moet worden opgegeven. Als u ervoor kiest **Ja**, en vervolgens moet een waarde worden opgegeven wanneer het runbook wordt gestart. Als u ervoor kiest **geen**, en vervolgens een waarde niet vereist is als het runbook wordt gestart en een standaardwaarde kan worden ingesteld. |
   | Standaardwaarde |Optioneel. Hiermee geeft u een waarde die voor de parameter worden gebruikt als een waarde niet is doorgegeven als het runbook wordt gestart. Een standaardwaarde kan worden ingesteld voor een parameter die is niet verplicht. Als u wilt een standaardwaarde instelt, kies **aangepaste**. Deze waarde wordt gebruikt, tenzij een andere waarde wordt opgegeven als het runbook wordt gestart. Kies **geen** als u niet wilt bieden een standaardwaarde. |
   
    ![Nieuwe invoer toevoegen](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. Twee parameters maken met de volgende eigenschappen die worden gebruikt door de **Get-AzureRmVm** activiteit:
   
   * **Parameter1:**
     
     * Naam - VMName
     * Type - tekenreeks
     * Verplicht - Nee
   * **Parameter2:**
     
     * Naam - resourceGroupName
     * Type - tekenreeks
     * Verplicht - Nee
     * Standaardwaarde - aangepast
     * Aangepaste standaardwaarde - \<naam van de resourcegroep met de virtuele machines >
5. Nadat u de parameters toevoegt, klikt u op **OK**.  U kunt nu deze bekijken in de **invoer en uitvoer blade**. Klik op **OK** opnieuw, en klik vervolgens op **opslaan** en **publiceren** uw runbook.

## <a name="configure-input-parameters-in-python-runbooks"></a>De invoerparameters in Python runbooks configureren

In tegenstelling tot PowerShell, PowerShell-werkstroom en grafische runbooks hebben Python-runbooks geen benoemde parameters.
Alle ingevoerde parameters worden geparseerd als een matrix met waarden van het argument.
U toegang tot de matrix door het importeren van de `sys` module in uw Python-script en klik vervolgens met behulp de `sys.argv` matrix.
Het is belangrijk te weten dat het eerste element van de matrix `sys.argv[0]`, is de naam van het script, zodat de eerste werkelijke invoerparameter `sys.argv[1]`.

Zie voor een voorbeeld van hoe u invoerparameters gebruikt in een runbook Python [Mijn eerste Python-runbook in Azure Automation](automation-first-runbook-textual-python2.md).

## <a name="assign-values-to-input-parameters-in-runbooks"></a>Geef waarden op de invoerparameters in runbooks

Waarden voor de invoerparameters in runbooks in de volgende scenario's kan worden doorgegeven.

### <a name="start-a-runbook-and-assign-parameters"></a>Een runbook Start en parameters toe te wijzen

Een runbook kan op veel verschillende manieren worden gestart: via de Azure portal, met een webhook, met PowerShell-cmdlets, met de REST-API of met de SDK. Hieronder besproken verschillende methoden voor een runbook starten en het toewijzen van parameters.

#### <a name="start-a-published-runbook-by-using-the-azure-portal-and-assign-parameters"></a>Een gepubliceerd runbook start met behulp van de Azure-portal en parameters toewijzen

Wanneer u [het runbook starten](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), wordt de **Runbook starten** blade wordt geopend en u kunt waarden opgeven voor de parameters die u zojuist hebt gemaakt.

![Aan de slag met de portal](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

In het label onder het invoervak ziet u de kenmerken die zijn ingesteld voor de parameter. Kenmerken zijn verplicht of optioneel, type en de standaardwaarde. In de help-ballon naast de parameternaam van de ziet u alle belangrijke gegevens die u moet nemen van beslissingen over invoer parameterwaarden. Deze informatie omvat of een parameter verplicht of optioneel is. Dit omvat ook het type en de standaardwaarde (indien aanwezig) en andere nuttige opmerkingen.

![Ballon met Help](media/automation-runbook-input-parameters/automation-05-helpbaloon.png)

> [!NOTE]
> Ondersteuning voor parameters van type String **leeg** tekenreekswaarden.  Invoeren **[EmptyString]** in de invoerparameter vak een lege tekenreeks wordt doorgegeven aan de parameter. Ook de parameters van het type tekenreeks niet ondersteunen **Null** waarden worden doorgegeven. Als u niet de waarde van een aan de tekenreeksparameter doorgeeft, wordt vervolgens PowerShell geïnterpreteerd als null.
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a>Een gepubliceerd runbook start met behulp van PowerShell-cmdlets en parameters toewijzen

* **Azure Resource Manager-cmdlets:** kunt u beginnen met een Automation-runbook is gemaakt in een resourcegroep met [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).
  
  **Voorbeeld:**
  
  ```
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* **Azure Service Management-cmdlets:** kunt u beginnen met een automation-runbook dat is gemaakt in een standaardresourcegroep met behulp van [Start AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).
  
  **Voorbeeld:**
  
  ```
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> Wanneer u een runbook start met behulp van PowerShell-cmdlets, een standaardparameter **MicrosoftApplicationManagementStartedBy** wordt gemaakt met de waarde **PowerShell**. U vindt deze parameter in de **taakgegevens** blade.  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a>Een runbook start met behulp van een SDK en parameters toe te wijzen

* **Azure Resource Manager-methode:** kunt u een runbook start met behulp van de SDK van een programmeertaal. Hieronder ziet u een C#-codefragment voor het starten van een runbook in uw Automation-account. U vindt de code aan onze [GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).  
  
  ```
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
* **Azure Service Management-methode:** kunt u een runbook start met behulp van de SDK van een programmeertaal. Hieronder ziet u een C#-codefragment voor het starten van een runbook in uw Automation-account. U vindt de code aan onze [GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).
  
  ```      
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
  
  Deze methode starten, een woordenlijst voor het opslaan van de runbookparameters, maakt **VMName** en **resourceGroupName**, en de bijbehorende waarden. Start het runbook. Hieronder vindt u de C#-codefragment voor het aanroepen van de methode die hierboven gedefinieerd.
  
  ```
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call the StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-the-rest-api-and-assign-parameters"></a>Een runbook start met behulp van de REST-API en parameters toe te wijzen
Een runbooktaak worden gemaakt en gestart met de REST-API van Azure Automation met behulp van de **plaatsen** methode met de volgende aanvraag-URI.

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

In de aanvraag-URI, vervangt u de volgende parameters:

* **abonnement-id:** uw Azure-abonnement-ID.  
* **cloud-service-naam:** de naam van de cloud service aan de aanvraag moet worden verzonden.  
* **Automation-account-name:** de naam van uw automation-account die wordt gehost binnen de opgegeven cloudservice.  
* **taak-id:** de GUID voor de taak. GUID's in PowerShell kunnen worden gemaakt met behulp van de **[GUID]::NewGuid(). ToString()** opdracht.

Gebruiken om de parameters doorgeven aan de runbooktaak, de aanvraagtekst. Het duurt de volgende twee eigenschappen in het JSON-indeling:

* **Runbooknaam:** vereist. De naam van het runbook voor de taak is gestart.  
* **Runbook-parameters:** optioneel. Een dictionary van de lijst met parameters in (naam, waarde) waar de naam moet van het type tekenreeks en de waarde is een geldig JSON-waarde opmaken.

Als u wilt beginnen de **Get-AzureVMTextual** runbook dat eerder is gemaakt met **VMName** en **resourceGroupName** als parameters, gebruikt u de volgende JSON-indeling voor de aanvraagtekst.

   ```
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WSVMClassic",
         "resourceGroupName":”WSCS1”}
        }
    }
   ```

Een HTTP-statuscode 201 wordt geretourneerd als de taak is gemaakt. Raadpleeg voor meer informatie over antwoordheaders en de antwoordtekst naar het artikel over het [een runbooktaak maken met behulp van de REST-API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)

### <a name="test-a-runbook-and-assign-parameters"></a>Een runbook testen en parameters toewijzen
Wanneer u [de conceptversie van uw runbook testen](automation-testing-runbook.md) met de optie test de **testen** blade wordt geopend en u kunt waarden configureren voor de parameters die u zojuist hebt gemaakt.

![Testen en parameters toewijzen](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a>Een planning aan een runbook koppelen en parameters toe te wijzen
U kunt [een planning koppelen](automation-schedules.md) aan uw runbook zodat het runbook wordt gestart op een bepaald tijdstip. U toewijzen invoerparameters wanneer u de planning maakt en het runbook deze waarden worden gebruikt wanneer deze is gestart volgens de planning. U kunt de planning niet opslaan, totdat alle verplichte parameterwaarden zijn opgegeven.

![Plannen en parameters toewijzen](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>Een webhook voor een runbook maken en toewijzen van parameters
Kunt u een [webhook](automation-webhooks.md) voor uw runbook en invoerparameters van runbook configureren. U kunt de webhook niet opslaan, totdat alle verplichte parameterwaarden zijn opgegeven.

![Webhook maken en toewijzen van parameters](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

Bij het uitvoeren van een runbook met behulp van een webhook, de vooraf gedefinieerde invoerparameter  **[Webhookdata](automation-webhooks.md#details-of-a-webhook)**  wordt verzonden, samen met de invoerparameters die u hebt gedefinieerd. U kunt klikken op om uit te breiden de **WebhookData** parameter voor meer informatie.

![WebhookData-parameter](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over runbookinvoer en uitvoer [Azure Automation: runbook invoer, uitvoer en geneste runbooks](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).
* Zie voor meer informatie over de verschillende manieren om een runbook te starten, [een runbook starten](automation-starting-a-runbook.md).
* Raadpleeg tekstueel runbook bewerken, [bewerken tekstuele runbooks](automation-edit-textual-runbook.md).
* Raadpleeg een grafisch runbook bewerken, [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).

