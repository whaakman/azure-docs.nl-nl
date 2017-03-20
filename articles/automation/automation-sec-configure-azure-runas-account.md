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
ms.date: 03/06/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 7230fb1a8d27708c40040950e3ec8950c6c04780
ms.lasthandoff: 03/07/2017


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

We laten u zien hoe u het Automation-account maakt vanuit Azure Portal, hoe u een Automation-account bijwerkt met PowerShell, hoe u de accountconfiguratie beheert en hoe u verificatie in runbooks uitvoert.

Er zijn echter enkele dingen die u moet begrijpen en waarmee u rekening moet houden voordat u doorgaat.

1. Dit heeft geen invloed op bestaande Automation-accounts die al in het klassieke of Resource Manager-implementatiemodel zijn gemaakt.  
2. Het werkt alleen voor Automation-accounts die zijn gemaakt via Azure Portal.  Wanneer u een account probeert te maken vanuit de klassieke portal, wordt de configuratie van het Uitvoeren als-account niet gerepliceerd.
3. Als u momenteel runbooks en activa hebt (bijvoorbeeld planningen, variabelen, enzovoort) die u eerder hebt gemaakt voor het beheer van klassieke resources, en u die runbooks wilt verifiëren met het nieuwe Klassiek uitvoeren als-account, moet u een Klassiek uitvoeren als-account maken met behulp van Een Uitvoeren als-account beheren, of het bestaande account bijwerken met onderstaand PowerShell-script.  
4. Als u wilt verifiëren met het nieuwe Uitvoeren als-account en het klassieke Uitvoeren als-Automation-account, moet u de bestaande runbooks wijzigen met de volgende voorbeeldcode.  **Houd er rekening mee** dat het Uitvoeren als-account is bedoeld voor verificatie met Resource Manager-resources met de service-principal op basis van certificaten, en dat het klassieke Uitvoeren als-account is bedoeld voor verificatie met Service Management-resources met het beheercertificaat.     

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Nieuw Automation-account maken vanuit Azure Portal
In deze sectie voert u de volgende stappen uit om vanuit Azure Portal een nieuw Azure Automation-account te maken.  Hiermee wordt zowel het Uitvoeren als- als het klassieke Uitvoeren als-account gemaakt.  

> [!NOTE]
> De gebruiker die deze stappen uitvoert, moet lid zijn van de rol Servicebeheerders of co-beheerder zijn van het abonnement dat de gebruiker toegang verleent tot het abonnement. De gebruiker moet ook als gebruiker worden toegevoegd aan de standaard Active Directory van dat abonnement. Het account hoeft niet te worden toegewezen aan een bevoorrechte rol. Gebruikers die, voordat ze worden toegevoegd aan de rol Co-beheerder van het abonnement, geen lid zijn van de Active Directory van dit abonnement, worden als gast toegevoegd aan Active Directory. De waarschuwing 'U bent niet gemachtigd om te maken...' wordt dan weergegeven op de blade **Automation-account toevoegen**. Gebruikers die zijn toegevoegd aan de rol Co-beheerder, kunnen worden verwijderd uit de Active Directory van het abonnement en opnieuw worden toegevoegd, zodat ze een volledige gebruiker worden in Active Directory. Deze situatie kan worden gecontroleerd in het deelvenster **Azure Active Directory** in Azure Portal door **Gebruikers en groepen** te selecteren. Selecteer vervolgens **Alle gebruikers**, selecteer de specifieke gebruiker en daarna **Profiel**.  De waarde van het kenmerk **Gebruikerstype** onder het gebruikersprofiel mag niet gelijk zijn aan **Gast**.  
> 

1. Meld u aan bij Azure Portal met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.
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

7. Terwijl in Azure het Automation-account wordt gemaakt, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

### <a name="resources-included"></a>Beschikbare resources
Wanneer het Automation-account is gemaakt, worden er automatisch verschillende resources voor u gemaakt.  In de volgende tabel vindt u een overzicht van de bronnen voor het Uitvoeren als-account.<br>

| Resource | Beschrijving |
| --- | --- |
| AzureAutomationTutorial Runbook |Een voorbeeld van een grafisch runbook dat laat zien hoe u verifieert met behulp van het Uitvoeren als-account en dat alle Resource Managers-resources ophaalt. |
| AzureAutomationTutorialScript Runbook |Een voorbeeld van een PowerShell-runbook dat laat zien hoe u verifieert met behulp van het Uitvoeren als-account en dat alle Resource Managers-resources ophaalt. |
| AzureRunAsCertificate |Certificaatasset dat automatisch tijdens het maken van het Automation-account of met onderstaand PowerShell-script voor een bestaande account wordt gemaakt.  Hiermee kunt u verifiëren met Azure zodat u Azure Resource Manager-resources kunt beheren vanuit runbooks.  Dit certificaat is één jaar geldig. |
| AzureRunAsConnection |Verbindingsasset dat automatisch tijdens het maken van het Automation-account of met onderstaand PowerShell-script voor een bestaande account wordt gemaakt. |

