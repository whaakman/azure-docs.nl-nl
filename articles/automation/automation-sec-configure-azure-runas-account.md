---
title: Uitvoeren als-account voor Azure configureren | Microsoft Docs
description: Zelfstudie die u helpt bij het maken en testen en bij het voorbeeldgebruik van de verificatie van beveiligingsprincipals in Azure Automation.
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: naam van service-principal, setspn, azure-verificatie
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 32afda1861c2c558f8bb9ffedf897cd8d1df0b5c


---
# <a name="authenticate-runbooks-with-azure-run-as-account"></a>Runbooks verifiëren met Azure Uitvoeren als-account
In dit onderwerp wordt beschreven hoe u vanuit Azure Portal een Automation-account configureert met behulp van de functie Uitvoeren als-account, om accounts voor het beheer van runbooks te verifiëren in Azure Resource Manager of Azure Service Management.

Wanneer u in Azure Portal een nieuw Automation-account maakt, wordt automatisch het volgende gemaakt:

* Uitvoeren als-account; hiermee wordt een nieuwe service-principal in Azure Active Directory gemaakt, een certificaat, en wordt het RBAC (op rollen gebaseerd toegangsbeheer) toegewezen, dat wordt gebruikt om Resource Manager-resources te beheren met runbooks.   
* Klassiek uitvoeren als-account; door een beheercertificaat te uploaden dat wordt gebruikt om Azure Service Management of klassieke resources te beheren met runbooks.  

Dit maakt het proces voor u eenvoudiger zodat u sneller runbooks kunt maken en implementeren, ter ondersteuning van uw automatiseringsbehoeften.      

Met een Uitvoeren als- en een Klassiek uitvoeren als-account kunt u:

* Een gestandaardiseerde manier voor Azure-verificatie bieden bij het beheer van Azure Resource Manager- of Azure Service Management-resources vanuit runbooks in Azure Portal.  
* Het gebruik automatiseren van globale runbooks die in Azure-waarschuwingen zijn geconfigureerd.

> [!NOTE]
> De [waarschuwingsintegratiefunctie](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) van Azure met globale Automation-runbooks vereist een Automation-account dat met een Uitvoeren als- en Klassiek uitvoeren als-account is geconfigureerd. Selecteer een Automation-account waarvoor al een gebruiker voor het Uitvoeren als- en Klassiek uitvoeren als-account is gedefinieerd of maak een nieuw Automation-account.
> 
> 

U ziet hoe u het Automation-account maakt vanuit Azure Portal, hoe u een Automation-account bijwerkt met PowerShell en hoe u verificatie in runbooks uitvoert.

Er zijn echter enkele dingen die u moet begrijpen en waarmee u rekening moet houden voordat u doorgaat.

