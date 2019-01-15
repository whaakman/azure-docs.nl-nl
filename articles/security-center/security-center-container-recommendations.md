---
title: Aanbevelingen voor de container in Azure Security Center | Microsoft Docs
description: Dit document wordt uitgelegd dat de Azure Security Center aanbevelingen voor het beveiligen van uw containers.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 2e76c7f7-a3dd-4d9f-add9-7e0e10e9324d
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: rkarlin
ms.openlocfilehash: b5cb1fe623062816955278da7b0a9e63cbc19254
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258472"
---
# <a name="understand-azure-security-center-container-recommendations"></a>Aanbevelingen voor Azure Security Center-container begrijpen

Bij het migreren van uw toepassingen monoliet om uit te voeren essentiële cloud-eigen containertoepassingen in productie, u kunt profiteren van de functies van containers, met inbegrip van eenvoudig en snel implementeren en bijwerken. Naarmate het aantal containers die zijn geïmplementeerd om te verhogen, moeten de beveiligingsoplossingen erin biedt u inzicht in de beveiligingsstatus van uw containers en ze beschermen tegen bedreigingen.

Azure Security Center biedt de volgende mogelijkheden om u te helpen u uw containers beveiligen:

- **Inzicht in containers die worden gehost op IaaS Linux-machines**<br>In Azure Security Center wordt het tabblad Containers alle virtuele machines die zijn geïmplementeerd met Docker. Bij het verkennen van de beveiligingsproblemen op een virtuele machine, biedt Security Center aanvullende informatie met betrekking tot de containers op de virtuele machine, zoals Docker-versie en het aantal afbeeldingen die worden uitgevoerd op de host.

    ![container-tabblad](./media/security-center-container-recommendations/docker-recommendation.png)


- **Aanbevelingen voor beveiliging op basis van configuratie-items benchmark voor Docker**<br>Security Center scant uw Docker-configuraties en geeft u inzicht in onjuiste configuraties door te geven van een lijst met alle mislukte regels die zijn beoordeeld. Security Center bevat richtlijnen waarmee u kunt deze problemen snel worden opgelost en bespaar tijd. Security Center wordt voortdurend beoordeelt de Docker-configuraties en biedt u de meest recente status.

    ![container-tabblad](./media/security-center-container-recommendations/container-cis-benchmark.png)

- **Detectie van bedreigingen van realtime-container**<br> Security Center biedt realtime bedreigingen voor uw containers op Linux-machines met AuditD-component. De waarschuwingen identificeren verschillende verdachte Docker-activiteiten, zoals het maken van een bevoegde container op een indicatie van Secure Shell (SSH)-server die worden uitgevoerd binnen een Docker-container of het gebruik van de cryptografische miners-host. U kunt deze informatie gebruiken om snel te herstellen van beveiligingsproblemen met zich mee en de beveiliging van uw containers te verbeteren.

    ![container-tabblad](./media/security-center-container-recommendations/docker-threat-detection.png)

## <a name="recommendations"></a>Aanbevelingen
Gebruik de onderstaande tabellen als referentie om te begrijpen van de beschikbare containers die worden gehost op IaaS Linux-machines en beoordelen van de beveiliging van de Docker-configuraties.

| Aanbeveling | Description | Herstel |
| --- | --- | --- |
|Beveiligingsproblemen in container-beveiligingsconfiguraties herstellen |Beveiligingsproblemen in container-beveiligingsconfiguraties op basis van aanbevolen procedures voor configuratie herstellen.| Door beveiligingslekken in de container-beveiligingsconfiguraties herstellen:<br>1. Bekijk de lijst met mislukte regels.<br>2. Elke regel op basis van de opgegeven instructies oplossen.|


## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over aanbevelingen die betrekking hebben op andere typen Azure-resources:

* [Identiteit en toegang bewaken in Azure Security Center](security-center-identity-access.md)
* [Protecting your network in Azure Security Center](security-center-network-recommendations.md) (Uw netwerk beveiligen in Azure Security Center)
* [Beveiligen van uw Azure SQL-service in Azure Security Center](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligen van uw computers en toepassingen in Azure Security Center](security-center-virtual-machine-protection.md)
* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.

