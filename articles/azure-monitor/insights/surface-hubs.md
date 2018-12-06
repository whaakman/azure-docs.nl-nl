---
title: Surface Hubs met Azure Log Analytics controleren | Microsoft Docs
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
ms.devlang: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: a2cedb95469f97b564aaad2baa0ff7c784d67bf8
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960988"
---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a>Surface Hubs met Log Analytics voor het bijhouden van hun status controleren

![Surface Hub-symbool](./media/surface-hubs/surface-hub-symbol.png)

Dit artikel wordt beschreven hoe u de Surface Hub-oplossing in Log Analytics kunt gebruiken voor het bewaken van Microsoft Surface Hub-apparaten. Log Analytics helpt u Volg de status van uw Surface Hubs goed te begrijpen hoe deze worden gebruikt.

Elke Surface Hub de Microsoft Monitoring Agent is geïnstalleerd. De door de agent dat u gegevens uit uw Surface Hub naar Log Analytics verzenden kunt. Logboekbestanden worden gelezen vanuit uw Surface Hubs en worden vervolgens naar Log Analytics worden verzonden. Problemen zoals servers die offline is, wordt de kalender niet gesynchroniseerd, of als het apparaataccount kan niet aanmelden bij Skype worden weergegeven in de Surface Hub-dashboard in Log Analytics. Met behulp van de gegevens in het dashboard, kunt u apparaten die niet worden uitgevoerd of die andere problemen en oplossingen voor de gedetecteerde problemen mogelijk worden toegepast identificeren.

## <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie om de oplossing te installeren en configureren. Als u wilt uw Surface Hubs in Log Analytics beheren, moet u het volgende:

* Een [Log Analytics-abonnement](https://azure.microsoft.com/pricing/details/log-analytics/) niveau die ondersteuning bieden voor het aantal apparaten dat u wilt bewaken. Prijzen voor log Analytics is afhankelijk van hoeveel apparaten zijn ingeschreven en hoeveel gegevens er processen. Moet u dit in aanmerking te nemen bij het plannen van de Surface Hub-implementatie.

Vervolgens wordt u een bestaande Log Analytics-werkruimte toevoegen of een nieuwe maken. Gedetailleerde instructies voor het gebruik van een van beide methoden is op [een Log Analytics-werkruimte maken in Azure portal](../../azure-monitor/learn/quick-create-workspace.md). Wanneer de Log Analytics-werkruimte is geconfigureerd, zijn er twee manieren om uw Surface Hub-apparaten te registreren:

* Automatisch via Intune
* Handmatig via **instellingen** op de Surface Hub-apparaat.

## <a name="set-up-monitoring"></a>Controle instellen
U kunt de status en activiteit van de Surface Hub met behulp van Log Analytics controleren. U kunt de Surface Hub inschrijven met behulp van Intune, of lokaal via **instellingen** op de Surface Hub.

## <a name="connect-surface-hubs-to-log-analytics-through-intune"></a>Surface Hubs verbinden met Log Analytics gebruikt via Intune
U moet de werkruimtesleutel en werkruimte-ID voor de Log Analytics-werkruimte waarmee uw Surface Hubs worden beheerd. U kunt die van de instellingen van de werkruimte ophalen in Azure portal.

Intune is een Microsoft-product waarmee u de Log Analytics-configuratie-instellingen die worden toegepast op een of meer van uw apparaten centraal te beheren. Volg deze stappen voor het configureren van uw apparaten via Intune:

1. Aanmelden bij Intune.
2. Navigeer naar **instellingen** > **verbonden bronnen**.
3. Maken of bewerken van een beleid op basis van de Surface Hub-sjabloon.
4. Ga naar de Azure Operational Insights-sectie van het beleid en de Log Analytics toevoegen *werkruimte-ID* en *Werkruimtesleutel* aan het beleid.
5. Sla het beleid.
6. Beleid koppelen aan de juiste groep van apparaten.

   ![Intune-beleid](./media/surface-hubs/intune.png)

Vervolgens worden gesynchroniseerd met Intune de instellingen van de Log Analytics met de apparaten in de doelgroep, deze inschrijft in uw Log Analytics-werkruimte.

## <a name="connect-surface-hubs-to-log-analytics-using-the-settings-app"></a>Surface Hubs verbinden met Log Analytics met behulp van de app instellingen
U moet de werkruimtesleutel en werkruimte-ID voor de Log Analytics-werkruimte waarmee uw Surface Hubs worden beheerd. U kunt ophalen uit de instellingen voor de Log Analytics-werkruimte in de Azure-portal.

Als u Intune niet gebruikt om uw omgeving te beheren, kunt u handmatig via apparaten registreren **instellingen** op elke Surface Hub:

1. Open in de Surface Hub, **instellingen**.
2. Voer de referenties van de beheerder apparaat wanneer u hierom wordt gevraagd.
3. Klik op **dit apparaat**, en de onder **bewaking**, klikt u op **logboekanalyse-instellingen configureren**.
4. Selecteer **bewaking inschakelen**.
5. Typ in het dialoogvenster van de instellingen voor Log Analytics de met Log Analytics **werkruimte-ID** en typt u de **Werkruimtesleutel**.  
   ![Instellingen](./media/surface-hubs/settings.png)
6. Klik op **OK** om de configuratie te voltooien.

Een bevestiging weergegeven waarin staat of de configuratie is toegepast op het apparaat. Als het geval is, wordt er een bericht weergegeven waarin staat dat de agent is verbonden met Log Analytics. Het apparaat wordt vervolgens gestart voor het verzenden van gegevens naar Log Analytics waar u kunt weergeven en erop reageren.

## <a name="monitor-surface-hubs"></a>Monitor voor Surface Hubs
Bewaking van uw Surface Hubs is met behulp van Log Analytics vergelijkbaar met bewaking van andere geregistreerde apparaten.

1. Meld u aan bij Azure Portal.
2. Navigeer naar uw Log Analytics-werkruimte en selecteer **overzicht**.
2. Klik op de Surface Hub-tegel.
3. Status van uw apparaat wordt weergegeven.

   ![Surface Hub-dashboard](./media/surface-hubs/surface-hub-dashboard.png)

U kunt maken [waarschuwingen](../../monitoring-and-diagnostics/monitoring-overview-alerts.md) op basis van bestaande of aangepaste zoekopdrachten. Met behulp van de gegevens die Log Analytics uit uw Surface Hubs verzamelt, kunt u zoeken naar problemen en een waarschuwing voor de voorwaarden die u voor uw apparaten definiëren.

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten in Logboeken in Log Analytics](../../azure-monitor/log-query/log-query-overview.md) om gedetailleerde Surface Hub-gegevens weer te geven.
* Maak [waarschuwingen](../../monitoring-and-diagnostics/monitoring-overview-alerts.md) om u te waarschuwen wanneer er problemen met uw Surface Hubs optreden.
