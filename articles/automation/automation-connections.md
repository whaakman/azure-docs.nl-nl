---
title: Verbindingsassets in Azure Automation | Microsoft Docs
description: Verbindingsassets in Azure Automation bevatten de benodigde informatie om te verbinden met een externe service of toepassing vanuit een runbook of de DSC-configuratie. Dit artikel wordt uitgelegd dat de details van verbindingen en hoe u ermee in tekstvorm en grafisch ontwerpen.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: f0239017-5c66-4165-8cca-5dcb249b8091
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/13/2017
ms.author: magoedte; bwren
ms.openlocfilehash: c1e56f00e46dc3d04f6ac3bb42df6c1935c5c8b0
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="connection-assets-in-azure-automation"></a>Verbindingsassets in Azure Automation

Een Automation-verbindingsasset bevat de benodigde informatie om te verbinden met een externe service of toepassing vanuit een runbook of de DSC-configuratie. Dit kan de vereiste informatie voor verificatie, zoals een gebruikersnaam en wachtwoord naast verbindingsinformatie zoals een URL of een poort omvatten. De waarde van een verbinding is het houden van alle eigenschappen voor het verbinden met een bepaalde toepassing in een actief in plaats van meerdere variabelen maken. De gebruiker kan de waarden voor een verbinding op één plek bewerken en kunt u de naam van een verbinding doorgeven aan een runbook of de DSC-configuratie in een enkele parameter. De eigenschappen van een verbinding kunnen worden geopend in het runbook of de DSC-configuratie met de **Get-AutomationConnection** activiteit. 