In de volgende tabel vindt u een overzicht van de bronnen voor het klassieke Uitvoeren als-account.<br>

| Resource | Beschrijving |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Een voorbeeld van een grafisch runbook dat alle klassieke virtuele machines in een abonnement ophaalt met het klassieke Uitvoeren als-account (certificaat) en vervolgens de VM-naam en -status weergeeft. |
| AzureClassicAutomationTutorial Script Runbook |Een voorbeeld van een PowerShell-runbook dat alle klassieke virtuele machines in een abonnement ophaalt met het klassieke Uitvoeren als-account (certificaat) en vervolgens de VM-naam en -status weergeeft. |
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

## <a name="managing-azure-run-as-account"></a>Azure Uitvoeren als-account beheren
Tijdens de levensduur van uw Automation-account moet u het certificaat vernieuwen voordat het verloopt. Als u denkt dat het account is aangetast, kunt u het Uitvoeren als-account verwijderen en opnieuw maken.  In deze sectie worden de stappen beschreven voor het uitvoeren van deze bewerkingen.  

### <a name="self-signed-certificate-renewal"></a>Zelfondertekend certificaat vernieuwen
Het zelfondertekend certificaat dat is gemaakt voor het Azure Uitvoeren als-account, kan op elk moment worden vernieuwd, totdat het verloopt (dit is één jaar na de aanmaakdatum).  Als u het certificaat vernieuwt, blijft het oude geldige certificaat behouden om ervoor te zorgen dat eventuele runbooks die nog in de wachtrij staan of nog actief zijn, en die worden geverifieerd met het Uitvoeren als-account, niet worden beïnvloed.  Het certificaat blijft bestaan tot de vervaldatum.    

> [!NOTE]
> Als u uw Uitvoeren als-account voor Automation hebt geconfigureerd om een certificaat te gebruiken dat is uitgegeven door de certificeringsinstantie van uw bedrijf en u deze optie gebruikt, wordt dat certificaat vervangen door een zelfondertekend certificaat.  

1. Open in Azure Portal het Automation-account.  
2. Selecteer in de blade Automation-account in het eigenschappendeelvenster voor het account onder de sectie **Accountinstellingen** de optie **Uitvoeren als-account**.<br><br> ![Eigenschappendeelvenster voor Automation-account](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)<br><br>
3. Selecteer in de blade **Uitvoeren als-accounts** het Uitvoeren als- of Klassiek uitvoeren als-account waarvoor u het certificaat wilt vernieuwen. En klik in de eigenschappenblade voor het geselecteerde account op **Certificaat vernieuwen**.<br><br> ![Certificaat vernieuwen voor Uitvoeren als-account](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)<br><br> U ontvangt een prompt waarin u wordt gevraagd of u wilt doorgaan.  
4. Terwijl het certificaat wordt gemaakt, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

### <a name="delete-run-as-account"></a>Uitvoeren als-account verwijderen
In de volgende stappen wordt beschreven hoe u uw Azure Uitvoeren als- of Klassiek uitvoeren als-account kunt verwijderen of opnieuw kunt maken.  Als u deze actie uitvoert, blijft het Automation-account behouden.  Nadat u het Uitvoeren als- of Klassiek uitvoeren als-account hebt verwijderd, kunt u het opnieuw maken in de portal.  

1. Open in Azure Portal het Automation-account.  
2. Selecteer in de blade Automation-account in het eigenschappendeelvenster voor het account onder de sectie **Accountinstellingen** de optie **Uitvoeren als-account**.
3. Selecteer in de blade **Uitvoeren als-accounts** het Uitvoeren als- of Klassiek uitvoeren als-account dat wilt verwijderen. En klik in de eigenschappenblade voor het geselecteerde account op **Verwijderen**.<br><br> ![Uitvoeren als-account verwijderen](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)<br><br>  U ontvangt een prompt waarin u wordt gevraagd of u wilt doorgaan.
4. Terwijl het account wordt verwijderd, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.  Nadat de verwijdering is voltooid, kunt u het account opnieuw maken door op de eigenschappenblade **Uitvoeren als-accounts** de optie **Azure Uitvoeren als-account** selecteren.<br><br> ![Het Automation uitvoeren als-account opnieuw maken](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)<br> 

### <a name="misconfiguration"></a>Onjuiste configuratie
Als bepaalde configuratie-items die nodig zijn voor het juist functioneren van het Uitvoeren als- of Klassiek uitvoeren als-account, worden verwijderd of niet juist zijn gemaakt tijdens de initiële configuratie, zoals:

