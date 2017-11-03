---
title: Traffic Manager-Eindpunttypen | Microsoft Docs
description: Dit artikel wordt uitgelegd voor verschillende soorten eindpunten die kunnen worden gebruikt met Azure Traffic Manager
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 4e506986-f78d-41d1-becf-56c8516e4d21
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: kumud
ms.openlocfilehash: 792712e3e529d77ff20a7603b5fbf028ca60f8c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="traffic-manager-endpoints"></a>Traffic Manager-eindpunten
Microsoft Azure Traffic Manager kunt u bepalen hoe netwerkverkeer wordt gedistribueerd naar de implementaties van toepassingen uitgevoerd in verschillende datacenters. U configureren elke implementatie van toepassing als een 'eindpunt' in Traffic Manager. Wanneer het Traffic Manager een DNS-aanvraag ontvangt, kiest deze een eindpunt beschikbaar in het DNS-antwoord retourneren. Het Traffic manager wordt de keuze gebaseerd op de huidige Eindpuntstatus van het en de methode verkeer routering. Zie voor meer informatie [hoe Traffic Manager werkt](traffic-manager-how-traffic-manager-works.md).

Er zijn drie soorten eindpunt ondersteund door Traffic Manager:
* **Azure-eindpunten** worden gebruikt voor services die worden gehost in Azure.
* **Externe eindpunten** worden gebruikt voor services die worden gehost buiten Azure, on-premises of bij een andere hostingprovider.
* **Genest eindpunten** worden gebruikt voor het combineren van Traffic Manager-profielen voor het maken van meer flexibiliteit voor verkeersroutering schema's voor de ondersteuning van grotere, complexe implementaties.

Er is geen beperking op hoe de eindpunten van verschillende typen worden gecombineerd in één Traffic Manager-profiel. Elk profiel kan elke combinatie van eindpunttypen bevatten.

De volgende secties beschrijven elk eindpunttype verdiepen.

## <a name="azure-endpoints"></a>Azure-eindpunten

Azure-eindpunten worden gebruikt voor op basis van een Azure-services in Traffic Manager. De volgende Azure-resource-typen worden ondersteund:

* 'Klassiek' IaaS VM's en PaaS-cloudservices.
* Web Apps
* PublicIPAddress resources (die kunnen worden verbonden met virtuele machines rechtstreeks of via een Azure Load Balancer). De publicIpAddress moet een DNS-naam toegewezen aan worden gebruikt in een Traffic Manager-profiel hebben.

PublicIPAddress resources zijn Azure Resource Manager-resources. Ze bestaan niet in het klassieke implementatiemodel. Ze zijn daarom alleen ondersteund in Traffic Manager van Azure Resource Manager-ervaringen. De andere endpoint-typen worden ondersteund via zowel Resource Manager en het klassieke implementatiemodel.

