---
title: Verbindingsassets in Azure Automation
description: Verbindingsassets in Azure Automation bevat de vereiste informatie op verbinding maken met een externe service of toepassing vanuit een runbook of DSC-configuratie. In dit artikel wordt uitgelegd dat de details van verbindingen en hoe u werkt met hen in tekstuele en grafische ontwerpen.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 01/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e00eb5756d34c7ca8cecc741b4832c583a6ed087
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54439014"
---
# <a name="connection-assets-in-azure-automation"></a>Verbindingsassets in Azure Automation

Een Automation-verbindingsasset bevat de vereiste informatie op verbinding maken met een externe service of toepassing vanuit een runbook of DSC-configuratie. Dit kan ook gegevens die vereist zijn voor verificatie, zoals een gebruikersnaam en wachtwoord, naast verbindingsinformatie, zoals een URL of een poort. De waarde van een verbinding is het houden van alle eigenschappen voor het verbinden met een bepaalde toepassing in een activum in plaats van het maken van meerdere variabelen. De gebruiker de waarden voor een verbinding op één plek kunt bewerken en u kunt de naam van een verbinding doorgeven aan een runbook of DSC-configuratie in één parameter. De eigenschappen van een verbinding kunnen worden geopend in het runbook of DSC-configuratie met de **Get-AutomationConnection** activiteit. 

