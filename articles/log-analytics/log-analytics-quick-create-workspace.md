---
title: Een werkruimte maken in Azure Log Analytics | Microsoft Docs
description: Informatie over het maken van een werkruimte voor logboekanalyse zodat management oplossingen en gegevensverzameling van uw cloud en on-premises omgevingen.
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: magoedte
ms.openlocfilehash: 8259a97d28effa7bfa9cfb9d7cd9cd2a14c9d906
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Een Log Analytics-werkruimte maken in de Azure portal
Dit is een unieke Log Analytics-omgeving met een eigen data-opslagplaats, gegevensbronnen en oplossingen in de Azure-Portal die kunt u een werkruimte voor logboekanalyse instellen.  De stappen in dit artikel zijn vereist als u van plan bent op het verzamelen van gegevens van de volgende bronnen:

* Azure-resources in uw abonnement
* Lokale computers worden bewaakt door System Center Operations Manager
* Apparaatverzamelingen van System Center Configuration Manager 
* Diagnose- of logboekgegevens van Azure Storage

Voor andere bronnen, zoals Azure VM's en Windows of Linux-computers in uw omgeving, Zie de volgende onderwerpen:

*  [Verzamelt gegevens over Azure Virtual Machines](log-analytics-quick-collect-azurevm.md) 
*  [Verzamelen van gegevens over Linux-computers](log-analytics-quick-collect-linux-computer.md)
*  [Verzamelen van gegevens over Windows-computers](log-analytics-quick-collect-windows-computer.md)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure-portal"></a>Aanmelden bij Azure portal
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Een werkruimte maken
1. Klik in de Azure-portal op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Klik op **maken**, en selecteer vervolgens de opties voor de volgende items:

  * Geef een naam op voor de nieuwe **OMS-werkruimte**, zoals *DefaultLAWorkspace*. 
  * Selecteer een **abonnement** om te koppelen door een selectie in de vervolgkeuzelijst te maken als de geselecteerde standaardwaarde niet juist is.
  * Voor **resourcegroep**, ervoor kiezen een bestaande resource gebruiken groep al setup of maak een nieuwe.  
  * Selecteer een beschikbaar **locatie**.  Zie voor aanvullende informatie die [Log Analytics is beschikbaar in regio's](https://azure.microsoft.com/regions/services/).
  * U kunt kiezen uit drie verschillende **Prijscategorieën** in Log Analytics, maar voor deze snelstartgids die u gaat selecteren de **gratis** laag.  Zie voor meer informatie over de specifieke lagen, [Log Analytics prijsinformatie](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Na het opgeven van de vereiste informatie op de **OMS-werkruimte** deelvenster, klikt u op **OK**.  

Terwijl de gegevens worden geverifieerd en de werkruimte wordt gemaakt, kunt u de voortgang bijhouden onder **Meldingen** in het menu. 

## <a name="next-steps"></a>Volgende stappen
Nu dat u een werkruimte die beschikbaar is hebt, kunt u verzamelen van de bewaking van telemetrie configureren, logboek zoeken en om die gegevens te analyseren en toevoegen van een oplossing om aanvullende gegevens en analytische inzichten te bieden. 

* Zie voor het inschakelen van gegevensverzameling van Azure-resources met Azure Diagnostics of Azure-opslag [verzamelen Azure service-logboeken en metrische gegevens voor gebruik in logboekanalyse](log-analytics-azure-storage.md).  
* [System Center Operations Manager toevoegen als een gegevensbron](log-analytics-om-agents.md) voor het verzamelen van gegevens van uw beheergroep van Operations Manager reporting agents en opslaan in uw opslagplaats Log Analytics-werkruimte. 
* Verbinding maken met [Configuration Manager](log-analytics-sccm.md) voor het importeren van computers die lid van verzamelingen in de hiërarchie zijn.  
* Controleer de [beheeroplossingen](/log-analytics-add-solutions.md) beschikbaar en het toevoegen of verwijderen van een oplossing uit uw werkruimte.

