---
title: Begrijpen en oplossen van problemen met Azure AD Application Proxy CORS
description: Biedt een goed begrip van CORS in Azure AD-toepassingsproxy en het identificeren en oplossen van problemen met CORS.
services: active-directory
author: jeevanbisht
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: afc0bb990f69521efb2557a6a086c0de5126f82c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440422"
---
# <a name="understand-and-solve-azure-active-directory-application-proxy-cors-issues"></a>Begrijpen en oplossen van problemen met Azure Active Directory Application Proxy CORS

[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) kan soms problemen opleveren voor de apps en API's u publiceren via de Azure Active Directory Application Proxy. Dit artikel wordt Azure AD Application Proxy CORS problemen en oplossingen.

Meestal voorkomt dat een webpagina AJAX-aanvragen naar een ander domein aanbrengen. Deze beperking heet de *beleid voor zelfde oorsprong*, en voorkomt u dat een kwaadwillende site gevoelige gegevens leest uit een andere site. Maar soms u mogelijk wilt dat andere sites die uw web-API aanroepen. CORS is een W3C-standaard waarmee een server die het beleid voor zelfde oorsprong versoepelen en sommige cross-origin-aanvragen toestaan terwijl andere worden afgewezen.

## <a name="understand-and-identify-cors-issues"></a>Identificeer problemen met CORS