Wanneer u een verbinding maakt, moet u een *verbindingstype*. Het verbindingstype is een sjabloon die een set eigenschappen definieert. De verbinding definieert de waarden voor elke eigenschap die is gedefinieerd in het verbindingstype. Verbindingstypen worden toegevoegd aan Azure Automation in integratiemodules of die zijn gemaakt met de [API van Azure Automation](https://msdn.microsoft.com/library/azure/mt163818.aspx) als de integratiemodule een verbindingstype bevat en wordt geïmporteerd in uw Automation-account. Anders moet u een bestand met metagegevens om op te geven van een Automation-verbindingstype te maken.  Zie voor meer informatie over dit [integratiemodules](automation-integration-modules.md).  

>[!NOTE]
>Beveiligde activa in Azure Automation zijn referenties, certificaten, verbindingen en gecodeerde variabelen. Deze apparaten worden versleuteld en opgeslagen in Azure Automation met behulp van een unieke sleutel die wordt gegenereerd voor elk automation-account. Deze sleutel wordt opgeslagen in een systeem beheerd Key Vault. Voordat u een beveiligd bedrijfsmiddel opslaat, is de sleutel geladen uit Key Vault en vervolgens worden gebruikt voor het versleutelen van de asset. Dit proces wordt beheerd door Azure Automation.

## <a name="connection-types"></a>Verbindingstypen

Er zijn drie typen van ingebouwde verbindingen beschikbaar in Azure Automation:

* **Azure** -deze verbinding kan worden gebruikt om klassieke resources te beheren.
* **AzureClassicCertificate** -deze verbinding wordt gebruikt door de **AzureClassicRunAs** account.
* **AzureServicePrincipal** -deze verbinding wordt gebruikt door de **AzureRunAs** account.

In de meeste gevallen niet hoeft te maken van een verbindingsresource wanneer deze is gemaakt bij het maken van een [RunAs-account](manage-runas-account.md).

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

De cmdlets in de volgende tabel worden gebruikt voor het maken en beheren van Automation-verbindingen met Windows PowerShell. Ze geleverd als onderdeel van de [Azure PowerShell-module](/powershell/azure/overview) die beschikbaar is voor gebruik in Automation-runbooks en DSC-configuraties.

|Cmdlet|Description|
|:---|:---|
|[Get-AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection)|Hiermee haalt u een verbinding. Bevat een hash-tabel met de waarden van de velden van de verbinding.|
|[New-AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection)|Hiermee maakt u een nieuwe verbinding.|
|[Remove-AzureRmAutomationConnection](/powershell/module/azurerm.automation/remove-azurermautomationconnection)|Verwijder een bestaande verbinding.|
|[Set-AzureRmAutomationConnectionFieldValue](/powershell/module/azurerm.automation/set-azurermautomationconnectionfieldvalue)|Hiermee stelt u de waarde van een bepaald veld voor een bestaande verbinding.|

## <a name="activities"></a>Activiteiten

De activiteiten in de volgende tabel worden gebruikt voor toegang tot verbindingen in een runbook of DSC-configuratie.

|Activiteiten|Description|
|---|---|
|[Get-AutomationConnection](/powershell/module/servicemanagement/azure/get-azureautomationconnection?view=azuresmps-3.7.0)|Hiermee haalt u een verbinding moet worden gebruikt. Retourneert een hashtabel met de eigenschappen van de verbinding.|

>[!NOTE] 
>Vermijd het gebruik van variabelen met de parameter-Name van **Get-AutomationConnection** omdat dit detecteren van afhankelijkheden tussen runbooks of DSC-configuraties en verbindingsassets tijdens de ontwerpfase bemoeilijken kan.

 
## <a name="python2-functions"></a>Python2-functies 
De functie in de volgende tabel wordt gebruikt voor toegang tot verbindingen in een Python2-runbook. 

| Function | Description | 
|:---|:---| 
| automationassets.get_automation_connection | Hiermee haalt u een verbinding. Retourneert een woordenlijst met de eigenschappen van de verbinding. | 

> [!NOTE] 
> U moet de module 'automationassets' aan de bovenkant van uw Python-runbook importeren om toegang tot de functies van de asset.

## <a name="creating-a-new-connection"></a>Het maken van een nieuwe verbinding

### <a name="to-create-a-new-connection-with-the-azure-portal"></a>Een nieuwe verbinding maken met de Azure-portal

1. Via uw automation-account, klikt u op de **activa** onderdeel om te openen de **activa** blade.
2. Klik op de **verbindingen** onderdeel om te openen de **verbindingen** blade.
3. Klik op **een verbinding toevoegen** aan de bovenkant van de blade.
4. In de **Type** vervolgkeuzelijst, selecteer het type verbinding die u wilt maken. Het formulier worden de eigenschappen voor dat type aanwezig.
5. Vul het formulier in en klikt u op **maken** om op te slaan van de nieuwe verbinding.

### <a name="to-create-a-new-connection-with-windows-powershell"></a>Een nieuwe verbinding maken met Windows PowerShell

Een nieuwe verbinding maken met Windows PowerShell met behulp van de [New-AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection) cmdlet. Deze cmdlet heeft een parameter met de naam **ConnectionFieldValues** die wordt verwacht dat een [hash-tabel](https://technet.microsoft.com/library/hh847780.aspx) waarden voor elk van de eigenschappen die zijn gedefinieerd door het verbindingstype definiëren.

Als u bekend met de automatisering bent [uitvoeren als-account](automation-sec-configure-azure-runas-account.md) om te verifiëren met behulp van de service-principal runbooks, de PowerShell-script, opgegeven als een alternatief voor het uitvoeren als-account maken vanuit de portal, maakt u een nieuwe verbinding de Asset met behulp van de volgende voorbeeldopdrachten.  

```powershell
$ConnectionAssetName = "AzureRunAsConnection"
$ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
```

U kunt het script gebruiken om de verbindingsasset niet maken omdat wanneer u uw Automation-account maakt, deze automatisch verschillende algemene modules standaard samen met het verbindingstype omvat zijn **AzureServicePrincipal** aan Maak de **AzureRunAsConnection** verbindingstype-asset.  Dit is belangrijk rekening moet houden, omdat het als u probeert te maken van een nieuwe verbindingsasset verbinding maken met een service of toepassing met een andere verificatiemethode, dit mislukt omdat het verbindingstype is niet gedefinieerd in uw Automation-account.  Voor meer informatie over het maken van uw eigen verbindingstype voor uw aangepaste of de module op basis van de [PowerShell Gallery](https://www.powershellgallery.com), Zie [integratiemodules](automation-integration-modules.md)
  
## <a name="using-a-connection-in-a-runbook-or-dsc-configuration"></a>Met behulp van een verbinding in een runbook of DSC-configuratie

Ophalen van een verbinding in een runbook of DSC-configuratie met de **Get-AutomationConnection** cmdlet.  U kunt geen gebruiken de [Get-AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection) activiteit.  Deze activiteit haalt de waarden van de andere velden in de verbinding en retourneert ze als een [hash-tabel](https://go.microsoft.com/fwlink/?LinkID=324844) die vervolgens kan worden gebruikt met de juiste opdrachten in het runbook of DSC-configuratie.

### <a name="textual-runbook-sample"></a>Tekstuele runbook-voorbeeld

De volgende voorbeeldopdrachten laten zien hoe de uitvoeren als-account eerder aangegeven, om te verifiëren met Azure Resource Manager-resources in uw runbook gebruiken.  Het verbindingsasset dat het Run As-account, die verwijst naar het certificaat op basis van service-principal, niet de referenties die wordt gebruikt.  

```powershell
$Conn = Get-AutomationConnection -Name AzureRunAsConnection 
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
```

> [!IMPORTANT]
> **Add-AzureRmAccount** is nu een alias voor **Connect-AzureRMAccount**. Wanneer uw bibliotheek zoeken items, als u niet ziet **Connect-AzureRMAccount**, kunt u **Add-AzureRmAccount**, of u kunt uw modules bijwerken in uw Automation-Account.

### <a name="graphical-runbook-samples"></a>Voorbeelden van een grafisch runbook

U wilt toevoegen een **Get-AutomationConnection** activiteit aan een grafisch runbook door te selecteren met de rechtermuisknop op de verbinding in het deelvenster bibliotheek van de grafische editor **toevoegen aan papier**.

![Toevoegen aan papier](media/automation-connections/connection-add-canvas.png)

De volgende afbeelding toont een voorbeeld van het gebruik van een verbinding in een grafisch runbook.  Dit is hetzelfde voorbeeld hierboven voor de verificatie met behulp van de uitvoeren als-account met een tekstuele runbook.  In dit voorbeeld wordt de **constante waarde** -gegevensset naar de **uitvoeren als-verbinding ophalen** activiteit die een verbindingsobject voor verificatie gebruikt.  Een [pijplijn koppeling](automation-graphical-authoring-intro.md#links-and-workflow) wordt gebruikt, omdat de parameterset ServicePrincipalCertificate één object verwacht.

![verbindingen ophalen](media/automation-connections/automation-get-connection-object.png)

### <a name="python2-runbook-sample"></a>Voorbeeld van Python2-runbook
Het volgende voorbeeld toont hoe u verifieert met behulp van de uitvoeren als-verbinding in een Python2-runbook.

```python
""" Tutorial to show how to authenticate against Azure resource manager resources """
import azure.mgmt.resource
import automationassets

def get_automation_runas_credential(runas_connection):
  """ Returns credentials to authenticate against Azure resoruce manager """
  from OpenSSL import crypto
  from msrestazure import azure_active_directory
  import adal

  # Get the Azure Automation Run As service principal certificate
  cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
  pks12_cert = crypto.load_pkcs12(cert)
  pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM, pks12_cert.get_privatekey())

  # Get Run As connection information for the Azure Automation service principal
  application_id = runas_connection["ApplicationId"]
  thumbprint = runas_connection["CertificateThumbprint"]
  tenant_id = runas_connection["TenantId"]

  # Authenticate with service principal certificate
  resource = "https://management.core.windows.net/"
  authority_url = ("https://login.microsoftonline.com/" + tenant_id)
  context = adal.AuthenticationContext(authority_url)
  return azure_active_directory.AdalAuthentication(
    lambda: context.acquire_token_with_client_certificate(
      resource,
      application_id,
      pem_pkey,
      thumbprint)
  )

# Authenticate to Azure using the Azure Automation Run As service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential(runas_connection)
```

## <a name="next-steps"></a>Volgende stappen

- Beoordeling [koppelingen in het grafisch ontwerpen](automation-graphical-authoring-intro.md#links-and-workflow) om te begrijpen hoe u directe en de Controlestroom van logica in uw runbooks.  

- Zie voor meer informatie over het gebruik van Azure Automation van PowerShell-modules en aanbevolen procedures voor het maken van uw eigen PowerShell-modules om te werken als integratiemodules binnen Azure Automation, [integratiemodules](automation-integration-modules.md).  

