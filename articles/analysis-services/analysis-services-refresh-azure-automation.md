---
title: Vernieuwen van Azure Analysis Services-modellen met Azure Automation | Microsoft Docs
description: Informatie over het model worden vernieuwd met behulp van Azure Automation-code.
author: chrislound
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: chlound
ms.openlocfilehash: 1897193f0ae781029a7303c42ca8eeaa51389892
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64920577"
---
# <a name="refresh-with-azure-automation"></a>Vernieuwen met Azure Automation

Met behulp van Azure Automation en PowerShell-Runbooks, kunt u geautomatiseerde gegevensvernieuwingsbewerkingen op uw modellen in tabelvorm Azure analyse uitvoeren.  

Het voorbeeld in dit artikel wordt de [PowerShell SqlServer-modules](https://docs.microsoft.com/powershell/module/sqlserver/?view=sqlserver-ps).

Een voorbeeld van een PowerShell-Runbook dat laat zien voor het vernieuwen van een model vindt u verderop in dit artikel.  

## <a name="authentication"></a>Verificatie

Alle-aanroepen moeten worden geverifieerd met een geldig token voor Azure Active Directory (OAuth 2).  Het voorbeeld in dit artikel wordt een SPN (Service Principal) gebruiken om te verifiëren met Azure Analysis Services.

Zie voor meer informatie over het maken van een Service-Principal,]

## <a name="prerequisites"></a>Vereisten

> [!IMPORTANT]
> Het volgende voorbeeld wordt ervan uitgegaan dat de Azure Analysis Services-firewall is uitgeschakeld. Als de firewall is ingeschakeld, moet het openbare IP-adres van de aanvrager van de aanvraag voor goedkeuring in de firewall.

### <a name="install-sqlserver-modules-from-powershell-gallery"></a>SQL Server-modules installeren vanuit PowerShell gallery.

1. Klik in uw Azure Automation-Account op **Modules**, klikt u vervolgens **bladeren in galerie**.

2. Zoek in de zoekbalk typt, **SqlServer**.

    ![Modules zoeken](./media/analysis-services-refresh-azure-automation/1.png)

3. Selecteer de SQL Server en klik vervolgens op **importeren**.
 
    ![Import-Module](./media/analysis-services-refresh-azure-automation/2.png)

4. Klik op **OK**.
 
### <a name="create-a-service-principal-spn"></a>Maken van een Service-Principal (SPN)

Zie voor meer informatie over het maken van een Service-Principal, [een service-principal maken met behulp van Azure portal](../active-directory/develop/howto-create-service-principal-portal.md).

### <a name="configure-permissions-in-azure-analysis-services"></a>Machtigingen te configureren in Azure Analysis Services
 
De Service-Principal die u maakt, moet machtigingen voor serverbeheerders hebben op de server. Zie voor meer informatie, [een service-principal toevoegen aan de serverbeheerdersrol](analysis-services-addservprinc-admins.md).

## <a name="design-the-azure-automation-runbook"></a>Azure Automation-Runbook ontwerpen

1. In het Automation-Account, maakt u een **referenties** resource die wordt gebruikt voor het veilig opslaan van de Service-Principal.

    ![referentie maken](./media/analysis-services-refresh-azure-automation/6.png)

2. Geef de details voor de referentie.  Voor de **gebruikersnaam**, voer de **SPN ClientId**, voor de **wachtwoord**, voer de **SPN geheim**.

    ![referentie maken](./media/analysis-services-refresh-azure-automation/7.png)

3. Automation-Runbook importeren

    ![Runbook importeren](./media/analysis-services-refresh-azure-automation/8.png)

4. Bladeren naar de **vernieuwen Model.ps1** bestand, bieden een **naam** en **beschrijving**, en klik vervolgens op **maken**.

    ![Runbook importeren](./media/analysis-services-refresh-azure-automation/9.png)

