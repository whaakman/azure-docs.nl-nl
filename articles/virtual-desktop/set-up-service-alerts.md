---
title: Servicewaarschuwingen instellen voor Windows virtueel bureaublad - Azure
description: Over het instellen van Azure Service Health service-meldingen voor virtuele Windows-bureaublad.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: v-chjenk
ms.openlocfilehash: cae75f16da2cad453c74b7e6e9fb62dd789fe5c7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081369"
---
# <a name="tutorial-set-up-service-alerts"></a>Zelfstudie: Servicewaarschuwingen instellen

U kunt Azure Service Health gebruiken voor het bewaken van problemen met de service en statusadviezen voor virtuele Windows-bureaublad. Azure Service Health kan een melding geven u met verschillende typen waarschuwingen (bijvoorbeeld e-mail of SMS), kunt u het effect van een probleem, en tijdens het probleem wordt opgelost. Azure Service Health kunt u downtime verminderen en voorbereiden van gepland onderhoud en wijzigingen die invloed kunnen zijn op de beschikbaarheid van uw resources.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak en servicewaarschuwingen configureren.

Zie voor meer informatie over Azure Service Health, de [documentatie over Azure Health](https://docs.microsoft.com/azure/service-health/).

## <a name="prerequisites"></a>Vereisten

- [Zelfstudie: Een tenant maken in Windows Virtual Desktop Preview](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory)
- [Zelfstudie: Service-principals en roltoewijzingen maken met PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-service-principal-role-powershell)
- [Zelfstudie: Een host-pool maken met Azure Marketplace](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace)

## <a name="create-service-alerts"></a>Servicewaarschuwingen maken

Deze sectie leest u hoe u Azure Service Health configureert en hoe u meldingen die u in Azure portal openen kunt kunt instellen. U kunt verschillende typen waarschuwingen instellen en ze om u te waarschuwen tijdig te plannen.

### <a name="recommended-service-alerts"></a>Aanbevolen servicewaarschuwingen

U wordt aangeraden dat u servicewaarschuwingen voor de volgende typen van de health-gebeurtenissen maken:

- **Probleem met de service:** Meldingen voor belangrijke problemen die invloed hebben op de connectiviteit van uw gebruikers met de service of met de mogelijkheid voor het beheren van uw tenant virtuele Windows-bureaublad.
- **Statusadvies:** Berichten die uw aandacht nodig hebben ontvangen. Hier volgen enkele voorbeelden van dit type melding:
    - Virtuele Machines (VM's) niet veilig is geconfigureerd als open poort 3389
    - Afschaffing van de functionaliteit

### <a name="configure-service-alerts"></a>Servicewaarschuwingen configureren

Servicewaarschuwingen configureren:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **Health-Service.**
3. Volg de instructies in [waarschuwingen voor activiteitenlogboek maken voor servicemeldingen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications?toc=%2Fazure%2Fservice-health%2Ftoc.json#alert-and-new-action-group-using-azure-portal) voor het instellen van uw waarschuwingen en meldingen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u kunt instellen en gebruiken van Azure Service Health om te controleren op problemen met de service en statusadviezen virtuele Windows-bureaublad. Voor meer informatie over hoe u zich aanmeldt bij Windows virtueel bureaublad, blijven het verbinding maken met Windows virtuele bureaublad uitleg.

> [!div class="nextstepaction"]
> [Verbinding maken met de extern bureaublad-client op Windows 7 en Windows 10](./connect-windows-7-and-10.md)
