---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/17/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: ab668a905b435287a4eaf96ff04b2fa5b54deb1d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38764719"
---
Er zijn verschillende redenen wanneer u mag niet beginnen of verbinding maken met een toepassing die wordt uitgevoerd op een Azure-machine (VM). Oorzaken zijn onder meer de toepassing niet wordt uitgevoerd of op de verwachte poorten luistert, de luisterende poort geblokkeerd of netwerken regels niet correct doorgegeven verkeer naar de toepassing. Dit artikel beschrijft een methodisch benadering voor het zoeken en los het probleem.

Als u hebt met het verbinding maken met uw virtuele machine via RDP of SSH problemen, moet u een van de volgende artikelen eerst zien:

* [Problemen oplossen met extern bureaublad-verbindingen met een Windows-gebaseerde virtuele Machine in Azure](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)
* [Problemen oplossen met Secure Shell (SSH)-verbindingen met een Linux-gebaseerde Azure-machine](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../articles/resource-manager-deployment-model.md). In dit artikel komen beide modellen aan de orde, maar u wordt aangeraden voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.

## <a name="quick-start-troubleshooting-steps"></a>Quick start-stappen voor probleemoplossing
Als u verbinding maken met een toepassing hebt, kunt u de volgende algemene stappen voor probleemoplossing. Probeer verbinding te maken voor uw toepassing opnieuw na elke stap:

* Start de virtuele machine
* Het eindpunt opnieuw / firewall-regels / netwerk regels voor de netwerkbeveiligingsgroep (NSG)
  * [Resource Manager-model - Netwerkbeveiligingsgroepen beheren](../articles/virtual-network/manage-network-security-group.md)
  * [Klassieke model - eindpunten voor Cloudservices beheren](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
* Verbinding maken vanaf een andere locatie, zoals een ander Azure virtual network
* De virtuele machine opnieuw implementeren
  * [Windows virtuele machine opnieuw implementeren](../articles/virtual-machines/windows/redeploy-to-new-node.md)
  * [Linux virtuele machine opnieuw implementeren](../articles/virtual-machines/linux/redeploy-to-new-node.md)
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
  * [Klassieke model - eindpunten voor Cloudservices beheren](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Resource Manager-model - Netwerkbeveiligingsgroepen beheren](../articles/virtual-network/manage-network-security-group.md)
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
  * Zie voor meer informatie, [instellen van eindpunten aan een virtuele Machine](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Voor virtuele machines die zijn gemaakt met het implementatiemodel van Resource Manager:
  
  * Controleer of dat de binnenkomende NAT-regelconfiguratie voor de virtuele machine het binnenkomende verkeer, met name het protocol (TCP of UDP) en de openbare en persoonlijke poortnummers toestaat.
  * Controleer of dat Netwerkbeveiligingsgroepen dat de binnenkomende aanvraag en uitgaand antwoordverkeer toestaat.
  * Zie voor meer informatie, [wat is er een netwerkbeveiligingsgroep?](../articles/virtual-network/security-overview.md)

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
[Problemen oplossen met extern bureaublad-verbindingen met een Windows-gebaseerde virtuele Machine in Azure](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)

[Problemen met Secure Shell (SSH)-verbindingen met een Linux-gebaseerde Azure-machine oplossen](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md)

