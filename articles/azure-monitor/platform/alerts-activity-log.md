---
title: Weergave, maken en beheren van waarschuwingen voor activiteitenlogboeken in Azure Monitor
description: Het maken van waarschuwingen voor activiteitenlogboeken van Azure Portal, Resource-sjabloon en PowerShell.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.openlocfilehash: c88fe7051519440056fe85e7ff9172ae0239bd41
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56234234"
---
# <a name="create-view-and-manage-activity-log-alerts-using-azure-monitor"></a>Maken, weergeven en beheren van waarschuwingen voor activiteitenlogboeken met behulp van Azure Monitor  

## <a name="overview"></a>Overzicht
Waarschuwingen voor activiteitenlogboeken zijn de waarschuwingen die worden geactiveerd wanneer er een nieuwe activiteit log-gebeurtenis optreedt die overeenkomt met de voorwaarden zijn opgegeven in de waarschuwing.

Deze waarschuwingen zijn voor Azure-resources kunnen worden gemaakt met behulp van een Azure Resource Manager-sjabloon. Ze kunnen ook worden gemaakt, bijgewerkt of verwijderd in Azure portal. Normaal gesproken u waarschuwingen voor activiteitenlogboek maken om meldingen te ontvangen wanneer bepaalde wijzigingen worden uitgevoerd in de resources in uw Azure-abonnement, vaak binnen het bereik van bepaalde resourcegroepen of resource. Bijvoorbeeld, u wilt mogelijk een melding wanneer een virtuele machine in (voorbeeld resourcegroep) **myProductionResourceGroup** wordt verwijderd, of u kunt een melding ontvangen als de nieuwe rollen zijn toegewezen aan een gebruiker in uw abonnement.

> [!IMPORTANT]
> Waarschuwingen op status van de Service-melding kunnen niet worden gemaakt via de interface voor het maken van activiteit log waarschuwingen. Zie voor meer informatie over het maken en gebruiken van servicestatusmeldingen [ontvangen van waarschuwingen voor activiteitenlogboeken op servicestatusmeldingen](alerts-activity-log-service-notifications.md).

## <a name="azure-portal"></a>Azure Portal

> [!NOTE]

>  Tijdens het maken van de regels voor waarschuwingen, Controleer het volgende:

> - Abonnement in het bereik is niet af van het abonnement waarin de waarschuwing wordt gemaakt.
- Criteria moet niveau/status/aanroeper / resourcegroep / resource-id / resourcetype / gebeurteniscategorie waarop de waarschuwing is geconfigureerd.
- Er is geen 'dragen' voorwaarde of geneste voorwaarden in de waarschuwingsconfiguratie JSON (in feite, slechts één allOf met geen verdere allOf/dragen is toegestaan).
- Wanneer is de categorie 'beheer'. U moet ten minste één van de bovenstaande criteria opgeven in de waarschuwing. U kunt een waarschuwing die wordt geactiveerd wanneer een gebeurtenis wordt gemaakt in de activiteitenlogboeken niet maken.

### <a name="create-with-azure-portal"></a>Maken met Azure portal

Gebruik de volgende procedure:

1. Selecteer in Azure portal, **Monitor** > **waarschuwingen**
2. Klik op **nieuwe waarschuwingsregel** aan de bovenkant van de **waarschuwingen** venster.

     ![Nieuwe waarschuwingsregel](media/alerts-activity-log/AlertsPreviewOption.png)

     De **maken regel** venster wordt weergegeven.

      ![Opties voor nieuwe waarschuwingsregel](media/alerts-activity-log/create-new-alert-rule-options.png)

