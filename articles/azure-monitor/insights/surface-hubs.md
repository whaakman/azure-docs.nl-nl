---
title: Surface Hubs bewaken met Azure met Azure Monitor | Microsoft Docs
description: Gebruik de Surface Hub-oplossing de status van uw Surface Hubs volgen en te begrijpen hoe deze worden gebruikt.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: magoedte
ms.openlocfilehash: 902cf62e53581785caf2730f63af3633d8e1e498
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56005434"
---
# <a name="monitor-surface-hubs-with-azure-monitor-to-track-their-health"></a>Surface Hubs met Azure Monitor om bij te houden van hun status controleren

![Surface Hub-symbool](./media/surface-hubs/surface-hub-symbol.png)

Dit artikel wordt beschreven hoe u de Surface Hub-oplossing in Azure Monitor kunt gebruiken voor het bewaken van Microsoft Surface Hub-apparaten. De oplossing helpt u Volg de status van uw Surface Hubs goed te begrijpen hoe deze worden gebruikt.

Elke Surface Hub de Microsoft Monitoring Agent is geïnstalleerd. De door de agent dat u gegevens uit uw Surface Hub bij een Log Analytics-werkruimte in Azure Monitor verzenden kunt. Logboekbestanden worden gelezen vanuit uw Surface Hubs en worden vervolgens naar Azure Monitor worden verzonden. Problemen zoals servers die offline is, wordt de kalender niet gesynchroniseerd, of als het apparaataccount kan niet aanmelden bij Skype worden weergegeven in de Surface Hub-dashboard in Azure Monitor. Met behulp van de gegevens in het dashboard, kunt u apparaten die niet worden uitgevoerd of die andere problemen en oplossingen voor de gedetecteerde problemen mogelijk worden toegepast identificeren.

## <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie om de oplossing te installeren en configureren. Beheer van uw Surface Hubs in Azure Monitor, moet u het volgende:

* Een [Log Analytics-abonnement](https://azure.microsoft.com/pricing/details/log-analytics/) niveau die ondersteuning bieden voor het aantal apparaten dat u wilt bewaken. Prijzen voor log Analytics is afhankelijk van hoeveel apparaten zijn ingeschreven en hoeveel gegevens er processen. Moet u dit in aanmerking te nemen bij het plannen van de Surface Hub-implementatie.

Vervolgens wordt u een bestaande Log Analytics-werkruimte toevoegen of een nieuwe maken. Gedetailleerde instructies voor het gebruik van een van beide methoden is op [een Log Analytics-werkruimte maken in Azure portal](../learn/quick-create-workspace.md). Wanneer de Log Analytics-werkruimte is geconfigureerd, zijn er twee manieren om uw Surface Hub-apparaten te registreren:

* Automatisch via Intune
* Handmatig via **instellingen** op de Surface Hub-apparaat.

## <a name="set-up-monitoring"></a>Controle instellen
U kunt de status en activiteit van de Surface Hub met behulp van Azure Monitor kunt bewaken. U kunt de Surface Hub inschrijven met behulp van Intune, of lokaal via **instellingen** op de Surface Hub.

## <a name="connect-surface-hubs-to-azure-monitor-through-intune"></a>Surface Hubs verbinden met Azure Monitor via Intune
U moet de werkruimtesleutel en werkruimte-ID voor de Log Analytics-werkruimte waarmee uw Surface Hubs worden beheerd. U kunt die van de instellingen van de werkruimte ophalen in Azure portal.

Intune is een Microsoft-product waarmee u de Log Analytics-werkruimte configuratie-instellingen die worden toegepast op een of meer van uw apparaten centraal te beheren. Volg deze stappen voor het configureren van uw apparaten via Intune:

1. Aanmelden bij Intune.
2. Navigeer naar **instellingen** > **verbonden bronnen**.
3. Maken of bewerken van een beleid op basis van de Surface Hub-sjabloon.
4. Ga naar de Azure Operational Insights-sectie van het beleid en de Log Analytics toevoegen *werkruimte-ID* en *Werkruimtesleutel* aan het beleid.
5. Sla het beleid.
6. Beleid koppelen aan de juiste groep van apparaten.

   ![Intune-beleid](./media/surface-hubs/intune.png)

Vervolgens worden gesynchroniseerd met Intune de instellingen van de Log Analytics met de apparaten in de doelgroep, deze inschrijft in uw Log Analytics-werkruimte.

## <a name="connect-surface-hubs-to-azure-monitor-using-the-settings-app"></a>Surface Hubs verbinden met Azure Monitor met de app instellingen
U moet de werkruimtesleutel en werkruimte-ID voor de Log Analytics-werkruimte waarmee uw Surface Hubs worden beheerd. U kunt ophalen uit de instellingen voor de Log Analytics-werkruimte in de Azure-portal.

Als u Intune niet gebruikt om uw omgeving te beheren, kunt u handmatig via apparaten registreren **instellingen** op elke Surface Hub:

1. Open in de Surface Hub, **instellingen**.
2. Voer de referenties van de beheerder apparaat wanneer u hierom wordt gevraagd.
3. Klik op **dit apparaat**, en de onder **bewaking**, klikt u op **logboekanalyse-instellingen configureren**.
4. Selecteer **bewaking inschakelen**.
5. Typ in het dialoogvenster van de instellingen voor Log Analytics de met Log Analytics **werkruimte-ID** en typt u de **Werkruimtesleutel**.  
   ![settings](./media/surface-hubs/settings.png)
6. Klik op **OK** om de configuratie te voltooien.

Een bevestiging weergegeven waarin staat of de configuratie is toegepast op het apparaat. Als het geval is, wordt er een bericht weergegeven waarin staat dat de agent is verbonden met Azure Monitor. Het apparaat wordt vervolgens gestart voor het verzenden van gegevens naar Azure Monitor kunt u weergeven en erop reageren.

## <a name="monitor-surface-hubs"></a>Monitor voor Surface Hubs
Bewaking van uw Surface Hubs is met behulp van Azure Monitor vergelijkbaar met bewaking van andere geregistreerde apparaten.

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

Wanneer u op de Surface Hub-tegel klikt, wordt de status van uw apparaat weergegeven.

   ![Surface Hub-dashboard](./media/surface-hubs/surface-hub-dashboard.png)

U kunt maken [waarschuwingen](../platform/alerts-overview.md) op basis van bestaande of aangepaste zoekopdrachten. Met behulp van de gegevens door die Azure Monitor worden verzameld uit uw Surface Hubs, kunt u zoeken naar problemen en een waarschuwing voor de voorwaarden die u voor uw apparaten definiëren.

## <a name="next-steps"></a>Volgende stappen
* Gebruik [query's bijgehouden in Azure Monitor](../log-query/log-query-overview.md) om gedetailleerde Surface Hub-gegevens weer te geven.
* Maak [waarschuwingen](../platform/alerts-overview.md) om u te waarschuwen wanneer er problemen met uw Surface Hubs optreden.
