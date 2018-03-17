---
title: Certificaatassets in Azure Automation
description: Certificaten kunnen veilig worden opgeslagen in Azure Automation zodat ze toegankelijk zijn voor runbooks of DSC-configuraties worden geverifieerd bij Azure en resources van derden.  Dit artikel wordt uitgelegd dat de details van certificaten en het werken met hen in tekstvorm en grafisch ontwerpen.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.devlang: na
ms.tgt_pltfrm: na
ms.openlocfilehash: d4e205365b884b683928e42d538c085c4df2d6ed
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="certificate-assets-in-azure-automation"></a>Certificaatassets in Azure Automation

Certificaten kunnen worden veilig opgeslagen in Azure Automation zodat ze toegankelijk zijn voor runbooks of DSC-configuraties met behulp van de **Get-AzureRmAutomationCertificate** activiteit voor Azure Resource Manager-resources. Deze mogelijkheid kunt u runbooks en DSC-configuraties die certificaten voor verificatie gebruiken maken of worden deze toegevoegd aan Azure of resources van derden.

>[!NOTE]
>Beveiligde activa in Azure Automation zijn referenties, certificaten, verbindingen en gecodeerde variabelen. Deze activa zijn versleuteld en opgeslagen in Azure Automation, met een unieke sleutel die wordt gegenereerd voor elk automation-account. Deze sleutel wordt opgeslagen in de Sleutelkluis. De sleutel is voordat het opslaan van een beveiligd bedrijfsmiddel is geladen vanuit de Sleutelkluis en vervolgens worden gebruikt voor het versleutelen van de asset.

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell-cmdlets
De cmdlets in de volgende tabel worden gebruikt voor AzureRM, maken en beheren van automation referentieassets met Windows PowerShell. Ze worden verzonden als onderdeel van de [AzureRM.Automation module](/powershell/azure/overview) die beschikbaar is voor gebruik in Automation-runbooks en DSC-configuraties.

|Cmdlets|Beschrijving|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate)|Hiermee haalt informatie over een certificaat wilt gebruiken in een runbook of de DSC-configuratie. U kunt alleen het certificaat zelf ophalen van Get-AutomationCertificate activiteit.|
|[New-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationcertificate)|Maakt een nieuw certificaat in Azure Automation.|
[Remove-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationcertificate)|Hiermee verwijdert u een certificaat van Azure Automation.|Maakt een nieuw certificaat in Azure Automation.
|[Set-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationcertificate)|Hiermee stelt u de eigenschappen voor een bestaand certificaat, met inbegrip van het certificaat te uploaden en het instellen van het wachtwoord voor een .pfx-bestand.|
|[Add-AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Een certificaat voor de opgegeven cloud-service geüpload.|

## <a name="activities"></a>Activiteiten
De activiteiten in de volgende tabel worden gebruikt voor toegang tot de certificaten in een runbook en DSC-configuraties.

| Activiteiten | Beschrijving |
|:---|:---|
|Get-AutomationCertificate|Hiermee haalt u een certificaat wilt gebruiken in een runbook of de DSC-configuratie. Retourneert een [System.Security.Cryptography.X509Certificates.X509Certificate2](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate2.aspx) object.|

> [!NOTE] 
> Vermijd het gebruik van variabelen in de parameter-Name van **Get-AutomationCertificate** in een runbook of de DSC-configuratie zoals deze ingewikkelder detecteren van afhankelijkheden tussen runbooks of DSC-configuratie en de Automation-variabelen tijdens de ontwerpfase.

## <a name="python2-functions"></a>Python2 functies

De functie in de volgende tabel wordt gebruikt voor toegang tot de certificaten in een runbook Python2.

| Functie | Beschrijving |
|:---|:---|
| automationassets.get_automation_certificate | Informatie over een certificaatasset opgehaald. |

> [!NOTE]
> U moet importeren de **automationassets** -module in het begin van uw runbook Python om toegang tot de functies van de asset.

## <a name="creating-a-new-certificate"></a>Een nieuw certificaat maken

Wanneer u een nieuw certificaat maakt, wordt het een cer- of pfx-bestand uploaden naar Azure Automation. Als u het certificaat als exporteerbaar markeren, kunt klikt u vervolgens u overbrengen deze buiten het Azure Automation-certificaatarchief. Als het is niet exporteerbaar, kan vervolgens alleen worden gebruikt voor het ondertekenen van binnen het runbook of de DSC-configuratie. Azure Automation vereist het certificaat voor de provider: **Microsoft Enhanced RSA and AES Cryptographic Provider**.

### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Een nieuw certificaat maken met de Azure-portal

1. Van uw Automation-account, klikt u op de **activa** tegel openen de **activa** blade.
1. Klik op de **certificaten** tegel openen de **certificaten** blade.
1. Klik op **toevoegen van een certificaat** boven aan de blade.
1. Typ een naam voor het certificaat in de **naam** vak.
1. Als u wilt bladeren naar een cer- of pfx-bestand, klikt u op **selecteert u een bestand** onder **uploaden van een certificaatbestand**. Als u een .pfx-bestand selecteert, geeft u een wachtwoord en of deze is toegestaan om te worden geëxporteerd.
1. Klik op **maken** om op te slaan van het nieuwe certificaat actief.

### <a name="to-create-a-new-certificate-with-windows-powershell"></a>Een nieuw certificaat maken met Windows PowerShell

Het volgende voorbeeld laat zien hoe u een nieuw Automation-certificaat maken en deze exporteerbaar markeren. Hiermee importeert een bestaande pfx-bestand.

```powershell-interactive
$certName = 'MyCertificate'
$certPath = '.\MyCert.pfx'
$certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup
```

## <a name="using-a-certificate"></a>Gebruik van een certificaat

U kunt een certificaat gebruiken de **Get-AutomationCertificate** activiteit. U kunt geen gebruiken de [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) cmdlet omdat deze informatie over het certificaat actief, maar niet het certificaat zelf retourneert.

### <a name="textual-runbook-sample"></a>Voorbeeld van tekstueel runbook

De volgende voorbeeldcode laat zien hoe een certificaat toevoegen aan een cloudservice in een runbook. In dit voorbeeld wordt wordt het wachtwoord opgehaald uit een versleutelde automation-variabele.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-sample"></a>Grafisch runbook-voorbeeld

U toevoegen een **Get-AutomationCertificate** aan een grafisch runbook door met de rechtermuisknop op het certificaat in het deelvenster bibliotheek van de grafische editor en **toevoegen aan papier**.

![Certificaat toevoegen aan het canvas](media/automation-certificates/automation-certificate-add-to-canvas.png)

De volgende afbeelding toont een voorbeeld van het gebruik van een certificaat in een grafisch runbook. Dit is hetzelfde als in het voorgaande voorbeeld voor het toevoegen van een certificaat aan een cloudservice van tekstueel runbook.

![Voorbeeld grafisch ontwerpen ](media/automation-certificates/graphical-runbook-add-certificate.png)

### <a name="python2-sample"></a>Voorbeeld van Python2
Het volgende voorbeeld laat zien hoe voor toegang tot de certificaten in Python2 runbooks.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert 
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het werken met koppelingen naar de logische stroom van uw runbook is ontworpen om uit te voeren activiteiten te regelen, [koppelingen in het grafisch ontwerpen](automation-graphical-authoring-intro.md#links-and-workflow). 
