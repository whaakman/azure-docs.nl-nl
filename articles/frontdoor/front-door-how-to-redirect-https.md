---
title: Maken van een voordeur met HTTP naar HTTPS-omleiding via Azure portal
description: Informatie over het maken van een voordeur met omgeleide verkeer van HTTP naar HTTPS met behulp van de Azure-portal.
services: front-door
author: sharad4u
ms.service: front-door
ms.topic: article
ms.date: 5/21/2019
ms.author: sharadag
ms.openlocfilehash: a07b19c49630cc925e719aaa1d46476a1edc58f5
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333158"
---
# <a name="create-a-front-door-with-http-to-https-redirection-using-the-azure-portal"></a>Maken van een voordeur met HTTP naar HTTPS-omleiding via Azure portal

U kunt de Azure-portal gebruiken om te maken een [voordeur](front-door-overview.md) met een certificaat voor SSL-beëindiging. Een regel voor doorsturen wordt gebruikt om HTTP-verkeer omleiden naar HTTPS.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een voordeur maken met een bestaande Web-App-resource
> * Een aangepast domein met SSL-certificaat toevoegen 
> * HTTPS-omleiding in het aangepaste domein instellen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-front-door-with-an-existing-web-app-resource"></a>Een voordeur maken met een bestaande Web-App-resource

