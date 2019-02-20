---
title: Automatisch afstemmen e-mailmeldingen gebruiksaanwijzing - Azure SQL Database | Microsoft Docs
description: E-mailmeldingen voor Azure SQL Database automatisch query afstemmen inschakelen.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 12/19/2018
ms.openlocfilehash: cdd709fa446ffe769c8c57aeb44fe592b12e92d4
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416103"
---
# <a name="email-notifications-for-automatic-tuning"></a>E-mailmeldingen voor automatisch afstemmen

SQL Database aanbevelingen voor afstemming worden gegenereerd door Azure SQL Database [automatisch afstemmen](sql-database-automatic-tuning.md). Deze oplossing wordt continu bewaakt en analyseert workloads van het leveren van de SQL-Databases aangepaste aanbevelingen voor elke afzonderlijke database met betrekking tot het maken van een index, index verwijderen en optimalisatie van de query-uitvoering plannen voor het afstemmen.

SQL Database automatisch afstemmen aanbevelingen kunnen worden weergegeven in de [Azure-portal](sql-database-advisor-portal.md), met de opgehaalde [REST-API](https://docs.microsoft.com/rest/api/sql/databaserecommendedactions/listbydatabaseadvisor) aanroept, of met behulp van [T-SQL](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) en [ PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabaserecommendedaction) opdrachten. In dit artikel is gebaseerd op een PowerShell-script gebruiken om op te halen van de aanbevelingen voor automatische afstemming.

## <a name="automate-email-notifications-for-automatic-tuning-recommendations"></a>E-mailmeldingen voor aanbevelingen voor automatische afstemming automatiseren

De volgende oplossing automatiseert het verzenden van e-mailmeldingen met aanbevelingen voor automatische afstemming. De oplossing beschreven bestaat uit het automatiseren van de uitvoering van een PowerShell-script voor het ophalen van de aanbevelingen voor afstemming met behulp van [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro), en automatisering van het plannen van e-levering met behulp van taak [Microsoft Flow ](https://flow.microsoft.com).

## <a name="create-azure-automation-account"></a>Azure Automation-account maken

Voor het gebruik van Azure Automation, is de eerste stap het maken van een automation-account en te configureren met Azure-resources te gebruiken voor de uitvoering van het PowerShell-script. Zie voor meer informatie over Azure Automation en de mogelijkheden ervan, [aan de slag met Azure automation](https://docs.microsoft.com/azure/automation/automation-offering-get-started).

Volg deze stappen voor het maken van Azure Automation-Account via de methode te selecteren en configureren van Automation-app vanuit de Marketplace:

- Meld u aan bij de Azure-portal
- Klik op '**+ een resource maken**' in de linkerbovenhoek
- Zoeken naar "**Automation**' (druk op enter)
- Klik op de Automation-app in de lijst met zoekresultaten

![Toevoegen van Azure automation](./media/sql-database-automatic-tuning-email-notifications/howto-email-01.png)

- Eenmaal in het deelvenster 'Een Automation-Account maken', klik op '**maken**"
- Vul de vereiste gegevens: Voer een naam in voor dit automation-account, selecteert u uw Azure-abonnement-ID en het Azure-resources moet worden gebruikt voor de uitvoering van het PowerShell-script
- Voor de '**Azure uitvoeren als-account maken**"optie, selecteer **Ja** het configureren van het type van welke PowerShell script wordt uitgevoerd met behulp van Azure Automation-account. Zie voor meer informatie over accounttypen [uitvoeren als-account](https://docs.microsoft.com/azure/automation/automation-create-runas-account)
- Het maken van het automation-account sluiten door te klikken op **maken**

> [!TIP]
> Uw Azure Automation-accountnaam, de abonnements-ID en de resources (zoals kopiëren en plakken in Kladblok) precies hetzelfde als de opgegeven tijdens het maken van de Automation-app te registreren. U hebt deze informatie later nodig.
>

Als u meerdere Azure-abonnementen waarvoor u wilt de dezelfde buildautomatisering hebt, moet u dit proces herhalen voor uw andere abonnementen.

## <a name="update-azure-automation-modules"></a>Azure Automation-modules bijwerken

Maakt gebruik van het PowerShell-script om op te halen voor automatisch afstemmen aanbeveling [Get-AzureRmResource](https://docs.microsoft.com/powershell/module/AzureRM.Resources/Get-AzureRmResource) en [Get-AzureRmSqlDatabaseRecommendedAction](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Get-AzureRmSqlDatabaseRecommendedAction) opdrachten waarvoor updatebeheer van Azure-Modules voor de versie 4 of hoger is vereist.

Volg deze stappen voor het bijwerken van Azure PowerShell-modules:

- Toegang tot het deelvenster voor Automation-app en selecteer '**Modules**' op het menu aan de linkerkant (Schuif naar beneden als dit menu-item bevindt zich onder gedeelde bronnen).
- Klik in het deelvenster met Modules op "**Update Azure-Modules**' aan de bovenkant en wacht totdat het bericht 'Azure-modules zijn bijgewerkt' wordt weergegeven. Dit proces kan enkele minuten duren.

![Azure automation-modules bijwerken](./media/sql-database-automatic-tuning-email-notifications/howto-email-02.png)

Vereiste versie van AzureRM.Resources en AzureRM.Sql modules moet versie 4 en hoger.

## <a name="create-azure-automation-runbook"></a>Azure Automation-Runbook maken

De volgende stap is het maken van een Runbook in Azure Automation waarbinnen het PowerShell-script voor het ophalen van de aanbevelingen voor het afstemmen zich bevindt.

Volg deze stappen voor het maken van een nieuw Azure Automation-runbook:

- Toegang tot het Azure Automation-account dat u in de vorige stap hebt gemaakt
- Eenmaal in het deelvenster met automation-account, klik op de '**Runbooks**"menu-item aan de linkerkant naar een nieuw Azure Automation-runbook maken met het PowerShell-script. Zie voor meer informatie over het maken van automation-runbooks, [het maken van een nieuw runbook](../automation/manage-runbooks.md#create-a-runbook).
- Als u wilt een nieuw runbook toevoegen, klikt u op de '**+ toevoegen van een runbook**"menu-optie en klik vervolgens op de '**snel maken: een nieuw runbook maken**'.
- Typ in het Runbook de naam van uw runbook (ten behoeve van dit voorbeeld '**AutomaticTuningEmailAutomation**' wordt gebruikt), selecteer het type van runbook als **PowerShell** en een beschrijving van schrijven Dit runbook om te beschrijven van het doel.
- Klik op de **maken** om een nieuw runbook maken voltooien

![Azure automation-runbook toevoegen](./media/sql-database-automatic-tuning-email-notifications/howto-email-03.png)

Volg deze stappen voor het laden van een PowerShell-script in het runbook gemaakt:

- In de '**PowerShell-Runbook bewerken**'venster'**RUNBOOKS**' in het menu structuur en de weergave uitvouwen tot u de naam van uw runbook (in dit voorbeeld ' **AutomaticTuningEmailAutomation**'). Selecteer dit runbook.
- Op de eerste regel van de 'PowerShell-Runbook bewerken' (te beginnen met het getal 1), kopiëren en plakken de code van het volgende PowerShell-script. Dit PowerShell-script wordt geleverd als-is aan de slag te gaan. Wijzig het script met suite uw behoeften.

In de header van het opgegeven PowerShell-script, moet u vervangen `<SUBSCRIPTION_ID_WITH_DATABASES>` met uw Azure-abonnement-ID. Zie voor informatie over het ophalen van uw Azure-abonnement-ID, [aan uw Azure-abonnement-GUID](https://blogs.msdn.microsoft.com/mschray/2016/03/18/getting-your-azure-subscription-guid-new-portal/).

In het geval van verschillende abonnementen, kunt u hen toevoegen als door komma's gescheiden met de eigenschap '$subscriptions' in de header van het script.

```PowerShell
# PowerShell script to retrieve Azure SQL Database Automatic tuning recommendations.
#
# Provided “as-is” with no implied warranties or support.
# The script is released to the public domain.
#
# Replace <SUBSCRIPTION_ID_WITH_DATABASES> in the header with your Azure subscription ID.
#
# Microsoft Azure SQL Database team, 2018-01-22.

# Set subscriptions : IMPORTANT – REPLACE <SUBSCRIPTION_ID_WITH_DATABASES> WITH YOUR SUBSCRIPTION ID
$subscriptions = ("<SUBSCRIPTION_ID_WITH_DATABASES>", "<SECOND_SUBSCRIPTION_ID_WITH_DATABASES>", "<THIRD_SUBSCRIPTION_ID_WITH_DATABASES>")

# Get credentials
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Define the resource types
$resourceTypes = ("Microsoft.Sql/servers/databases")
$advisors = ("CreateIndex", "DropIndex");
$results = @()

# Loop through all subscriptions
foreach($subscriptionId in $subscriptions) {
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    $rgs = Get-AzureRmResourceGroup

    # Loop through all resource groups
    foreach($rg in $rgs) {
        $rgname = $rg.ResourceGroupName;

        # Loop through all resource types
        foreach($resourceType in $resourceTypes) {
            $resources = Get-AzureRmResource -ResourceGroupName $rgname -ResourceType $resourceType

            # Loop through all databases
            # Extract resource groups, servers and databases
            foreach ($resource in $resources) {
                $resourceId = $resource.ResourceId
                if ($resourceId -match ".*RESOURCEGROUPS/(?<content>.*)/PROVIDERS.*") {
                    $ResourceGroupName = $matches['content']
                } else {
                    continue
                }
                if ($resourceId -match ".*SERVERS/(?<content>.*)/DATABASES.*") {
                    $ServerName = $matches['content']
                } else {
                    continue
                }
                if ($resourceId -match ".*/DATABASES/(?<content>.*)") {
                    $DatabaseName = $matches['content']
                } else {
                    continue
                }

                # Skip if master
                if ($DatabaseName -eq "master") {
                    continue
                }

                # Loop through all Automatic tuning recommendation types
                foreach ($advisor in $advisors) {
                    $recs = Get-AzureRmSqlDatabaseRecommendedAction -ResourceGroupName $ResourceGroupName -ServerName $ServerName  -DatabaseName $DatabaseName -AdvisorName $advisor
                    foreach ($r in $recs) {
                        if ($r.State.CurrentValue -eq "Active") {
                            $object = New-Object -TypeName PSObject
                            $object | Add-Member -Name 'SubscriptionId' -MemberType Noteproperty -Value $subscriptionId
                            $object | Add-Member -Name 'ResourceGroupName' -MemberType Noteproperty -Value $r.ResourceGroupName
                            $object | Add-Member -Name 'ServerName' -MemberType Noteproperty -Value $r.ServerName
                            $object | Add-Member -Name 'DatabaseName' -MemberType Noteproperty -Value $r.DatabaseName
                            $object | Add-Member -Name 'Script' -MemberType Noteproperty -Value $r.ImplementationDetails.Script
                            $results += $object
                        }
                    }
                }
            }
        }
    }
}

# Format and output results for the email
$table = $results | Format-List
Write-Output $table
```

Klik op de '**opslaan**"knop in de rechterbovenhoek om op te slaan van het script. Wanneer u tevreden met het script bent, klikt u op de '**publiceren**"knop om dit runbook te publiceren.

In het deelvenster belangrijkste runbook kunt u op de '**Start**"knop **testen** het script. Klik op de '**uitvoer**' om weer te geven resultaten van het script dat wordt uitgevoerd. Deze uitvoer is het verstandig om de inhoud van uw e-mailadres. Voorbeeld van uitvoer van het script te zien in de volgende schermafbeelding.

![Weergave automatische aanbevelingen met Azure Automation voor het afstemmen van Run](./media/sql-database-automatic-tuning-email-notifications/howto-email-04.png)

Zorg ervoor dat de inhoud aanpassen door het PowerShell-script op uw behoeften aanpassen.

De PowerShell-script voor het ophalen van de aanbevelingen voor automatische afstemming is geladen met de bovenstaande stappen in Azure Automation. De volgende stap is om te automatiseren en de e-mail delivery-taak plannen.

## <a name="automate-the-email-jobs-with-microsoft-flow"></a>De e-taken met Microsoft Flow te automatiseren

Voor het voltooien van de oplossing, als de laatste stap wordt een automation-gegevensstroom te maken in Microsoft Flow die bestaat uit drie acties (taken):

1. "**Azure Automation - taak maken**": wordt gebruikt voor het uitvoeren van de PowerShell-script om op te halen voor automatisch afstemmen van de aanbevelingen in Azure Automation-runbook
2. "**Azure Automation - Get-taakuitvoer**": wordt gebruikt voor het ophalen van uitvoer van de uitgevoerde PowerShell-script
3. "**Office 365 Outlook: een e-mail verzenden**": wordt gebruikt voor het verzenden van e-mail. E-mailberichten worden verzonden met behulp van de Office 365-account van de persoon die het maken van de stroom.

Zie voor meer informatie over de mogelijkheden van Microsoft Flow, [aan de slag met Microsoft Flow](https://docs.microsoft.com/flow/getting-started).

Vereiste voor deze stap is voor het aanmelden bij [Microsoft Flow](https://flow.microsoft.com) account en om aan te melden. Eenmaal in de oplossing, volg deze stappen voor het instellen van een **nieuwe stroom**:

- Toegang "**mijn stromen**" menu-item
- In mijn stromen, selecteert u de '**+ maken met een lege App**' koppelen aan de bovenkant van de pagina
- Klik op de koppeling '**honderden connectors en triggers zoeken naar**' aan de onderkant van de pagina
- In het veld Zoektype "**terugkeerpatroon**', en selecteer '**planning - terugkeerpatroon**' in de zoekresultaten voor het plannen van de taak e-mail delivery om uit te voeren.
- Selecteer in het deelvenster terugkeerpatroon in het veld frequentie de planning frequentie voor deze stroom wilt uitvoeren, zoals geautomatiseerde verzenden per e-mail elke minuut, uur, dag, Week, enzovoort.

De volgende stap is het toevoegen van drie taken (create, get-uitvoer en verzendt een e-mail) naar de zojuist gemaakte terugkerende stroom. U doet de vereiste taken toe te voegen aan de stroom door de volgende stappen uit:

1. Actie voor het uitvoeren van PowerShell-script voor het ophalen van de aanbevelingen voor afstemming maken

   - Selecteer "**+ nieuwe stap**', gevolgd door'**een actie toevoegen**' in het deelvenster van de stroom terugkeerpatroon
   - In het veld Zoektype "**automation**'en selecteer'**Azure Automation-taak maken**' in de zoekresultaten
   - In het deelvenster van de taak maken door de taakeigenschappen te configureren. Voor deze configuratie, moet u de details van uw Azure-abonnement-ID, resourcegroep en het Automation-Account **eerder opgenomen** op de **Automation-Account deelvenster**. Zie voor meer informatie over de opties die beschikbaar zijn in deze sectie, [Azure Automation - taak maken](https://docs.microsoft.com/connectors/azureautomation/#create-job).
   - Voltooi het maken van deze actie door te klikken op '**stroom opslaan**"

2. Actie voor het ophalen van uitvoer van de uitgevoerde PowerShell-script maken

   - Selecteer "**+ nieuwe stap**', gevolgd door'**een actie toevoegen**' in het deelvenster van de stroom terugkeerpatroon
   - In het zoekvak ingediend type "**automation**'en selecteer'**Azure Automation: Get-taakuitvoer**' in de zoekresultaten. Zie voor meer informatie over de opties die beschikbaar zijn in deze sectie, [Azure Automation: Get-taakuitvoer](https://docs.microsoft.com/connectors/azureautomation/#get-job-output).
   - Vullen velden vereist (vergelijkbaar met het maken van de vorige taak): het vullen van uw Azure-abonnement-ID, resourcegroep, en het Automation-Account (zoals opgegeven in het deelvenster met Automation-Account)
   - Klik in het veld '**taak-ID**' voor de '**dynamische inhoud**"menu wordt weergegeven. In dit menu, selecteer de optie '**taak-ID**'.
   - Voltooi het maken van deze actie door te klikken op '**stroom opslaan**"

3. Actie voor het verzenden van e-mail met behulp van Office 365-integratie maken

   - Selecteer "**+ nieuwe stap**', gevolgd door'**een actie toevoegen**' in het deelvenster van de stroom terugkeerpatroon
   - In het zoekvak ingediend type "**een e-mailbericht verzenden**'en selecteer'**Office 365 Outlook: een e-mail verzenden**' in de zoekresultaten
   - In de '**naar**"veldtype in het e-mailadres waarnaar u wilt de e-mailmelding verzenden
   - In de '**onderwerp**"veldtype in het onderwerp van uw e-mailadres, bijvoorbeeld"automatisch aanbevelingen voor afstemming e-mailmelding"
   - Klik in het veld '**hoofdtekst**' voor de '**dynamische inhoud**"menu wordt weergegeven. Uit binnen dit menu onder '**ophalen taakuitvoer**", selecteer"**inhoud**"
   - Voltooi het maken van deze actie door te klikken op '**stroom opslaan**"

> [!TIP]
> Voor het verzenden van automatische e-mailberichten naar andere ontvangers, afzonderlijke stromen te maken. In deze aanvullende stromen de geadresseerde e-mailadres in het veld 'Aan' en de regel voor het onderwerp van e-mailadres in het veld 'Onderwerp' te wijzigen. Het maken van nieuwe runbooks in Azure Automation met aangepaste PowerShell-scripts (zoals bij wijziging van de Azure-abonnement-ID) kunt verder aanpassen van geautomatiseerde scenario's, zoals bijvoorbeeld afzonderlijke ontvangers voor automatisch afstemmen is mailen aanbevelingen voor de afzonderlijke abonnementen.
>

De bovenstaande concludeert vereiste stappen voor het configureren van de werkstroom voor e-mail delivery-taak. De hele stroom die bestaat uit drie acties die zijn ingebouwd wordt in de volgende afbeelding weergegeven.

![De functie automatisch afstemmen van e-mailmeldingen stroom weergeven](./media/sql-database-automatic-tuning-email-notifications/howto-email-05.png)

Als u wilt de stroom testen, klikt u op '**nu uitvoeren**' in de rechterbovenhoek in het deelvenster van de stroom.

Statistieken van het uitvoeren van de geautomatiseerde taken, met succes van e-mailmeldingen verzonden, kunt u zien door het deelvenster van de stroom analyse.

![Uitvoeren van de stroom voor automatisch afstemmen e-mailmeldingen](./media/sql-database-automatic-tuning-email-notifications/howto-email-06.png)

De stroomanalyse is nuttig voor het bewaken van het succes van taakuitvoeringen, en indien nodig voor het oplossen van problemen.  In het geval van het oplossen van problemen ook raadzaam om te controleren van de PowerShell-script mobiel uitvoeringslogboek toegankelijk zijn via Azure Automation-app.

De uiteindelijke uitvoer van de geautomatiseerde e-mailbericht lijkt op het volgende e-mailbericht ontvangen nadat het bouwen en uitvoeren van deze oplossing:

![Voorbeeld e-mailadres van de uitvoer van automatisch afstemmen e-mailmeldingen](./media/sql-database-automatic-tuning-email-notifications/howto-email-07.png)

Door het aanpassen van het PowerShell-script, kunt u de uitvoer en de opmaak van de geautomatiseerde e-mailbericht aan uw behoeften aanpassen.

U kunt de oplossing voor het bouwen van e-mailmeldingen op basis van een bepaalde gebeurtenis afstemmen en naar meerdere ontvangers, voor meerdere abonnementen of -databases, afhankelijk van uw aangepaste scenario's mogelijk verder aanpassen.

## <a name="next-steps"></a>Volgende stappen

- Leer meer over hoe automatisch afstemmen, kunt u betere databaseprestaties, Zie [automatisch afstemmen in Azure SQL Database](sql-database-automatic-tuning.md).
- Inschakelen van automatisch afstemmen in Azure SQL Database voor het beheren van uw workload [automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md).
- Zie voor het handmatig controleren en automatisch aanbevelingen voor het afstemmen van toepassing, [zoeken en toepassen van aanbevelingen voor prestaties](sql-database-advisor-portal.md).
