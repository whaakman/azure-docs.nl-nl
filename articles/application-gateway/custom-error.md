---
title: Azure Application Gateway aangepaste foutpagina's maken
description: Dit artikel ziet u hoe u aangepaste foutpagina's voor Application Gateway maakt.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 2/14/2019
ms.author: victorh
ms.openlocfilehash: abfe33ff679bef125d9bf5b78e1790a1a4c64863
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301601"
---
# <a name="create-application-gateway-custom-error-pages"></a>Aangepaste foutpagina's voor Application Gateway maken

Met Application Gateway kunt u aangepaste foutpagina's maken in plaats van standaardfoutpagina's weer te geven. U kunt uw eigen huisstijl en lay-out hanteren door een aangepaste foutpagina te gebruiken.

U kunt bijvoorbeeld uw eigen onderhoudspagina definiëren als uw webtoepassing niet bereikbaar is. Of u kunt een niet-geautoriseerde toegangspagina maken als een kwaadwillende aanvraag wordt verzonden naar een web-App.

Aangepaste foutpagina's worden ondersteund voor de volgende twee scenario's:

- **Onderhoudspagina** -deze aangepaste foutpagina wordt verzonden in plaats van een 502 Ongeldige gateway-pagina. Deze wordt weergegeven wanneer de Application Gateway heeft geen verkeer routeren naar back-end. Bijvoorbeeld wanneer er is gepland onderhoud of wanneer een onverwacht probleem gevolgen voor toegang tot back-end-pool.
- **Pagina met niet-geautoriseerde toegang** -deze aangepaste foutpagina wordt verzonden in plaats van een pagina 403 onbevoegde toegang. Deze wordt weergegeven wanneer de Application Gateway WAF schadelijk verkeer detecteert en blokkeert.

Als er een fout is afkomstig van de back-endservers, wordt vervolgens het doorgestuurd ongewijzigd terug naar de oproepende functie. Een aangepaste foutpagina wordt niet weergegeven. Application-gateway kan een aangepaste foutpagina weergegeven wanneer een aanvraag tijdelijk niet bereikbaar voor de back-end.

Aangepaste foutpagina's kunnen worden gedefinieerd op het niveau van de globale en het niveau van de listener:

- **Algemeen niveau** -de foutpagina van toepassing is op het verkeer voor alle webtoepassingen die zijn geïmplementeerd op deze application gateway.
- **Het niveau van de listener** -de foutpagina wordt toegepast op verkeer ontvangen dat-listener voor.
- **Beide** -de aangepaste foutpagina gedefinieerd op het niveau van de listener overschrijft de is ingesteld op globaal niveau.

Voor het maken van een aangepaste foutpagina weergegeven, moet u het volgende hebben:

- een HTTP-antwoordstatuscode.
- de bijbehorende locatie voor de foutpagina. 
- een openbaar toegankelijke Azure storage-blob voor de locatie.
- een uitbreidingstype *.htm of *.html. 

De grootte van de foutpagina moet minder dan 1 MB. Als er installatiekopieën in de foutpagina gekoppeld, moeten ze openbaar toegankelijke absolute URL's of base64-gecodeerd afbeelding inline in de aangepaste foutpagina. Relatieve koppelingen met afbeeldingen op dezelfde locatie van de blob worden momenteel niet ondersteund. 

Nadat u een foutpagina opgeeft, wordt de application gateway downloaden vanaf de locatie van de blob storage en slaat deze op de lokale application gateway-cache. Vervolgens wordt de foutpagina geleverd rechtstreeks vanuit de toepassingsgateway. Als u wilt een bestaande aangepaste foutpagina wijzigen, moet u verwijzen naar een andere blob-locatie in de configuratie van de application gateway. De application gateway controleert periodiek de blob-locatie om op te halen van nieuwe versies.

## <a name="portal-configuration"></a>Portalconfiguratie

1. Navigeer naar de Application Gateway in de portal en kiest u een toepassingsgateway.

    ![AG-overzicht](media/custom-error/ag-overview.png)
2. Klik op **Listeners** en navigeer naar een bepaalde listener waar u om op te geven van een foutpagina.

    ![Application Gateway listeners](media/custom-error/ag-listener.png)
3. Een aangepaste foutpagina weergegeven voor een 403 WAF-fout of een 502 onderhoudspagina configureren op het niveau van de listener.

    > [!NOTE]
    > Het maken van globale niveau aangepaste foutpagina's vanuit de Azure-portal is momenteel niet ondersteund.

4. Geef een openbaar toegankelijke blob-URL voor een bepaalde status van de foutcode en klikt u op **opslaan**. De Application Gateway is nu geconfigureerd met de aangepaste foutpagina.

   ![Foutcodes voor Application Gateway](media/custom-error/ag-error-codes.png)

## <a name="azure-powershell-configuration"></a>Azure PowerShell-configuratie

U kunt Azure PowerShell gebruiken om te configureren van een aangepaste foutpagina. Bijvoorbeeld, een wereldwijde aangepaste foutpagina weergegeven:

`$updatedgateway = Add-AzApplicationGatewayCustomError -ApplicationGateway $appgw -StatusCode HttpStatus502 -CustomErrorPageUrl $customError502Url`

Of een listener niveau foutpagina weergegeven:

`$updatedlistener = Add-AzApplicationGatewayHttpListenerCustomError -HttpListener $listener01 -StatusCode HttpStatus502 -CustomErrorPageUrl $customError502Url`

Zie voor meer informatie, [toevoegen AzApplicationGatewayCustomError](https://docs.microsoft.com/powershell/module/az.network/add-azapplicationgatewaycustomerror?view=azps-1.2.0) en [toevoegen AzApplicationGatewayHttpListenerCustomError](https://docs.microsoft.com/powershell/module/az.network/add-azapplicationgatewayhttplistenercustomerror?view=azps-1.3.0).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Application Gateway diagnostics [Back-endstatus, diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md).