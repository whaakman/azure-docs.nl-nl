---
title: Een e-mail verzenden vanuit een Azure Automation-runbook
description: Leer hoe u SendGrid gebruiken voor het verzenden van een e-mailbericht van binnen een runbook.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 07/15/2019
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: ce05aadb53cc3ad24ed65ea139594010e1aef047
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234981"
---
# <a name="tutorial-send-an-email-from-an-azure-automation-runbook"></a>Zelfstudie: Een e-mail verzenden vanuit een Azure Automation-runbook

U kunt een e-mailbericht verzenden vanuit een runbook met [SendGrid](https://sendgrid.com/solutions) met behulp van PowerShell. Deze zelfstudie leert u over het maken van een herbruikbare runbook dat een e-mailbericht met een API-sleutel die zijn opgeslagen verzendt in [Azure Key Vault](/azure/key-vault/).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
>
> * Een Azure Key Vault maken
> * Uw SendGrid-API-sleutel Store in Key Vault
> * Een runbook maken dat opgehaald van uw API-sleutel en een e-mailbericht verzonden

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om deze zelfstudie te voltooien:

* Azure-abonnement: Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Een SendGrid-Account maken](/azure/sendgrid-dotnet-how-to-send-email#create-a-sendgrid-account).
* [Automation-account](automation-offering-get-started.md) met **Az** modules en [uitvoeren als-verbinding](automation-create-runas-account.md)om op te slaan en het runbook uit te voeren.

## <a name="create-an-azure-keyvault"></a>Een Azure Key Vault maken

U kunt een Azure Key Vault met behulp van de volgende PowerShell-script maken. Vervang de waarden van variabelen met waarden die specifiek zijn voor uw omgeving. Gebruik de ingesloten Azure Cloud Shell via het <kbd>uitproberen</kbd> knop in de rechterbovenhoek van het codeblok. U kunt ook kopiëren en voert u de code lokaal hebt u de [Azure PowerShell-Module](/powershell/azure/install-az-ps) geïnstalleerd op uw lokale computer.

> [!NOTE]
> Om op te halen van uw API-sleutel, gebruikt u de stappen in [uw SendGrid-API-sleutel vinden](/azure/sendgrid-dotnet-how-to-send-email#to-find-your-sendgrid-api-key).

```azurepowershell-interactive
$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
# If you omit the SubscriptionId parameter, the default subscription is selected.
Connect-AzAccount -SubscriptionId $SubscriptionId

# Use Get-AzLocation to see your available locations.
$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "<Enter a universally unique vault name>"
$SendGridAPIKey = "<SendGrid API key>"
$AutomationAccountName = "testaa"

# Create new Resource Group, or omit this step if you already have a resource group.
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region
$resourceId = $newKeyVault.ResourceId

# Convert the SendGrid API key into a SecureString
$Secret = ConvertTo-SecureString -String $SendGridAPIKey -AsPlainText -Force
Set-AzKeyVaultSecret -VaultName $VaultName -Name 'SendGridAPIKey' -SecretValue $Secret

# Grant access to the KeyVault to the Automation RunAs account.
$connection = Get-AzAutomationConnection -ResourceGroupName $KeyVaultResourceGroupName -AutomationAccountName $AutomationAccountName -Name AzureRunAsConnection
$appID = $connection.FieldDefinitionValues.ApplicationId
Set-AzKeyVaultAccessPolicy -VaultName $VaultName -ServicePrincipalName $appID -PermissionsToSecrets Set, Get
```

Zie voor andere manieren om te maken van een Azure Key Vault en een geheim opslaan, [KeyVault Quickstarts](/azure/key-vault/).

## <a name="import-required-modules-to-your-automation-account"></a>Vereiste modules importeren in uw Automation-Account

Voor het gebruik van Azure Key Vault vanuit een runbook, moet uw Automation-Account de volgende modules:

* [Az.Profile](https://www.powershellgallery.com/packages/Az.Profile).
* [Az.KeyVault](https://www.powershellgallery.com/packages/Az.KeyVault).

Klik op <kbd>implementeren in Azure Automation</kbd> op het tabblad Azure Automation onder Opties voor de installatie. Deze actie opent u de Azure-portal. Op de pagina voor het importeren, selecteert u uw Automation-Account en klikt u op <kbd>OK</kbd>.

Zie voor aanvullende methoden voor het toevoegen van de vereiste modules, [importeren van Modules](/azure/automation/shared-resources/modules#import-modules).

## <a name="create-the-runbook-to-send-an-email"></a>Het runbook voor het verzenden van een e-mailbericht te maken

Nadat u hebt een Sleutelkluis gemaakt en opgeslagen uw SendGrid-API-sleutel, is het tijd voor het maken van het runbook dat wordt opgehaald van de API-sleutel en een e-mailbericht ontvangt.

Dit runbook maakt gebruik van de AzureRunAsConnection [uitvoeren als-account](automation-create-runas-account.md) voor verificatie met Azure om op te halen van het geheim van Azure Key Vault.

In dit voorbeeld gebruiken om te maken van een runbook met de naam **verzenden GridMailMessage**. U kunt het PowerShell-script wijzigen, en deze opnieuw te gebruiken voor verschillende scenario's.

1. Ga naar uw Azure Automation-account.
2. Onder **procesautomatisering**, selecteer **Runbooks**.
3. Selecteer aan de bovenkant van de lijst met runbooks, **+ maken van een runbook**.
4. Op de **Runbook toevoegen** pagina **verzenden GridMailMessage** voor de naam van het runbook. Selecteer voor het runbooktype **PowerShell**. Ten slotte selecteert u **Create**.
   ![Runbook maken](./media/automation-send-email/automation-send-email-runbook.png)
5. Het runbook is gemaakt en de pagina **PowerShell-runbook bewerken** wordt geopend.
   ![Het Runbook bewerken](./media/automation-send-email/automation-send-email-edit.png)
6. Kopieer de volgende PowerShell-voorbeeld in de **bewerken** pagina. Zorg ervoor dat de `$VaultName` is de naam die u hebt opgegeven tijdens het maken van uw Key Vault.

    ```powershell-interactive
    Param(
      [Parameter(Mandatory=$True)]
      [String] $destEmailAddress,
      [Parameter(Mandatory=$True)]
      [String] $fromEmailAddress,
      [Parameter(Mandatory=$True)]
      [String] $subject,
      [Parameter(Mandatory=$True)]
      [String] $content
    )

    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Out-Null
    $VaultName = "<Enter your vault name>"
    $SENDGRID_API_KEY = (Get-AzKeyVaultSecret -VaultName $VaultName -Name "SendGridAPIKey").SecretValueText
    $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
    $headers.Add("Authorization", "Bearer " + $SENDGRID_API_KEY)
    $headers.Add("Content-Type", "application/json")

    $body = @{
    personalizations = @(
        @{
            to = @(
                    @{
                        email = $destEmailAddress
                    }
            )
        }
    )
    from = @{
        email = $fromEmailAddress
    }
    subject = $subject
    content = @(
        @{
            type = "text/plain"
            value = $content
        }
    )
    }

    $bodyJson = $body | ConvertTo-Json -Depth 4

    $response = Invoke-RestMethod -Uri https://api.sendgrid.com/v3/mail/send -Method Post -Headers $headers -Body $bodyJson
    ```

7. Selecteer **publiceren** opslaan en publiceren van het runbook.

U kunt de stappen onder volgen om te verifiëren dat het runbook correct wordt uitgevoerd [een runbook testen](manage-runbooks.md#test-a-runbook) of [een runbook starten](start-runbooks.md).
Als u uw test-e-mailadres niet in eerste instantie ziet, controleert u uw **ongewenste** en **Spam** mappen.

## <a name="clean-up"></a>Opschonen

Wanneer u het runbook niet meer nodig hebt, kunt u het verwijderen. Om dit te doen, selecteert u het runbook in de runbooklijst en klikt u op **Verwijderen**.

Verwijderen van de key vault met behulp van de [Remove-AzureRMKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault?view=azurermps) cmdlet.

```azurepowershell-interactive
$VaultName = "<your KeyVault name>"
$ResourceGroupName = "<your ResourceGroup name>"
Remove-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor problemen met het maken of bij het starten van uw runbook [fouten met runbooks oplossen](./troubleshoot/runbooks.md).
* Zie voor het bijwerken van modules in uw Automation-Account, [het bijwerken van Azure PowerShell-modules in Azure Automation](automation-update-azure-modules.md)].
* Zie voor het controleren van de uitvoering van runbook [taakstatus en taakstromen van Automation doorsturen naar de logboeken van Azure Monitor](automation-manage-send-joblogs-log-analytics.md).
* Zie voor het activeren van een runbook met behulp van een waarschuwing, [gebruik van een waarschuwing voor het activeren van een Azure Automation-runbook](automation-create-alert-triggered-runbook.md).
