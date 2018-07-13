---
title: Het configureren van een cloudservice (portal) | Microsoft Docs
description: Leer hoe u cloudservices configureren in Azure. Leer hoe u externe toegang tot de rolinstanties, configureren en bijwerken van de configuratie van de cloud-service. Deze voorbeelden gebruiken de Azure-portal.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: jeconnoc
ms.openlocfilehash: 904056363c685ef0a16b229ce72383eb80701a39
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006410"
---
# <a name="how-to-configure-cloud-services"></a>Cloudservices configureren

U kunt de meestgebruikte instellingen voor een cloudservice configureren in Azure portal. Het is ook mogelijk uw configuratiebestand rechtstreeks bij te werken. Download in dat geval een serviceconfiguratiebestand om bij te werken, upload vervolgens het bijgewerkte bestand en werk de cloudservice bij met de configuratiewijzigingen. Welke methode u ook gebruikt, de configuratie-updates worden doorgegeven aan alle rolinstanties.

U kunt ook de exemplaren van uw cloud service-rollen, of extern bureaublad in deze beheren.

Azure kan alleen beschikbaarheid van 99,95 procent service dat tijdens de configuratie-updates hebt u minimaal twee rolinstanties voor elke rol. Waarmee een virtuele machine voor het verwerken van aanvragen van clients, terwijl de andere wordt bijgewerkt. Zie voor meer informatie, [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Wijzigen van een service in de cloud

Na opening de [Azure-portal](https://portal.azure.com/), gaat u naar uw service in de cloud. Hier kunt beheren u veel aspecten van deze.

![Instellingenpagina](./media/cloud-services-how-to-configure-portal/cloud-service.png)

De **instellingen** of **alle instellingen** koppelingen geopend **instellingen** waar u kunt wijzigen de **eigenschappen**, wijzigt de  **Configuratie**, beheren van de **certificaten**Stel **waarschuwingsregels**, en beheren van de **gebruikers** die toegang hebben tot deze cloudservice.

![Azure-cloud service-instellingen](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a>Versie van het Gast-besturingssysteem beheren

Standaard werkt Azure periodiek de Gast-OS naar de nieuwste ondersteunde installatiekopie binnen het type besturingssysteem dat u hebt opgegeven in de serviceconfiguratie (.cscfg), zoals Windows Server 2016.

Als u richten op een specifieke versie van het besturingssysteem wilt, kunt u dit instellen **configuratie**.

![Versie van het besturingssysteem instellen](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)

>[!IMPORTANT]
> Kiezen van dat een specifieke versie van het besturingssysteem schakelt automatische besturingssysteem-updates en kunt u uw eigen verantwoordelijkheid patches. U moet ervoor zorgen dat uw rolinstanties zijn ontvangen van updates of u uw toepassing om beveiligingsproblemen te kan weergeven.

## <a name="monitoring"></a>Bewaking

U kunt waarschuwingen toevoegen aan uw cloudservice. Klik op **instellingen** > **waarschuwingsregels** > **waarschuwing toevoegen**.

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Hier kunt u een waarschuwing instellen. Met de **Metric** vervolgkeuzelijst, kunt u instellen een waarschuwing voor de volgende typen gegevens.

* Schijf lezen
* Schijf schrijven
* Het netwerk
* Netwerk-uitgaand
* CPU-percentage

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Bewaking van een metrische tegel configureren

In plaats van **instellingen** > **waarschuwingsregels**, kunt u klikken op een van de metrische gegevens tegels in de **bewaking** sectie van de cloudservice.

![Cloudservice controleren](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Hier kunt u de grafiek gebruikt in combinatie met de tegel aanpassen of een waarschuwingsregel toevoegen.

## <a name="reboot-reimage-or-remote-desktop"></a>Opnieuw opstarten, installatiekopie terugzetten, of extern bureaublad

U kunt instellen met extern bureaublad via de [Azure-portal (ingesteld met extern bureaublad)](cloud-services-role-enable-remote-desktop-new-portal.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), of via [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md).

Selecteer het cloud service-exemplaar opnieuw opstarten, terugzetten, of extern verbinding maken met een Cloudservice.

![Cloud Service-exemplaar](./media/cloud-services-how-to-configure-portal/cs-instance.png)

U kunt vervolgens initiëren een verbinding met extern bureaublad, op afstand opnieuw opstarten van het exemplaar of op afstand herstellen met een installatiekopie (beginnen met een nieuwe installatiekopie) het exemplaar.

![Cloud Service-exemplaar knoppen](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a>Configureren van uw cscfg-bestand

U moet mogelijk opnieuw configureren van uw cloudservice via de [service config (cscfg)](cloud-services-model-and-package.md#cscfg) bestand. U moet eerst uw cscfg-bestand downloaden, wijzigen en vervolgens uploaden.

1. Klik op de **instellingen** pictogram of de **alle instellingen** koppeling openen **instellingen**.

    ![Instellingenpagina](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. Klik op de **configuratie** item.

    ![Configuratieblade](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. Klik op de knop **Downloaden**.

    ![Downloaden](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. Na het bijwerken van het serviceconfiguratiebestand, uploaden en de configuratie-updates toepassen:

    ![Uploaden](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. Selecteer het cscfg-bestand en klik op **OK**.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [implementeren van een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md).
* [Uw cloudservice beheren](cloud-services-how-to-manage-portal.md).
* Configureer [ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md).