Wanneer u een verbinding maakt, moet u een *verbindingstype*. Het verbindingstype is een sjabloon die een set eigenschappen definieert. Waarden voor elke eigenschap die is gedefinieerd in het bijbehorende verbindingstype Hiermee definieert u de verbinding. Verbindingstypen worden toegevoegd aan Azure Automation in integratiemodules of gemaakt met de [Azure Automation-API](http://msdn.microsoft.com/library/azure/mt163818.aspx) als de integratiemodule een verbindingstype bevat en is geïmporteerd in uw Automation-account. Anders, moet u voor het maken van een bestand met metagegevens om op te geven van een Automation-verbindingstype.  Zie voor meer informatie over dit [integratiemodules](automation-integration-modules.md).  

>[!NOTE] 
>Beveiligde activa in Azure Automation zijn referenties, certificaten, verbindingen en gecodeerde variabelen. Deze activa zijn versleuteld en opgeslagen in de Azure Automation met een unieke sleutel die wordt gegenereerd voor elk automation-account. Deze sleutel is versleuteld met een basiscertificaat en opgeslagen in Azure Automation. Voordat u een beveiligd bedrijfsmiddel op te slaan, wordt de sleutel voor het automation-account worden ontsleuteld met het basiscertificaat en vervolgens worden gebruikt voor het versleutelen van de asset.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

De cmdlets in de volgende tabel worden gebruikt voor het maken en beheren van Automation-verbindingen met Windows PowerShell. Ze worden verzonden als onderdeel van de [Azure PowerShell-module](/powershell/azure/overview) die beschikbaar is voor gebruik in Automation-runbooks en DSC-configuraties.

|Cmdlet|Beschrijving|
|:---|:---|
|[Get-AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection)|Hiermee haalt u een verbinding. Bevat een hashtabel met de waarden van de velden van de verbinding.|
|[Nieuwe AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection)|Maakt een nieuwe verbinding.|
|[Verwijder AzureRmAutomationConnection](/powershell/module/azurerm.automation/remove-azurermautomationconnection)|Verwijder een bestaande verbinding.|
|[Set-AzureRmAutomationConnectionFieldValue](/powershell/module/azurerm.automation/set-azurermautomationconnectionfieldvalue)|Hiermee stelt u de waarde van een bepaald veld voor een bestaande verbinding.|

## <a name="activities"></a>Activiteiten

De activiteiten in de volgende tabel worden gebruikt voor toegang tot verbindingen in een runbook of de DSC-configuratie.

|Activiteiten|Beschrijving|
|---|---|
|[Get-AutomationConnection](/powershell/module/azure/get-azureautomationconnection?view=azuresmps-3.7.0)|Hiermee haalt u een verbinding te gebruiken. Retourneert een hashtabel met de eigenschappen van de verbinding.|

>[!NOTE] 
>Vermijd het gebruik van variabelen met de parameter-Name van **Get - AutomationConnection** omdat dit detecteren van afhankelijkheden tussen runbooks of DSC-configuraties en verbindingsassets in de ontwerpfase kan bemoeilijken.

 
## <a name="python2-functions"></a>Python2 functies 
De functie in de volgende tabel wordt gebruikt voor toegang tot verbindingen in een runbook Python2. 

| Functie | Beschrijving | 
|:---|:---| 
| automationassets.get_automation_connection | Hiermee haalt u een verbinding. Retourneert een woordenlijst met de eigenschappen van de verbinding. | 

> [!NOTE] 
> U moet de module 'automationassets' boven aan uw runbook Python importeren om de toegang tot de functies van de asset.

## <a name="creating-a-new-connection"></a>Een nieuwe verbinding maken

### <a name="to-create-a-new-connection-with-the-azure-portal"></a>Een nieuwe verbinding maken met de Azure-portal

1. Van uw automation-account, klikt u op de **activa** onderdeel openen de **activa** blade.
2. Klik op de **verbindingen** onderdeel openen de **verbindingen** blade.
3. Klik op **een verbinding toevoegen** boven aan de blade.
4. In de **Type** vervolgkeuzelijst, selecteer het type verbinding die u wilt maken. Het formulier worden de eigenschappen voor dat type aanwezig.
5. Vul het formulier en klik op **maken** om op te slaan van de nieuwe verbinding.

### <a name="to-create-a-new-connection-with-windows-powershell"></a>Een nieuwe verbinding maken met Windows PowerShell

Een nieuwe verbinding maken met Windows PowerShell de [nieuw AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection) cmdlet. Deze cmdlet heeft een parameter genaamd **ConnectionFieldValues** die verwacht een [hash-tabel](http://technet.microsoft.com/library/hh847780.aspx) waarden voor elk van de eigenschappen gedefinieerd door het verbindingstype definiëren.

Als u bekend met de automatisering bent [Run As-account](automation-sec-configure-azure-runas-account.md) om te verifiëren met behulp van de service-principal runbooks, de PowerShell-script, opgegeven als een alternatief voor het uitvoeren als-account maken vanuit de portal een nieuwe verbinding maken de Asset met behulp van de volgende voorbeeldopdrachten.  

```powershell
$ConnectionAssetName = "AzureRunAsConnection"
$ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues 
```

Kunt u het script te maken van de verbindingsasset omdat wanneer u uw Automation-account maakt, wordt automatisch meerdere globale modules standaard samen met het verbindingstype bevat **AzurServicePrincipal** maken de **AzureRunAsConnection** verbindingstype-asset.  Dit is belangrijk rekening te houden, als u probeert te maken van een nieuwe verbindingsasset verbinding maken met een service of toepassing met een andere verificatiemethode, mislukken zal omdat het verbindingstype is niet gedefinieerd in uw Automation-account.  Voor meer informatie over het maken van uw eigen verbindingstype voor uw aangepaste of de module op basis van de [PowerShell Gallery](https://www.powershellgallery.com), Zie [integratiemodules](automation-integration-modules.md)
  
## <a name="using-a-connection-in-a-runbook-or-dsc-configuration"></a>Met behulp van een verbinding in een runbook of de DSC-configuratie

Ophalen van een verbinding in een runbook of de DSC-configuratie met de **Get-AutomationConnection** cmdlet.  U kunt geen gebruiken de [Get-AzureRmAutomationConnection](https://docs.microsoft.com/powershell/resourcemanager/azurerm.automation/v1.0.12/Get-AzureRmAutomationConnection?redirectedfrom=msdn) activiteit.  Deze activiteit haalt de waarden van de verschillende velden in de verbinding en retourneert ze als een [hash-tabel](http://go.microsoft.com/fwlink/?LinkID=324844) die vervolgens kan worden gebruikt met de juiste opdrachten in het runbook of de DSC-configuratie.

### <a name="textual-runbook-sample"></a>Voorbeeld van tekstueel runbook

De volgende voorbeeldopdrachten laten zien hoe de Run As-account eerder voor verificatie met Azure Resource Manager-resources in uw runbook gebruiken.  Dit maakt gebruik van de verbindingsasset die de Run As-account, die verwijst naar de service principal op basis van certificaten, geen referenties vertegenwoordigt.  

```powershell
$Conn = Get-AutomationConnection -Name AzureRunAsConnection 
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint 
```

### <a name="graphical-runbook-samples"></a>Grafische runbook-voorbeelden

U toevoegen een **Get-AutomationConnection** activiteit aan een grafisch runbook door met de rechtermuisknop op de verbinding in het deelvenster bibliotheek met de grafische editor en **toevoegen aan papier**.

![](media/automation-connections/connection-add-canvas.png)

De volgende afbeelding toont een voorbeeld van het gebruik van een verbinding in een grafisch runbook.  Dit is het hetzelfde voorbeeld hierboven weergegeven voor het verifiëren van het Run As-account gebruiken met tekstueel runbook.  In dit voorbeeld wordt de **constante waarde** gegevensset voor de **RunAs-verbinding ophalen** activiteit die een verbindingsobject voor verificatie gebruikt.  Een [pijplijnkoppeling](automation-graphical-authoring-intro.md#links-and-workflow) wordt hier gebruikt omdat de parameterset ServicePrincipalCertificate een enkel object verwacht.

![](media/automation-connections/automation-get-connection-object.png)

### <a name="python2-runbook-sample"></a>Voorbeeld van Python2 runbook
Het volgende voorbeeld laat zien hoe verifiëren met behulp van de uitvoeren als-verbinding in een runbook Python2.

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

- Bekijk [koppelingen in het grafisch ontwerpen](automation-graphical-authoring-intro.md#links-and-workflow) om te begrijpen hoe rechtstreekse en beheren van de stroom van de logica in uw runbooks.  

- Zie voor meer informatie over het gebruik van Azure Automation van PowerShell-modules en aanbevolen procedures voor het maken van uw eigen PowerShell-modules werken als integratiemodules binnen Azure Automation, [integratiemodules](automation-integration-modules.md).  
