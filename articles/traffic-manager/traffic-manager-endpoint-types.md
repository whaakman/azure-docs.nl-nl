---
title: Traffic Manager-Eindpunttypen | Microsoft Docs
description: Dit artikel wordt uitgelegd voor verschillende soorten eindpunten die kunnen worden gebruikt met Azure Traffic Manager
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: timlt
editor: ''
ms.assetid: 4e506986-f78d-41d1-becf-56c8516e4d21
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: kumud
ms.openlocfilehash: 06965c43408e943922048804099f8f28d69c8540
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248282"
---
# <a name="traffic-manager-endpoints"></a>Traffic Manager-eindpunten
Microsoft Azure Traffic Manager kunt u bepalen hoe netwerkverkeer wordt gedistribueerd naar implementaties van toepassingen die worden uitgevoerd in verschillende datacenters. U configureren elke implementatie van toepassing als een 'eindpunt' in Traffic Manager. Bij het Traffic Manager een DNS-aanvraag ontvangt, kiest deze een eindpunt beschikbaar om te retourneren in de DNS-antwoord. Traffic manager wordt de basis van de keuze van de huidige status van endpoint en de verkeersrouteringsmethode. Zie voor meer informatie, [hoe Traffic Manager werkt](traffic-manager-how-it-works.md).

Er zijn drie typen van ondersteund door Traffic Manager-eindpunt:
* **Azure-eindpunten** worden gebruikt voor services die worden gehost in Azure.
* **Externe eindpunten** worden gebruikt voor IPv4/IPv6-adressen of voor services die worden gehost buiten Azure waarmee u kunt on-premises zijn of met een andere hosting provider.
* **Geneste eindpunten** worden gebruikt voor het combineren van Traffic Manager-profielen voor het maken van meer flexibele routering van verkeer schema's voor de ondersteuning van de behoeften van grotere, complexere implementaties.

Er is geen beperking op hoe de eindpunten van verschillende typen worden gecombineerd in één Traffic Manager-profiel. Elk profiel kan elke combinatie van eindpunttypen bevatten.

De volgende secties beschrijven elk eindpunt van het type in de opslagprestaties.

## <a name="azure-endpoints"></a>Azure-eindpunten

Azure-eindpunten worden gebruikt voor services op basis van Azure in Traffic Manager. De volgende Azure-resource-typen worden ondersteund:

* PaaS-cloudservices.
* Web Apps
* Web-Appsites
* PublicIPAddress-resources (die kunnen worden verbonden met virtuele machines rechtstreeks of via een Azure Load Balancer). Het openbare IP-adres moet een DNS-naam die moet worden gebruikt in een Traffic Manager-profiel is toegewezen.

PublicIPAddress-resources zijn Azure Resource Manager-resources. Ze bestaan niet in het klassieke implementatiemodel. Dus zijn ze alleen ondersteund in Traffic Manager van Azure Resource Manager-ervaringen. Het andere eindpunttypen worden ondersteund via zowel Resource Manager en het klassieke implementatiemodel.