1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
3. Zoeken naar **voordeur** staaf- en nadat u het resourcetype gevonden, klikt u op met de zoekfunctie **maken**.
4. Kies een abonnement en vervolgens gebruik een bestaande resourcegroep of maak een nieuwe. Opmerking: de locatie gevraagd in de gebruikersinterface voor de resourcegroep alleen is. De configuratie van de voordeur wordt geïmplementeerd voor alle [Azure-voordeur van POP-locaties](https://docs.microsoft.com/azure/frontdoor/front-door-faq#what-are-the-pop-locations-for-azure-front-door-service).

    ![Basisprincipes voor nieuwe voordeur configureren](./media/front-door-url-redirect/front-door-create-basics.png)

5. Klik op **volgende** in te voeren van het tabblad configuratie. De configuratie voor de voordeur gebeurt in drie stappen - toe te voegen een frontend standaardhost, back-ends toe te voegen in een back endpool en het maken van regels voor doorsturen om toe te wijzen het routeringsgedrag voor front-host. 

     ![Voordeur configuration designer](./media/front-door-url-redirect/front-door-designer.png)

6. Klik op de ' **+** ' pictogram op de _Frontend hosts_ invoeren voor het maken van een frontend-host, een wereldwijd unieke naam voor uw standaardhost voor front-end voor uw voordeur (`\<**name**\>.azurefd.net`). Klik op **toevoegen** om door te gaan met de volgende stap.

     ![Een front-end-host toevoegen](./media/front-door-url-redirect/front-door-create-fehost.png)

7. Klik op de ' **+** ' pictogram op de _back-endpools_ om een back endpool te maken. Geef een naam op voor de back-endgroep en klik vervolgens op '**toevoegen van een back-end**'.
8. Selecteer het Type back-end-Host als _appservice_. Selecteer het abonnement waar uw web-app wordt gehost en selecteer vervolgens de specifieke web-app in de vervolgkeuzelijst voor **back-end-hostnaam**.
9. Klik op **toevoegen** opslaan van de back-end en klikt u op **toevoegen** opnieuw op te slaan van de configuratie van de back-end-pool.   ![Een back-end in een back-endpool toevoegen](./media/front-door-url-redirect/front-door-create-backendpool.png)

10. Klik op de ' **+** ' pictogram op de _Routing regels_ een route te maken. Geef een naam op voor de route, zeg 'HttpToHttpsRedirect' en stel vervolgens de _geaccepteerd protocollen_ veld **'Alleen HTTP'** . Zorg ervoor dat de juiste _frontend host_ is geselecteerd.  
11. Op de _Route Details_ sectie, stelt u de _routetype_ naar **omleiden**, zorg ervoor dat de _omleiden type_ is ingesteld op  **Gevonden (302)** en _omleiden protocol_ is ingesteld op **alleen HTTPS**. 
12. Klik op toevoegen om op te slaan van de regel voor het doorsturen voor HTTP omleiden naar HTTPS.
     ![Een HTTP naar HTTPS-omleiding route toevoegen](./media/front-door-url-redirect/front-door-redirect-config-example.png)
13. Voeg een andere regel voor het doorsturen voor de verwerking van het HTTPS-verkeer. Klik op de ' **+** ' Meld u aan bij de _Routing regels_ en geef een naam op voor de route, zeg 'DefaultForwardingRoute' en stel vervolgens de _geaccepteerd protocollen_ veld naar **'Alleen HTTPS'** . Zorg ervoor dat de juiste _frontend host_ is geselecteerd.
14. Stel op de sectie Details van de Route de _routetype_ naar **doorsturen**, zorg ervoor dat de juiste back-endpool is geselecteerd en de _doorsturen Protocol_ is ingesteld op  **Alleen HTTPS**. 
15. Klik op toevoegen om op te slaan van de regel voor het doorsturen voor het doorsturen van de aanvraag.
     ![Voeg een forward route voor HTTPS-verkeer toe](./media/front-door-url-redirect/front-door-forward-route-example.png)
16. Klik op **revisie + maken** en vervolgens **maken**, om uw voordeur-profiel te maken. Ga naar de resource eenmaal hebt gemaakt.

## <a name="add-a-custom-domain-to-your-front-door-and-enable-https-on-it"></a>Een aangepast domein toevoegen aan de voordeur en HTTPS inschakelen
De volgende stappen laten zien hoe u een aangepast domein toevoegen aan een bestaande resource in de voordeur en schakel vervolgens HTTP naar HTTPS-omleiding erop. 

### <a name="add-a-custom-domain"></a>Een aangepast domein toevoegen

In dit voorbeeld voegt u toe een CNAME-record voor de `www` subdomein (bijvoorbeeld `www.contosonews.com`).

#### <a name="create-the-cname-record"></a>Het CNAME-record maken

Voeg een CNAME-record voor een subdomein toewijzen aan de voordeur frontend standaardhost (`<name>.azurefd.net`, waarbij `<name>` is de naam van uw profiel voordeur).

Voor de `www.contoso.com` domein toevoegen als u bijvoorbeeld een CNAME-record die de naam wordt toegewezen `www` naar `<name>.azurefd.net`.

Nadat u de CNAME toevoegt, lijkt de pagina DNS-records op het volgende voorbeeld:

![CNAME-aangepast domein aan de voordeur](./media/front-door-url-redirect/front-door-dns-cname.png)

#### <a name="onboard-the-custom-domain-on-your-front-door"></a>Onboarding het aangepaste domein in de voordeur

1. Op het tabblad van de voordeur-ontwerper, klikt u op '+' pictogram op de front-hosts deel een nieuw aangepast domein toevoegt. 
2. Voer de volledig gekwalificeerde aangepaste DNS-naam in het naamveld aangepaste host voorbeeld `www.contosonews.com`. 
3. Als de CNAME-toewijzing van het domein aan de deur wordt gevalideerd, klikt u op **toevoegen** om toe te voegen van het aangepaste domein.
4. Klik op **opslaan** de wijzigingen verzendt.

![Menu voor aangepaste domeinen](./media/front-door-url-redirect/front-door-add-custom-domain.png)

### <a name="enable-https-on-your-custom-domain"></a>HTTPS voor uw aangepaste domein inschakelen

1. Klik op het aangepaste domein dat is toegevoegd en onder de sectie **HTTPS voor aangepaste domeinen**, wijzigt de status van de **ingeschakeld**.
2. U kunt laten de **management certificaattype** ingesteld op _voordeur beheerd_ onderhouden voor het gratis certificaat, beheerde en autorotated door voordeur. U kunt ook uw eigen aangepaste SSL-certificaat dat is opgeslagen met Azure Key Vault gebruiken. In deze zelfstudie wordt ervan uitgegaan dat het gebruik van de voordeur certificaat beheerd.
![Inschakeling van HTTPS voor aangepast domein](./media/front-door-url-redirect/front-door-custom-domain-https.png)

3. Klik op **Update** opslaan van de selectie en klik vervolgens op **opslaan**.
4. Klik op **vernieuwen** na een paar minuten en klik vervolgens op het aangepaste domein opnieuw om te zien van de voortgang van de inrichting van certificaten. 

> [!WARNING]
> HTTPS inschakelen voor een aangepast domein enkele duurt minuten en is ook afhankelijk eigendom domeinvalidatie als de CNAME niet rechtstreeks aan de voordeur host toegewezen is `<name>.azurefd.net`. Meer informatie over [HTTPS inschakelen voor een aangepast domein](./front-door-custom-domain-https.md).

## <a name="configure-the-routing-rules-for-the-custom-domain"></a>De routeringsregels voor het aangepaste domein configureren

1. Klik op de omleidings-routeringsregel eerder hebt gemaakt.
2. Klik op de vervolgkeuzelijst voor front-hosts en selecteer uw aangepaste domein om toe te passen van deze route voor uw domein ook.
3. Klik op **Bijwerken**.
4. Dat wil zeggen, dezelfde bewerking voor de andere routeringsregel ook doen voor uw doorstuurroute om toe te voegen van het aangepaste domein.
5. Klik op **opslaan** uw wijzigingen wilt indienen.

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).
- Meer informatie over [URL-omleiding op voordeur](front-door-url-redirect.md).
