---
title: Azure Monitor (preview) inschakelen voor een hybride omgeving | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure Monitor inschakelen voor virtuele machines voor een hybride cloudomgeving die een of meer virtuele machines bevat.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/07/2019
ms.author: magoedte
ms.openlocfilehash: bc26cc0654aac9416bf31ffccf426648e3a8b8d2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67122549"
---
# <a name="enable-azure-monitor-for-vms-preview-for-a-hybrid-environment"></a>Azure Monitor voor virtuele machines (preview) inschakelen voor een hybride omgeving

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Dit artikel wordt uitgelegd hoe u Azure Monitor voor virtuele machines (preview) inschakelen voor virtuele machines of fysieke computers die worden gehost in uw datacenter of andere cloudomgeving. Aan het einde van dit proces, u wordt is begonnen met het controleren van uw virtuele machines in uw omgeving en leer als problemen met prestaties of beschikbaarheid ondervinden. 

Voordat u begint, moet u controleren de [vereisten](vminsights-enable-overview.md) en controleer of uw abonnement en resources voldoen aan de vereisten. Bekijk de vereisten en methoden voor het implementeren van de [Log Analytics Linux en Windows-agent](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

>[!NOTE]
>De Azure-Monitor voor agent voor afhankelijkheden van virtuele machines toewijzen niet de gegevens zelf worden verzonden en er is geen wijzigingen in de firewalls en poorten vereist. De kaartgegevens worden altijd verzonden door de Log Analytics-agent naar de service Azure Monitor, hetzij rechtstreeks of via de [Operations Management Suite-gateway](../../azure-monitor/platform/gateway.md) als uw IT-beveiligingsbeleid niet toestaat computers in het netwerk dat verbinding maken met internet.

De stappen voor het voltooien van deze taak worden als volgt samengevat:

1. Log Analytics-agent voor Windows of Linux installeren. Voordat u de agent hebt geïnstalleerd, controleert u de [Log Analytics-agent overzicht](../platform/log-analytics-agent.md) artikel voor informatie over systeemvereisten en methoden voor het implementeren.

2. Download en installeer de Azure-Monitor voor agent voor virtuele machines kaart afhankelijkheden voor [Windows](https://aka.ms/dependencyagentwindows) of [Linux](https://aka.ms/dependencyagentlinux).

3. Schakel het verzamelen van prestatiemeteritems.

4. Implementeren met Azure Monitor voor virtuele machines.

## <a name="install-the-dependency-agent-on-windows"></a>De afhankelijkheidsagent installeren op Windows
U kunt de agent voor afhankelijkheden handmatig installeren op Windows-computers door uit te voeren `InstallDependencyAgent-Windows.exe`. Als u dit uitvoerbare bestand zonder opties uitvoert, wordt er een setup-wizard die u volgen kunt om de installatie interactief gestart.

>[!NOTE]
>*Beheerder* bevoegdheden zijn vereist om te installeren of verwijderen van de agent.

De volgende tabel ziet u de parameters die worden ondersteund door het installatieprogramma voor de agent vanaf de opdrachtregel.

| Parameter | Description |
|:--|:--|
| /? | Retourneert een lijst van de opdrachtregelopties. |
| /S | Voert een installatie op de achtergrond zonder tussenkomst van de gebruiker. |

Bijvoorbeeld, om uit te voeren van het installatieprogramma met de `/?` parameter, voer **InstallDependencyAgent Windows.exe /?** .

Bestanden voor de agent voor Windows-afhankelijkheden zijn geïnstalleerd in *C:\Program Files\Microsoft Afhankelijkheidsagent* standaard. Als de agent voor afhankelijkheden niet kunt starten nadat de installatie is voltooid, controleert u de logboeken voor uitgebreide foutgegevens. De logboekmap is *%Programfiles%\Microsoft afhankelijkheid Agent\logs*.

## <a name="install-the-dependency-agent-on-linux"></a>De afhankelijkheidsagent installeren in Linux
De agent voor afhankelijkheden is geïnstalleerd op Linux-servers van *InstallDependencyAgent Linux64.bin*, een shell-script met een zichzelf uitpakkend binair bestand. U kunt het bestand uitvoeren met behulp van `sh` of toe te voegen uitvoermachtigingen naar het bestand zelf.

>[!NOTE]
> Toegang tot de hoofdmap is vereist om de agent te installeren of configureren.
>

| Parameter | Description |
|:--|:--|
| -help | Een lijst met de opdrachtregelopties ophalen. |
| -s | Een installatie op de achtergrond uitvoeren zonder gebruikersvragen. |
| --controleren | Controleer machtigingen en het besturingssysteem, maar de agent niet installeren. |

Bijvoorbeeld, om uit te voeren van het installatieprogramma met de `-help` parameter, voer **InstallDependencyAgent Linux64.bin-help**.

De agent voor Linux-afhankelijkheden als root installeren met de opdracht `sh InstallDependencyAgent-Linux64.bin`.

Als de agent voor afhankelijkheden niet kan worden gestart, controleert u de logboeken voor uitgebreide foutgegevens. Op Linux-agents, de logboekmap is */var/opt/microsoft/dependency-agent/log*.

Bestanden voor de agent voor afhankelijkheden worden geplaatst in de volgende mappen:

| Bestanden | Locatie |
|:--|:--|
| Kernbestanden | /opt/microsoft/dependency-agent |
| Logboekbestanden | /var/opt/microsoft/dependency-agent/log |
| Configuratiebestanden | /etc/opt/microsoft/dependency-agent/config |
| Uitvoerbare bestanden van de service | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binaire opslagbestanden | /var/opt/microsoft/dependency-agent/storage |

## <a name="enable-performance-counters"></a>Inschakelen van prestatiemeteritems
Als de Log Analytics-werkruimte waarnaar wordt verwezen door de oplossing niet is al geconfigureerd voor het verzamelen van de prestatiemeteritems die is vereist voor de oplossing, moet u ze inschakelen. U kunt dit op twee manieren doen:
* Handmatig, zoals beschreven in [Windows en Linux-gegevensbronnen van de prestaties die u in Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md)
* Door te downloaden en uitvoeren van een PowerShell-script die beschikbaar is in de [Azure PowerShell Gallery](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

## <a name="deploy-azure-monitor-for-vms"></a>Azure Monitor voor virtuele machines implementeren
Deze methode bevat een JSON-sjabloon waarmee de configuratie voor het inschakelen van de oplossingsonderdelen in uw Log Analytics-werkruimte.

Als u niet hoe u resources implementeren weet met behulp van een sjabloon, Zie:
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Voor het gebruik van de Azure CLI, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.27 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="create-and-execute-a-template"></a>Maken en uitvoeren van een sjabloon

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                            "promotionCode": ""
                        }
                    },
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },
                        "plan": {
                            "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Sla dit bestand als *installsolutionsforvminsights.json* naar een lokale map.

1. Vastleggen van de waarden voor *WorkspaceName*, *ResourceGroupName*, en *WorkspaceLocation*. De waarde voor *WorkspaceName* is de naam van uw Log Analytics-werkruimte. De waarde voor *WorkspaceLocation* is de regio in de werkruimte is gedefinieerd.

1. Bent u klaar voor het implementeren van deze sjabloon met behulp van de volgende PowerShell-opdracht:

    ```powershell
    New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

    ```powershell
    provisioningState       : Succeeded
    ```
   Wanneer u bewaking inschakelt, is het duurt ongeveer 10 minuten voordat u de status en metrische gegevens voor de hybride-computer kunt weergeven.

## <a name="next-steps"></a>Volgende stappen

Nu dat de controle is ingeschakeld voor uw virtuele machines, is deze informatie is beschikbaar voor analyse met Azure Monitor voor virtuele machines.
 
- Zie voor meer informatie over het gebruik van de Health-functie, [weergave Azure Monitor voor virtuele machines health](vminsights-health.md).
- Afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md).
- Zie voor het identificeren van knelpunten en het algehele gebruik met de prestaties van de virtuele machine, [weergave Azure VM-prestaties](vminsights-performance.md).
- Afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md).