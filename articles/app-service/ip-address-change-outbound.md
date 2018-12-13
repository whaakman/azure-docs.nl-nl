---
title: Voorbereiden voor uitgaande IP-adres wijzigen - Azure App Service
description: Als uw uitgaande IP-adres wordt gebruikt om te worden gewijzigd, leert u wat te doen zodat uw app blijft gewoon werken na de wijziging.
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
ms.openlocfilehash: f611733400b8bfd18a135fe07f84606f13e59704
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275707"
---
# <a name="how-to-prepare-for-an-outbound-ip-address-change"></a>Voorbereiden voor een uitgaande IP-adres wijzigen

Als u een melding dat de uitgaande IP-adressen van uw app in Azure App Service wijzigen ontvangen wilt, volg de instructies in dit artikel.

## <a name="determine-if-you-have-to-do-anything"></a>Bepalen of u hebt verder niets te doen

* Optie 1: Als uw App Service-app niet voor IP-filtering, een lijst met expliciete opgenomen of speciale verwerking van uitgaand verkeer, zoals routering of firewall gebruikt wordt, is geen actie vereist.

* Optie 2: Als uw app er speciale handelingen voor de uitgaande IP-adressen (Zie de onderstaande voorbeelden), de nieuwe uitgaande IP-adressen toevoegen, waar ook de bestaande worden weergegeven. De bestaande IP-adressen niet worden vervangen. U kunt de nieuwe uitgaande IP-adressen vinden door de instructies in de volgende sectie.

  Bijvoorbeeld, een uitgaande IP-adres kan expliciet worden opgenomen in een firewall buiten uw app of een externe betalingsservice mogelijk een toegestane lijst met de uitgaande IP-adres voor uw app. Als uw uitgaande adres is geconfigureerd in een lijst met een willekeurige plaats buiten uw app, die moeten worden gewijzigd.

## <a name="find-the-outbound-ip-addresses-in-the-azure-portal"></a>De uitgaande IP-adressen niet vinden in de Azure-portal

De nieuwe uitgaande IP-adressen worden weergegeven in de portal voordat deze van kracht. Wanneer Azure wordt gestart met behulp van de nieuwe licenties, wordt de oude versie niet meer gebruikt. Slechts één set tegelijk wordt gebruikt, zodat items in de lijsten oude en nieuwe IP-adressen om te voorkomen dat een storing als de switch gebeurt moeten hebben. 

1.  Open de [Azure Portal](https://portal.azure.com).

2.  Selecteer in het navigatiemenu aan **App Services**.

3.  Selecteer uw App Service-app in de lijst.

1.  Als de app een functie-app is, raadpleegt u [functie app uitgaande IP-adressen](../azure-functions/ip-addresses.md#find-outbound-ip-addresses).

4.  Onder de **instellingen** kop, klikt u op **eigenschappen** in de navigatiebalk aan de linkerkant en zoek de sectie met het label **uitgaande IP-adressen**.

5. Kopiëren van de IP-adressen en toe te voegen aan uw speciale verwerking van uitgaand verkeer, zoals een filter of lijst met toegestane. Verwijder de bestaande IP-adressen in de lijst niet.

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt uitgelegd hoe u voorbereidt een IP-adres wijzigen die door Azure is gestart. Zie voor meer informatie over IP-adressen in Azure App Service, [inkomende en uitgaande IP-adressen in Azure App Service](app-service-ip-addresses.md).