5. Wanneer het Runbook is gemaakt, gaat deze automatisch naar de bewerkingsmodus.  Selecteer **Publiceren**.

    ![Runbook publiceren](./media/analysis-services-refresh-azure-automation/10.png)

    > [!NOTE]
    > De referentie-resource die is gemaakt eerder is opgehaald door het runbook met behulp van de **Get-AutomationPSCredential** opdracht.  Met deze opdracht wordt vervolgens doorgegeven aan de **Invoke-ProcessASADatabase** PowerShell-opdracht voor het uitvoeren van de verificatie met Azure Analysis Services.

6. Het runbook testen door te klikken op **Start**.

    ![Het Runbook starten](./media/analysis-services-refresh-azure-automation/11.png)

7. Vul de **DATABASENAME**, **ANALYSISSERVER**, en **REFRESHTYPE** parameters en klik vervolgens op **OK**. De **WEBHOOKDATA** parameter is niet vereist wanneer het Runbook handmatig wordt uitgevoerd.

    ![Het Runbook starten](./media/analysis-services-refresh-azure-automation/12.png)

Als het Runbook met succes is uitgevoerd, ontvangt u de volgende uitvoer:

![Geslaagde uitvoering](./media/analysis-services-refresh-azure-automation/13.png)

## <a name="use-a-self-contained-azure-automation-runbook"></a>Een zelfstandig Azure Automation-Runbook gebruiken

Het Runbook kan worden geconfigureerd voor het Azure Analysis Services-model vernieuwen op basis van de geplande trigger.

Dit kan als volgt worden geconfigureerd:

1. Klik in het Automation-Runbook **planningen**, klikt u vervolgens **een schema toevoegen**.
 
    ![Planning maken](./media/analysis-services-refresh-azure-automation/14.png)

2. Klik op **planning** > **maakt u een nieuwe planning**, en vul vervolgens de gegevens.

    ![Planning configureren](./media/analysis-services-refresh-azure-automation/15.png)

3. Klik op **Create**.

4. Vul de parameters voor de planning in. Deze gebruikt telkens wanneer het Runbook wordt geactiveerd. De **WEBHOOKDATA** parameter moet leeg worden gelaten wanneer via een planning wordt uitgevoerd.

    ![Parameters configureren](./media/analysis-services-refresh-azure-automation/16.png)

5. Klik op **OK**.

## <a name="consume-with-data-factory"></a>Gebruiken met Data Factory

Het runbook met behulp van Azure Data Factory gebruiken, maakt u eerst een **Webhook** voor het runbook. De **Webhook** biedt een URL die kan worden aangeroepen via een Azure Data Factory-web-activiteit.

> [!IMPORTANT]
> Maakt een **Webhook**, de status van het Runbook moet **gepubliceerd**.

1. Klik in uw Automation-Runbook **Webhooks**, en klik vervolgens op **Webhook toevoegen**.

   ![Webhook toevoegen](./media/analysis-services-refresh-azure-automation/17.png)

2. De Webhook Geef een naam en een vervaldatum.  De naam alleen identificeert de Webhook in Automation-Runbook, deze geen deel uitmaken van de URL.

   >[!CAUTION]
   >Zorg ervoor dat u de URL kopiëren voordat de wizard wordt gesloten als u niet meer eenmaal gesloten terughalen.
    
   ![Webhook configureren](./media/analysis-services-refresh-azure-automation/18.png)

    De parameters voor de webhook kunnen leeg blijven.  Bij het configureren van de webactiviteit Azure Data Factory, kunnen de parameters worden doorgegeven in de hoofdtekst van de webaanroep.

3. Configureer in Data Factory, een **web-activiteit**

### <a name="example"></a>Voorbeeld

   ![Voorbeeld-Web-activiteit](./media/analysis-services-refresh-azure-automation/19.png)

De **URL** is de URL die is gemaakt op basis van de Webhook.

De **hoofdtekst** is een JSON-document waarin de volgende eigenschappen:


|Eigenschap  |Value  |
|---------|---------|
|**AnalysisServicesDatabase**     |De naam van de Azure Analysis Services-database <br/> Voorbeeld: AdventureWorksDB         |
|**AnalysisServicesServer**     |De naam van de Azure Analysis Services-server. <br/> Voorbeeld: https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/         |
|**DatabaseRefreshType**     |Het type van vernieuwen om uit te voeren. <br/> Voorbeeld: Volledig         |

Voorbeeld van JSON-hoofdtekst:

```json
{
    "AnalysisServicesDatabaseName": "AdventureWorksDB",
    "AnalysisServicesServer": "asazure://westeurope.asazure.windows.net/MyAnalysisServer",
    "DatabaseRefreshType": "Full"
}
```

Deze parameters worden gedefinieerd in het runbook PowerShell-script.  Wanneer de webactiviteit wordt uitgevoerd, is de JSON-nettolading doorgegeven WEBHOOKDATA.

Dit is gedeserialiseerd en opgeslagen als PowerShell-parameters, die vervolgens worden gebruikt door de Invoke-ProcesASDatabase PowerShell-opdracht.

![Gedeserialiseerde Webhook](./media/analysis-services-refresh-azure-automation/20.png)

## <a name="use-a-hybrid-worker-with-azure-analysis-services"></a>Een Hybrid Worker gebruiken met Azure analyseservices

Een Azure-Machine met een statisch openbaar IP-adres kan worden gebruikt als een Azure Automation Hybrid Worker.  Dit openbare IP-adres kan vervolgens worden toegevoegd aan de firewall van Azure Analysis Services.

> [!IMPORTANT]
> Zorg ervoor dat het openbare IP-adres van virtuele Machine is geconfigureerd als statische.
>
>Zie voor meer informatie over het configureren van Azure Automation Hybrid Workers [resources in uw datacentrum en de cloud automatiseren met behulp van Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md#install-a-hybrid-runbook-worker).

Wanneer een Hybrid Worker is geconfigureerd, maakt u een Webhook zoals beschreven in de sectie [verbruiken met Data Factory](#consume-with-data-factory).  Het enige verschil is hier om te selecteren wordt de **uitvoeren op** > **Hybrid Worker** optie bij het configureren van de Webhook.

Voorbeeld van de webhook met behulp van Hybrid Worker:

![Voorbeeld van de Hybrid Worker-Webhook](./media/analysis-services-refresh-azure-automation/21.png)

## <a name="sample-powershell-runbook"></a>Voorbeeld PowerShell-Runbook

Het volgende codefragment wordt een voorbeeld van het uitvoeren van het vernieuwen van de Azure Analysis Services-model met behulp van een PowerShell-Runbook.

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData,

    [Parameter (Mandatory = $false)]
    [String] $DatabaseName,
    [Parameter (Mandatory = $false)]
    [String] $AnalysisServer,
    [Parameter (Mandatory = $false)]
    [String] $RefreshType
)

$_Credential = Get-AutomationPSCredential -Name "ServicePrincipal"

# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData)
{ 
    # Retrieve AAS details from Webhook request body
    $atmParameters = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)
    Write-Output "CredentialName: $($atmParameters.CredentialName)"
    Write-Output "AnalysisServicesDatabaseName: $($atmParameters.AnalysisServicesDatabaseName)"
    Write-Output "AnalysisServicesServer: $($atmParameters.AnalysisServicesServer)"
    Write-Output "DatabaseRefreshType: $($atmParameters.DatabaseRefreshType)"
    
    $_databaseName = $atmParameters.AnalysisServicesDatabaseName
    $_analysisServer = $atmParameters.AnalysisServicesServer
    $_refreshType = $atmParameters.DatabaseRefreshType
 
    Invoke-ProcessASDatabase -DatabaseName $_databaseName -RefreshType $_refreshType -Server $_analysisServer -ServicePrincipal -Credential $_credential
}
else 
{
    Invoke-ProcessASDatabase -DatabaseName $DatabaseName -RefreshType $RefreshType -Server $AnalysisServer -ServicePrincipal -Credential $_Credential
}
```


## <a name="next-steps"></a>Volgende stappen

[Voorbeelden](analysis-services-samples.md)  
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)