* certificaatasset 
* Verbindingsasset 
* Uitvoeren als-account is verwijderd uit de rol Inzender
* Service-principal of toepassing in Azure AD

Met Automation worden deze wijzigingen gedetecteerd en ontvangt u een melding met daarin de status **Onvolledig** in de eigenschappenblade **Uitvoeren als-accounts** voor het account.<br><br> ![Statusbericht Onvolledige Uitvoeren als-configuratie](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)<br><br>Als u het Uitvoeren als-account selecteert, wordt de volgende fout weergegeven in het eigenschappendeelvenster van het account:<br><br> ![Waarschuwingsbericht Onvolledige Uitvoeren als-configuratie](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).<br>  
Als uw Uitvoeren als-account onjuist is geconfigureerd, kunt u dit snel oplossen door het Uitvoeren als-account te verwijderen en opnieuw te maken.   

## <a name="update-an-automation-account-using-powershell"></a>Automation-account bijwerken met behulp van PowerShell
In de volgende gevallen kunt u nu PowerShell gebruiken om het bestaande Automation-account bij te werken:

1. U hebt wel een Automation-account, maar geen Uitvoeren als-account gemaakt 
2. U hebt al een Automation-account voor het beheer van Resource Manager-resources en u wilt dit bijwerken met het Uitvoeren als-account voor runbookverificatie
4. U hebt al een Automation-account voor het beheer van klassieke resources en u wilt dit bijwerken, zodat u het klassieke Uitvoeren als-account kunt gebruiken in plaats van een nieuw account te maken, en uw runbooks en activa daarnaartoe te migreren   
5. U wilt een Uitvoeren als-account voor Azure maken en een klassiek Uitvoeren als-account via een certificaat dat is uitgegeven door de certificeringsinstantie van uw bedrijf

Dit script heeft de volgende vereisten:

1. Uitvoeren van dit script wordt alleen ondersteund op Windows 10 en Windows Server 2016 waarop met Azure Resource Manager-modules 2.01 en hoger is geïnstalleerd.  Uitvoeren wordt niet ondersteund in eerdere versies van Windows.  
2. Azure PowerShell 1.0 en hoger. Zie [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) voor meer informatie over deze release en de installatie ervan.
3. U hebt een Automation-account gemaakt.  In het onderstaande script wordt naar dit account verwezen als de waarde voor de parameters -AutomationAccountName en -ApplicationDisplayName.