3. **Onder de voorwaarde voor waarschuwing definiëren,** geeft u de volgende informatie en op **gedaan**.

    - **Waarschuwingsdoel:** Als u wilt weergeven en selecteren van het doel voor de nieuwe waarschuwing, gebruikt u **filteren op abonnement** / **filteren op resourcetype** en selecteer de resource of resourcegroep in de lijst die wordt weergegeven.

    > [!NOTE]

    > u kunt een resource, resourcegroep of een hele abonnement voor de activiteit logboeksignaal selecteren.

    **Target voorbeeldgegevens waarschuwingsweergave** ![doel selecteren](media/alerts-activity-log/select-target.png)

    - Onder **doel Criteria**, klikt u op **criteria toevoegen** en alle beschikbare signalen voor het doel worden weergegeven, met inbegrip van die uit verschillende categorieën van **activiteitenlogboek**; met naam van de apparaatcategorie in toegevoegd **-controleservice** naam.

    - Selecteer het signaal uit de lijst weergegeven van de verschillende bewerkingen mogelijk voor het type **activiteitenlogboek**.

    U kunt de tijdlijn van de geschiedenis van logboekbestanden en de bijbehorende waarschuwingslogica voor dit doel signaal selecteren:

    **Scherm criteria toevoegen**

    ![criteria toevoegen](media/alerts-activity-log/add-criteria.png)

    **Tijd van de geschiedenis**: Gebeurtenissen beschikbaar voor de geselecteerde bewerking is, kunnen worden weergegeven gedurende de afgelopen 6/12/24 uur (of) in de afgelopen Week.

    **Waarschuwing logische**:

     - **Gebeurtenisniveau**-de ernst van de gebeurtenis. _Uitgebreide_, _informatief_, _waarschuwing_, _fout_, of _kritieke_.
     - **Status**: De status van de gebeurtenis. _Aan de slag_, _mislukt_, of _geslaagd_.
     - **Gebeurtenis gestart door**: Ook wel bekend als de oproepende functie De e-mailadres of Azure Active Directory-id van de gebruiker die de bewerking heeft uitgevoerd.

        Voorbeeld signaal graph met waarschuwingslogica toegepast:

        ![ criteria die zijn geselecteerd](media/alerts-activity-log/criteria-selected.png)

4. Onder **waarschuwingsregels details definiëren**, geef de volgende informatie op:

    - **Naam waarschuwingsregel** – naam voor de nieuwe waarschuwingsregel
    - **Beschrijving** – beschrijving voor de nieuwe waarschuwingsregel
    - **Waarschuwing niet opslaan op resourcegroep** : Selecteer de resourcegroep, waar u deze nieuwe regel.

5. Onder **actiegroep**, uit de vervolgkeuzelijst, geef de actiegroep die u wilt toewijzen aan deze nieuwe waarschuwingsregel. U kunt ook [maken van een nieuwe actiegroep](../../azure-monitor/platform/action-groups.md) en toewijzen aan de nieuwe regel. Klik op om een nieuwe groep **+ nieuwe groep**.

6. Als de regels nadat u dit hebt gemaakt, klikt u op **Ja** voor **regel inschakelen bij het maken van** optie.
7. Klik op **waarschuwingsregel maken**.

    De nieuwe waarschuwingsregel voor het activiteitenlogboek is gemaakt en een bevestigingsbericht wordt weergegeven aan de bovenkant van het venster.

    U kunt inschakelen, uitschakelen, bewerken of verwijderen van een regel. Meer informatie over het beheren van activiteit log regels.


