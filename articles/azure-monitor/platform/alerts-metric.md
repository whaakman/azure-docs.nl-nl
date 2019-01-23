---
title: Maken, weergeven en beheren van metrische waarschuwingen met behulp van Azure Monitor
description: Leer hoe u Azure portal of de CLI gebruiken voor het maken, weergeven en beheren van waarschuwingsregels voor metrische gegevens.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: d871cecda8e7957e3d0d3c37078b8a1fa2ab5214
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54451252"
---
# <a name="create-view-and-manage-metric-alerts-using-azure-monitor"></a>Maken, weergeven en beheren van metrische waarschuwingen met behulp van Azure Monitor

Metrische waarschuwingen in Azure Monitor biedt een manier om te worden geïnformeerd wanneer een van de metrische gegevens een drempelwaarde overschrijden. Metrische waarschuwingen werken op een scala aan platform multi-dimensionale metrische gegevens, aangepaste metrische gegevens, Application Insights-standaard en aangepaste metrische gegevens. In dit artikel wordt wordt beschreven hoe u maken, weergeven en beheren van regels voor metrische waarschuwingen via Azure portal en Azure CLI. U kunt ook de waarschuwingsregels voor metrische gegevens gebruik met behulp van Azure Resource Manager-sjablonen die wordt beschreven in maken [een apart artikel](../../azure-monitor/platform/alerts-enable-template.md).

U kunt meer informatie over hoe metrische waarschuwingen werken vanaf [metriek overzicht van waarschuwingen](alerts-metric-overview.md).

## <a name="create-with-azure-portal"></a>Maken met Azure portal

De volgende procedure wordt beschreven hoe u een waarschuwingsregel voor metrische gegevens maken in Azure portal:

1. In [Azure-portal](https://portal.azure.com), klikt u op **Monitor**. De blade Monitor consolideert alle controle-instellingen en gegevens in één weergave.

2. Klik op **waarschuwingen** klikt u vervolgens op **+ nieuwe waarschuwingsregel**.

    > [!TIP]
    > De meeste resourceblades hebben ook **waarschuwingen** in hun resourcemenu onder **bewaking**, u kunt waarschuwingen maken ook.

3. Klik op **doel selecteren**, in het deelvenster context die wordt geladen, selecteert u een doelresource die u een waarschuwing wilt maken. Gebruik **abonnement** en **resourcetype** vervolgkeuzelijsten om te vinden van de resource die u wilt bewaken. U kunt ook de zoekbalk te vinden van uw resource.

4. Als de geselecteerde resource voor metrische gegevens die u kunt waarschuwingen maken op **beschikbare signalen** aan de onderkant rechts metrische gegevens bevat. U vindt de volledige lijst met resourcetypen die worden ondersteund voor metrische waarschuwingen in deze [artikel](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported).

5. Als u een doelbron hebt geselecteerd, klikt u op **voorwaarde toevoegen**.

6. U ziet een lijst van signalen die worden ondersteund voor de resource, selecteer de metrische gegevens die u maken van een waarschuwing wilt op.

7. (Optioneel) de metriek verfijnen door aan te passen **periode** en **aggregatie**. Als de metriek dimensies heeft, ziet u **dimensies** tabel weergegeven. Selecteer een of meer waarden per dimensie. De waarschuwing voor metrische gegevens wordt uitgevoerd evalueren van de voorwaarde voor alle combinaties van waarden die zijn geselecteerd. [Meer informatie over waarschuwingen voor de multi-dimensionale metrische gegevens over de werking van](alerts-metric-overview.md). U kunt ook **Selecteer \***  voor het gebruik van de dimensies. **Selecteer \***  wordt dynamisch schalen de selectie op alle huidige en toekomstige waarden voor een dimensie.

8. U ziet een grafiek voor de metrische gegevens voor de afgelopen 6 uur. De parameters voor waarschuwing; definiëren **Voorwaardetype**, **frequentie**, **Operator** en **drempelwaarde** of **gevoeligheid**, deze wordt Bepaal de logica die de waarschuwingsregel voor metrische gegevens worden geëvalueerd. [Meer informatie over drempelwaarden voor dynamische voorwaarde type en de gevoeligheid opties](alerts-dynamic-thresholds.md).

9. Als u een statische drempelwaarde gebruikt, kunt u de grafiek met metrische gegevens bepalen wat een redelijke drempelwaarde kan zijn. Als u van een dynamische drempelwaarden gebruikmaakt, wordt de berekende drempelwaarden op basis van recente gegevens weergegeven in de grafiek met metrische gegevens.

10. Klik op **Gereed**.

11. (Optioneel) een andere criteria toevoegen als u wilt bewaken van een complexe waarschuwingsregel. Gebruikers kunnen momenteel waarschuwingsregels met dynamische drempelwaarden criteria als één criterium hebben.

12. Vul in **Waarschuwingsdetails** zoals **waarschuwing regelnaam**, **beschrijving** en **ernst**

13. Een actiegroep toevoegen aan de waarschuwing door een bestaande actiegroep selecteren of door een nieuwe actiegroep maken.

14. Klik op **gedaan** om op te slaan van de waarschuwingsregel voor metrische gegevens.

> [!NOTE]
> Waarschuwing voor metrische gegevens regels die zijn gemaakt via de portal worden gemaakt in dezelfde resourcegroep bevinden als de doelresource.

## <a name="view-and-manage-with-azure-portal"></a>Weergeven en beheren met Azure portal

U kunt weergeven en beheren van waarschuwingsregels voor metrische gegevens gebruik met behulp van de blade van de regels beheren onder waarschuwingen. De onderstaande procedure ziet u hoe uw waarschuwingsregels voor metrische gegevens weergeven en bewerken van een van beide.

1. Navigeer in Azure portal naar **Monitor**

2. Klik op **waarschuwingen** en **regels beheren**

3. In de **regels beheren** blade vindt u de waarschuwingsregels voor abonnementen. U kunt de regels met behulp van verder filteren **resourcegroep**, **resourcetype** en **Resource**. Als u zien alleen metrische waarschuwingen wilt, selecteert u **type signaal** als metrische gegevens.

    > [!TIP]
    > In de **regels beheren** blade kunt u meerdere regels voor waarschuwingen en ze inschakelen/uitschakelen. Dit kan handig zijn wanneer bepaalde doelresources moeten worden geplaatst in onderhoud

4. Klik op de naam van de metrische waarschuwingsregel die u wilt bewerken

5. In de regel bewerken, klikt u op de **waarschuwen criteria** u wilt bewerken. U kunt de metrische gegevens, drempelvoorwaarde en zo nodig andere velden wijzigen

    > [!NOTE]
    > U kunt niet bewerken de **Doelresource** en **waarschuwing regelnaam** nadat de waarschuwing voor metrische gegevens is gemaakt.

6. Klik op **gedaan** om op te slaan, uw bewerkingen.

## <a name="with-azure-cli"></a>Met Azure CLI

In de vorige secties beschreven hoe u kunt maken, weergeven en beheren van waarschuwingsregels voor metrische gegevens gebruik met Azure portal. In deze sectie wordt beschreven hoe u doet hetzelfde met platformonafhankelijke [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest). Snelste manier om te beginnen met behulp van Azure CLI is via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest). Voor dit artikel gebruiken we cloudshell.

