---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/09/2018
ms.author: cephalin
ms.openlocfilehash: f42a97cdd74d360bc047ef561cbe626d526f9e4a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58124485"
---
Domain Name System (DNS) wordt gebruikt om dingen vinden op het internet. Bijvoorbeeld, wanneer u een adres in uw browser invoeren, of klikt u op een koppeling op een webpagina, gebruikt DNS voor de omzetting van het domein in een IP-adres. Het IP-adres is vergelijkbaar met een adres, maar het is niet geschikt voor zeer menselijke. Bijvoorbeeld, is het veel gemakkelijker om te onthouden van een DNS-naam, zoals **contoso.com** dan het is om te onthouden van een IP-adres, zoals 192.168.1.88 of 2001:0:4137:1f67:24a2:3888:9cce:fea3.

Het DNS-systeem is gebaseerd op *records*. Records koppelen een specifieke *naam*, zoals **contoso.com**, met een IP-adres of een andere DNS-naam. Wanneer een toepassing, zoals een webbrowser, u een naam in DNS zoekt, de record vindt en wat deze verwijst naar het adres gebruikt. Als de waarde voor die deze naar verwijst een IP-adres is, wordt die waarde in de browser gebruiken. Als deze naar een andere DNS-naam verwijst, klikt u vervolgens heeft de toepassing oplossing opnieuw te doen. Uiteindelijk wordt alle naamomzetting beëindigd in een IP-adres.

Wanneer u een Azure-Website maakt, wordt automatisch een DNS-naam toegewezen aan de site. Deze naam heeft de vorm van  **&lt;yoursitename&gt;. azurewebsites.net**. Wanneer u uw website als een Azure Traffic Manager-eindpunt toevoegt, uw website vervolgens is toegankelijk via de  **&lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domein.

> [!NOTE]
> Als uw website is geconfigureerd als een Traffic Manager-eindpunt, gebruikt u de **. trafficmanager.net** adres bij het maken van DNS-records.
> 
> U kunt CNAME-records met Traffic Manager alleen gebruiken
> 
> 

Er zijn ook meerdere typen records, elk met hun eigen functies en beperkingen, maar voor websites als eindpunten Traffic Manager geconfigureerd, we alleen vinden over een; *CNAME* records.

### <a name="cname-or-alias-record"></a>CNAME- of Alias-record
Een CNAME-record verwijst een *specifieke* DNS-naam, zoals **mail.contoso.com** of **www\.contoso.com**, naar een andere (canonieke) domeinnaam. In het geval van Azure Websites met Traffic Manager, de canonieke domeinnaam is de  **&lt;Mijntoep >. trafficmanager.net** domeinnaam van uw Traffic Manager-profiel. Nadat u hebt gemaakt, de CNAME maakt u een alias voor de  **&lt;Mijntoep >. trafficmanager.net** domeinnaam. De CNAME-vermelding wordt omgezet in het IP-adres van uw  **&lt;Mijntoep >. trafficmanager.net** domeinnaam automatisch, zodat als het IP-adres van de website wordt gewijzigd, hoeft u geen actie te ondernemen.

Zodra er verkeer binnenkomt in Traffic Manager, wordt vervolgens het verkeer gerouteerd naar uw website, met behulp van de taakverdelingsmethode die is geconfigureerd voor. Dit is volledig transparant voor bezoekers van uw website. Ze zien alleen de aangepaste domeinnaam in hun browser.

> [!NOTE]
> Sommige registratieservices domein kunnen u subdomeinen toewijzen bij het gebruik van een CNAME-record, zoals alleen **www\.contoso.com**, en niet de hoofd-namen, zoals **contoso.com**. Zie voor meer informatie over CNAME-records, de documentatie van uw registrar <a href="https://en.wikipedia.org/wiki/CNAME_record">de Wikipedia-vermelding op CNAME-record</a>, of de <a href="https://tools.ietf.org/html/rfc1035">IETF-domeinnamen - implementatie en -specificatie</a> document.

