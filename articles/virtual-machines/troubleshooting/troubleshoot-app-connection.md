---
title: Problemen oplossen met toegang voor VM-toepassing in Azure | Microsoft Docs
description: Deze gedetailleerde stappen voor probleemoplossing gebruiken voor het isoleren van problemen in de verbinding te maken met toepassingen die worden uitgevoerd op virtuele machines in Azure.
services: virtual-machines
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: Kan toepassing te starten, het programma kan niet worden geopend, poort geblokkeerd, kan niet worden geblokkeerd poort voor luisteren start het programma, luisteren
ms.assetid: b9ff7cd0-0c5d-4c3c-a6be-3ac47abf31ba
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: ae5a720ce6ddd6b773229d0968bc283aa5cfa5ba
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413700"
---
# <a name="troubleshoot-application-connectivity-issues-on-virtual-machines-in-azure"></a>Verbindingsproblemen van toepassing op virtuele machines in Azure oplossen

Er zijn verschillende redenen wanneer u mag niet beginnen of verbinding maken met een toepassing die wordt uitgevoerd op een Azure-machine (VM). Oorzaken zijn onder meer de toepassing niet wordt uitgevoerd of op de verwachte poorten luistert, de luisterende poort geblokkeerd of netwerken regels niet correct doorgegeven verkeer naar de toepassing. Dit artikel beschrijft een methodisch benadering voor het zoeken en los het probleem.

Als u hebt met het verbinding maken met uw virtuele machine via RDP of SSH problemen, moet u een van de volgende artikelen eerst zien:

* [Problemen oplossen met extern bureaublad-verbindingen met een Windows-gebaseerde virtuele Machine in Azure](troubleshoot-rdp-connection.md)
* [Problemen oplossen met Secure Shell (SSH)-verbindingen met een Linux-gebaseerde Azure-machine](troubleshoot-ssh-connection.md).

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.

## <a name="quick-start-troubleshooting-steps"></a>Quick start-stappen voor probleemoplossing
Als u verbinding maken met een toepassing hebt, kunt u de volgende algemene stappen voor probleemoplossing. Probeer verbinding te maken voor uw toepassing opnieuw na elke stap:

* Start de virtuele machine
* Het eindpunt opnieuw / firewall-regels / netwerk regels voor de netwerkbeveiligingsgroep (NSG)
  * [Resource Manager-model - Netwerkbeveiligingsgroepen beheren](../../virtual-network/manage-network-security-group.md)
  * [Klassieke model - eindpunten voor Cloudservices beheren](../../cloud-services/cloud-services-enable-communication-role-instances.md)
* Verbinding maken vanaf een andere locatie, zoals een ander Azure virtual network
* De virtuele machine opnieuw implementeren
  * [Windows virtuele machine opnieuw implementeren](redeploy-to-new-node-windows.md)
  * [Linux virtuele machine opnieuw implementeren](redeploy-to-new-node-linux.md)
* De virtuele machine opnieuw maken