Twee URL's hebben dezelfde oorsprong als ze hetzelfde schema's, hosts en -poorten hebben ([RFC 6454](https://tools.ietf.org/html/rfc6454)), zoals:

-   http:\//contoso.com/foo.html
-   http:\//contoso.com/bar.html

De volgende URL's hebben verschillende oorsprongen dan de vorige twee:

-   http:\//contoso.net - ander domein
-   http:\//contoso.com:9000/foo.html - andere poort
-   https:\//contoso.com/foo.html - ander schema
-   http:\//www.contoso.com/foo.html - verschillende subdomein

Beleid voor zelfde oorsprong wordt voorkomen dat apps toegang krijgen tot resources vanuit andere oorsprongen, tenzij ze het juiste access control-koppen. Als de CORS-headers niet aanwezig of is onjuist zijn, mislukt de cross-origin-aanvragen. 

U kunt CORS problemen vaststellen met behulp van hulpprogramma's voor foutopsporing browser:

1. Start de browser en blader naar de web-app.
1. Druk op **F12** om de console voor foutopsporing.
1. Probeer te reproduceren van de transactie en controleer de Consolebericht. Een schending CORS produceert een fout over de oorsprong van de console.

In de volgende schermafbeelding, selecteren de **uitproberen** knop veroorzaakt een foutbericht CORS die https:\//corswebclient-contoso.msappproxy.net is niet gevonden in de Access-Control-Allow-Origin-header.

![CORS-probleem](./media/application-proxy-understand-cors-issues/image3.png)

## <a name="cors-challenges-with-application-proxy"></a>CORS-uitdagingen met Application Proxy

Het volgende voorbeeld toont een typische Azure AD Application Proxy CORS-scenario. De interne server-hosts een **CORSWebService** web-API-controller, en een **CORSWebClient** die roept **CORSWebService**. Er is een AJAX-aanvraag van **CORSWebClient** naar **CORSWebService**.

![On-premises zelfde oorsprong aanvraag](./media/application-proxy-understand-cors-issues/image1.png)

De app CORSWebClient werkt wanneer u het host on-premises, maar kan niet laden of fouten af wanneer gepubliceerd via Azure AD-toepassingsproxy. Als u de apps CORSWebClient en CORSWebService afzonderlijk als andere apps via toepassingsproxy gepubliceerd, worden de twee apps worden gehost in verschillende domeinen bevinden. Een AJAX-aanvraag van CORSWebClient naar CORSWebService is een cross-origin-aanvraag en deze is mislukt.

![Application Proxy CORS-aanvragen](./media/application-proxy-understand-cors-issues/image2.png)

## <a name="solutions-for-application-proxy-cors-issues"></a>Oplossingen voor problemen met Application Proxy CORS

U kunt de voorgaande CORS-probleem in een van de verschillende manieren oplossen.

### <a name="option-1-set-up-a-custom-domain"></a>Optie 1: Een aangepast domein instellen

Een Azure AD-toepassingsproxy gebruiken [aangepast domein](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) om te publiceren vanaf dezelfde oorsprong, zonder dat u hoeft wijzigingen aanbrengen in de app oorsprongen, code of headers. 

### <a name="option-2-publish-the-parent-directory"></a>Optie 2: Publiceren van de bovenliggende map

De bovenliggende map van beide apps publiceren. Deze oplossing werkt met name goed als u slechts twee apps op de webserver hebt. In plaats van elke app afzonderlijk publiceert, kunt u de algemene bovenliggende map, wat in dezelfde oorsprong resulteert.

De volgende voorbeelden ziet de Azure AD Application Proxy-pagina voor de app CORSWebClient-portal.  Wanneer de **interne URL** is ingesteld op *contoso.com/CORSWebClient*, de app kan geen aanbrengen geslaagde aanvragen aan de *contoso.com/CORSWebService* directory, omdat cross-origin in feite. 

![Afzonderlijk app publiceren](./media/application-proxy-understand-cors-issues/image4.png)

Stel in plaats daarvan de **interne URL** voor het publiceren van de bovenliggende map, waaronder zowel de *CORSWebClient* en *CORSWebService* mappen:

![Bovenliggende map publiceren](./media/application-proxy-understand-cors-issues/image5.png)

De resulterende app-URL's kunt effectief de CORS-probleem oplossen door:

- https:\//corswebclient-contoso.msappproxy.net/CORSWebService
- https:\//corswebclient-contoso.msappproxy.net/CORSWebClient

### <a name="option-3-update-http-headers"></a>Optie 3: HTTP-headers bijwerken

Een aangepaste HTTP-antwoordheader toevoegen van de webservice zodat deze overeenkomen met de oorspronkelijke aanvraag. Voor websites in Internet Information Services (IIS) wordt uitgevoerd, moet u IIS-beheer gebruiken om te wijzigen van de header:

![Aangepast antwoord-header toevoegen in IIS-beheer](./media/application-proxy-understand-cors-issues/image6.png)

Deze wijziging vereist geen codewijzigingen. U kunt dit controleren in de traceringen Fiddler:

**Na het toevoegen van koptekst**\
HTTP/1.1 200 OK\
Cache-Control: no-cache\
Pragma: no-cache\
Content-Type: text/plain; charset=utf-8\
Verloopt:-1\
Verschillen: Accept-Encoding\
Server: Microsoft-IIS/8.5 Microsoft-HTTPAPI/2.0\
**Access-Control-Allow-Origin: https://corswebclient-contoso.msappproxy.net** \
X-AspNet-Version: 4.0.30319\
X mogelijk gemaakt door: ASP.NET\
Content-Length: 17

### <a name="option-4-modify-the-app"></a>Optie 4: De app wijzigen

U kunt uw app om CORS te ondersteunen door toe te voegen van de Access-Control-Allow-Origin-header met de juiste waarden wijzigen. De manier waarop de header toevoegen, is afhankelijk van de taal van de app. De code te wijzigen, is de minste aanbevolen optie, omdat het meeste werk vereist is.

### <a name="option-5-extend-the-lifetime-of-the-access-token"></a>Optie 5: De levensduur van het toegangstoken uitbreiden

Sommige problemen CORS kunnen niet worden omgezet, zoals wanneer uw app wordt omgeleid naar *login.microsoftonline.com* om te verifiëren, en het toegangstoken is verlopen. De CORS aanroepen en mislukt. Een tijdelijke oplossing voor dit scenario is het uitbreiden van de levensduur van het toegangstoken om te voorkomen dat deze tijdens een gebruikerssessie is verlopen. Zie voor meer informatie over hoe u dit doet, [configureerbare levensduur van tokens in Azure AD](../develop/active-directory-configurable-token-lifetimes.md).

## <a name="see-also"></a>Zie ook
- [Zelfstudie: Toevoegen van een on-premises toepassing voor externe toegang via Application Proxy in Azure Active Directory](application-proxy-add-on-premises-application.md) 
- [Een Azure AD Application Proxy-implementatie plannen](application-proxy-deployment-plan.md) 
- [Externe toegang tot on-premises toepassingen via Azure Active Directory-toepassingsproxy](application-proxy.md) 
