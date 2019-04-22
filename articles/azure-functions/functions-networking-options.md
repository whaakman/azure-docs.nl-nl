---
title: Azure Functions netwerkopties
description: Een overzicht van alle netwerkopties die beschikbaar zijn in Azure Functions
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: b7af0149a690e3cc3a357a5cb769751e3674d374
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698198"
---
# <a name="azure-functions-networking-options"></a>Azure Functions netwerkopties

Dit artikel beschrijft de functies voor netwerkbeheer beschikbaar voor de hosting-opties voor Azure Functions. Alle van de volgende netwerkopties bieden een mogelijkheid toegang krijgen tot bronnen zonder via internet routeerbare adressen, of toegang tot internet beperken tot een functie-app. 

De hosting modellen hebben verschillende niveaus van netwerkisolatie beschikbaar. Het juiste abonnement kiezen kunt u voldoen aan de vereisten van uw netwerk isoleren.

U kunt de functie-apps in een aantal manieren hosten:

* Er is een set opties die worden uitgevoerd op een multitenant-infrastructuur, met verschillende niveaus van verbinding met het virtuele netwerk- en schaalopties:
    * De [verbruiksabonnement](functions-scale.md#consumption-plan), die dynamisch schalen in reactie op laden en biedt opties voor minimale netwerk isolatie.
    * De [Premium-abonnement](functions-scale.md#premium-plan-public-preview), die ook kan worden geschaald dynamisch, terwijl meer uitgebreide netwerkisolatie.
    * De Azure [App Service-plan](functions-scale.md#app-service-plan), dat werkt op een vaste schaal en biedt vergelijkbare netwerkisolatie naar het Premium-abonnement.
* U kunt functies uitvoeren in een [App Service-omgeving](../app-service/environment/intro.md). Deze methode wordt de functie wordt geïmplementeerd in uw virtuele netwerk en biedt volledige netwerk besturingselement en isolatie.

## <a name="matrix-of-networking-features"></a>Matrix van functies voor netwerkbeheer

|                |[Verbruiksabonnement](functions-scale.md#consumption-plan)|⚠ [Premium-abonnement](functions-scale.md#premium-plan-public-preview)|[App Service-plan](functions-scale.md#app-service-plan)|[App Service-omgeving](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[Inkomende IP-adresbeperkingen](#inbound-ip-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|
|[Integratie van virtueel netwerk](#virtual-network-integration)|❌No|❌No|✅Yes|✅Yes|
|[Preview-versie van de integratie van virtuele netwerken (Azure ExpressRoute en service-eindpunten)](#preview-version-of-virtual-network-integration)|❌No|⚠Ja|⚠Ja|✅Yes|
|[Hybride verbindingen](#hybrid-connections)|❌No|❌No|✅Yes|✅Yes|
|[toegang tot de persoonlijke site](#private-site-access)|❌No| ❌No|❌No|✅Yes|

⚠ Deze preview-functie is niet beschikbaar voor gebruik in productieomgevingen.

## <a name="inbound-ip-restrictions"></a>Inkomende IP-adresbeperkingen

U kunt IP-beperkingen gebruiken om een prioriteit geordende lijst IP-adressen die zijn toegestaan/geweigerd toegang tot uw app te definiëren. De lijst kan IPv4 en IPv6-adressen bevatten. Wanneer er een of meer vermeldingen, bestaat een impliciete 'Alles weigeren' aan het einde van de lijst. IP-beperkingen werkt met alle opties voor het hosten van de functie.

> [!NOTE]
> Voor het gebruik van de Azure portal-editor, moet de portal kunnen rechtstreeks toegang tot uw functie-app die wordt uitgevoerd. Ook moet het apparaat dat u gebruikt voor toegang tot de portal de goedgekeurde IP-hebben. Met netwerkbeperkingen op locatie, u kunt nog steeds toegang tot alle functies op de **platformfuncties** tabblad.

Zie voor meer informatie, [statische toegangsbeperkingen voor Azure App Service](../app-service/app-service-ip-restrictions.md).

## <a name="virtual-network-integration"></a>Integratie van virtueel netwerk

Integratie van virtuele netwerken kunt uw functie-app voor toegang tot resources in een virtueel netwerk. Deze functie is beschikbaar in zowel de Premium-abonnement en de App Service-plan. Als uw app zich in een App Service-omgeving, is al in een virtueel netwerk en het gebruik van de integratie van virtuele netwerken tot bronnen in hetzelfde virtuele netwerk niet nodig.

Integratie van virtuele netwerken kunt u uw functie-app-toegang tot resources in uw virtuele netwerk, maar niet verlenen [toegang tot de persoonlijke site](#private-site-access) aan uw functie-app vanuit het virtuele netwerk.

U kunt de integratie van virtuele netwerken gebruiken om toegang te krijgen vanuit apps met databases en webservices die worden uitgevoerd in het virtuele netwerk. Met integratie van virtuele netwerken hoeft u geen beschikbaar maken van een openbaar eindpunt voor toepassingen op de virtuele machine. U kunt in plaats daarvan de persoonlijke, niet-internet routeerbare adressen gebruiken.

De algemeen beschikbare versie van de integratie van virtuele netwerken, is afhankelijk van een VPN-gateway functie-apps verbinden met een virtueel netwerk. Het is beschikbaar in de functies die worden gehost in een App Service-plan. Zie voor informatie over het configureren van deze functie, [uw app integreren met een Azure virtual network](../app-service/web-sites-integrate-with-vnet.md#enabling-vnet-integration).

### <a name="preview-version-of-virtual-network-integration"></a>Preview-versie van de integratie van virtuele netwerken

Er is een nieuwe versie van het virtuele netwerk-integratiefunctie in preview. Deze niet afhankelijk is van de punt-naar-site VPN. Deze biedt ondersteuning voor toegang tot resources via ExpressRoute of service-eindpunten. Het is beschikbaar in het Premium-abonnement en in App Service-abonnementen naar PremiumV2 vergroot of verkleind.

Hier volgen enkele kenmerken van deze versie:

* U hoeft geen gateway te gebruiken.
* U hebt toegang tot resources in ExpressRoute-verbindingen zonder extra configuratie naast de integratie met het virtuele ExpressRoute-netwerk waarmee verbinding is gemaakt.
* U kunt de service-eindpunt-beveiligde resources van het uitvoeren van functions verbruiken. Inschakelen om dit te doen, service-eindpunten op het subnet dat wordt gebruikt voor de integratie van virtuele netwerken.
* U kunt triggers voor het gebruik van service-eindpunt-beveiligde resources niet configureren. 
* Zowel de functie-app en het virtuele netwerk moeten zich in dezelfde regio bevinden.
* De nieuwe functie moet een niet-gebruikte subnet in het virtuele netwerk dat u hebt geïmplementeerd via Azure Resource Manager.
* Productieworkloads worden niet ondersteund wanneer de functie in preview is.
* Routetabellen en wereldwijde peering zijn nog niet beschikbaar met de functie.
* Een adres wordt gebruikt voor elke mogelijke exemplaar van een functie-app. Omdat u kunt de grootte van het gatewaysubnet niet nadat de toewijzing wijzigen, gebruikt u een subnet dat de grootte van uw maximale schaal eenvoudig kan worden ondersteund. Bijvoorbeeld, ter ondersteuning van een Premium-abonnement die kan worden geschaald tot 80 exemplaren, wordt aangeraden een `/25` subnet waarmee 126 adressen van de host.

Zie voor meer informatie over het gebruik van de preview-versie van de integratie van virtuele netwerken, [een functie-app integreren met een Azure virtual network](functions-create-vnet.md).

## <a name="hybrid-connections"></a>Hybride verbindingen

[Hybride verbindingen](../service-bus-relay/relay-hybrid-connections-protocol.md) is een functie van Azure Relay die u gebruiken kunt voor toegang tot toepassingsresources in andere netwerken. Deze kunt openen vanuit uw app naar een toepassingseindpunt. U kunt deze niet gebruiken voor toegang tot uw toepassing. Hybride verbindingen is beschikbaar voor functies die worden uitgevoerd in een [App Service-plan](functions-scale.md#app-service-plan) en een [App Service-omgeving](../app-service/environment/intro.md).

Als in Azure Functions gebruikt, wordt elke hybride verbinding overeenkomt met een enkele TCP-host en poort combinatie. Dit betekent dat eindpunt van de hybride verbinding kan zich op elk besturingssysteem en elke toepassing, zolang u bij het openen van een TCP-poort voor luisteren. De functie Hybrid Connections niet weet of wat het toepassingsprotocol is, of wat u toegang wilt krijgen tot van belang. Het gewoon biedt toegang tot het netwerk.

Zie voor meer informatie, de [App Service-documentatie voor hybride verbindingen](../app-service/app-service-hybrid-connections.md), die ondersteuning biedt voor functies in een App Service-plan.

## <a name="private-site-access"></a>Toegang tot privésite

Toegang tot de persoonlijke site verwijst naar het maken van uw app alleen toegankelijk vanuit een particulier netwerk zoals uit binnen een virtueel Azure-netwerk. Persoonlijke sitetoegang is alleen met een App Service Environment is geconfigureerd met een interne load balancer (ILB) beschikbaar. Zie voor meer informatie, [maken en gebruiken van een interne load balancer met een App Service Environment](../app-service/environment/create-ilb-ase.md).

Er zijn veel manieren voor toegang tot resources in andere hostingopties virtueel netwerk. Maar een App Service Environment is de enige manier om toe te staan van triggers voor een functie vindt plaats via een virtueel netwerk.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over netwerken en Azure Functions: 

* [Volg de zelfstudie over aan de slag met integratie van virtuele netwerken](./functions-create-vnet.md)
* [Lees de veelgestelde vragen over netwerken functies](./functions-networking-faq.md)
* [Meer informatie over de integratie van virtueel netwerk met App Service/functies](../app-service/web-sites-integrate-with-vnet.md)
* [Meer informatie over virtuele netwerken in Azure](../virtual-network/virtual-networks-overview.md)
* [Inschakelen van meer netwerken functies en controle met App Service-omgevingen](../app-service/environment/intro.md)
* [Verbinding maken met afzonderlijke on-premises bronnen zonder wijzigingen van de firewall via hybride verbindingen](../app-service/app-service-hybrid-connections.md)
