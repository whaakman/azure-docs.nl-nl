---
title: Een Uitvoeren als-account voor Azure configureren | Microsoft Docs
description: Deze zelfstudie helpt u bij het maken en testen, en bij het voorbeeldgebruik van de verificatie van beveiligingsprincipals in Azure Automation.
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
keywords: naam van service-principal, setspn, azure-verificatie
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/06/2017
ms.author: magoedte
ROBOTS: NOINDEX
redirect_url: /azure/automation/
redirect_document_id: TRUE
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 90570886b3a1ae0c48683691cb438b5a61195d76
ms.contentlocale: nl-nl
ms.lasthandoff: 04/27/2017


---

# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a>Runbooks verifiëren met een Azure Uitvoeren als-account
In dit artikel wordt beschreven hoe u een Azure Automation-account configureert in Azure Portal. Hiertoe gebruikt u de functie Uitvoeren als-account om runbooks te verifiëren waarmee resources worden beheerd in Azure Resource Manager of Azure Service Management.

Wanneer u in Azure Portal een Automation-account maakt, maakt u automatisch twee accounts:

* Een Uitvoeren als-account. Door dit account worden een service-principal in Azure Active Directory (Azure AD) en een certificaat gemaakt. Ook wordt door dit account de inzender voor op rollen gebaseerd toegangsbeheer (RBAC) toegewezen, die resources van Resource Manager beheert met runbooks.
* Een klassiek Uitvoeren als-account. Door dit account wordt een beheercertificaat geüpload, dat wordt gebruikt om resources van Service Management of klassieke resources te beheren met runbooks.

Het maken van een Automation-account vereenvoudigt voor u het proces, zodat u sneller runbooks kunt maken en implementeren ter ondersteuning van uw automatiseringsbehoeften.

Met een Uitvoeren als- en een klassiek Uitvoeren als-account kunt u:

* Een gestandaardiseerde manier voor Azure-verificatie bieden bij het beheer van Resource Manager- of Service Management-resources vanuit runbooks in Azure Portal.
* Het gebruik automatiseren van globale runbooks die u kunt configureren in Azure-waarschuwingen.

> [!NOTE]
> De [waarschuwingsintegratiefunctie van Azure](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) met globale Automation-runbooks vereist een Automation-account dat met een Uitvoeren als- en klassiek Uitvoeren als-account is geconfigureerd. U kunt een Automation-account selecteren waarvoor al een Uitvoeren als- en klassiek Uitvoeren als-account zijn gedefinieerd, maar u kunt er ook voor kiezen een nieuw Automation-account te maken.
>  

In dit artikel wordt uitgelegd hoe u een Automation-account maakt vanuit Azure Portal, hoe u een Automation-account bijwerkt met Azure PowerShell, hoe u de accountconfiguratie beheert en hoe u verificatie in runbooks uitvoert.

Voordat u begint met het maken van een Automation-account, is het belangrijk dat u het volgende voor ogen houdt:

* Het maken van een Automation-account heeft geen invloed op Automation-accounts die u mogelijk al hebt gemaakt in het klassieke of het Resource Manager-implementatiemodel.
* Het proces werkt alleen voor Automation-accounts die u in Azure Portal maakt. Wanneer u een account probeert te maken vanuit de klassieke Azure-portal, wordt de configuratie van het Uitvoeren als-account niet gerepliceerd.
* Als u al runbooks en assets (zoals planningen of variabelen) hebt voor het beheren van klassieke resources en u wilt dat runbooks worden geverifieerd met het nieuwe klassieke Uitvoeren als-account, gaat u op een van de volgende twee manieren te werk:

  * Volg de instructies in de sectie 'Uw Uitvoeren als-account beheren' om een klassiek Uitvoeren als-account te maken. 
  * Gebruik het PowerShell-script in de sectie 'Uw Automation-account bijwerken met behulp van PowerShell' om uw bestaande account bij te werken.