U kunt ook een eenvoudige vergelijking voor informatie over voorwaarden waarop de waarschuwingsregels voor activiteitenlogboeken, kunnen worden gemaakt is om te verkennen of filteren van gebeurtenissen via [activiteitenlogboek in Azure portal](../../azure-monitor/platform/activity-logs-overview.md#query-the-activity-log-in-the-azure-portal). In Azure controleren: activiteitenlogboek, een kunt filteren of nodig gebeurtenis zoeken en vervolgens een waarschuwing te maken met behulp van de **waarschuwing voor activiteitenlogboek toevoegen** knop; vervolgens stappen 4 en hoger zoals vermeld in de zelfstudie hierboven.
    
 ![ waarschuwing toevoegen met het activiteitenlogboek](media/alerts-activity-log/add-activity-log.png)
    

### <a name="view-and-manage-in-azure-portal"></a>Weergeven en beheren in Azure portal

1. Vanuit Azure-portal, klikt u op **Monitor** > **waarschuwingen** en klikt u op **regels beheren** op linksboven in het venster.

    ![ regels voor waarschuwingen beheren](media/alerts-activity-log/manage-alert-rules.png)

    De lijst met beschikbare regels wordt weergegeven.

2. Zoek de regel voor het logboek van activiteit te wijzigen.

    ![ waarschuwingsregels activiteit zoeken](media/alerts-activity-log/searth-activity-log-rule-to-edit.png)

    U kunt de beschikbare filters - _abonnement_, _resourcegroep_, _Resource_, _signaaltype_, of _Status_  te vinden van de activiteit regel die u wilt bewerken.

    > [!NOTE]

    > U kunt alleen bewerken **beschrijving** , **criteria als doel** en **actiegroepen**.

3.  Selecteer de regel en dubbelklik erop om het bewerken van de opties voor de regel. De benodigde wijzigingen aanbrengen en klik vervolgens op **opslaan**.

    ![ regels voor waarschuwingen beheren](media/alerts-activity-log/activity-log-rule-edit-page.png)

4.  U kunt uitschakelen, inschakelen of verwijderen van een regel. Selecteer de gewenste optie aan de bovenkant van het venster nadat u de regel zoals beschreven in stap 2 hebt geselecteerd.


## <a name="azure-resource-template"></a>Azure Resource-sjabloon
Als u wilt een waarschuwing voor activiteitenlogboek maken met behulp van Resource Manager-sjabloon, maken van een resource van het type `microsoft.insights/activityLogAlerts`. U Vul vervolgens alle verwante eigenschappen. Hier volgt een sjabloon die een waarschuwing voor activiteitenlogboek maakt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```
Het bovenstaande voorbeeld-json als (bijvoorbeeld) sampleActivityLogAlert.json ten behoeve van deze walkthrough kunnen worden opgeslagen en kunnen worden geïmplementeerd met [Azure Resource Manager in Azure portal](../../azure-resource-manager/resource-group-template-deploy-portal.md).

> [!NOTE]
> Het kan tot vijf minuten duren voor de een nieuwe waarschuwingsregel voor activiteitenlogboeken actief

## <a name="rest-api"></a>REST-API 
[Azure Monitor - activiteit Log waarschuwingen API](https://docs.microsoft.com/rest/api/monitor/activitylogalerts) is een REST-API en volledig compatibel met Azure Resource Manager REST API. Daarom kan deze worden gebruikt via Powershell met Resource Manager-cmdlet, evenals Azure CLI.

## <a name="powershell"></a>PowerShell

Waarschuwingen voor activiteitenlogboeken zijn speciaal bestemd voor PowerShell-cmdlets beschikbaar:

- [Set-AzureRmActivityLogAlert](https://docs.microsoft.com/powershell/module/azurerm.insights/set-azurermactivitylogalert): Maak een nieuwe of bestaande resource van het logboek waarschuwingsregel activiteit bijwerken
- [Get-AzureRmActivityLogAlert](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermactivitylogalert): Een of meer activiteiten log waarschuwingsregel resources ophalen
- [Remove-AzureRmActivityLogAlert](https://docs.microsoft.com/powershell/module/azurerm.insights/remove-azurermactivitylogalert): Activiteit log waarschuwingsregel resource met de bevestiging van de gebruiker verwijderen
- [Enable-AzureRmActivityLogAlert](https://docs.microsoft.com/powershell/module/azurerm.insights/enable-azurermactivitylogalert): Om in te schakelen van een bestaande activiteit log waarschuwingsregel resource
- [Disable-AzureRmActivityLogAlert](https://docs.microsoft.com/powershell/module/azurerm.insights/disable-azurermactivitylogalert): Een bestaande resource van activiteit log waarschuwingsregel uitschakelen

## <a name="cli"></a>CLI

Azure CLI-opdrachten in de set toegewezen [az monitor activiteitenlogboek waarschuwing](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert) zijn beschikbaar voor het beheren van activiteit-logboekwaarschuwingsregels.

Gebruik voor het maken van een nieuwe waarschuwingsregel voor activiteitenlogboeken in de aangegeven volgorde:

1. [maken van de AZ monitor activiteitenlogboek waarschuwing](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-create): Nieuwe activiteit log waarschuwingsregel resource maken
1. [AZ monitor activiteitenlogboek waarschuwing bereik](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/scope): Bereik voor de activiteit waarschuwingsregel toevoegen
1. [AZ monitor activiteitenlogboek waarschuwing-actiegroep](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/action-group): Actiegroep toevoegen aan de waarschuwingsregel voor activiteitenlogboeken

Ophalen van een activiteit log waarschuwingsregel resource, de Azure CLI-opdracht [az monitor activiteitenlogboek waarschuwing weergeven](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-show
) kan worden gebruikt. En gebruik voor alle activiteit waarschuwingsregel logboekresource weergeven in een resourcegroep, [az monitor activiteitenlogboek waarschuwing lijst](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-list).
Activiteit log waarschuwingsregel resources kunnen worden verwijderd met behulp van Azure CLI-opdracht [az monitor activiteitenlogboek waarschuwing verwijderen](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-delete).

## <a name="next-steps"></a>Volgende stappen

- [Webhook-schema voor activiteitenlogboeken](../../azure-monitor/platform/activity-log-alerts-webhook.md)
- [Overzicht van activiteitenlogboeken](../../azure-monitor/platform/activity-log-alerts.md) 
- Meer informatie over [actiegroepen](../../azure-monitor/platform/action-groups.md).  
- Meer informatie over [health servicemeldingen](../../azure-monitor/platform/service-notifications.md).
