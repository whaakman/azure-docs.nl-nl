---
title: Netwerken opties van Azure Functions
description: Een overzicht van alle netwerkopties die beschikbaar zijn in Azure Functions
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: conceptual
ms.date: 1/14/2019
ms.author: alkarche
ms.openlocfilehash: 8bbc44e7af68f005f30fff143741bc4bfe0adcf2
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896456"
---
# <a name="azure-functions-networking-options"></a>Netwerken opties van Azure Functions

Dit document beschrijft de suite van netwerk functies die beschikbaar zijn via de Azure-functies die als host fungeert voor opties. Alle van de volgende netwerkopties bieden een mogelijkheid toegang krijgen tot bronnen zonder via internet routeerbare adressen, of toegang tot internet beperken tot een functie-App. De host alle modellen hebben verschillende niveaus van netwerkisolatie beschikbaar en het juiste abonnement kiezen, kunt u voldoen aan de vereisten van uw netwerk isoleren.

Functie-Apps kan worden gehost op verschillende manieren.

* Er zijn een reeks opties die worden uitgevoerd op meerdere tenants-infrastructuur, met verschillende niveaus van VNET-connectiviteit- en schaalopties.
    1. Het plan verbruik, dat kan worden geschaald dynamisch antwoord om te laden en biedt opties voor minimale netwerk isolatie.
    1. Het Premium-abonnement, die ook kan worden geschaald dynamisch, terwijl meer uitgebreide netwerkisolatie.
    1. Het App Service-Plan, die werkt op een vaste schaal, en biedt vergelijkbare netwerkisolatie naar het Premium-abonnement.
* Functions kunnen ook worden uitgevoerd op een App Service Environment (ASE) die uw functie implementeert in uw VNet en biedt volledige netwerk besturingselement en isolatie.

## <a name="networking-feature-matrix"></a>Functiematrix voor netwerken