Zie voor meer informatie, [probleemoplossing Endpoint Connectivity (RDP/SSH/HTTP, enz. fouten)](https://social.msdn.microsoft.com/Forums/azure/en-US/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows).

## <a name="detailed-troubleshooting-overview"></a>Gedetailleerd overzicht van het oplossen van problemen
Er zijn vier hoofdgebieden om op te lossen van de toegang van een toepassing die wordt uitgevoerd op een virtuele machine van Azure.

![problemen oplossen kan toepassing niet starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1. De toepassing die wordt uitgevoerd op virtuele machine van Azure.
   * De toepassing zelf correct wordt uitgevoerd?
2. Virtuele machine van Azure.
   * De virtuele machine zelf is correct uitgevoerd en het reageren op aanvragen?
3. Azure-netwerk-eindpunten.
   * Cloud service-eindpunten voor virtuele machines in het klassieke implementatiemodel.
   * Netwerkbeveiligingsgroepen en binnenkomende NAT-regels voor virtuele machines in Resource Manager-implementatiemodel.
   * Kan de stroom van gebruikers aan de virtuele machine/toepassing op de verwachte poorten verkeer?
4. Uw Internet-edge-apparaat.
   * Firewall-regels in plaats verhinderen dat verkeer stroomt correct?

Voor clientcomputers die toegang de toepassing via een site-naar-site VPN of ExpressRoute-verbinding tot zijn, zijn de belangrijkste gebieden die leiden tot problemen met kunnen de toepassing en de virtuele machine van Azure.

Volg deze stappen om te bepalen van de bron van het probleem en de correctie.

## <a name="step-1-access-application-from-target-vm"></a>Stap 1: Toegang tot de toepassing van de doel-VM
Probeert te krijgen tot de toepassing met de juiste client-programma van de virtuele machine waarop deze wordt uitgevoerd. Naam van de lokale host, het lokale IP-adres of de loopbackadres (127.0.0.1) gebruiken.

![toepassing rechtstreeks vanuit de virtuele machine starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Bijvoorbeeld, als de toepassing een webserver, open een browser op de virtuele machine en probeert te krijgen tot een webpagina die wordt gehost op de virtuele machine.

Als u de toepassing openen kunt, gaat u naar [stap 2](#step2).

Als u geen toegang de toepassing tot, controleert u of de volgende instellingen:

* De toepassing wordt uitgevoerd op de virtuele doelmachine.
* De toepassing luistert op de verwachte TCP en UDP-poorten.

Gebruik op Windows en Linux gebaseerde virtuele machines, de **netstat - a** opdracht om de actieve luisterpoorten weer te geven. Bekijk de uitvoer voor de verwachte poorten waarop uw toepassing moet luisteren. De toepassing opnieuw starten of om de verwachte poorten indien nodig kunnen gebruiken en probeer opnieuw toegang krijgen tot de toepassing lokaal configureren.

## <a id="step2"></a>Stap 2: Toegang tot de toepassing van een andere virtuele machine in hetzelfde virtuele netwerk
Probeer toegang tot de toepassing van een andere virtuele machine, maar in hetzelfde virtuele netwerk, met behulp van de VM-hostnaam of het Azure-toegewezen openbare, persoonlijke of provider IP-adres. Voor virtuele machines die zijn gemaakt met het klassieke implementatiemodel, moet u het openbare IP-adres van de cloudservice niet gebruiken.

![toepassing starten vanuit een andere virtuele machine](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Bijvoorbeeld, als de toepassing een webserver is, probeert te krijgen tot een webpagina vanuit een browser op een andere virtuele machine in hetzelfde virtuele netwerk.

Als u de toepassing openen kunt, gaat u naar [stap 3](#step3).

Als u geen toegang de toepassing tot, controleert u of de volgende instellingen:

* De firewall van de host op de doel-VM toe dat de binnenkomende aanvraag en uitgaand antwoordverkeer.
* Indringers detecteren of het netwerk bewaken die worden uitgevoerd op de doel-VM wordt het verkeer wordt toegestaan.
* Cloud Services-eindpunten of Netwerkbeveiligingsgroepen toestaat dat het verkeer:
  * [Klassieke model - eindpunten voor Cloudservices beheren](../../cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Resource Manager-model - Netwerkbeveiligingsgroepen beheren](../../virtual-network/manage-network-security-group.md)
* Een afzonderlijke component die wordt uitgevoerd in de virtuele machine in het pad tussen de test en uw VM's, zoals een load balancer of een firewall, toestaat het verkeer.

Op een Windows-gebaseerde virtuele machine, gebruikt u Windows Firewall met geavanceerde beveiliging om te bepalen of de firewall-regels van uw toepassing binnenkomend en uitgaand verkeer uitsluiten.

## <a id="step3"></a>Stap 3: Toegang tot de toepassing van buiten het virtuele netwerk
Probeer toegang tot de toepassing vanaf een computer buiten het virtuele netwerk als de virtuele machine waarop de toepassing wordt uitgevoerd. Gebruik een ander netwerk als de oorspronkelijke clientcomputer.

![toepassing starten op een computer buiten het virtuele netwerk](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Bijvoorbeeld, als de toepassing een webserver is, probeert te krijgen tot de webpagina wordt weergegeven in een browser op een computer die zich niet in het virtuele netwerk.

Als u geen toegang de toepassing tot, controleert u of de volgende instellingen:

* Voor virtuele machines die zijn gemaakt met het klassieke implementatiemodel:
  
  * Controleer of dat de configuratie van eindpunten voor de virtuele machine het binnenkomende verkeer, met name het protocol (TCP of UDP) en de openbare en persoonlijke poortnummers toestaat.
  * Controleer of dat toegangsbeheerlijsten (ACL's) op het eindpunt niet voorkomen inkomend verkeer van Internet dat.
  * Zie voor meer informatie, [instellen van eindpunten aan een virtuele Machine](../windows/classic/setup-endpoints.md).
* Voor virtuele machines die zijn gemaakt met het implementatiemodel van Resource Manager:
  
  * Controleer of dat de binnenkomende NAT-regelconfiguratie voor de virtuele machine het binnenkomende verkeer, met name het protocol (TCP of UDP) en de openbare en persoonlijke poortnummers toestaat.
  * Controleer of dat Netwerkbeveiligingsgroepen dat de binnenkomende aanvraag en uitgaand antwoordverkeer toestaat.
  * Zie voor meer informatie, [wat is er een netwerkbeveiligingsgroep?](../../virtual-network/security-overview.md)

Als de virtuele machine of het eindpunt een lid van een load balancer-set is:

* Controleer of de test-protocol (TCP of UDP) en het poortnummer juist zijn.
* Als de test-protocol en poort is anders dan het load balancer-set-protocol en poort:
  * Controleer of de toepassing luistert op de test-protocol (TCP of UDP) en het poortnummer (Gebruik **netstat – a** op de doel-VM).
  * Controleer of dat de firewall van de host op de doel-VM is waardoor de aanvraag voor een binnenkomende statustest en antwoordverkeer uitgaande test.

Als u de toepassing openen kunt, zorgt u ervoor dat uw edge-apparaat voor Internet toestaat:

* Het verkeer voor het aanvragen van uitgaande aanvraag vanaf uw clientcomputer met de Azure-machine.
* De binnenkomende aanvraag-antwoordverkeer van de virtuele machine van Azure.

## <a name="step-4-if-you-cannot-access-the-application-use-ip-verify-to-check-the-settings"></a>Stap 4 als u geen toegang de toepassing tot gebruiken IP-Controleer of de instellingen te controleren. 

Zie voor meer informatie, [Azure-netwerk bewakingsoverzicht](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="additional-resources"></a>Aanvullende resources
[Problemen oplossen met extern bureaublad-verbindingen met een Windows-gebaseerde virtuele Machine in Azure](troubleshoot-rdp-connection.md)

[Problemen met Secure Shell (SSH)-verbindingen met een Linux-gebaseerde Azure-machine oplossen](troubleshoot-ssh-connection.md)


