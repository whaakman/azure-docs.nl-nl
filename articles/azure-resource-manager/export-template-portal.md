---
title: Azure Resource Manager-sjabloon exporteren met behulp van Azure portal
description: Azure portal gebruiken voor een Azure Resource Manager-sjabloon exporteren uit resources in uw abonnement.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: tomfitz
ms.openlocfilehash: e482bf99013b9bec9dfbf64c4e8ad5a8a43ff540
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296311"
---
# <a name="single-and-multi-resource-export-to-template-in-azure-portal"></a>Één of meerdere resources exporteren naar de sjabloon in Azure portal

Om te helpen bij het maken van Azure Resource Manager-sjablonen, kunt u een sjabloon exporteren uit bestaande resources. De geëxporteerde sjabloon krijgt u inzicht in de JSON-syntaxis en de eigenschappen die uw resources implementeren. Beginnen met de geëxporteerde sjabloon voor het automatiseren van toekomstige implementaties en aanpassen voor uw scenario.

Resource Manager kunt u een of meer resources voor het exporteren van een sjabloon kiezen. U kunt zich richten op exact de resources die u nodig hebt in de sjabloon.

In dit artikel laat zien hoe om te exporteren sjablonen via de portal. U kunt ook [Azure CLI](manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](manage-resource-groups-powershell.md#export-resource-groups-to-templates), of [REST-API](/rest/api/resources/resourcegroups/exporttemplate).

## <a name="choose-the-right-export-option"></a>Kies de optie voor het juiste exporteren

Er zijn twee manieren om een sjabloon te exporteren:

* **Exporteren van de resourcegroep of resource**. Deze optie genereert een nieuwe sjabloon uit bestaande resources. De geëxporteerde sjabloon is een 'snapshot' van de huidige status van de resourcegroep. U kunt een hele resourcegroep of een specifieke resources in die resourcegroep exporteren.

* **Vóór de implementatie of exporteren uit geschiedenis**. Deze optie wordt een exacte kopie van een sjabloon die wordt gebruikt voor de implementatie van opgehaald.

Afhankelijk van welke optie die u kiest, wordt met de geëxporteerde sjablonen verschillende kenmerken hebben.

| Via een resourcegroep of resource | Vóór de implementatie of van geschiedenis |
| --------------------- | ----------------- |
| Momentopname van de sjabloon van de huidige status van de resources. Het bevat alle handmatige wijzigingen na de implementatie. | Sjabloon toont alleen de status van resources op het moment van implementatie. Handmatige wijzigingen die u aangebracht nadat de implementatie worden niet opgenomen. |
| U kunt welke resources selecteren van een resourcegroep te exporteren. | Alle resources voor een specifieke implementatie zijn opgenomen. U kan kiezen van een subset van deze resources of resources die zijn toegevoegd aan een andere tijd toevoegen. |
| Sjabloon bevat alle eigenschappen voor de resources, met inbegrip van sommige eigenschappen die u normaal gesproken wouldn't ingesteld tijdens de implementatie. Het is raadzaam om te verwijderen of opschonen van deze eigenschappen vóór het hergebruik van de sjabloon. | Sjabloon bevat alleen de eigenschappen die nodig zijn voor de implementatie. De sjabloon is een kant-en-klare. |
| Sjabloon bevatten waarschijnlijk niet alle van de parameters die u nodig hebt voor hergebruik. De meeste eigenschapswaarden zijn vastgelegd in de sjabloon. Als u wilt implementeren de sjabloon in andere omgevingen, die u wilt toevoegen van de parameters die verhogen van de mogelijkheid om de resources te configureren. | Sjabloon bevat parameters die eenvoudig te implementeren in verschillende omgevingen. |

De sjabloon exporteren vanuit een resourcegroep of resource, wanneer:

* U moet wijzigingen aan de resources die zijn gemaakt na de oorspronkelijke implementatie vast te leggen.
* U wilt selecteren welke resources zijn geëxporteerd.

De sjabloon vóór de implementatie of van de geschiedenis exporteren wanneer:

* U wilt dat de sjabloon van een eenvoudig te hergebruiken.
* U hoeft niet te zijn wijzigingen die u hebt aangebracht na de oorspronkelijke implementatie.

## <a name="export-template-from-resource-group"></a>Sjabloon exporteren uit resourcegroep

Een of meer resources exporteren vanuit een resourcegroep:

1. Selecteer de resourcegroep met de resources die u wilt exporteren.

1. Als u wilt exporteren alle resources in de resourcegroep, Alles selecteren en vervolgens **sjabloon exporteren**. De **sjabloon exporteren** optie alleen wordt ingeschakeld nadat u ten minste één resource hebt geselecteerd.

   ![Alle resources exporteren](./media/export-template-portal/select-all-resources.png)

1. Als u wilt kiezen specifieke resources voor het exporteren, schakel de selectievakjes in naast deze resources. Selecteer **sjabloon exporteren**.

   ![Te exporteren resources selecteren](./media/export-template-portal/select-resources.png)

1. De geëxporteerde sjabloon wordt weergegeven en is beschikbaar voor downloaden.

   ![Sjabloon weergeven](./media/export-template-portal/show-template.png)

## <a name="export-template-from-resource"></a>Sjabloon exporteren uit resource

Voor het exporteren van een resource:

1. Selecteer de resourcegroep met de resource die u wilt exporteren.

1. Selecteer de resource te exporteren.

   ![Resource selecteren](./media/export-template-portal/select-link-resource.png)

1. Selecteer voor de resource **sjabloon exporteren** in het linkerdeelvenster.

   ![Resource exporteren](./media/export-template-portal/export-single-resource.png)

1. De geëxporteerde sjabloon wordt weergegeven en is beschikbaar voor downloaden. De sjabloon bevat alleen de afzonderlijke resource.

## <a name="export-template-before-deployment"></a>Sjabloon exporteren voor implementatie

1. Selecteer de Azure-service die u wilt implementeren.

1. Vul de waarden voor de nieuwe service.

1. Na de validatie wordt doorgegeven, maar voordat u begint met de implementatie, selecteer **een sjabloon voor automation downloaden**.

   ![Een sjabloon downloaden](./media/export-template-portal/download-before-deployment.png)

1. De sjabloon wordt weergegeven en is beschikbaar voor downloaden.

   ![Sjabloon weergeven](./media/export-template-portal/show-template-before-deployment.png)

## <a name="export-template-after-deployment"></a>Sjabloon exporteren na de implementatie

U kunt de sjabloon die is gebruikt voor het implementeren van bestaande resources exporteren. De sjabloon die u krijgt is precies degene die is gebruikt voor implementatie.

1. Selecteer de resourcegroep die u wilt exporteren.

1. Selecteer de koppeling onder **implementaties**.

   ![Selecteer de implementatiegeschiedenis](./media/export-template-portal/select-deployment-history.png)

1. Selecteer een van de implementaties van geschiedenis van de implementatie.

   ![Selecteer implementatie](./media/export-template-portal/select-details.png)

1. Selecteer **sjabloon**. De sjabloon die wordt gebruikt voor deze implementatie wordt weergegeven en is beschikbaar voor downloaden.

   ![Sjabloon selecteren](./media/export-template-portal/show-template-from-history.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het exporteren van sjablonen met [Azure CLI](manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](manage-resource-groups-powershell.md#export-resource-groups-to-templates), of [REST-API](/rest/api/resources/resourcegroups/exporttemplate).
- Zie voor meer informatie de syntaxis van de Resource Manager-sjabloon, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](./resource-group-authoring-templates.md).
- Zie voor meer informatie over het ontwikkelen van sjablonen, de [zelfstudies met stapsgewijze instructies](/azure/azure-resource-manager/).
- Als u de Azure Resource Manager-sjabloon schema's, Zie [sjabloonverwijzing](/azure/templates/).