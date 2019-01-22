---
title: Certificaatassets in Azure Automation
description: Certificaten kunnen veilig worden opgeslagen in Azure Automation zodat ze kunnen worden geopend door runbooks of DSC-configuraties voor verificatie op basis van Azure en resources van derden.  In dit artikel wordt uitgelegd dat de details van certificaten en hoe u werkt met hen in tekstuele en grafische ontwerpen.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 01/15/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 714bf5f915cb588a99552a8bbbc5ffa76aa16846
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54426128"
---
# <a name="certificate-assets-in-azure-automation"></a>Certificaatassets in Azure Automation

Certificaten kunnen worden veilig opgeslagen in Azure Automation zodat ze kunnen worden geopend door runbooks of DSC-configuraties met behulp van de **Get-AzureRmAutomationCertificate** activiteit voor Azure Resource Manager-resources. Deze mogelijkheid kunt u runbooks en DSC-configuraties die gebruikmaken van certificaten voor verificatie maken of toegevoegd aan Azure of externe resources.

>[!NOTE]
>Beveiligde activa in Azure Automation zijn referenties, certificaten, verbindingen en gecodeerde variabelen. Deze apparaten worden versleuteld en opgeslagen in Azure Automation met behulp van een unieke sleutel die wordt gegenereerd voor elk automation-account. Deze sleutel wordt opgeslagen in een systeem beheerd Key Vault. Voordat u een beveiligd bedrijfsmiddel opslaat, is de sleutel geladen uit Key Vault en vervolgens worden gebruikt voor het versleutelen van de asset. Dit proces wordt beheerd door Azure Automation.

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell-cmdlets
De cmdlets in de volgende tabel worden gebruikt voor AzureRM, maken en beheren van automation-referentie-assets met Windows PowerShell. Ze geleverd als onderdeel van de [AzureRM.Automation module](/powershell/azure/overview) die beschikbaar is voor gebruik in Automation-runbooks en DSC-configuraties.

|Cmdlets|Description|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate)|Haalt informatie op over een certificaat wilt gebruiken in een runbook of DSC-configuratie. U kunt alleen het certificaat zelf ophalen van Get-AutomationCertificate activiteit.|
|[New-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationcertificate)|Hiermee maakt u een nieuw certificaat in Azure Automation.|
[Remove-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationcertificate)|Hiermee verwijdert u een certificaat van Azure Automation.|Hiermee maakt u een nieuw certificaat in Azure Automation.
|[Set-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationcertificate)|Hiermee stelt u de eigenschappen voor een bestaand certificaat, met inbegrip van het certificaatbestand uploaden en het instellen van het wachtwoord voor een pfx-bestand.|
|[Add-AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Hiermee wordt een certificaat voor de opgegeven cloudservice geüpload.|

## <a name="activities"></a>Activiteiten
De activiteiten in de volgende tabel worden gebruikt voor toegang tot de certificaten in een runbook en DSC-configuraties.

| Activiteiten | Description |
|:---|:---|
|Get-AutomationCertificate|Hiermee haalt u een certificaat wilt gebruiken in een runbook of DSC-configuratie. Retourneert een [System.Security.Cryptography.X509Certificates.X509Certificate2](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate2.aspx) object.|

> [!NOTE] 
> Vermijd het gebruik van variabelen in de parameter-Name van **Get-AutomationCertificate** in een runbook of DSC-configuratie omdat deze ingewikkelder, detecteren van afhankelijkheden tussen runbooks of DSC-configuratie en Automation-variabelen tijdens de ontwerpfase.

## <a name="python2-functions"></a>Python2-functies

De functie in de volgende tabel wordt gebruikt voor toegang tot de certificaten in een Python2-runbook.

| Function | Description |
|:---|:---|
| automationassets.get_automation_certificate | Informatie over een certificaatasset opgehaald. |

> [!NOTE]
> U moet importeren de **automationassets** module in het begin van de Python-runbook voor toegang tot de functies van de asset.

## <a name="creating-a-new-certificate"></a>Een nieuw certificaat maken

Wanneer u een nieuw certificaat maakt, kunt u een cer- of pfx-bestand uploaden naar Azure Automation. Als u het certificaat als exporteerbaar markeren, kunt klikt u vervolgens u overbrengen deze uit het certificaatarchief van de Azure Automation. Als deze niet kan worden geëxporteerd, kan klikt u vervolgens deze alleen worden gebruikt voor het ondertekenen van binnen het runbook of DSC-configuratie. Het certificaat om de provider is vereist voor Azure Automation: **Microsoft Enhanced RSA en AES Cryptographic Provider**.

### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Een nieuw certificaat maken met de Azure-portal

1. Via uw Automation-account, klikt u op de **activa** tegel om te openen de **activa** blade.
1. Klik op de **certificaten** tegel om te openen de **certificaten** blade.
1. Klik op **toevoegen van een certificaat** aan de bovenkant van de blade.
1. Typ een naam voor het certificaat in de **naam** vak.
1. Als u wilt bladeren naar een cer- of pfx-bestand, klikt u op **selecteert u een bestand** onder **een certificaatbestand uploaden**. Als u een pfx-bestand selecteert, geeft u een wachtwoord en of het is toegestaan om te worden geëxporteerd.
1. Klik op **maken** om op te slaan van het nieuwe certificaatasset.

### <a name="to-create-a-new-certificate-with-windows-powershell"></a>Een nieuw certificaat maken met Windows PowerShell

Het volgende voorbeeld ziet u hoe u een nieuw Automation-certificaat maken en deze kan worden geëxporteerd. Een bestaand pfx-bestand wordt geïmporteerd.

```powershell-interactive
$certName = 'MyCertificate'
$certPath = '.\MyCert.pfx'
$certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup
```

## <a name="using-a-certificate"></a>Met behulp van een certificaat

Als u een certificaat, gebruikt de **Get-AutomationCertificate** activiteit. U kunt geen gebruiken de [Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate?view=azurermps-6.6.0) cmdlet omdat deze informatie over het certificaatasset, maar niet het certificaat zelf retourneert.

### <a name="textual-runbook-sample"></a>Tekstuele runbook-voorbeeld

De volgende voorbeeldcode laat zien hoe u een certificaat toevoegen aan een cloudservice in een runbook. In dit voorbeeld wordt wordt het wachtwoord opgehaald uit een versleutelde automation-variabele.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-sample"></a>Voorbeeld van een grafisch runbook

U wilt toevoegen een **Get-AutomationCertificate** aan een grafisch runbook door te selecteren met de rechtermuisknop op het certificaat in het deelvenster bibliotheek van de grafische editor **toevoegen aan papier**.

![Certificaat toevoegen aan het canvas](media/automation-certificates/automation-certificate-add-to-canvas.png)

De volgende afbeelding toont een voorbeeld van het gebruik van een certificaat in een grafisch runbook. Dit is hetzelfde als in het voorgaande voorbeeld voor het toevoegen van een certificaat aan een cloudservice vanuit een tekstuele runbook.

![Voorbeeld van grafisch ontwerpen ](media/automation-certificates/graphical-runbook-add-certificate.png)

### <a name="python2-sample"></a>Python2-voorbeeld
Het volgende voorbeeld laat zien hoe toegang tot de certificaten in Python2-runbooks.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert 
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het werken met koppelingen voor het beheren van de logische stroom van uw runbook is ontworpen om uit te voeren activiteiten, [koppelingen in het grafisch ontwerpen](automation-graphical-authoring-intro.md#links-and-workflow). 