1. Dit heeft geen invloed op bestaande Automation-accounts die al in het klassieke of Resource Manager-implementatiemodel zijn gemaakt.  
2. Het werkt alleen voor Automation-accounts die zijn gemaakt via Azure Portal.  Wanneer u een account probeert te maken vanuit de klassieke portal, wordt de configuratie van het Uitvoeren als-account niet gerepliceerd.
3. Als u momenteel runbooks en activa hebt (bijvoorbeeld schema's, variabelen, enzovoort) die u eerder hebt gemaakt voor het beheer van klassieke resources, en u die runbooks wilt verifiëren met het nieuwe klassieke Uitvoeren als-account, moet u deze migreren naar het nieuwe Automation-account of het bestaande account bijwerken met onderstaand PowerShell-script.  
4. Als u wilt verifiëren met het nieuwe Uitvoeren als-account en het klassieke Uitvoeren als-Automation-account, moet u de bestaande runbooks wijzigen met de volgende voorbeeldcode.  **Houd er rekening mee** dat het Uitvoeren als-account is bedoeld voor verificatie met Resource Manager-resources met de service-principal op basis van certificaten, en dat het klassieke Uitvoeren als-account is bedoeld voor verificatie met Service Management-resources met het beheercertificaat.     

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Nieuw Automation-account maken vanuit Azure Portal
In deze sectie voert u de volgende stappen uit om vanuit Azure Portal een nieuw Azure Automation-account te maken.  Hiermee wordt zowel het Uitvoeren als- als het klassieke Uitvoeren als-account gemaakt.  

> [!NOTE]
> De gebruiker die deze stappen uitvoert, *moet* lid zijn van de rol Abonnementsbeheerders en medebeheerder zijn van het abonnement dat toegang verleent tot het abonnement voor de gebruiker.  De gebruiker moet ook als gebruiker worden toegevoegd aan de standaard Active Directory van dat abonnement. Het account hoeft niet te worden toegewezen aan een bevoorrechte rol.
> 
> 

1. Meld u bij Azure Portal aan met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.
2. Selecteer **Automation-accounts**.
3. Klik op de blade Automation-accounts op **Toevoegen**.<br>![Automation-account toevoegen](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)
   
   > [!NOTE]
   > Als u op de blade **Automation-account toevoegen** de volgende waarschuwing ziet, is uw account geen lid van de rol Abonnementsbeheerders en geen medebeheerder van het abonnement.<br>![Waarschuwing bij Automation-account toevoegen](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   > 
   > 
4. Typ op de blade **Automation-account toevoegen** in het vak **Naam** een naam voor uw nieuwe Automation-account.
5. Als u meer dan één abonnement hebt, geeft u er een op voor het nieuwe account, evenals een nieuwe of bestaande **resourcegroep** en de **locatie** van een Azure-datacenter.
6. Controleer of de waarde **Ja** is geselecteerd voor de optie **Een Uitvoeren als-account voor Azure maken** en klik op de knop **Maken**.  
   
   > [!NOTE]
   > Als u de optie **Nee** selecteert omdat u geen Uitvoeren als-account wilt maken, wordt een waarschuwing weergegeven op de blade **Automation-account toevoegen**.  Hoewel het account wordt gemaakt in Azure Portal, heeft het geen overeenkomstige verificatie-id in de adreslijstservice van het klassieke of Resource Manager-abonnement en daardoor ook geen toegang tot resources in uw abonnement.  Hierdoor kunnen alle runbooks die naar dit account verwijzen, geen taken verifiëren en uitvoeren met resources in die implementatiemodellen.
   > 
   > ![Waarschuwing bij Automation-account toevoegen](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)<br>
   > Wanneer de service-principal niet is gemaakt, wordt de rol Inzender niet toegewezen.
   > 
   > 
7. Terwijl in Azure het Automation-account wordt gemaakt, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

### <a name="resources-included"></a>Beschikbare resources
Wanneer het Automation-account is gemaakt, worden er automatisch verschillende resources voor u gemaakt.  In de volgende tabel vindt u een overzicht van de bronnen voor het Uitvoeren als-account.<br>

| Resource | Beschrijving |
| --- | --- |
| AzureAutomationTutorial Runbook |Een voorbeeld van een PowerShell-runbook dat laat zien hoe u verifieert met behulp van het Uitvoeren als-account en dat alle Resource Managers-resources ophaalt. |
| AzureAutomationTutorialScript Runbook |Een voorbeeld van een PowerShell-runbook dat laat zien hoe u verifieert met behulp van het Uitvoeren als-account en dat alle Resource Managers-resources ophaalt. |
| AzureRunAsCertificate |Certificaatasset dat automatisch tijdens het maken van het Automation-account of met onderstaand PowerShell-script voor een bestaande account wordt gemaakt.  Hiermee kunt u verifiëren met Azure zodat u Azure Resource Manager-resources kunt beheren vanuit runbooks.  Dit certificaat is één jaar geldig. |
| AzureRunAsConnection |Verbindingsasset dat automatisch tijdens het maken van het Automation-account of met onderstaand PowerShell-script voor een bestaande account wordt gemaakt. |

In de volgende tabel vindt u een overzicht van de bronnen voor het klassieke Uitvoeren als-account.<br>

| Resource | Beschrijving |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Een voorbeeld van een runbook dat alle klassieke virtuele machines in een abonnement ophaalt met het klassieke Uitvoeren als-account (certificaat) en vervolgens de VM-naam en -status weergeeft. |
| AzureClassicAutomationTutorial Script Runbook |Een voorbeeld van een runbook dat alle klassieke virtuele machines in een abonnement ophaalt met het klassieke Uitvoeren als-account (certificaat) en vervolgens de VM-naam en -status weergeeft. |
| AzureClassicRunAsCertificate |Certificaatasset dat automatisch wordt gemaakt en wordt gebruikt voor verificatie met Azure, zodat u klassieke Azure-resources kunt beheren vanuit runbooks.  Dit certificaat is één jaar geldig. |
| AzureClassicRunAsConnection |Verbindingsasset dat automatisch wordt gemaakt en wordt gebruikt voor verificatie met Azure, zodat u klassieke Azure-resources kunt beheren vanuit runbooks. |

## <a name="verify-run-as-authentication"></a>Uitvoeren als-verificatie verifiëren
Nu gaat u een kleine test uitvoeren om te bevestigen dat u daadwerkelijk kunt verifiëren met het nieuwe Uitvoeren als-account.     

1. Open in Azure Portal het Automation-account dat u eerder hebt gemaakt.  
2. Klik op de tegel **Runbooks** om de lijst met runbooks te openen.
3. Selecteer het runbook **AzureAutomationTutorialScript** en klik op **Start** om het runbook te starten.  U ontvangt een prompt waarin u wordt gevraagd of u het runbook wilt starten.
4. Er wordt een [runbooktaak](automation-runbook-execution.md) gemaakt, de blade Taak wordt weergegeven en de taakstatus wordt weergegeven in de tegel **Taaksamenvatting**.  
5. In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot in de cloud een runbook-werkrol beschikbaar is. De taakstatus verandert daarna in *Starten* wanneer een werkrol de taak claimt en daarna in *Wordt uitgevoerd* wanneer het runbook daadwerkelijk wordt uitgevoerd.  
6. Wanneer de runbooktaak is voltooid, is de status **Voltooid**.<br> ![Runbooktest beveiligingsprincipal](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)<br>
7. Klik op de tegel **Uitvoer** als u de gedetailleerde resultaten van het runbook wilt bekijken.
8. Op de blade **Uitvoer** ziet u dat de verificatie is geslaagd. Er wordt ook een lijst met alle beschikbare resources in de resourcegroep weergegeven.
9. Sluit de blade **Uitvoer** om terug te keren naar de blade **Taaksamenvatting**.
10. Sluit de blade **Taaksamenvatting** en het bijbehorende runbook **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Klassieke Uitvoeren als-verificatie verifiëren
Nu gaat u een kleine test uitvoeren om te bevestigen dat u daadwerkelijk kunt verifiëren met het klassieke Uitvoeren als-account.     

1. Open in Azure Portal het Automation-account dat u eerder hebt gemaakt.  
2. Klik op de tegel **Runbooks** om de lijst met runbooks te openen.
3. Selecteer het runbook **AzureClassicAutomationTutorialScript** en klik op **Start** om het runbook te starten.  U ontvangt een prompt waarin u wordt gevraagd of u het runbook wilt starten.
4. Er wordt een [runbooktaak](automation-runbook-execution.md) gemaakt, de blade Taak wordt weergegeven en de taakstatus wordt weergegeven in de tegel **Taaksamenvatting**.  
5. In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot in de cloud een runbook-werkrol beschikbaar is. De taakstatus verandert daarna in *Starten* wanneer een werkrol de taak claimt en daarna in *Wordt uitgevoerd* wanneer het runbook daadwerkelijk wordt uitgevoerd.  
6. Wanneer de runbooktaak is voltooid, is de status **Voltooid**.<br> ![Runbooktest beveiligingsprincipal](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
7. Klik op de tegel **Uitvoer** als u de gedetailleerde resultaten van het runbook wilt bekijken.
8. Op de blade **Uitvoer** ziet u dat de verificatie is geslaagd. Er wordt ook een lijst met alle klassieke virtuele machines in het abonnement weergegeven.
9. Sluit de blade **Uitvoer** om terug te keren naar de blade **Taaksamenvatting**.
10. Sluit de blade **Taaksamenvatting** en de bijbehorende runbookblade **AzureClassicAutomationTutorialScript**.

## <a name="update-an-automation-account-using-powershell"></a>Automation-account bijwerken met behulp van PowerShell
In de volgende gevallen kunt u nu PowerShell gebruiken om het bestaande Automation-account bij te werken:

1. U hebt wel een Automation-account, maar geen Uitvoeren als-account gemaakt
2. U hebt al een Automation-account voor het beheer van Resource Manager-resources en u wilt dit bijwerken met het Uitvoeren als-account voor runbookverificatie
3. U hebt al een Automation-account voor het beheer van klassieke resources en u wilt dit bijwerken, zodat u het klassieke Uitvoeren als-account kunt gebruiken in plaats van een nieuw account te maken, en uw runbooks en activa daarnaartoe te migreren   

Voordat u doorgaat, controleert u het volgende:

1. U hebt [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855) gedownload en geïnstalleerd indien u Windows 7 gebruikt.   
    Als u Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1 of Windows 7 SP1 gebruikt, is [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395) beschikbaar voor installatie.
2. Azure PowerShell 1.0. Zie [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) voor meer informatie over deze release en de installatie ervan.
3. U hebt een Automation-account gemaakt.  In beide scripts hieronder wordt naar dit account verwezen als de waarde voor de parameters -AutomationAccountName en -ApplicationDisplayName.

Om de waarden voor *SubscriptionID*, *ResourceGroup* en *AutomationAccountName* te verkrijgen (verplichte parameters voor de scripts), selecteert u in Azure Portal het Automation-account op de blade **Automation-account**. Selecteer vervolgens **Alle instellingen**.  Selecteer op de blade **Alle instellingen** onder **Accountinstellingen** de optie **Eigenschappen**.  Noteer de waarden die op de blade **Eigenschappen** worden weergegeven.<br> ![Eigenschappen van Automation-account](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-run-as-account-powershell-script"></a>PowerShell-script voor Uitvoeren als-account maken
Met onderstaand PowerShell-script wordt het volgende geconfigureerd:

* Een Azure AD-toepassing die zal worden geverifieerd met het zelfondertekende certificaat, het maken van een service-principalaccount voor deze toepassing in Azure AD en het toewijzen van de rol Inzender (u kunt dit wijzigen in Eigenaar of een andere rol) voor dit account in uw huidige abonnement.  Raadpleeg voor meer informatie het artikel [Op rollen gebaseerd toegangsbeheer in Azure Automation](automation-role-based-access-control.md).
* Een Automation-certificaatasset in het opgegeven Automation-account, **AzureRunAsCertificate** genaamd, dat het certificaat bevat dat door de service-principal wordt gebruikt.
* Een Automation-verbindingsasset in het opgegeven Automation-account **AzureRunAsConnection**, dat de applicationId, tenantId, subscriptionId en de vingerafdruk van het certificaat bevat.    

De volgende stappen helpen u bij de uitvoering van het script.

1. Sla het volgende script op uw computer op.  Sla het bestand in dit voorbeeld op met de bestandsnaam **Nieuw AzureServicePrincipal.ps1**.  
   
        #Requires -RunAsAdministrator
        Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,
   
        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,
   
        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,
   
        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,
   
        [Parameter(Mandatory=$true)]
        [String] $CertPlainPassword,
   
        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )
   
        Login-AzureRmAccount
        Import-Module AzureRM.Resources
        Select-AzureRmSubscription -SubscriptionId $SubscriptionId
   
        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")
   
        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"
   
        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose
   
        $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate -ArgumentList @($CertPath, $CertPlainPassword)
        $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())
   
        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= $EndDate
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.Type = "AsymmetricX509Cert"
        $KeyCredential.Usage = "Verify"
        $KeyCredential.Value = $KeyValue
   
        # Use Key credentials
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $ApplicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $keyCredential
   
        New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId | Write-Verbose
        Get-AzureRmADServicePrincipal | Where {$_.ApplicationId -eq $Application.ApplicationId} | Write-Verbose
   
        $NewRole = $null
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
           Sleep 5
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           Sleep 10
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
   
        # Get the tenant id for this subscription
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
   
        # Create the automation resources
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name AzureRunAsCertificate -Password $CertPassword -Exportable | write-verbose
   
        # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        $ConnectionAssetName = "AzureRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
2. Start op uw computer **Windows PowerShell** op vanaf het **Start**scherm met verhoogde gebruikersrechten.
3. Navigeer vanuit de opdrachtregel-shell met verhoogde gebruikersrechten van PowerShell naar de map die het script bevat dat in stap 1 is gemaakt en voer het script uit dat de waarden wijzigt voor de parameters *– ResourceGroup*, *- AutomationAccountName*, *- ApplicationDisplayName*, *- SubscriptionId* en *- CertPlainPassword*.<br>
   
   > [!NOTE]
   > Nadat u het script hebt uitgevoerd, wordt u gevraagd zich te verifiëren bij Azure. U moet zich aanmelden met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.
   > 
   > 
   
        .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName>
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"  
   <br>

Wanneer het script is voltooid, raadpleegt u onderstaande [voorbeeldcode](#sample-code-to-authenticate-with-resource-manager-resources) om verificatie met Resource Manager-resources uit te voeren en de configuratie van referenties te valideren.

### <a name="create-classic-run-as-account-powershell-script"></a>PowerShell-script voor klassiek Uitvoeren als-account maken
Met onderstaand PowerShell-script wordt het volgende geconfigureerd:

* Een Automation-certificaatasset in het opgegeven Automation-account **AzureClassicRunAsCertificate**, dat het certificaat bevat waarmee de runbooks worden geverifieerd.
* Een Automation-verbindingsasset in het opgegeven Automation-account **AzureClassicRunAsConnection**, dat de naam van het abonnement, subscriptionId en naam van het certificaatasset bevat.

Het script maakt een zelfondertekend certificaat en slaat dit op in de map met tijdelijke bestanden op uw computer onder het gebruikersprofiel waarmee de PowerShell-sessie - *%USERPROFILE%\AppData\Local\Temp* wordt uitgevoerd.  Wanneer het script is uitgevoerd, moet u het Azure-beheercertificaat uploaden naar het beheerarchief voor het abonnement waarin het Automation-account is gemaakt.  De volgende stappen helpen u bij de uitvoering van het script en het uploaden van het certificaat.  

1. Sla het volgende script op uw computer op.  Sla het bestand in dit voorbeeld op met de bestandsnaam **Nieuw- AzureClassicRunAsAccount.ps1**.
   
        #Requires -RunAsAdministrator
        Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,
   
        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,
   
        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,
   
        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,
   
        [Parameter(Mandatory=$true)]
        [String] $CertPlainPassword,
   
        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )
   
        Login-AzureRmAccount
        Import-Module AzureRM.Resources
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId
        $SubscriptionName = $subscription.Subscription.SubscriptionName
   
        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")
        $CertPathCer = Join-Path $env:TEMP ($ApplicationDisplayName + ".cer")
   
        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"
   
        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPathCer -Type CERT | Write-Verbose
   
        # Create the automation resources
        $ClassicCertificateAssetName = "AzureClassicRunAsCertificate"
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name $ClassicCertificateAssetName  -Password $CertPassword -Exportable | write-verbose
   
        # Create a Automation connection asset named AzureClassicRunAsConnection in the Automation account. This connection uses the ClassicCertificateAssetName.
        $ConnectionAssetName = "AzureClassicRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicCertificateAssetName}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureClassicCertificate -ConnectionFieldValues $ConnectionFieldValues
   
        Write-Host -ForegroundColor red "Please upload the cert $CertPathCer to the Management store by following the steps below."
        Write-Host -ForegroundColor red "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates."
        Write-Host -ForegroundColor red "Then click Upload and upload the certificate $CertPathCer"
2. Start op uw computer **Windows PowerShell** op vanaf het **Start**scherm met verhoogde gebruikersrechten.  
3. Navigeer vanuit de opdrachtregel-shell met verhoogde gebruikersrechten van PowerShell naar de map die het script bevat dat in stap 1 is gemaakt en voer het script uit dat de waarden wijzigt voor de parameters *– ResourceGroup*, *- AutomationAccountName*, *- ApplicationDisplayName*, *- SubscriptionId* en *- CertPlainPassword*.<br>
   
   > [!NOTE]
   > Nadat u het script hebt uitgevoerd, wordt u gevraagd zich te verifiëren bij Azure. U moet zich aanmelden met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.
   > 
   > 
   
        .\New-AzureClassicRunAsAccount.ps1 -ResourceGroup <ResourceGroupName>
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"

Wanneer het script is voltooid, moet u het certificaat kopiëren dat is gemaakt in de map **Temp** van uw gebruikersprofiel.  Volg de stappen om [een API-beheercertificaat te uploaden](../azure-api-management-certs.md) naar de klassieke Azure Portal en raadpleeg de [voorbeeldcode](#sample-code-to-authenticate-with-service-management-resources) om de configuratie van de referenties te valideren met Service Management-resources.

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Voorbeeldcode voor verificatie bij Resource Manager-resources
U kunt de bijgewerkte voorbeeldcode hieronder, die is overgenomen uit het voorbeeldrunbook **AzureAutomationTutorialScript**, gebruiken om de verificatie uit te voeren met behulp van het Uitvoeren als-account voor het beheer van Resource Manager-resources met uw runbooks.   

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Logging in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }


Het script bevat twee extra regels met code voor de ondersteuning van verwijzingen naar de context van een abonnement, zodat u eenvoudig tussen meerdere abonnementen kunt schakelen. Een variabele met de naam SubscriptionId bevat de id van het abonnement en na de instructie van de cmdlet Add-AzureRmAccount wordt de [cmdlet Set-AzureRmContext](https://msdn.microsoft.com/library/mt619263.aspx) vermeld met de parameterset *-SubscriptionId*. Als de naam van de variabele te algemeen is, wijzigt u de naam van de variabele door er een voorvoegsel aan toe te voegen of door er een andere naamgevingsconventie op toe te passen, zodat u de variabele gemakkelijker kunt identificeren voor uw doeleinden. U kunt ook de parameter -SubscriptionName in plaats van -SubscriptionId gebruiken met een bijbehorende variabeleasset.  

De cmdlet die wordt gebruikt voor verificatie in het runbook - **Add-AzureRmAccount**, gebruikt de parameterset *ServicePrincipalCertificate*.  In plaats van referenties wordt voor verificatie het certificaat van de service-principal gebruikt.  

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Voorbeeldcode voor verificatie met Service Management-resources
U kunt de bijgewerkte voorbeeldcode hieronder, die is overgenomen uit het voorbeeldrunbook **AzureClassicAutomationTutorialScript**, gebruiken om verificatie uit te voeren met het klassieke Uitvoeren als-account, voor het beheer van klassieke resources met uw runbooks.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID


## <a name="next-steps"></a>Volgende stappen
* Zie [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md) (Toepassingsobjecten en service-principalobjecten) voor meer informatie over service-principals.
* Zie [Op rollen gebaseerd toegangsbeheer in Azure Automation](automation-role-based-access-control.md) voor meer informatie over het op rollen gebaseerd toegangsbeheer in Azure Automation.
* Voor meer informatie over certificaten en Azure-services raadpleegt u [Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md) (Overzicht van certificaten voor Azure Cloud Services)




<!--HONumber=Dec16_HO1-->


