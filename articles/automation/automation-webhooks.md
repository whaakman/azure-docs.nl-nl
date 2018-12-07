---
title: Een Azure Automation-runbook starten met een webhook
description: Een webhook waarmee een client van een runbook in Azure Automation starten vanuit een HTTP-aanroep.  Dit artikel wordt beschreven hoe u een webhook maakt en over het aanroepen van een voor het starten van een runbook.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 10/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 381f8c5fb59379c0494dabcd22f4675be9535837
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53016688"
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Een Azure Automation-runbook starten met een webhook

Een *webhook* kunt u een bepaald runbook in Azure Automation starten via een afzonderlijke HTTP-aanvraag. Hierdoor kan externe services, zoals Azure DevOps-Services, GitHub, Azure Log Analytics of aangepaste toepassingen runbooks starten zonder het implementeren van een volledige oplossing met behulp van de API van Azure Automation.  
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

U kunt vergelijken webhooks voor andere methoden voor het starten van een runbook in [een runbook starten in Azure Automation](automation-starting-a-runbook.md)

## <a name="details-of-a-webhook"></a>Details van een webhook

De volgende tabel beschrijft de eigenschappen die u voor een webhook configureren moet.

| Eigenschap | Description |
|:--- |:--- |
| Name |U kunt een willekeurige naam die u wilt gebruiken voor een webhook omdat deze geen toegang heeft tot de client opgeven. Dit wordt alleen gebruikt voor u om te identificeren van het runbook in Azure Automation. <br> Als een best practice, moet u de webhook geeft een naam die betrekking hebben op de client die wordt gebruikt. |
| URL |De URL van de webhook is het unieke adres waarmee een client wordt aangeroepen met een HTTP POST naar het runbook dat is gekoppeld aan de webhook starten. Er wordt automatisch gegenereerd bij het maken van de webhook. U kunt een aangepaste URL niet opgeven. <br> <br> De URL bevat een beveiligingstoken waarmee het runbook worden aangeroepen door een derde partij zonder verdere verificatie. Daarom moet deze worden behandeld als een wachtwoord. Uit veiligheidsoverwegingen kunt u alleen de URL in de Azure-portal weergeven op het moment dat de webhook wordt gemaakt. Houd er rekening mee de URL op een veilige locatie op voor toekomstig gebruik. |
| Verloopdatum |Als een certificaat heeft elke webhook een vervaldatum die op dat moment deze kan niet meer worden gebruikt. Deze datum van afloop voor kan worden gewijzigd nadat de webhook wordt gemaakt, zolang de webhook niet is verlopen. |
| Ingeschakeld |Een webhook is standaard ingeschakeld wanneer deze wordt gemaakt. Als u dit op uitgeschakeld instellen, zijn er geen client is kunnen gebruiken. U kunt instellen dat de **ingeschakeld** eigenschap wanneer u de webhook of op elk gewenst moment nadat deze is gemaakt. |

### <a name="parameters"></a>Parameters

Een webhook kunt waarden voor runbookparameters die worden gebruikt wanneer het runbook wordt gestart door die webhook definiëren. De webhook waarden voor de verplichte parameters van het runbook moet bevatten en waarden voor optionele parameters kan bevatten. Een parameterwaarde die is geconfigureerd om een webhook kan worden gewijzigd nadat de webhook wordt gemaakt. Meerdere webhooks die zijn gekoppeld aan één runbook kunt elke andere parameterwaarden gebruiken.

Wanneer een client wordt gestart voor een runbook met behulp van een webhook, overschrijven niet het van de parameterwaarden die zijn gedefinieerd in de webhook. Voor het ontvangen van gegevens van de client, het runbook met de naam één parameter kunt accepteren **$WebhookData** van het type [object] die gegevens bevat die de client in de POST-aanvraag bevat.

![Webhookdata-eigenschappen](media/automation-webhooks/webhook-data-properties.png)

De **$WebhookData** object heeft de volgende eigenschappen:

| Eigenschap | Description |
|:--- |:--- |
| WebhookName |De naam van de webhook. |
| RequestHeader |Hash-tabel met de headers van de binnenkomende POST-aanvraag. |
| RequestBody |De hoofdtekst van de binnenkomende POST-aanvraag. Dit blijft behouden alle opmaak, zoals tekenreeks, JSON, XML, of formulier gecodeerde gegevens. Het runbook moet worden geschreven om te werken met de gegevensindeling die wordt verwacht. |

Er is geen configuratie van de webhook vereist ter ondersteuning van de **$WebhookData** parameter en het runbook is niet vereist om te accepteren. Als het runbook wordt de parameter niet opgeeft, wordt er details van de aanvraag is verzonden vanaf de client genegeerd.

Als u een waarde voor $WebhookData opgeven bij het maken van de webhook, dat de waarde wanneer de webhook wordt het runbook met de gegevens van de client POST-aanvraag gestart wordt genegeerd, zelfs als de client geen gegevens in de aanvraagtekst bevatten. Als u een runbook met $WebhookData met behulp van een andere methode dan een webhook start, kunt u een waarde opgeven voor $Webhookdata die wel wordt herkend door het runbook. Deze waarde moet een object met dezelfde [eigenschappen](#details-of-a-webhook) als $Webhookdata zodat het runbook correct ermee werken kan alsof het is in combinatie met de werkelijke WebhookData die is doorgegeven door een webhook.

Bijvoorbeeld, als u de volgende runbook vanuit de Azure-portal starten zijn en een aantal voorbeelden WebhookData doorgeven wilt voor het testen, omdat de WebhookData is een object, moet deze worden doorgegeven als JSON in de gebruikersinterface.

![WebhookData-parameter van de gebruikersinterface](media/automation-webhooks/WebhookData-parameter-from-UI.png)

Voor de volgende runbook, hebt u de volgende eigenschappen voor de parameter WebhookData:

* WebhookName: *MyWebhook*
* RequestBody: *[{'ResourceGroup': 'myResourceGroup', 'Name': "vm01"}, {'ResourceGroup': 'myResourceGroup', 'Name': "vm02"}]*

Vervolgens wilt u de volgende JSON-waarde in de gebruikersinterface voor de parameter WebhookData doorgeven. Retourneert het volgende voorbeeld door de regelterugloop en regeltekens komt overeen met de indeling die wordt doorgegeven vanuit een webhook.

```json
{"WebhookName":"mywebhook","RequestBody":"[\r\n {\r\n \"ResourceGroup\": \"vm01\",\r\n \"Name\": \"vm01\"\r\n },\r\n {\r\n \"ResourceGroup\": \"vm02\",\r\n \"Name\": \"vm02\"\r\n }\r\n]"}
```

![WebhookData-parameter starten vanuit de gebruikersinterface](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> De waarden van alle invoerparameters die zijn opgegeven, worden geregistreerd met de runbooktaak. Dit betekent dat alle invoer geleverd door de client in de webhookaanvraag is geregistreerd en beschikbaar zijn voor iedereen met toegang tot de automation-taak.  Daarom moet u voorzichtig met gevoelige informatie in de webhookaanroepen.

## <a name="security"></a>Beveiliging

De beveiliging van een webhook, is afhankelijk van de privacy van de URL, waarin een beveiligingstoken dat kan worden aangeroepen. Azure Automation wordt geen verificatie uitgevoerd op de aanvraag, zolang deze is gemaakt met de juiste URL. Om deze reden moet webhooks niet worden gebruikt voor runbooks die uiterst gevoelige functies uitvoeren zonder gebruik van een alternatieve methode voor het valideren van de aanvraag.

U kunt opnemen logische binnen het runbook om te bepalen dat deze door een webhook is aangeroepen door het controleren van de **WebhookName** eigenschap van de parameter $WebhookData. Het runbook kan verdere validatie uitvoeren door te zoeken naar bepaalde informatie in de **RequestHeader** of **RequestBody** eigenschappen.

Een andere strategie is dat het runbook dat sommige validatie van een voorwaarde voor het extern uitvoeren wanneer deze een webhook-aanvraag ontvangen. Neem bijvoorbeeld een runbook dat wordt aangeroepen door GitHub wanneer er sprake is van een nieuwe doorvoer naar een GitHub-opslagplaats. Het runbook kan verbinding maken met GitHub om te valideren dat een nieuwe wijziging voordat u doorgaat opgetreden.

## <a name="creating-a-webhook"></a>Het maken van een webhook

Gebruik de volgende procedure om een nieuwe webhook maken gekoppeld aan een runbook in Azure portal.

1. Uit de **Runbooks pagina** in Azure portal, klikt u op het runbook dat de webhook wordt gestart om de detailpagina ervan weer te geven. Zorg ervoor dat het runbook **Status** is **gepubliceerd**.
2. Klik op **Webhook** aan de bovenkant van de pagina te openen de **Webhook toevoegen** pagina.
3. Klik op **nieuwe webhook maken** openen de **pagina van de webhook maken**.
4. Geef een **naam**, **vervaldatum** voor de webhook en of het moet worden ingeschakeld. Zie [Details van een webhook](#details-of-a-webhook) voor meer informatie deze eigenschappen.
5. Klik op het pictogram voor kopiëren en druk op Ctrl + C om te kopiëren van de URL van de webhook. Noteer het op een veilige plaats. **Als u de webhook maakt, kan u niet de URL opnieuw ophalen.**

   ![Webhook-URL](media/automation-webhooks/copy-webhook-url.png)

1. Klik op **Parameters** waarden opgeven voor de runbookparameters. Als het runbook verplichte parameters heeft, klikt u vervolgens bent u niet de webhook te maken tenzij er waarden zijn verstrekt.
1. Klik op **maken** te maken van de webhook.

## <a name="using-a-webhook"></a>Met behulp van een webhook

Als u wilt een webhook gebruiken nadat deze is gemaakt, moet u de clienttoepassing een HTTP POST met de URL voor de webhook uitgeven. De syntaxis van de webhook is in de volgende indeling:

```http
http://<Webhook Server>/token?=<Token Value>
```

De client ontvangt een van de volgende retourcodes van de POST-aanvraag.

| Code | Tekst | Description |
|:--- |:--- |:--- |
| 202 |Geaccepteerd |De aanvraag is geaccepteerd en het runbook is met succes in de wachtrij geplaatst. |
| 400 |Onjuiste aanvraag |De aanvraag is niet geaccepteerd voor een van de volgende redenen: <ul> <li>De webhook is verlopen.</li> <li>De webhook is uitgeschakeld.</li> <li>Het token in de URL is ongeldig.</li>  </ul> |
| 404 |Niet gevonden |De aanvraag is niet geaccepteerd voor een van de volgende redenen: <ul> <li>De webhook is niet gevonden.</li> <li>Het runbook is niet gevonden.</li> <li>Het account is niet gevonden.</li>  </ul> |
| 500 |Interne serverfout |De URL is geldig, maar er is een fout opgetreden. Verzend de aanvraag. |

Ervan uitgaande dat de aanvraag is geslaagd, bevat het antwoord voor webhook de taak-ID in JSON-indeling als volgt. Het kan een enkele taak-ID bevatten, maar de JSON-indeling kan voor mogelijke toekomstige verbeteringen.

```json
{"JobIds":["<JobId>"]}
```

De client kan niet bepalen wanneer de runbooktaak is voltooid of de status van de voltooiing van de webhook. Kan het deze gegevens met behulp van de taak-ID met een andere methode zoals bepalen [Windows PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationjob) of de [API van Azure Automation](/rest/api/automation/job).

## <a name="renew-webhook"></a>Vernieuwen van een webhook

Wanneer een webhook wordt gemaakt heeft een geldigheidsduur van één jaar. Na dat jaar tijd de webhook automatisch verloopt. Zodra een webhook is verlopen kunnen worden niet opnieuw worden geactiveerd, moet worden verwijderd en opnieuw gemaakt. Als de verlooptijd niet door een webhook is bereikt, kan deze kan worden uitgebreid.

Als u wilt uitbreiden een webhook, gaat u naar het runbook dat de webhook bevat. Selecteer **Webhooks** onder **Resources**. Klik op de webhook die u wilt uitbreiden, Hiermee opent u de **Webhook** pagina.  Kies een nieuwe datum en tijd en klik op **opslaan**.

## <a name="sample-runbook"></a>Voorbeeldrunbook

Het volgende voorbeeldrunbook accepteert de webhookgegevens die zijn en start de virtuele machines dat is opgegeven in de aanvraagtekst. Dit runbook testen in uw Automation-Account onder **Runbooks**, klikt u op **+ toevoegen van een runbook**. Als u niet hoe een runbook te maken weet, raadpleegt u [het maken van een runbook](automation-quickstart-create-runbook.md).

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData
)



# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

    # Check header for message to validate request
    if ($WebhookData.RequestHeader.message -eq 'StartedbyContoso')
    {
        Write-Output "Header has required information"}
    else
    {
        Write-Output "Header missing required information";
        exit;
    }

    # Retrieve VM's from Webhook request body
    $vms = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.

    Write-Output "Authenticating to Azure with service principal and certificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    Write-Output "Get connection asset: $ConnectionAssetName"

    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Output "Authenticating to Azure with service principal." 
            Add-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Output

        # Start each virtual machine
        foreach ($vm in $vms)
        {
            $vmName = $vm.Name
            Write-Output "Starting $vmName"
            Start-AzureRMVM -Name $vm.Name -ResourceGroup $vm.ResourceGroup
        }
}
else {
    # Error
    write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="test-the-example"></a>Het voorbeeld testen

Windows PowerShell wordt het volgende voorbeeld een runbook starten met een webhook. Elke taal waarvoor een HTTP-aanvraag kan een webhook; gebruiken Windows PowerShell wordt gebruikt als een voorbeeld.

Het runbook wordt een lijst met virtuele machines die zijn opgemaakt in JSON in de hoofdtekst van de aanvraag verwacht. Het runbook valideert ook dat de headers een specifiek gedefinieerd bevatten bericht voor het valideren van de beller webhook is ongeldig.

```azurepowershell-interactive
$uri = "<webHook Uri>"

$vms  = @(
            @{ Name="vm01";ResourceGroup="vm01"},
            @{ Name="vm02";ResourceGroup="vm02"}
        )
$body = ConvertTo-Json -InputObject $vms
$header = @{ message="StartedbyContoso"}
$response = Invoke-RestMethod -Method Post -Uri $uri -Body $body -Headers $header
$jobid = (ConvertFrom-Json ($response.Content)).jobids[0]
```

Het volgende voorbeeld ziet u de hoofdtekst van de aanvraag die beschikbaar is voor het runbook in de **RequestBody** eigenschap van **WebhookData**. Dit is opgemaakt als JSON omdat dat de indeling die is opgenomen in de hoofdtekst van de aanvraag is.

```json
[
    {
        "Name":  "vm01",
        "ResourceGroup":  "myResourceGroup"
    },
    {
        "Name":  "vm02",
        "ResourceGroup":  "myResourceGroup"
    }
]
```

De volgende afbeelding ziet u de aanvraag is verzonden vanaf de Windows PowerShell en het resulterende antwoord. De taak-ID is geëxtraheerd uit het antwoord en geconverteerd naar een tekenreeks.

![Knop Webhooks](media/automation-webhooks/webhook-request-response.png)

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het gebruik van Azure Automation actie te ondernemen op Azure-waarschuwingen, [gebruik van een waarschuwing voor het activeren van een Azure Automation-runbook](automation-create-alert-triggered-runbook.md).