Om de waarden voor *SubscriptionID*, *ResourceGroup* en *AutomationAccountName* te verkrijgen (verplichte parameters voor de scripts), selecteert u in Azure Portal het Automation-account op de blade **Automation-account**. Selecteer vervolgens **Alle instellingen**.  Selecteer op de blade **Alle instellingen** onder **Accountinstellingen** de optie **Eigenschappen**.  Noteer de waarden die op de blade **Eigenschappen** worden weergegeven.<br><br> ![Eigenschappen van Automation-account](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-run-as-account-powershell-script"></a>PowerShell-script voor Uitvoeren als-account maken
Dit PowerShell-script biedt ondersteuning voor de volgende configuraties: 

* Een Uitvoeren als-account voor Azure maken met een zelfondertekend certificaat
* Een Uitvoeren als-account voor Azure en een klassiek Uitvoeren als-account voor Azure maken met een zelfondertekend certificaat
* Een Uitvoeren als-account voor Azure en een klassiek Uitvoeren als-account voor Azure maken met een zakelijk certificaat
* Een Uitvoeren als-account voor Azure en een klassiek Uitvoeren als-account voor Azure maken met een zelfondertekend certificaat in de cloud van Azure Government

Afhankelijk van de configuratieoptie die u selecteert, wordt het volgende gemaakt:

* Een Azure AD-toepassing die wordt geverifieerd met het zelfondertekende certificaat of het zakelijke certificaat, het maken van een service-principalaccount voor deze toepassing in Azure AD en het toewijzen van de rol Inzender (u kunt dit wijzigen in Eigenaar of een andere rol) voor dit account in uw huidige abonnement.  Raadpleeg voor meer informatie het artikel [Op rollen gebaseerd toegangsbeheer in Azure Automation](automation-role-based-access-control.md).
* Een Automation-certificaatasset in het opgegeven Automation-account, **AzureRunAsCertificate** genaamd, dat het certificaat bevat dat door de service-principal wordt gebruikt.
* Een Automation-verbindingsasset in het opgegeven Automation-account **AzureRunAsConnection**, dat de applicationId, tenantId, subscriptionId en de vingerafdruk van het certificaat bevat.    

Klassieke Uitvoeren als-account:

* Een Automation-certificaatasset in het opgegeven Automation-account **AzureClassicRunAsCertificate**, dat het zelfondertekend of door een certificeringsinstantie uitgegeven certificaat bevat waarmee de runbooks worden geverifieerd.
* Een Automation-verbindingsasset in het opgegeven Automation-account **AzureClassicRunAsConnection**, dat de naam van het abonnement, subscriptionId en naam van het certificaatasset bevat.

Als u de optie selecteert om het zelfondertekend certificaat te gebruiken voor het klassieke Uitvoeren als-account, maakt het script een zelfondertekend certificaat en slaat het dit op in de map met tijdelijke bestanden op uw computer onder het gebruikersprofiel waarmee de PowerShell-sessie wordt uitgevoerd: *%USERPROFILE%\AppData\Local\Temp*.  Wanneer het script is uitgevoerd, moet u het Azure-beheercertificaat uploaden naar het beheerarchief voor het abonnement waarin het Automation-account is gemaakt.  De volgende stappen helpen u bij de uitvoering van het script en het uploaden van het certificaat.  

1. Sla het volgende script op uw computer op.  Sla het bestand in dit voorbeeld op met de bestandsnaam **New-RunAsAccount.ps1**.  
   
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
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,
 
        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,[string] $certPath, [string] $certPathCer, [string] $noOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose 
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid 

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use Key credentials and create AAD Application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId 
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id
   
        # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues 
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
          Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
          return
        }
 
        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create Run As Account using Service Principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"
 
        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
           $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
           $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
           $CertificateName = $AutomationAccountName+$CertifcateAssetName
           $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
           $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
           $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
           CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $NoOfMonthsUntilExpired
        }

        # Create Service Principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

         # Create the automation certificate asset
         CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

         # Populate the ConnectionFieldValues
         $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
         $TenantID = $SubscriptionInfo | Select TenantId -First 1
         $Thumbprint = $PfxCert.Thumbprint
         $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId} 

         # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
         CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
           # Create Run As Account using Service Principal
           $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
           $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
           $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
           $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#" 
 
            if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
         } else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $NoOfMonthsUntilExpired
         }

         # Create the automation certificate asset
         CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

         # Populate the ConnectionFieldValues
         $SubscriptionName = $subscription.Subscription.SubscriptionName
         $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

         # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
         CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

         Write-Host -ForegroundColor red $UploadMessage
         }

2. Start op uw computer **Windows PowerShell** op vanaf het **Start**scherm met verhoogde gebruikersrechten.
3. In de verhoogde PowerShell-opdrachtregelshell navigeert u naar de map waarin het script zich bevindt dat u in stap 1 hebt gemaakt en voert u de scriptinstelling uit met de vereiste parameterwaarden op basis van de configuratie die u nodig hebt.  

    **Een Uitvoeren als-account voor Azure maken met een zelfondertekend certificaat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword>` 

    **Een Uitvoeren als-account voor Azure en een klassiek Uitvoeren als-account voor Azure maken met een zelfondertekend certificaat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Een Uitvoeren als-account voor Azure en een klassiek Uitvoeren als-account voor Azure maken met een zakelijk certificaat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Een Uitvoeren als-account voor Azure en een klassiek Uitvoeren als-account voor Azure maken met een zelfondertekend certificaat in de cloud van Azure Government**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`
 
    > [!NOTE]
    > Nadat u het script hebt uitgevoerd, wordt u gevraagd zich te verifiëren bij Azure. U moet zich aanmelden met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.
    > 
    > 

Wanneer het script is voltooid en u een klassiek Uitvoeren als-account hebt gemaakt, moet u het certificaat kopiëren dat is gemaakt in de map **Temp** van uw gebruikersprofiel.  Volg de stappen om [een API-beheercertificaat te uploaden](../azure-api-management-certs.md) naar de klassieke Azure Portal en raadpleeg de [voorbeeldcode](#sample-code-to-authenticate-with-service-management-resources) om de configuratie van de referenties te valideren met Service Management-resources.  Als u geen klassiek Uitvoeren als-account hebt gemaakt, raadpleegt u de [voorbeeldcode](#sample-code-to-authenticate-with-resource-manager-resources) hieronder om te verifiëren met Resource Manager-resources. Valideer de configuratie of raadpleeg de [voorbeeldcode](#sample-code-to-authenticate-with-service-management-resources) om de verwijzingsconfiguratie met servicebeheerresources te valideren.

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Voorbeeldcode voor verificatie bij Resource Manager-resources
U kunt de bijgewerkte voorbeeldcode hieronder, die is overgenomen uit het voorbeeldrunbook **AzureAutomationTutorialScript**, gebruiken om de verificatie uit te voeren met behulp van het Uitvoeren als-account voor het beheer van Resource Manager-resources met uw runbooks.   

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in to Azure..."
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