1. Ga naar Azure portal, klikt u op **Cloud shell**.

2. Bij de prompt, kunt u opdrachten met ``--help`` optie voor meer informatie over de opdracht en het gebruik ervan. Bijvoorbeeld, de volgende opdracht toont u de lijst met opdrachten die beschikbaar zijn voor het maken, weergeven en beheren van metrische waarschuwingen

    ```azurecli
    az monitor metrics alert --help
    ```

3. U kunt een eenvoudige waarschuwingsregel voor metrische gegevens die bewaakt als groter is dan 90 gemiddeld Percentage CPU op een virtuele machine maken

    ```azurecli
    az monitor metrics alert create -n {nameofthealert} -g {ResourceGroup} --scopes {VirtualMachineResourceID} --condition "avg Percentage CPU > 90" --description {descriptionofthealert}
    ```

4. U kunt de metrische waarschuwingen weergeven in een resourcegroep met de volgende opdracht

    ```azurecli
    az monitor metrics alert list  -g {ResourceGroup}
    ```

5. Hier ziet u de details van een bepaalde waarschuwingsregel voor metrische gegevens met de naam of de resource-ID van de regel.

    ```azurecli
    az monitor metrics alert show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor metrics alert show --ids {RuleResourceId}
    ```

6. U kunt een waarschuwingsregel voor metrische gegevens met behulp van de volgende opdracht uitschakelen.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} --enabled false
    ```

7. U kunt een waarschuwingsregel voor metrische gegevens met behulp van de volgende opdracht verwijderen.

    ```azurecli
    az monitor metrics alert delete -g {ResourceGroup} -n {AlertRuleName}
    ```

## <a name="next-steps"></a>Volgende stappen

- [Metrische waarschuwingen met behulp van Azure Resource Manager-sjablonen maken](../../azure-monitor/platform/alerts-enable-template.md).
- [Begrijpen hoe metrische waarschuwingen werk](alerts-metric-overview.md).
- [Begrijpen hoe metrische waarschuwingen met dynamische drempelwaarden voorwaarde werk](alerts-dynamic-thresholds.md).
- [Inzicht in de web-hook-schema voor metrische waarschuwingen](../../azure-monitor/platform/alerts-metric-near-real-time.md#payload-schema)

