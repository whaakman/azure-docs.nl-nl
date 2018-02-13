---
title: Bewaken met Surface Hubs met Azure Log Analytics | Microsoft Docs
description: Gebruik de Surface Hub-oplossing de status van uw Surface Hubs bijhouden en begrijpen hoe deze worden gebruikt.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 525b3ced979834a956f91ef8c6f647b659ca21f1
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a>Surface Hubs met logboekanalyse voor het bijhouden van hun status controleren

![Surface Hub symbool](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

Dit artikel wordt beschreven hoe u kunt de Surface Hub-oplossing in logboekanalyse voor het bewaken van de Microsoft Surface Hub-apparaten. Log Analytics kunt u de status van uw Surface Hubs goed begrijpen hoe deze worden gebruikt.

Elke Surface Hub heeft Microsoft Monitoring Agent geïnstalleerd. De via de agent die u gegevens kunt verzenden vanuit uw Surface Hub met logboekanalyse. Logboekbestanden worden gelezen uit uw Surface Hubs en worden vervolgens met logboekanalyse worden verzonden. Problemen, zoals servers offline is, de kalender niet synchroniseert, of als het account zich niet aanmelden bij Skype worden weergegeven in het dashboard Surface Hub in logboekanalyse. Met behulp van de gegevens in het dashboard, kunt u apparaten die niet worden uitgevoerd, of die zijn andere problemen en oplossingen voor de gedetecteerde problemen mogelijk van toepassing te identificeren.

## <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie om te installeren en configureren van de oplossing. Om uw Surface Hubs in logboekanalyse te beheren, hebt u het volgende nodig:

* Een [logboekanalyse abonnement](https://azure.microsoft.com/pricing/details/log-analytics/) niveau die ondersteuning bieden voor het aantal apparaten dat u wilt bewaken. Log Analytics prijzen varieert afhankelijk van hoeveel apparaten zijn ingeschreven en hoeveel gegevens deze processen. Moet u dit in aanmerking te nemen bij het plannen van uw implementatie Surface Hub.

Vervolgens wordt u een bestaande werkruimte voor logboekanalyse toevoegen of een nieuwe maken. Gedetailleerde instructies voor het gebruik van beide methoden is op [aan de slag met logboekanalyse](log-analytics-get-started.md). Zodra de werkruimte voor logboekanalyse is geconfigureerd, zijn er twee manieren om uw Surface Hub-apparaten te registreren:

* Automatisch via Intune
* Handmatig via **instellingen** op uw apparaat Surface Hub.

## <a name="set-up-monitoring"></a>Controle instellen
U kunt de status en activiteit van uw Surface Hub met logboekanalyse controleren. U kunt de Surface Hub inschrijven met behulp van Intune of lokaal via **instellingen** op de Surface Hub.

## <a name="connect-surface-hubs-to-log-analytics-through-intune"></a>Verbinding maken met Surface Hubs met logboekanalyse via Intune
Hebt u de werkruimte-ID en werkruimtesleutel voor de werkruimte voor logboekanalyse die uw Surface Hubs zullen beheren. U krijgt die uit de werkruimte-instellingen in de Azure portal.

Intune is een Microsoft-product waarmee u de configuratie-instellingen voor logboekanalyse die worden toegepast op een of meer van uw apparaten centraal te beheren. Volg deze stappen voor het configureren van uw apparaten via Intune:

1. Aanmelden bij Intune.
2. Navigeer naar **instellingen** > **verbonden gegevensbronnen**.
3. Maken of bewerken van een beleid op basis van de sjabloon die Surface Hub.
4. Navigeer naar de sectie OMS (Azure Operational Insights) van het beleid en voeg de logboekanalyse *werkruimte-ID* en *Werkruimtesleutel* aan het beleid.
5. Sla het beleid.
6. Koppelt het beleid met de juiste groep apparaten.

   ![Intune-beleid](./media/log-analytics-surface-hubs/intune.png)

De instellingen voor logboekanalyse Intune vervolgens gesynchroniseerd met de apparaten in de doelgroep ze inschrijven in de werkruimte voor logboekanalyse.

## <a name="connect-surface-hubs-to-log-analytics-using-the-settings-app"></a>Verbinding maken met Surface Hubs met behulp van de app instellingen logboekanalyse
Hebt u de werkruimte-ID en werkruimtesleutel voor de werkruimte voor logboekanalyse die uw Surface Hubs zullen beheren. U kunt ophalen uit de instellingen voor de werkruimte voor logboekanalyse in de Azure portal.

Als u Intune niet gebruikt om uw omgeving te beheren, kunt u handmatig via apparaten inschrijven **instellingen** op elke Surface Hub:

1. Open in uw Surface Hub **instellingen**.
2. Voer de beheerdersreferenties apparaat wanneer u wordt gevraagd.
3. Klik op **dit apparaat**, en de onder **bewaking**, klikt u op **OMS-instellingen configureren**.
4. Selecteer **bewaking inschakelen**.
5. Typ in het dialoogvenster OMS-instellingen voor de logboekanalyse **werkruimte-ID** en typt u de **Werkruimtesleutel**.  
   ![Instellingen](./media/log-analytics-surface-hubs/settings.png)
6. Klik op **OK** om de configuratie te voltooien.

Een bevestiging wordt weergegeven waarin staat of de configuratie is toegepast op het apparaat. Als dit het geval is, wordt er een bericht weergegeven dat de agent is verbonden met logboekanalyse. Het apparaat wordt vervolgens gestart voor het verzenden van gegevens met logboekanalyse waar u kunt weergeven en erop reageren.

## <a name="monitor-surface-hubs"></a>Monitor met Surface Hubs
Bewaking van uw Surface Hubs lijkt met behulp van logboekanalyse veel op de bewaking van andere geregistreerde apparaten.

1. Meld u aan bij Azure Portal.
2. Navigeer naar de werkruimte voor logboekanalyse en selecteer **overzicht**.
2. Klik op de tegel Surface Hub.
3. Status van uw apparaat wordt weergegeven.

   ![Surface Hub-dashboard](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

U kunt maken [waarschuwingen](log-analytics-alerts.md) op basis van bestaande of aangepaste logboek zoekopdrachten. Log Analytics uit uw Surface Hubs verzamelt gegevens gebruikt, kunt u zoeken naar problemen en een waarschuwing voor de voorwaarden die u voor uw apparaten definieert.

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten aanmelden met logboekanalyse](log-analytics-log-searches.md) om gedetailleerde Surface Hub-gegevens te bekijken.
* Maak [waarschuwingen](log-analytics-alerts.md) om u te waarschuwen wanneer er problemen optreden met uw Surface Hubs.