Wanneer u Azure-eindpunten, detecteert de Traffic Manager als een 'Classic' IaaS-VM, cloudservice of een Web-App is gestopt en gestart. Deze status wordt weergegeven in de Eindpuntstatus. Zie [eindpuntcontrole van Traffic Manager](traffic-manager-monitoring.md#endpoint-and-profile-status) voor meer informatie. Wanneer de onderliggende service is gestopt, wordt Traffic Manager niet uitvoeren van eindpunten of verkeer naar het eindpunt. Er zijn geen facturering Traffic Manager-gebeurtenissen optreden voor het exemplaar gestopt. Wanneer de service opnieuw wordt opgestart, komt hervat facturering en het eindpunt in aanmerking om verkeer te ontvangen. Deze detectie is niet van toepassing op PublicIpAddress-eindpunten.

## <a name="external-endpoints"></a>Externe eindpunten

Externe eindpunten worden gebruikt voor beide IPv4/IPv6-adressen of voor services die buiten Azure. Gebruik van IPv4/IPv6-adres-eindpunten kunt traffic manager om te controleren of de status van de eindpunten van zonder een DNS-naam voor deze. Traffic Manager kan als gevolg hiervan, reageren op query's met A/AAAA-records wanneer dat eindpunt in een antwoord geretourneerd. Services buiten Azure zijn bijvoorbeeld een service die wordt gehost on-premises of aan een andere provider. Externe eindpunten kunnen afzonderlijk worden gebruikt of in combinatie met Azure-eindpunten in dezelfde Traffic Manager-profiel, met uitzondering van eindpunten die zijn opgegeven als IPv4 of IPv6-adressen die alleen externe eindpunten kunnen worden. Azure-eindpunten met externe eindpunten combineren, kunt verschillende scenario's:

* Verbeterde redundantie bieden voor een bestaande on-premises toepassing in beide een actief-actief of actief-passief-failover-model met behulp van Azure. 
* Verkeer routeren naar eindpunten waarop geen een DNS-naam die is gekoppeld. Bovendien de algehele DNS-lookup-latentie verminderen door het verwijderen van de noodzaak om uit te voeren van een tweede DNS-query om een IP-adres van een DNS-naam die wordt geretourneerd. 
* Verminder de latentie van de toepassing voor gebruikers over de hele wereld, een bestaande on-premises toepassing naar extra geografische locaties in Azure uit te breiden. Zie voor meer informatie, [Traffic Manager 'Prestaties' Routering van verkeer in](traffic-manager-routing-methods.md#performance).
* Geef extra capaciteit voor een bestaande on-premises toepassing, continu of als een 'burst-to-cloud'-oplossing om te voldoen aan een piek in de vraag met behulp van Azure.

In bepaalde gevallen is het nuttig om externe eindpunten gebruiken om te verwijzen naar Azure-services (Zie voor voorbeelden van de [Veelgestelde vragen over](traffic-manager-faqs.md#traffic-manager-endpoints)). In dit geval statuscontroles worden in rekening gebracht tegen het tarief voor Azure-eindpunten, niet de snelheid van de externe eindpunten. Echter, in tegenstelling tot Azure-eindpunten, als u stoppen of verwijderen van de onderliggende service statuscontrole facturering gaat door totdat u uitschakelen of verwijderen van het eindpunt in Traffic Manager.

## <a name="nested-endpoints"></a>Geneste eindpunten

Geneste eindpunten combineren meerdere Traffic Manager-profielen voor het maken van flexibele schema's routering van verkeer en ondersteuning van de behoeften van grotere, complexe implementaties. Met geneste eindpunten, een profiel 'onderliggende' toegevoegd als een eindpunt dat aan een profiel van de 'parent'. Zowel de onderliggende en bovenliggende profielen kunnen andere eindpunten van elk type zijn, met inbegrip van andere geneste profielen bevatten. Zie voor meer informatie, [geneste Traffic Manager-profielen](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps als eindpunten

Enkele aanvullende overwegingen zijn van toepassing bij het configureren van Web-Apps als eindpunten in Traffic Manager:

1. Alleen Web-Apps op de 'Standaard'-SKU of hoger komen in aanmerking voor gebruik met Traffic Manager. Pogingen om toe te voegen een Web-App van een lagere SKU mislukken. Het downgraden van de SKU van een bestaande Web-App, resulteert in Traffic Manager, niet meer verkeer verzenden naar die Web-App.
2. Wanneer een eindpunt een HTTP-aanvraag ontvangt, gebruikt de header 'host' in de aanvraag om te bepalen welke Web-App moet de aanvraag-service. De host-header bevat de DNS-naam gebruikt voor het starten van de aanvraag, bijvoorbeeld contosoapp.azurewebsites.net. Voor het gebruik van een andere DNS-naam met uw Web-App, moet de DNS-naam als een aangepaste domeinnaam voor de App worden geregistreerd. Bij het toevoegen van een Web-App-eindpunt als een Azure-eindpunt, wordt de DNS-naam van het Traffic Manager-profiel wordt automatisch geregistreerd voor de App. Deze registratie automatisch verwijderd wanneer het eindpunt wordt verwijderd.
3. Elke Traffic Manager-profiel kan maximaal één Web-App-eindpunt hebben van elke Azure-regio. Als tijdelijke oplossing voor deze beperking, kunt u een Web-App configureren als een extern eindpunt. Zie voor meer informatie de [Veelgestelde vragen over](traffic-manager-faqs.md#traffic-manager-endpoints).

## <a name="enabling-and-disabling-endpoints"></a>Inschakelen en uitschakelen van eindpunten

Uitschakelen van een eindpunt in Traffic Manager kan nuttig zijn tijdelijk verkeer verwijderen uit een eindpunt dat in de onderhoudsmodus staat of opnieuw wordt geïmplementeerd zijn. Zodra het eindpunt opnieuw wordt uitgevoerd, kan deze niet opnieuw worden ingeschakeld.

Eindpunten kunnen worden ingeschakeld en worden uitgeschakeld via de Traffic Manager-portal, PowerShell, CLI of REST-API.

> [!NOTE]
> Uitschakelen van een Azure-eindpunt heeft geen invloed op de implementatiestatus in Azure. Een Azure-service (zoals een virtuele machine of de Web-App blijft actief en kunnen ontvangen verkeer, zelfs als uitgeschakeld in Traffic Manager. Verkeer kan worden aangepakt rechtstreeks naar het service-exemplaar in plaats van via de DNS-naam van het Traffic Manager-profiel. Zie voor meer informatie, [hoe Traffic Manager werkt](traffic-manager-how-it-works.md).

De huidige geschiktheid van alle eindpunten om verkeer te ontvangen, is afhankelijk van de volgende factoren:

* De status van het profiel (ingeschakeld/uitgeschakeld)
* De Eindpuntstatus (ingeschakeld/uitgeschakeld)
* De resultaten van de status wordt gecontroleerd op dat eindpunt

Zie voor meer informatie, [eindpuntcontrole van Traffic Manager](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Sinds de Traffic Manager werkt op DNS-niveau, is het niet van invloed zijn op bestaande verbindingen met een willekeurig eindpunt. Wanneer u een eindpunt niet beschikbaar is, stuurt Traffic Manager nieuwe verbindingen met een ander beschikbaar eindpunt. De host achter het eindpunt uitgeschakeld of niet in orde kan echter nog steeds verkeer via bestaande verbindingen ontvangen totdat deze sessies worden beëindigd. Toepassingen moeten de sessieduur van de zodat verkeer uit bestaande verbindingen beperken.

Als alle eindpunten in een profiel zijn uitgeschakeld, of als het profiel zelf is uitgeschakeld, stuurt Traffic Manager een antwoord 'NXDOMAIN' naar een nieuwe DNS-query.


## <a name="next-steps"></a>Volgende stappen

* Informatie over [hoe Traffic Manager werkt](traffic-manager-how-it-works.md).
* Meer informatie over Traffic Manager [eindpunt bewakings- en automatische failover](traffic-manager-monitoring.md).
* Meer informatie over Traffic Manager [verkeersrouteringsmethoden](traffic-manager-routing-methods.md).