Wanneer u Azure-eindpunten, detecteert de Traffic Manager als een 'Classic' IaaS VM, cloudservice of een Web-App is gestopt en gestart. Deze status wordt weergegeven in de Eindpuntstatus. Zie [Traffic Manager eindpuntcontrole](traffic-manager-monitoring.md#endpoint-and-profile-status) voor meer informatie. Wanneer de onderliggende service is gestopt, voert Traffic Manager geen eindpunt statuscontroles of directe verkeer naar het eindpunt. Er zijn geen facturering Traffic Manager-gebeurtenissen optreden voor het exemplaar gestopt. Wanneer de service is gestart, is facturering wordt hervat en het eindpunt in aanmerking voor verkeer. Deze detectie geldt niet voor PublicIpAddress-eindpunten.

## <a name="external-endpoints"></a>Externe eindpunten

Externe eindpunten worden gebruikt voor services buiten Azure. Bijvoorbeeld een service die lokaal worden gehost of met een andere provider. Externe eindpunten worden afzonderlijk gebruikt of worden gecombineerd met de Azure-eindpunten in Traffic Manager-profiel. Azure-eindpunten met externe eindpunten combineren, kunt verschillende scenario's:

* In beide een actief-actief of actief / passief failover-model gebruiken Azure vergrote redundantie voor een bestaande lokale toepassing.
* De latentie van de toepassing voor gebruikers overal ter wereld te verminderen, het uitbreiden van een bestaande on-premises toepassing tot extra geografische locaties in Azure. Zie voor meer informatie [Traffic Manager 'Prestaties' voor verkeersroutering](traffic-manager-routing-methods.md#performance).
* Gebruik Azure voor extra capaciteit voor een bestaande lokale toepassing, continu of als een 'burst-naar-cloud'-oplossing om te voldoen aan een piek in de aanvraag.

In bepaalde gevallen is het nuttig om externe eindpunten gebruiken om te verwijzen naar Azure-services (Zie voor voorbeelden van de [Veelgestelde vragen over](traffic-manager-faqs.md#traffic-manager-endpoints)). In dit geval wordt statuscontroles gefactureerd op de snelheid van de Azure-eindpunten, niet op de externe eindpunten-snelheid. Echter, in tegenstelling tot Azure-eindpunten, als u stoppen of verwijderen van de onderliggende service health controleren facturering wordt vervolgd totdat u uitschakelen of verwijderen van het in Traffic Manager-eindpunt.

## <a name="nested-endpoints"></a>Geneste eindpunten

Geneste eindpunten combineren meerdere Traffic Manager-profielen voor het maken van flexibele verkeersroutering schema's en ondersteuning biedt voor grotere, complexe implementaties. Een profiel 'onderliggend item' wordt als een eindpunt met geneste eindpunten toegevoegd aan een profiel van de 'parent'. Zowel de onderliggende en bovenliggende profielen kunnen andere eindpunten van elk type, met inbegrip van andere geneste profielen bevatten. Zie voor meer informatie [Traffic Manager-profielen genest](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps als eindpunten

Enkele aanvullende overwegingen bij het configureren van Web-Apps als eindpunten in Traffic Manager van toepassing:

1. Alleen Web-Apps op de SKU van de 'Standaard' of hoger, komen in aanmerking voor gebruik met Traffic Manager. Het toevoegen van een Web-App van een lagere SKU is mislukt. De SKU van een bestaande Web-App downgraden resulteert in Traffic Manager, niet meer verkeer kunnen verzenden naar deze Web-App.
2. Als een eindpunt ontvangt een HTTP-aanvraag, gebruikt de header 'host' in de aanvraag om te bepalen welke Web-App moet de aanvraag verwerkt. De host-header bevat de DNS-naam gebruikt voor het initiëren van de aanvraag kan bijvoorbeeld contosoapp.azurewebsites.net. Voor het gebruik van een andere DNS-naam met uw Web-App, moet u de DNS-naam geregistreerd als een aangepaste domeinnaam voor de App. Wanneer een eindpunt van de Web-App als een Azure-eindpunt toe te voegen, wordt de DNS-naam van het Traffic Manager-profiel automatisch geregistreerd voor de App. Deze registratie automatisch verwijderd wanneer het eindpunt wordt verwijderd.
3. Elke Traffic Manager-profiel kan maximaal één eindpunt van Web-App van elke Azure-regio hebben. Om te omzeilen voor deze beperking, kunt u een Web-App configureren als een extern eindpunt. Zie voor meer informatie de [Veelgestelde vragen over](traffic-manager-faqs.md#traffic-manager-endpoints).

## <a name="enabling-and-disabling-endpoints"></a>Inschakelen en uitschakelen van eindpunten

Uitschakelen van een eindpunt in Traffic Manager kan nuttig zijn voor tijdelijk verkeer verwijderen uit een eindpunt dat in de onderhoudsmodus of opnieuw wordt geïmplementeerd. Zodra het eindpunt opnieuw actief is, kan het zijn opnieuw worden ingeschakeld.

Eindpunten kunnen worden ingeschakeld en worden uitgeschakeld via het Traffic Manager-portal, PowerShell, CLI of REST API, die allemaal in Resource Manager en het klassieke implementatiemodel worden ondersteund.

> [!NOTE]
> Uitschakelen van een Azure-eindpunt heeft geen invloed op de implementatiestatus in Azure. Een Azure-service (zoals een virtuele machine of Web-App blijft actief en kan ontvangen verkeer, zelfs wanneer uitgeschakeld in Traffic Manager. Verkeer kan worden verholpen rechtstreeks aan het service-exemplaar in plaats van via de DNS-naam van het Traffic Manager-profiel. Zie voor meer informatie [de werking van Traffic Manager](traffic-manager-how-traffic-manager-works.md).

De huidige geschiktheid van elk eindpunt verkeer ontvangt, is afhankelijk van de volgende factoren:

* De status van het profiel (ingeschakeld/uitgeschakeld)
* De status van endpoint (ingeschakeld/uitgeschakeld)
* De resultaten van de status controleert op dat eindpunt

Zie voor meer informatie [Traffic Manager eindpuntcontrole](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Aangezien Traffic Manager werkt op het niveau van de DNS-, is het niet van invloed zijn op bestaande verbindingen met een willekeurig eindpunt. Wanneer u een eindpunt niet beschikbaar is, stuurt Traffic Manager nieuwe verbindingen met een andere beschikbare eindpunt. De host achter het eindpunt uitgeschakeld of slecht kan blijven echter verkeer via bestaande verbindingen ontvangen totdat deze sessies worden beëindigd. Toepassingen moeten de sessieduur voor verkeer uit bestaande verbindingen beperken.

Als alle eindpunten in een profiel zijn uitgeschakeld of als het profiel zelf is uitgeschakeld, klikt u vervolgens Traffic Manager een reactie 'NXDOMAIN' naar een nieuwe DNS-query stuurt.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [de werking van Traffic Manager](traffic-manager-how-traffic-manager-works.md).
* Meer informatie over het Traffic Manager [eindpunt bewakings- en automatische failover](traffic-manager-monitoring.md).
* Meer informatie over het Traffic Manager [verkeersrouteringsmethoden](traffic-manager-routing-methods.md).
