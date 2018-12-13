---
title: Voorbereiden voor inkomende IP-adres wijzigen - Azure App Service
description: Als uw inkomende IP-adres wordt gebruikt om te worden gewijzigd, leert u wat te doen zodat uw app blijft gewoon werken na de wijziging.
services: app-service\web
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 2a494b318011d601609033bc877134f0b0eeff09
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53249325"
---
# <a name="how-to-prepare-for-an-inbound-ip-address-change"></a>Voorbereiden voor een inkomende IP-adres wijzigen

Als u een melding dat het wijzigen van de inkomende IP-adres van uw app in Azure App Service ontvangen, volg de instructies in dit artikel.

## <a name="determine-if-you-have-to-do-anything"></a>Bepalen of u hebt verder niets te doen

* Optie 1: Als uw App Service-app heeft geen een aangepast domein, is geen actie vereist.

* Optie 2: Als er slechts een CNAME-record (DNS-record die verwijst naar een URI) is geconfigureerd in uw domein-Registratieportal (externe DNS-Provider of de Azure DNS), is geen actie vereist.

* Optie 3: Als een A-record (DNS-record die verwijst rechtstreeks naar uw IP-adres) is geconfigureerd in uw domein-Registratieportal (externe DNS-Provider of de Azure DNS), moet u de bestaande IP-adres vervangen door de nieuwe. U kunt het nieuwe IP-adres vinden door de instructies in de volgende sectie.

* Optie 4: Als uw toepassing zich achter een load balancer, IP-Filter of een ander IP-mechanisme dat IP-adres van uw app is vereist, moet u de bestaande IP-adres vervangen door de nieuwe. U kunt het nieuwe IP-adres vinden door de instructies in de volgende sectie.

## <a name="find-the-new-inbound-ip-address-in-the-azure-portal"></a>De nieuwe inkomende IP-adres vinden in Azure portal

Het nieuwe inkomende IP-adres die aan uw app is in de portal in de **virtueel IP-adres** veld. Zowel dit nieuwe IP-adres en de oude heeft zijn verbonden met uw app nu en later de oude versie wordt verbroken.

1.  Open de [Azure Portal](https://portal.azure.com).

2.  Selecteer in het navigatiemenu aan **App Services**.

3.  Selecteer uw App Service-app in de lijst.

1.  Als de app een functie-app is, raadpleegt u [functie-app inkomende IP-adres](../azure-functions/ip-addresses.md#function-app-inbound-ip-address).

4.  Onder de **instellingen** kop, klikt u op **eigenschappen** in de navigatiebalk aan de linkerkant en zoek de sectie met het label **virtueel IP-adres**.

5. Het IP-adres te kopiëren en opnieuw configureren van uw domein-record of het IP-mechanisme.

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt uitgelegd hoe u voorbereidt een IP-adres wijzigen die door Azure is gestart. Zie voor meer informatie over IP-adressen in Azure App Service, [inkomende en uitgaande IP-adressen in Azure App Service](app-service-ip-addresses.md).