|                |[Verbruiksabonnement](functions-scale.md#consumption-plan)|⚠ [Premium-abonnement](functions-scale.md##premium-plan-public-preview)|[App Service-plan](functions-scale.md#app-service-plan)|[App Service-omgeving](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[**Inkomende IP-beperkingen**](#inbound-ip-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|
|[**VNET-integratie**](#vnet-integration)|❌No|⚠ Ja|✅Yes|✅Yes|
|[**Voorbeeld van VNET-integratie (Express Route en Service-eindpunten)**](#preview-vnet-integration)|❌No|⚠ Ja|⚠ Ja|✅Yes|
|[**Hybride verbindingen**](#hybrid-connections)|❌No|❌No|✅Yes|✅Yes|
|[**Toegang tot de persoonlijke Site**](#private-site-access)|❌No| ❌No|❌No|✅Yes|

⚠ Preview-functie niet voor gebruik in productieomgevingen

## <a name="inbound-ip-restrictions"></a>Inkomende IP-beperkingen

IP-beperkingen kunt u een prioriteit geordende lijst toestaan/weigeren van IP-adressen die zijn toegestaan voor toegang tot uw app te definiëren. De acceptatielijst kan IPv4 en IPv6-adressen bevatten. Wanneer er een of meer vermeldingen, is er een impliciete weigeren alle die aan het einde van de lijst voorkomt. De mogelijkheid IP-beperkingen werkt met alle functie hosting-opties.

> ! [BELANGRIJK] Als u de Azure portal-editor gebruiken, de portal moeten kunnen rechtstreeks toegang tot uw functie-app die wordt uitgevoerd en het apparaat dat u gebruikt voor toegang tot de portal moet de IP-endexemplaren. Met netwerkbeperkingen op locatie, u kunt nog steeds toegang tot alle functies in de **platformfuncties** tabblad.

[Hier voor meer informatie](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions)

## <a name="vnet-integration"></a>VNET-integratie

VNET-integratie kunt uw functie-app voor toegang tot resources binnen een VNET. VNET-integratie is beschikbaar in de Premium-abonnement en de App Service-plan. Als uw app zich in een App Service Environment, vervolgens wordt al een VNet en vereist geen gebruik van de VNet-integratiefunctie tot resources in hetzelfde VNet.

VNet-integratie kunt u uw functie-app-toegang tot resources in uw virtuele netwerk, maar niet verlenen [toegang tot de persoonlijke site](#private-site-access) aan uw functie-app vanuit het virtuele netwerk.

VNet-integratie wordt vaak gebruikt om de toegang van apps met een databases en -services die worden uitgevoerd in uw VNet. Met VNet-integratie hoeft u geen een openbaar eindpunt beschikbaar maken voor toepassingen op uw virtuele machine maar kan de particuliere niet internet routeerbare adressen in plaats daarvan gebruikt.

De algemeen beschikbare versie van de VNET-integratie is afhankelijk van een VPN-gateway functie-Apps verbinden met een virtueel netwerk. Het is beschikbaar in de functies die worden gehost in een app service-plan. Zie voor informatie over het configureren van deze functie, de [App Service-document van de functie](../app-service/web-sites-integrate-with-vnet.md#enabling-vnet-integration).

### <a name="preview-vnet-integration"></a>Voorbeeld van VNET-integratie

Er is een nieuwe versie van de VNet-integratie-functie die is beschikbaar als preview. Het niet afhankelijk van punt-naar-site VPN en biedt ook ondersteuning voor toegang tot resources via ExpressRoute of Service-eindpunten. Deze functie is beschikbaar in het Premium-abonnement, en in App Service-plannen geschaald naar PremiumV2.

De nieuwe versie van VNet-integratie, dat zich momenteel in preview, biedt de volgende voordelen:

* Er is geen gateway is vereist voor het gebruik van de nieuwe functie voor VNet-integratie
* U kunt toegang tot resources via ExpressRoute-verbindingen zonder extra configuratie dan integreren met de ExpressRoute VNet aangesloten.
* U kunt beveiligde bronnen van het uitvoeren van Functions-Service-eindpunt verbruiken. Om dit te doen, moet u service-eindpunten op het subnet dat wordt gebruikt voor VNet-integratie inschakelen.
* U kunt triggers voor het gebruik van Service-eindpunt beveiligde bronnen met behulp van de nieuwe functie van de VNet-integratie niet configureren. 
* Zowel de functie-app als het VNet moet zich in dezelfde regio bevinden.
* De nieuwe functie moet een niet-gebruikte in uw Resource Manager-VNet-subnet.
* Productieworkloads worden niet ondersteund op de nieuwe versie van VNet-integratie, terwijl het in de Preview-versie.
* Routetabellen en wereldwijde peering zijn nog niet beschikbaar met de nieuwe VNet-integratie.
* Een adres wordt gebruikt voor elke mogelijke functie-app-exemplaar. Omdat de grootte van het gatewaysubnet kan niet worden gewijzigd nadat de toewijzing, gebruikt u een subnet dat de grootte van uw maximale schaal eenvoudig kan worden ondersteund. Bijvoorbeeld, ter ondersteuning van een Premium-abonnement die kan worden geschaald tot 80 exemplaren, wordt aangeraden een `/25` subnet waarmee 126 adressen van de host.

Zie voor meer informatie over het gebruik van VNET-integratie voor Preview-versie, [een functie-app integreren met een Azure Virtual Network](functions-create-vnet.md).

## <a name="hybrid-connections"></a>Hybride verbindingen

[Hybride verbindingen](../service-bus-relay/relay-hybrid-connections-protocol.md) is een functie van Azure Relay die kunnen worden gebruikt voor toegang tot toepassingsresources in andere netwerken. Deze kunt openen vanuit uw app naar een toepassingseindpunt. Het kan niet worden gebruikt voor toegang tot uw toepassing. Hybride verbindingen is beschikbaar voor functies die worden uitgevoerd in een [App Service-plan](functions-scale.md#app-service-plan) en een [App Service-omgeving](../app-service/environment/intro.md).

Als in de functies gebruikt, wordt elke hybride verbinding overeenkomt met een enkele TCP-host en poort combinatie. Dit betekent dat het eindpunt van de hybride verbinding kan zich op elk besturingssysteem en elke toepassing, mits u toegang hebben tot een TCP-poort voor luisteren. De functie Hybrid Connections niet weet of zorgt dat er wat het toepassingsprotocol is, of wat u wilt openen. Het gewoon levert toegang tot het netwerk.

Zie voor meer informatie, de [App Service-documentatie voor hybride verbindingen](../app-service/app-service-hybrid-connections.md), die ondersteuning biedt voor zowel Functions als Web-Apps.

## <a name="private-site-access"></a>Toegang tot de persoonlijke Site

Toegang tot de persoonlijke site verwijst naar het maken van uw app alleen toegankelijk is vanaf een particulier netwerk zoals uit binnen een virtueel Azure-netwerk. Toegang tot de persoonlijke site is alleen beschikbaar met een as-omgeving geconfigureerd met een interne Load Balancer (ILB). Zie voor meer informatie over het gebruik van een ILB as-omgeving [maken en gebruiken van een ILB as-omgeving](../app-service/environment/create-ilb-ase.md).

Er zijn veel manieren voor toegang tot resources in andere hostingopties VNET, maar een as-omgeving is de enige manier om toe te staan van triggers voor een functie vindt plaats via een VNET.
