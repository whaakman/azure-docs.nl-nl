---
title: IP-firewall voor Azure Cosmos-accounts
description: Leer hoe u Azure Cosmos DB-gegevens beveiligen met behulp van beleid voor IP-toegangsbeheer voor firewallondersteuning van de.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: 2bb3665f0d0ab15f212741a6ce032fcd7ac2620d
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358110"
---
# <a name="ip-firewall-for-azure-cosmos-accounts"></a>IP-firewall voor Azure Cosmos-accounts

Als u wilt beveiligen gegevens die zijn opgeslagen in uw account, Azure Cosmos DB biedt ondersteuning voor een geheim op basis van autorisatiemodel dat gebruikmaakt van een sterke Hash HMAC-based Message Authentication Code (). Azure Cosmos DB ondersteunt bovendien besturingselementen voor toegang op basis van IP voor firewallregels voor binnenkomend ondersteuning. Dit model is vergelijkbaar met de firewall-regels van een traditionele database-systeem en biedt een extra beveiligingsniveau aan uw account. Met firewalls, kunt u uw Azure Cosmos-account zijn alleen toegankelijk vanuit een goedgekeurde set computers en/of cloud services configureren. Toegang tot gegevens die zijn opgeslagen in uw Azure Cosmos-database van deze goedgekeurde sets van machines en services is nog steeds vereist voor de oproepende functie om weer te geven van een geldige Autorisatietoken.

## <a id="ip-access-control-overview"></a>IP-Toegangsbeheer-overzicht

Uw Azure Cosmos-account is standaard toegankelijk is vanaf internet, zolang de aanvraag wordt vergezeld gaan van een geldige Autorisatietoken. Voor het configureren van IP-toegangsbeheer op basis van beleid, moet de gebruiker de set met IP-adressen of IP-adresbereiken in CIDR (Classless Inter-Domain Routing) formulier om op te nemen als toegestane lijst van client-IP's voor toegang tot een bepaald Azure-Cosmos-account opgeven. Zodra deze configuratie is toegepast, ontvangen alle aanvragen die afkomstig zijn van computers buiten deze toegestane lijst 403 (verboden)-antwoord. Bij het gebruik van IP-firewall, is het raadzaam om toe te staan van Azure portal voor toegang tot uw account. Toegang is vereist om gebruik van data explorer ook over het ophalen van metrische gegevens voor uw account die wordt weergegeven op de Azure-portal.

U kunt IP-gebaseerde firewall combineren met subnet en VNET-toegangsbeheer. Door deze worden gecombineerd, kunt u toegang voor elke bron waarvoor een openbaar IP-adres en/of van een specifiek subnet binnen het VNET te beperken. Voor meer informatie over het gebruik van subnet en toegangsbeheer op basis van een VNET Zie [toegang tot Azure Cosmos DB-resources van virtuele netwerken](vnet-service-endpoint.md).

Om samen te vatten, is Autorisatietoken altijd vereist voor toegang tot een Azure Cosmos-account. Als het IP-firewall- en VNET-toegangsbeheerlijst (ACL's) zijn niet ingesteld, wordt de Azure Cosmos-account toegankelijk door het Autorisatietoken. Nadat de IP-firewall of VNET-ACL's of beide zijn ingesteld op de Azure Cosmos-account, krijgt alleen aanvragen die afkomstig zijn uit de bronnen die u hebt opgegeven (en door het Autorisatietoken) geldige antwoorden. 

## <a name="next-steps"></a>Volgende stappen

U kunt vervolgens IP-firewall of VNET-service-eindpunt voor uw account configureert met behulp van de volgende documenten:

* [IP-firewall voor uw Azure Cosmos-account configureren](how-to-configure-firewall.md)
* [Toegang tot Azure Cosmos DB-resources van virtuele netwerken](vnet-service-endpoint.md)
* [Service-eindpunt voor virtueel netwerk voor uw Azure Cosmos-account configureren](how-to-configure-vnet-service-endpoint.md)