* Als u wilt verifiëren met het nieuwe Uitvoeren als-account en het klassieke Uitvoeren als-Automation-account, moet u de bestaande runbooks wijzigen met de voorbeeldcode uit de sectie [Voorbeelden van verificatiecode](#authentication-code-examples).

    >[!NOTE]
    >Het Uitvoeren als-account is bedoeld voor verificatie met Resource Manager-resources met behulp van de principal-service op basis van certificaten. Het klassieke Uitvoeren als-account is bedoeld voor verificatie met Service Management-resources met een beheercertificaat.

## <a name="create-an-automation-account-from-the-azure-portal"></a>Een Automation-account maken vanuit Azure Portal
In deze sectie kunt u een Azure Automation-account maken vanuit Azure Portal, dat op zijn beurt zowel een Uitvoeren als-account als een klassiek Uitvoeren als-account maakt.

>[!NOTE]
>Voor het maken van een Automation-account moet u lid zijn van de rol Servicebeheerders of medebeheerder zijn van het abonnement dat toegang verleent tot het abonnement. U moet ook als gebruiker worden toegevoegd aan het standaard Active Directory-exemplaar van dat abonnement. Aan het account hoeft geen bevoorrechte rol te worden toegewezen.
>
>Als u geen lid bent van het Active Directory-exemplaar van dat abonnement voordat u wordt toegevoegd aan de rol van medebeheerder van het abonnement, wordt u als gast toegevoegd aan Active Directory. In dat geval wordt de waarschuwing 'U hebt geen machtigingen voor het maken...' weergegeven op de blade **Automation-account toevoegen**.
>
>Gebruikers die zijn toegevoegd aan de rol Medebeheerder, kunnen worden verwijderd uit het Active Directory-exemplaar van het abonnement en opnieuw worden toegevoegd, zodat ze een volledige gebruiker worden in Active Directory. U kunt deze situatie controleren in het deelvenster **Azure Active Directory** in Azure Portal door **Gebruikers en groepen** te selecteren. Selecteer vervolgens **Alle gebruikers**, daarna de specifieke gebruiker en tot slot **Profiel**. De waarde van het kenmerk **Gebruikerstype** onder het gebruikersprofiel mag niet gelijk zijn aan **Gast**.
>

1. Meld u aan bij Azure Portal met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.

2. Selecteer **Automation-accounts**.

3. Klik op de blade **Automation-accounts** op **Toevoegen**.
De blade **Automation-account toevoegen** wordt geopend.

 ![De blade 'Automation-account toevoegen'](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > Als uw account geen lid is van de rol Abonnementsbeheerders en geen medebeheerder van het abonnement is, wordt op de blade **Automation-account toevoegen** de volgende waarschuwing weergegeven:
   >
   >![Waarschuwing bij Automation-account toevoegen](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. Typ op de blade **Automation-account toevoegen** in het vak **Naam** een naam voor uw nieuwe Automation-account.

5. Als u meer dan één abonnement hebt, gaat u als volgt te werk:

    a. Geef er onder **Abonnement** een op voor het nieuwe account.

    b. Klik onder **Resourcegroep** op **Nieuwe maken** of **Bestaande gebruiken**.

    c. Geef onder **Locatie** een Azure-datacenter op.

6. Selecteer onder **Een Uitvoeren als-account voor Azure maken** de optie **Ja** en klik vervolgens op **Maken**.

   > [!NOTE]
   > Als u niet kiest voor het maken van het Uitvoeren als-account (door **Nee** te selecteren), wordt er een waarschuwing weergegeven op de blade **Automation-account toevoegen**. Hoewel het account wordt gemaakt in Azure Portal, heeft het geen overeenkomstige verificatie-id in de adreslijstservice van het klassieke of Resource Manager-abonnement. Als gevolg daarvan heeft het account geen toegang tot resources in uw abonnement. Met dit scenario wordt voorkomen dat runbooks die naar dit account verwijzen, taken verifiëren en uitvoeren met resources in die implementatiemodellen.
   >
   > ![Waarschuwingsbericht op de blade 'Automation-account toevoegen'](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > Omdat de service-principal niet is gemaakt, wordt de rol Inzender bovendien niet toegewezen.
   >

7. Terwijl in Azure het Automation-account wordt gemaakt, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

### <a name="resources"></a>Resources
Wanneer het Automation-account is gemaakt, worden er automatisch verschillende resources voor u gemaakt. De resources worden in de volgende twee tabellen samengevat:

#### <a name="run-as-account-resources"></a>Resources voor Uitvoeren als-account

| Resource | Beschrijving |
| --- | --- |
| AzureAutomationTutorial Runbook | Een voorbeeld van een grafisch runbook dat laat zien hoe u verifieert met behulp van het Uitvoeren als-account en dat alle Resource Managers-resources ophaalt. |
| AzureAutomationTutorialScript Runbook | Een voorbeeld van een PowerShell-runbook dat laat zien hoe u verifieert met behulp van het Uitvoeren als-account en dat alle Resource Manager-resources ophaalt. |
| AzureRunAsCertificate | Het certificaatasset dat automatisch wordt gemaakt tijdens het maken van een Automation-account. U kunt ook het volgende PowerShell-script gebruiken voor een bestaand account. Met het certificaat kunt u verifiëren met Azure zodat u Azure Resource Manager-resources kunt beheren vanuit runbooks. Het certificaat is één jaar geldig. |
| AzureRunAsConnection | Het verbindingsasset dat automatisch wordt gemaakt tijdens het maken van een Automation-account. U kunt ook het PowerShell-script gebruiken voor een bestaand account. |

#### <a name="classic-run-as-account-resources"></a>Resources voor klassiek Uitvoeren als-account

| Resource | Beschrijving |
| --- | --- |
| AzureClassicAutomationTutorial Runbook | Een voorbeeld van een grafisch runbook dat alle klassieke virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel in een abonnement, ophaalt met het klassieke Uitvoeren als-account (certificaat) en vervolgens de VM-naam en -status schrijft. |
| AzureClassicAutomationTutorial Script Runbook | Een voorbeeld van een PowerShell-runbook dat alle klassieke virtuele machines in een abonnement ophaalt met het klassieke Uitvoeren als-account (certificaat) en vervolgens de VM-naam en -status schrijft. |
| AzureClassicRunAsCertificate | Het certificaatasset dat automatisch wordt gemaakt en dat u gebruikt voor verificatie met Azure, zodat u klassieke Azure-resources kunt beheren vanuit runbooks. Het certificaat is één jaar geldig. |
| AzureClassicRunAsConnection | Het verbindingsasset dat automatisch wordt gemaakt en dat u gebruikt voor verificatie met Azure, zodat u klassieke Azure-resources kunt beheren vanuit runbooks. |

## <a name="verify-run-as-authentication"></a>Uitvoeren als-verificatie verifiëren
Voer een kleine test uit om te bevestigen dat u kunt verifiëren met het nieuwe Uitvoeren als-account.

1. Open in Azure Portal het Automation-account dat u eerder hebt gemaakt.

2. Klik op de tegel **Runbooks** om de lijst met runbooks te openen.

3. Selecteer het runbook **AzureAutomationTutorialScript** en klik op **Start** om het runbook te starten. De volgende gebeurtenissen vinden plaats:
 * Er wordt een [runbooktaak](automation-runbook-execution.md) gemaakt, de blade **Taak** wordt weergegeven en in de tegel **Taaksamenvatting** wordt de taakstatus weergegeven.
 * In eerste instantie is de taakstatus **In de wachtrij geplaatst**. Hiermee wordt aangegeven dat er wordt gewacht tot er in de cloud een runbook worker beschikbaar is.
 * De status wordt **Wordt gestart** wanneer de taak wordt geclaimd door een worker.
 * De status wordt **Wordt uitgevoerd** wanneer de uitvoering van het runbook start.
 * Wanneer de runbooktaak is voltooid, wordt de status **Voltooid** weergegeven.

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. Klik op de tegel **Uitvoer** als u de gedetailleerde resultaten van het runbook wilt bekijken.  
De blade **Uitvoer** wordt weergegeven. Hierop ziet u dat het runbook alle beschikbare resources in de resourcegroep heeft geverifieerd en er een lijst van heeft opgehaald.

5. Sluit de blade **Uitvoer** om terug te keren naar de blade **Taaksamenvatting**.

6. Sluit de blade **Taaksamenvatting** en het bijbehorende runbook **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Klassieke Uitvoeren als-verificatie verifiëren
Voer een soortgelijke test uit om te bevestigen dat u kunt verifiëren met het nieuwe klassieke Uitvoeren als-account.

1. Open in Azure Portal het Automation-account dat u eerder hebt gemaakt.

2. Klik op de tegel **Runbooks** om de lijst met runbooks te openen.

3. Selecteer het runbook **AzureClassicAutomationTutorialScript** en klik op **Start** om het runbook te starten. De volgende gebeurtenissen vinden plaats:

 * Er wordt een [runbooktaak](automation-runbook-execution.md) gemaakt, de blade **Taak** wordt weergegeven en in de tegel **Taaksamenvatting** wordt de taakstatus weergegeven.
 * In eerste instantie is de taakstatus **In de wachtrij geplaatst**. Hiermee wordt aangegeven dat er wordt gewacht tot er in de cloud een runbook worker beschikbaar is.
 * De status wordt **Wordt gestart** wanneer de taak wordt geclaimd door een worker.
 * De status wordt **Wordt uitgevoerd** wanneer de uitvoering van het runbook start.
 * Wanneer de runbooktaak is voltooid, wordt de status **Voltooid** weergegeven.

    ![Runbooktest beveiligingsprincipal](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. Klik op de tegel **Uitvoer** als u de gedetailleerde resultaten van het runbook wilt bekijken.  
De blade **Uitvoer** wordt weergegeven. Hierop ziet u dat het runbook alle klassieke virtuele machines heeft geverifieerd en er een lijst van heeft opgehaald.

5. Sluit de blade **Uitvoer** om terug te keren naar de blade **Taaksamenvatting**.

6. Sluit de blade **Taaksamenvatting** en het bijbehorende runbook **AzureAutomationTutorialScript**.

## <a name="managing-your-run-as-account"></a>Uw Uitvoeren als-account beheren
Op een bepaald moment voordat uw Automation-account verloopt, moet u het certificaat vernieuwen. Als u denkt dat het Uitvoeren als-account is aangetast, kunt u het verwijderen en opnieuw maken. In deze sectie wordt besproken hoe u deze bewerkingen uitvoert.

### <a name="self-signed-certificate-renewal"></a>Zelfondertekend certificaat vernieuwen
Het zelfondertekende certificaat dat u voor het Uitvoeren als-account hebt gemaakt, verloopt één jaar na de aanmaakdatum. U kunt het certificaat op elk gewenst moment vernieuwen voordat het verloopt. Als u het certificaat vernieuwt, blijft het huidige geldige certificaat behouden om ervoor te zorgen dat eventuele runbooks die nog in de wachtrij staan of nog actief zijn, en die worden geverifieerd met het Uitvoeren als-account, niet negatief worden beïnvloed. Het certificaat blijft geldig tot de vervaldatum.

> [!NOTE]
> Als u uw Uitvoeren als-account voor Automation hebt geconfigureerd om een certificaat te gebruiken dat is uitgegeven door de certificeringsinstantie van uw bedrijf en u deze optie gebruikt, wordt het bedrijfscertificaat vervangen door een zelfondertekend certificaat.

Ga als volgt te werk om het certificaat te vernieuwen:

1. Open in Azure Portal het Automation-account.

2. Selecteer op de blade **Automation-account** in het deelvenster **Eigenschappen van account** onder **Accountinstellingen** de optie **Uitvoeren als-accounts**.

    ![Eigenschappendeelvenster voor Automation-account](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. Selecteer op de blade **Uitvoeren als-accounts** het Uitvoeren als- of klassieke Uitvoeren als-account waarvoor u het certificaat wilt vernieuwen.

4. Klik op de blade **Eigenschappen** voor het geselecteerde account op **Certificaat vernieuwen**.

    ![Certificaat vernieuwen voor Uitvoeren als-account](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. Terwijl het certificaat wordt gemaakt, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

### <a name="delete-a-run-as-or-classic-run-as-account"></a>Een Uitvoeren als- of klassiek Uitvoeren als-account verwijderen
In dit gedeelte wordt beschreven hoe u uw Uitvoeren als- of klassieke Uitvoeren als-account kunt verwijderen en opnieuw kunt maken. Als u deze actie uitvoert, blijft het Automation-account behouden. Nadat u het Uitvoeren als- of klassieke Uitvoeren als-account hebt verwijderd, kunt u het opnieuw maken in Azure Portal.

1. Open in Azure Portal het Automation-account.

2. Selecteer op de blade **Automation-account** in het eigenschappendeelvenster van het account de optie **Uitvoeren als-accounts**.

3. Selecteer op de blade **Uitvoeren als-accounts** het Uitvoeren als- of klassieke Uitvoeren als-account dat u wilt verwijderen. Klik vervolgens op de blade **Eigenschappen** voor het geselecteerde account op **Verwijderen**.

 ![Uitvoeren als-account verwijderen](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. Terwijl het account wordt verwijderd, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

5. Nadat het account is verwijderd, kunt u het opnieuw maken door op de eigenschappenblade **Uitvoeren als-accounts** de optie **Azure Uitvoeren als-account** te selecteren.

 ![Het Automation uitvoeren als-account opnieuw maken](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a>Onjuiste configuratie
Bepaalde configuratie-items die nodig zijn voor het juist functioneren van het Uitvoeren als- of klassieke Uitvoeren als-account, zijn mogelijk verwijderd of niet juist gemaakt tijdens de initiële configuratie. Dit zijn onder meer de volgende items:

* Certificaatasset
* Verbindingsasset
* Uitvoeren als-account is verwijderd uit de rol Inzender
* Service-principal of toepassing in Azure AD

In het voorgaande en andere gevallen van onjuiste configuratie detecteert het Automation-account de wijzigingen en geeft het de status *Onvolledig* weer op de eigenschappenblade **Uitvoeren als-accounts** voor het account.

![Onvolledige Uitvoeren als-configuratiestatus](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

Als u het Uitvoeren als-account selecteert, wordt het volgende foutbericht weergegeven in het deelvenster **Eigenschappen** van het account:

![Waarschuwingsbericht Onvolledige Uitvoeren als-configuratie](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).

U kunt deze problemen met het Uitvoeren als-account snel oplossen door het account te verwijderen en opnieuw te maken.

## <a name="update-your-automation-account-by-using-powershell"></a>Uw Automation-account bijwerken met behulp van PowerShell
In de volgende gevallen kunt u PowerShell gebruiken om uw bestaande Automation-account bij te werken:

* U maakt wel een Automation-account, maar geen Uitvoeren als-account.
* U gebruikt al een Automation-account voor het beheer van Resource Manager-resources en u wilt het account bijwerken met het Uitvoeren als-account voor runbookverificatie.
* U gebruikt al een Automation-account voor het beheer van klassieke resources en u wilt dit bijwerken, zodat u het klassieke Uitvoeren als-account kunt gebruiken in plaats van een nieuw account te maken, en uw runbooks en activa daarnaartoe te migreren.   
* U wilt een Uitvoeren als-account maken en een klassiek Uitvoeren als-account via een certificaat dat is uitgegeven door de certificeringsinstantie van uw bedrijf.

Het script heeft de volgende vereisten:

* Het script kan alleen worden uitgevoerd op Windows 10 en Windows Server 2016 met Azure Resource Manager-modules 2.01 en hoger. Uitvoeren wordt niet ondersteund in eerdere versies van Windows.
* Azure PowerShell 1.0 en hoger. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie over de PowerShell 1.0-release.
* Een Automation-account waarnaar wordt verwezen als de waarde voor de parameter *– AutomationAccountName* en *- ApplicationDisplayName* in het volgende PowerShell-script.

Ga als volgt te werk om de waarden op te halen voor *SubscriptionID*, *ResourceGroup* en *AutomationAccountName*, die vereiste parameters zijn voor de scripts:
1. Selecteer in Azure Portal uw Automation-account op de blade **Automation-account** en selecteer **Alle instellingen**. 
2. Selecteer op de blade **Alle instellingen** onder **Accountinstellingen** de optie **Eigenschappen**. 
3. Let op de waarden op de blade **Eigenschappen**.

![De blade Eigenschappen voor het Automation-account](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a>Een PowerShell-script voor Uitvoeren als-account maken
Dit PowerShell-script biedt ondersteuning voor de volgende configuraties:

* Een Uitvoeren als-account maken met een zelfondertekend certificaat.
* Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat.
* Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een bedrijfscertificaat.
* Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat in de cloud van Azure Government.

Afhankelijk van de configuratieoptie die u selecteert, maakt het script de volgende items.

**Voor Uitvoeren als-accounts:**

* Er wordt een Azure AD-toepassing gemaakt die wordt geëxporteerd met het zelfondertekende certificaat of de openbare sleutel van het bedrijfscertificaat en een service-principalaccount voor deze toepassing in Azure AD. Daarnaast wordt voor dit account de rol Inzender toegewezen in uw huidige abonnement. U kunt deze instelling wijzigen in Eigenaar of een andere rol. Zie [Op rollen gebaseerd toegangsbeheer in Azure Automation](automation-role-based-access-control.md) voor meer informatie.
* Er wordt een Automation-certificaatasset gemaakt met de naam *AzureRunAsCertificate* in het opgegeven Automation-account. Het certificaatasset bevat de persoonlijke sleutel van het certificaat die wordt gebruikt door de Azure AD-toepassing.
* Er wordt een Automation-verbindingsasset gemaakt met de naam *AzureRunAsConnection* in het opgegeven Automation-account. Het verbindingsasset bevat de toepassing-id, tenant-id, abonnement-id en certificaatvingerafdruk.

**Voor klassieke uitvoeren als-accounts:**

* Er wordt een Automation-certificaatasset gemaakt met de naam *AzureClassicRunAsCertificate* in het opgegeven Automation-account. Het certificaatasset bevat de persoonlijke sleutel van het certificaat die wordt gebruikt door het beheercertificaat.
* Er wordt een Automation-verbindingsasset gemaakt met de naam *AzureClassicRunAsConnection* in het opgegeven Automation-account. Het verbindingsasset bevat de naam van het abonnement, de abonnements-id en de certificaatassetnaam.

>[!NOTE]
> Als u ervoor kiest om een klassiek Uitvoeren als-account te maken, moet u na het uitvoeren van het script het openbare certificaat (bestandsnaamextensie .cer) uploaden naar het beheerarchief voor het abonnement waarin het Automation-account is gemaakt.
> 

Ga als volgt te werk om het script uit te voeren en het certificaat te uploaden:

1. Sla het volgende script op uw computer op. Sla het bestand in dit voorbeeld op met de bestandsnaam *New-RunAsAccount.ps1*.

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
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

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
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
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

        # Create a Run As account by using a service principal
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
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
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
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Klik op uw computer op **Start** en start vervolgens **Windows PowerShell** met verhoogde gebruikersrechten.

3. Ga vanuit de verhoogde PowerShell opdrachtregel-shell naar de map die het script bevat dat u in stap 1 hebt gemaakt.

4. Voer het script uit met de parameterwaarden voor de configuratie die u nodig hebt.

    **Een Uitvoeren als-account maken met een zelfondertekend certificaat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een bedrijfscertificaat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Een Uitvoeren als-account en een klassiek Uitvoeren als-account maken met een zelfondertekend certificaat in de cloud van Azure Government**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Nadat het script is uitgevoerd, wordt u gevraagd zich te verifiëren met Azure. Meld u aan met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.
    >
    >

Let op het volgende nadat het script is uitgevoerd:
* Als u een klassiek Uitvoeren als-account hebt gemaakt met een zelfondertekend openbaar certificaat (.cer-bestand), wordt het door het script gemaakt en opgeslagen in de map met tijdelijke bestanden op uw computer, onder het gebruikersprofiel *%USERPROFILE%\AppData\Local\Temp*, dat u hebt gebruikt voor het uitvoeren van de PowerShell-sessie.
* Gebruik dit certificaat als u een klassiek Uitvoeren als-account hebt gemaakt met een openbaar certificaat (.cer-bestand). Volg de instructies voor het [uploaden van een API-beheercertificaat naar de klassieke Azure-portal](../azure-api-management-certs.md) en valideer vervolgens de configuratie van de referenties met Service Management-resources met behulp van de [voorbeeldcode om te verifiëren met Service Management-resources](#sample-code-to-authenticate-with-service-management-resources). 
* Als u *geen* klassiek Uitvoeren als-account hebt gemaakt, verifieert u met Resource Manager-resources en valideert u de configuratie van de referenties met behulp van de [voorbeeldcode om verificatie met Service Management-resources uit te voeren](#sample-code-to-authenticate-with-resource-manager-resources).

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Voorbeeldcode voor verificatie bij Resource Manager-resources
U kunt de volgende bijgewerkte voorbeeldcode, die is overgenomen uit het voorbeeldrunbook *AzureAutomationTutorialScript*, gebruiken om de verificatie uit te voeren met behulp van het Uitvoeren als-account voor het beheer van Resource Manager-resources met uw runbooks.

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

Om u te helpen eenvoudig tussen meerdere abonnementen te schakelen, bevat het script twee extra regels met code voor de ondersteuning van verwijzingen naar de context van een abonnement. Een variabele asset met de naam *SubscriptionId* bevat de id van het abonnement. Na de cmdlet-instructie `Add-AzureRmAccount` wordt de cmdlet [ `Set-AzureRmContext` ](/powershell/module/azurerm.profile/set-azurermcontext) vermeld met de parameter *-SubscriptionId*. Als de naam van de variabele te algemeen is, kunt u deze wijzigen door er een voorvoegsel aan toe te voegen of door er een andere naamgevingsconventie op toe te passen, zodat u de variabele gemakkelijker kunt identificeren. U kunt ook de parameter *-SubscriptionName* in plaats van *-SubscriptionId* gebruiken met een bijbehorende variabele asset.

De cmdlet die u gebruikt voor verificatie in het runbook, `Add-AzureRmAccount`, gebruikt de parameterset *ServicePrincipalCertificate*. In plaats van gebruikersreferenties wordt voor verificatie het certificaat van de service-principal gebruikt.

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Voorbeeldcode voor verificatie met Service Management-resources
U kunt de volgende voorbeeldcode, die is overgenomen uit het voorbeeldrunbook *AzureClassicAutomationTutorialScript*, gebruiken om verificatie uit te voeren met het klassieke Uitvoeren als-account, voor het beheer van klassieke resources met uw runbooks.

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
* [Toepassings- en service-principalobjecten in Azure AD](../active-directory/active-directory-application-objects.md)
* [Op rollen gebaseerd toegangsbeheer in Azure Automation](automation-role-based-access-control.md)
* [Overzicht van certificaten voor Azure Cloud Services](../cloud-services/cloud-services-certs-create.md)

