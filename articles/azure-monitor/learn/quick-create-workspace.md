---
title: Een Log Analytics-werkruimte maken in Azure Portal | Microsoft Docs
description: Informatie over het maken van een Log Analytics-werkruimte om in te schakelen management-oplossingen en -gegevens verzamelen van uw cloud en on-premises omgevingen in Azure portal.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: db0945d9151303e29ac4d585222818fa1d71dbab
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975030"
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Een Log Analytics-werkruimte maken in Azure portal
Dit is een unieke Log Analytics-omgeving met een eigen gegevensopslagplaats, gegevensbronnen en oplossingen in Azure portal die kunt u een Log Analytics-werkruimte instellen.  De stappen in dit artikel zijn vereist als u van plan bent voor het verzamelen van gegevens uit de volgende bronnen:

* Azure-resources in uw abonnement
* On-premises computers bewaakt door System Center Operations Manager
* Apparaatverzamelingen van System Center Configuration Manager 
* Diagnostische gegevens of logboek gegevens uit Azure storage

Zie de volgende onderwerpen voor andere bronnen, zoals Azure-VM's en Windows of Linux-VM's in uw omgeving:

*  [Verzamelen van gegevens van virtuele machines van Azure](../../log-analytics/log-analytics-quick-collect-azurevm.md) 
*  [Gegevens verzamelen van hybride Linux-computer](../../log-analytics/log-analytics-quick-collect-linux-computer.md)
*  [Gegevens verzamelen van hybride Windows-computer](quick-collect-windows-computer.md)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure-portal"></a>Meld u aan bij Azure Portal
Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Een werkruimte maken
1. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.

    ![Azure Portal](media/quick-create-workspace/azure-portal-01.png)
  
2. Klik op **Maken** en geef uw keuze aan voor de volgende items:

  * Geef een naam op voor de nieuwe **Log Analytics-werkruimte**, bijvoorbeeld *StandaardLAWerkruimte*. 
  * Selecteer een **abonnement** om te koppelen door een selectie in de vervolgkeuzelijst te maken als de geselecteerde standaardwaarde niet juist is.
  * Voor **resourcegroep**, kiest u een bestaande resource gebruiken groep al setup of maak een nieuwe.  
  * Selecteer een beschikbare **locatie**.  Zie voor meer informatie, die [Log Analytics is beschikbaar in regio's](https://azure.microsoft.com/regions/services/).
  * Als u een werkruimte maakt in een nieuw abonnement dat na 2 april 2018 is gemaakt, gebruikt deze automatisch de prijscategorie *Per GB*. De optie voor het selecteren van een prijscategorie is dan niet beschikbaar.  Als u een werkruimte voor een bestaand abonnement gemaakt vóór 2 April, of voor een abonnement dat is gekoppeld aan een bestaande Enterprise Agreement (EA)-inschrijving maakt, selecteert u uw gewenste prijscategorie.  Zie voor meer informatie over de afzonderlijke lagen [prijsinformatie voor Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/quick-create-workspace/create-loganalytics-workspace-02.png)  

3. Nadat u de vereiste gegevens hebt opgegeven in het deelvenster **Log Analytics-werkruimte**, klikt u op **OK**.  

Terwijl de gegevens worden geverifieerd en de werkruimte wordt gemaakt, kunt u de voortgang bijhouden onder **Meldingen** in het menu. 

## <a name="next-steps"></a>Volgende stappen
Nu dat u een werkruimte beschikbaar hebt, kunt u verzamelen van telemetrie bewaking configureren, uitvoeren van zoekopdrachten in Logboeken om die gegevens te analyseren en toevoegen van een oplossing om aanvullende gegevens en analytische informatie te bieden. 

* Zie voor het inschakelen van verzamelen van gegevens uit Azure-resources met Azure Diagnostics of Azure storage [Azure verzamelen van Logboeken en metrische gegevens voor gebruik in Log Analytics](../../azure-monitor/platform/collect-azure-metrics-logs.md).  
* [System Center Operations Manager als een gegevensbron toevoegen](../../azure-monitor/platform/om-agents.md) voor het verzamelen van gegevens van agents die rapporteren van uw Operations Manager-beheergroep en sla deze op in uw Log Analytics-werkruimte. 
* Verbinding maken met [Configuration Manager](../../log-analytics/log-analytics-sccm.md) voor het importeren van computers die lid van verzamelingen in de hiërarchie zijn.  
* Controleer de [beheeroplossingen](https://docs.microsoft.com/azure/monitoring/monitoring-solutions-inventory?toc=%2fazure%2flog-analytics%2ftoc.json) beschikbaar en hoe u kunt toevoegen of verwijderen van een oplossing uit uw werkruimte.
