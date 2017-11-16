---
title: Het configureren van een cloudservice (portal) | Microsoft Docs
description: Informatie over het configureren van cloud-services in Azure. Informatie over het bijwerken van de configuratie van de cloud-service en het configureren van externe toegang tot de rolinstanties. Deze voorbeelden worden de Azure portal gebruiken.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: adegeo
ms.openlocfilehash: 409b3bb26648ef1b811dfaaf37690c8220046729
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="how-to-configure-cloud-services"></a>Het configureren van Cloud-Services
U kunt de meest gebruikte instellingen voor een cloudservice in de Azure portal. Het is ook mogelijk uw configuratiebestand rechtstreeks bij te werken. Download in dat geval een serviceconfiguratiebestand om bij te werken, upload vervolgens het bijgewerkte bestand en werk de cloudservice bij met de configuratiewijzigingen. Welke methode u ook gebruikt, de configuratie-updates worden doorgegeven aan alle rolinstanties.

U kunt ook de exemplaren van uw cloud service-rollen of extern bureaublad in ze beheren.

Azure zorgt alleen servicebeschikbaarheid 99,95% tijdens de configuratie-updates hebt u ten minste twee rolexemplaren voor elke rol. Waarmee één virtuele machine voor het verwerken van aanvragen van clients, terwijl de andere wordt bijgewerkt. Zie voor meer informatie [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Een cloudservice wijzigen
Na opening de [Azure-portal](https://portal.azure.com/), gaat u naar de cloudservice. Hier kunt beheren u veel aspecten van deze.

![Pagina met instellingen](./media/cloud-services-how-to-configure-portal/cloud-service.png)

De **instellingen** of **alle instellingen** koppelingen geopend de **instellingen** blade waar kunt u de **eigenschappen**, de wijzigen **Configuratie**, beheren de **certificaten**, setup **waarschuwing regels**, en beheren van de **gebruikers** die toegang hebben tot deze cloudservice.

![Blade van Azure cloud service-instellingen](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a>Gast OS-versie beheren

Standaard werkt Azure periodiek het gastbesturingssysteem op de meest recente ondersteunde installatiekopie binnen de OS-familie die u hebt opgegeven in de serviceconfiguratie (.cscfg), zoals Windows Server 2016.

Als u een specifieke versie van het besturingssysteem als doel moet, kunt u dit instellen de **configuratie** blade.

![Versie van het besturingssysteem instellen](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> U kiest dat een specifieke versie van het besturingssysteem schakelt automatische OS bijgewerkt waardoor het uw verantwoordelijkheid patchen. U moet ervoor zorgen dat uw rolinstanties zijn ontvangen van updates of u uw toepassing beveiligingsproblemen kan blootstellen.

## <a name="monitoring"></a>Bewaking
U kunt waarschuwingen toevoegen aan uw service in de cloud. Klik op **instellingen** > **waarschuwing regels** > **waarschuwing toevoegen**.

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Hier kunt kunt u een waarschuwing instellen. Met de **metriek** vervolgkeuzelijst, kunt u een waarschuwing voor de volgende soorten gegevens instellen.

* Schijf lezen
* Schijf schrijven
* Het netwerk
* Uit het netwerk
* CPU-percentage

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Bewaking van een metrische tegel configureren
In plaats van **instellingen** > **waarschuwingsregels**, kunt u klikken op een van de metrische tegels in de **bewaking** sectie van de **Cloudservice**  blade.

![Cloudservice controleren](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Hier kunt u de grafiek gebruikt met de tegel aanpassen of een waarschuwingsregel toevoegen.

## <a name="reboot-reimage-or-remote-desktop"></a>Opnieuw opstarten of terugzetten van de installatiekopie extern bureaublad
U kunt instellen met extern bureaublad via de [Azure-portal (extern bureaublad instellen)](cloud-services-role-enable-remote-desktop-new-portal.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), of via [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).

Opnieuw opstarten, klik terugzetten van de installatiekopie of extern in een Cloudservice, op het cloud service-exemplaar.

![Cloud Service-exemplaar](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Op de blade dat wordt geopend initiëren van een verbinding met extern bureaublad, op afstand opnieuw opstarten van het exemplaar of op afstand installatiekopie (met een nieuwe installatiekopie) het exemplaar beginnen.

![Cloud Service-exemplaar knoppen](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a>Uw cscfg-bestand configureren
U moet wellicht opnieuw configureren van uw cloudservice via de [(cscfg) van de serviceconfiguratie](cloud-services-model-and-package.md#cscfg) bestand. U moet eerst uw cscfg-bestand downloaden, wijzigen en vervolgens te uploaden.

1. Klik op de **instellingen** pictogram of de **alle instellingen** koppeling te openen van de **instellingen** blade.

    ![Pagina met instellingen](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. Klik op de **configuratie** item.

    ![Configuratie-Blade](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. Klik op de **downloaden** knop.

    ![Downloaden](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. Na het bijwerken van het configuratiebestand van de service, uploaden en updates voor de configuratie van toepassing:

    ![Uploaden](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. Selecteer het cscfg-bestand en klik op **OK**.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [implementeren van een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md).
* [Beheren van uw cloudservice](cloud-services-how-to-manage-portal.md).
* Configureer [ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md).
