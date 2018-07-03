---
title: Voorbereiden voor een SSL IP-adres wijzigen - Azure
description: Als uw SSL-IP-adres wordt gebruikt om te worden gewijzigd, leert u wat te doen zodat uw app blijft gewoon werken na de wijziging.
services: app-service\web
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: tdykstra
ms.openlocfilehash: 47626ae4298327d4166705e8797e525331e8a3d1
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342918"
---
# <a name="how-to-prepare-for-an-ssl-ip-address-change"></a>Voorbereiden voor een SSL IP-adres wijzigen

Als u een melding dat het wijzigen van het SSL-IP-adres van uw app in Azure App Service is ontvangen, volg de instructies in dit artikel voor het vrijgeven van de bestaande SSL-IP-adres en toewijzen van een nieuwe.

## <a name="release-ssl-ip-addresses-and-assign-new-ones"></a>SSL-IP-adressen vrijgeven en nieuwe labels toewijzen

1.  Open de [Azure Portal](https://portal.azure.com).

2.  Selecteer in het navigatiemenu aan **App Services**.

3.  Selecteer uw App Service-app in de lijst.

4.  Onder de **instellingen** kop, klikt u op **SSL-instellingen** in het linkernavigatievenster.

5. Selecteer de host-record in de sectie SSL-bindingen. Kies in de editor die wordt geopend, **SNI SSL** op de **SSL-Type** vervolgkeuzelijst en klik op **toevoegen Binding**. Wanneer u de bewerkingsbericht ziet, is de bestaande IP-adres vrijgegeven.

6.  In de **SSL-bindingen** sectie, selecteer de dezelfde hostrecord met het certificaat opnieuw. Kies in de editor die wordt geopend, ditmaal **IP gebaseerd SSL** op de **SSL-Type** vervolgkeuzelijst en klik op **Binding toevoegen**. Wanneer u de bewerkingsbericht ziet, kunt u een nieuw IP-adres hebt aangeschaft.

7.  Als een A-record (DNS-record die verwijst rechtstreeks naar uw IP-adres) is geconfigureerd in uw domein-Registratieportal (externe DNS-Provider of de Azure DNS), vervangt u de bestaande IP-adres met de zojuist gegenereerde naam. U kunt het nieuwe IP-adres vinden door de instructies in de volgende sectie.

## <a name="find-the-new-ssl-ip-address-in-the-azure-portal"></a>Het nieuwe SSL-IP-adres vinden in de Azure-Portal

1.  Wacht een paar minuten en open vervolgens de [Azure-portal](https://portal.azure.com).

2.  Selecteer in het navigatiemenu aan **App Services**.

3.  Selecteer uw App Service-app in de lijst.

4.  Onder de **instellingen** kop, klikt u op **eigenschappen** in de navigatiebalk aan de linkerkant en zoek de sectie met het label **virtueel IP-adres**.

5. Het IP-adres te kopiëren en opnieuw configureren van uw domein-record of het IP-mechanisme.

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt uitgelegd hoe u voorbereidt een IP-adres wijzigen die door Azure is gestart. Zie voor meer informatie over IP-adressen in Azure App Service, [SSL en SSL-IP-adressen in Azure App Service](app-service-ip-addresses.md).
