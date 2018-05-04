---
title: 'Zelfstudie: HTTPS configureren op een aangepast Azure CDN-domein | Microsoft Docs'
description: In deze zelfstudie leert u hoe u HTTPS kunt inschakelen en uitschakelen op het Azure CDN-eindpunt van een aangepast domein.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/12/2018
ms.author: rli
ms.custom: mvc
ms.openlocfilehash: a8f2da5a68552c35a55a7bbb764afc7b36af6962
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-configure-https-on-an-azure-cdn-custom-domain"></a>Zelfstudie: HTTPS op een aangepast Azure CDN-domein configureren

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

In deze zelfstudie ziet u hoe u het HTTP-protocol kunt inschakelen voor een aangepast domein dat is gekoppeld aan een Azure CDN-eindpunt (Content Delivery Network). Door het HTTPS-protocol te gebruiken in uw aangepaste domein, (bijvoorbeeld https:\//www.contoso.com) zorgt u ervoor dat uw gevoelige gegevens veilig worden afgeleverd met SSL-versleuteling wanneer ze via internet worden verzonden. HTTPS biedt betrouwbaarheid en verificatie, en biedt uw webtoepassingen beveiliging tegen aanvallen. De werkstroom voor het inschakelen van HTTPS is vereenvoudigd tot activering via één klik en biedt volledig certificaatbeheer, allemaal zonder extra kosten.

Azure CDN biedt standaard ondersteuning voor HTTPS voor een hostnaam van een CDN-eindpunt. Als u een CDN-eindpunt maakt (bijvoorbeeld https:\//contoso.azureedge.net), wordt HTTPS automatisch ingeschakeld.  

Enkele belangrijke kenmerken van de HTTPS-functie zijn:

- Geen extra kosten: er worden geen kosten in rekening gebracht voor het verwerven of vernieuwen van certificaten, of voor HTTPS-verkeer. U betaalt alleen voor de uitgaande GB van het CDN.

- Eenvoudig inschakelen: inrichten met één klik is beschikbaar in [Azure Portal](https://portal.azure.com). U kunt ook REST API of andere hulpprogramma’s voor ontwikkelaars gebruiken om de functie in te schakelen.

- Volledig certificaatbeheer: alle aanschaf en beheer van certificaten wordt voor u afgehandeld. Certificaten worden automatisch ingericht en vernieuwd vóór de verloopdatum. Hierdoor loopt u niet het risico dat de service wordt onderbroken omdat een certificaat is verlopen.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> - Het HTTPS-protocol inschakelen in uw aangepast domein
> - Het domein valideren
> - Het HTTPS-protocol uitschakelen in uw aangepast domein

## <a name="prerequisites"></a>Vereisten

Voordat u de stappen in deze zelfstudie kunt voltooien, moet u eerst een CDN-profiel en ten minste één CDN-eindpunt maken. Zie voor meer informatie [Snelstart: Een Azure CDN-profiel en een eindpunt maken](cdn-create-new-endpoint.md).

Daarnaast moet u een aangepast Azure CDN-domein koppelen aan uw CDN-eindpunt. Zie [Zelfstudie: Een aangepast domein toevoegen aan uw Azure CDN-eindpunt](cdn-map-content-to-custom-domain.md) voor meer informatie

## <a name="enable-the-https-feature"></a>De HTTPS-functie inschakelen

Volg deze stappen om HTTPS in te schakelen in een aangepast domein:

1. Ga in [Azure Portal](https://portal.azure.com) naar uw CDN-profiel in **Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon**.

2. Selecteer in de lijst met CDN-eindpunten het eindpunt met het aangepaste domein.

    ![Lijst met eindpunten](./media/cdn-custom-ssl/cdn-select-custom-domain-endpoint.png)

    De pagina **Eindpunt** wordt weergegeven.

3. Selecteer in de lijst met aangepaste domeinen het aangepaste domein waarvoor u HTTPS wilt inschakelen.

    ![Lijst met aangepaste domeinen](./media/cdn-custom-ssl/cdn-custom-domain.png)

    De pagina **Aangepast domein** wordt weergegeven.

4. Selecteer **Aan** om HTTPS in te schakelen. Selecteer vervolgens **Toepassen**.

    ![HTTP-status voor aangepast domein](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="validate-the-domain"></a>Het domein valideren

Als u al een aangepast domein gebruikt dat is toegewezen aan uw aangepaste eindpunt met een CNAME-record, gaat u verder met  
[Er is een aangepast domein toegewezen aan uw CDN-eindpunt](#custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record). In andere gevallen, als de CNAME-record voor uw eindpunt niet meer bestaat of als de record het subdomein cdnverify bevat, gaat u verder met [Er is geen aangepast domein toegewezen aan uw CDN-eindpunt](#custom-domain-is-not-mapped-to-your-cdn-endpoint).

### <a name="custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record"></a>Er is een aangepast domein toegewezen aan uw CDN-eindpunt met een CNAME-record

Toen u een aangepast domein toevoegde aan uw eindpunt, hebt u een CNAME-record gemaakt in de DNS-tabel van de domeinregistrar om het domein toe te wijzen aan de hostnaam van het CDN-eindpunt. Als deze CNAME-records nog steeds bestaat en niet het subdomein cdnverify bevat, wordt het met de DigiCert-CA (certificeringsinstantie) gebruikt om het eigendom van uw aangepaste domein te valideren. 

Uw CNAME-record moet de volgende indeling hebben, waarbij *Naam* de naam van het aangepaste domein is, en *Waarde* de hostnaam van het CDN-eindpunt:

| Naam            | Type  | Waarde                 |
|-----------------|-------|-----------------------|
| www.contoso.com | CNAME | contoso.azureedge.net |

Zie [Create the CNAME DNS record](https://docs.microsoft.com/en-us/azure/cdn/cdn-map-content-to-custom-domain#create-the-cname-dns-records) (De CNAME DNS-record maken) voor meer informatie over CNAME-records.

Als de CNAME-record de juiste indeling heeft, wordt de naam van het aangepaste domein automatisch geverifieerd met DigiCert en toegevoegd aan het SAN-certificaat (alternatieve namen voor onderwerpen). U ontvangt via DigiCert geen verificatie-e-mail en u hoeft uw aanvraag niet goed te keuren. Het certificaat is één jaar geldig en wordt, vóórdat het verloopt, automatisch vernieuwd. Ga verder met [Wachten op doorgifte](#wait-for-propagation). 

De automatische validatie duurt gewoonlijk enkele minuten. Als het domein na een uur nog niet is gevalideerd, opent u een ondersteuningsticket.

>[!NOTE]
>Als u beschikt over een CAA-record (Certificate Authority Authorization) bij uw DNS-provider, moet deze DigiCert bevatten als een geldige CA. Met een CAA-record kunnen domeineigenaars bij hun DNS-provider opgeven welke CA’s zijn geautoriseerd om certificaten te verlenen voor hun domein. Als een CA een bestelling ontvangt voor een certificaat voor een domein met een CAA-record, en deze CA wordt niet vermeld als een geautoriseerde verlener, mag de CA het certificaat niet verlenen aan dit domein of subdomein. Zie [CAA-records beheren](https://support.dnsimple.com/articles/manage-caa-record/) voor meer informatie over het beheren van CAA-records. Zie [CAA-record Helper](https://sslmate.com/caa/) voor een hulpprogramma voor CAA-records.

### <a name="custom-domain-is-not-mapped-to-your-cdn-endpoint"></a>Er is geen aangepast domein toegewezen aan uw CDN-eindpunt

Als de CNAME-record voor uw eindpunt niet meer bestaat of als deze het subdomein cdnverify bevat, volgt u de rest van de instructies in deze stap.

Nadat u HTTPS hebt ingeschakeld voor uw aangepaste domein, wordt met de DigiCert-CA (certificeringsinstantie) het eigendom van het domein gevalideerd door contact op te nemen met de bijbehorende registrator, op basis van de [WHOIS](http://whois.domaintools.com/)-registratiegegevens van het domein. Contact verloopt via het e-mailadres (standaard) of het telefoonnummer dat staat vermeld in de WHOIS-registratie. U moet de domeinvalidatie voltooien voordat HTTPS actief is voor uw aangepaste domein. U hebt zes werkdagen de tijd om het domein goed te keuren. Aanvragen die niet binnen zes werkdagen zijn goedgekeurd, worden automatisch geannuleerd. 

![WHOIS-record](./media/cdn-custom-ssl/whois-record.png)

Via DigiCert wordt ook een verificatie-e-mail verzonden naar extra e-mailadressen. Als de WHOIS-registratiegegevens privé zijn, verifieert u dat u direct kunt goedkeuren vanaf een van de volgende adressen:

admin@&lt;uw-domeinnaam.com&gt;  
administrator@&lt;uw-domeinnaam.com&gt;  
webmaster@&lt;uw-domeinnaam.com&gt;  
hostmaster@&lt;uw-domeinnaam.com&gt;  
postmaster@&lt;uw-domeinnaam.com&gt;  

U ontvangt binnen enkele minuten een e-mailbericht, vergelijkbaar met het bericht in het volgende voorbeeld, waarin u wordt gevraagd om de aanvraag goed te keuren. Als u een spamfilter gebruikt, voegt u admin@digicert.com toe aan de lijst met toegestane e-mailadressen. Als u na 24 uur nog geen e-mailbericht hebt ontvangen, neemt u contact op met Microsoft Ondersteuning.
    
![E-mailbericht voor domeinvalidatie](./media/cdn-custom-ssl/domain-validation-email.png)

Als u op de goedkeuringskoppeling klikt, wordt u naar het volgende onlinegoedkeuringsformulier geleid: 
    
![Formulier voor domeinvalidatie](./media/cdn-custom-ssl/domain-validation-form.png)

Volg de instructies op het formulier. U hebt twee verificatieopties:

- U kunt alle toekomstige bestellingen goedkeuren die met hetzelfde account zijn geplaatst voor hetzelfde hoofddomein, bijvoorbeeld contoso.com. Deze methode wordt aangeraden als u van plan bent om extra aangepaste domeinen toe te voegen voor hetzelfde hoofddomein.

- U kunt alleen de specifieke hostnaam goedkeuren die wordt gebruikt in deze aanvraag. Voor volgende aanvragen is extra goedkeuring is vereist.

Na de goedkeuring wordt het aangepaste domein via DigiCert toegevoegd aan het SAN-certificaat. Het certificaat is één jaar geldig en wordt, vóórdat het verloopt, automatisch vernieuwd.

## <a name="wait-for-propagation"></a>Wachten op doorgifte

Nadat de domeinnaam is gevalideerd, duurt het maximaal 6 tot 8 uur voordat de HTTPS-functie van het aangepaste domein is geactiveerd. Wanneer het proces is voltooid, is de aangepaste HTTPS-status in Azure Portal ingesteld op **Ingeschakeld** en zijn de vier bewerkingsstappen in het dialoogvenster Aangepast domein gemarkeerd als Voltooid. Het aangepaste domein is nu klaar voor gebruik van HTTPS.

![Dialoogvenster HTTPS inschakelen](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Bewerkingsvoortgang

In de volgende tabel wordt de bewerkingsvoortgang weergegeven die plaatsvindt nadat u HTTPS hebt ingeschakeld. Als u HTTPS hebt ingeschakeld, worden vier bewerkingsstappen weergegeven in het dialoogvenster Aangepast domein. Telkens als een van deze stappen actief wordt, verschijnen, tijdens de uitvoering, extra substappen onder de stap. Niet al deze substappen worden uitgevoerd. Nadat een stap is voltooid, wordt naast deze stap een groen vinkje weergegeven. 

| Bewerkingsstap | Details van bewerkingssubstap | 
| --- | --- |
| 1 Aanvraag verzenden | Aanvraag verzenden |
| | De HTTPS-aanvraag wordt verzonden. |
| | De HTTPS-aanvraag is verzonden. |
| 2 Domeinvalidatie | Het domein wordt automatisch gevalideerd als de CNAME is toegewezen aan het CDN-eindpunt. In andere gevallen wordt een verificatieaanvraag verzonden naar het e-mailadres dat wordt vermeld in de registratierecord (WHOIS-registrator) van uw domein. Verifieer het domein zo snel mogelijk. |
| | Uw domeineigendom is gevalideerd. |
| | Validatieaanvraag voor eigendom van het domein is verlopen (de klant heeft waarschijnlijk niet binnen zes dagen gereageerd). HTTPS wordt niet ingeschakeld voor uw domein. * |
| | Validatieaanvraag voor eigendom van het domein is geweigerd door de klant. HTTPS wordt niet ingeschakeld voor uw domein. * |
| 3 Certificaat inrichten | De certificeringsinstantie verleent momenteel het certificaat dat nodig is om HTTPS in te schakelen in uw domein. |
| | Het certificaat is verleend en wordt momenteel geïmplementeerd in het CDN-netwerk. Dit duurt maximaal 6 uur. |
| | Het certificaat is geïmplementeerd in het CDN-netwerk. |
| 4 Voltooien | HTTPS is ingeschakeld in uw domein. |

\* Dit bericht wordt niet weergegeven tenzij er een fout is opgetreden. 

Als er een fout optreedt voordat de aanvraag is verzonden, wordt het volgende foutbericht weergegeven:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="clean-up-resources---disable-https"></a>Resources opschonen - HTTPS uitschakelen

In de voorgaande stappen hebt u het HTTPS-protocol in uw aangepaste domein ingeschakeld. Als u het aangepaste domein niet meer wilt gebruiken met HTTPS, kunt u HTTPS uitschakelen door de volgende stappen uit te voeren:

### <a name="disable-the-https-feature"></a>De HTTPS-functie uitschakelen 

1. Ga in [Azure Portal](https://portal.azure.com) naar uw CDN-profiel in **Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon**.

2. Klik in de lijst met eindpunten op het eindpunt met het aangepaste domein.

3. Klik op het aangepaste domein waarvoor u HTTPS wilt uitschakelen.

    ![Lijst met aangepaste domeinen](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. Klik op **Uit** om HTTPS uit te schakelen. Klik vervolgens op **Toepassen**.

    ![Dialoogvenster Aangepaste HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="wait-for-propagation"></a>Wachten op doorgifte

Nadat de HTTPS-functie voor aangepaste domeinen is uitgeschakeld, duurt het maximaal 6 tot 8 uur voordat dit is doorgevoerd. Wanneer het proces is voltooid, is de aangepaste HTTPS-status in Azure Portal ingesteld op **Uitgeschakeld** en zijn de drie bewerkingsstappen in het dialoogvenster Aangepast domein gemarkeerd als Voltooid. Het aangepaste domein kan niet meer gebruikmaken van HTTPS.

![Dialoogvenster HTTPS uitschakelen](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

#### <a name="operation-progress"></a>Bewerkingsvoortgang

In de volgende tabel wordt de bewerkingsvoortgang weergegeven die plaatsvindt nadat u HTTPS hebt uitgeschakeld. Als u HTTPS hebt uitgeschakeld, worden drie bewerkingsstappen weergegeven in het dialoogvenster Aangepast domein. Telkens als een van deze stappen actief wordt, verschijnen extra gegevens onder de stap. Nadat een stap is voltooid, wordt naast deze stap een groen vinkje weergegeven. 

| Bewerkingsvoortgang | Bewerkingsdetails | 
| --- | --- |
| 1 Aanvraag verzenden | De aanvraag verzenden |
| 2 Inrichting van het certificaat ongedaan maken | Certificaat verwijderen |
| 3 Voltooien | Certificaat is verwijderd |

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

1. *Wie is de certificaatprovider en welk type certificaat is gebruikt?*

    Microsoft gebruikt een SAN-certificaat (alternatieve naam voor onderwerp) dat is geleverd via DigiCert. Met een SAN-certificaat kunnen meerdere volledig gekwalificeerde domeinnamen worden beveiligd met één certificaat.

2. *Kan ik mijn toegewezen certificaat gebruiken?*
    
    Nee, momenteel niet, maar dit staat op de roadmap.

3. *Wat moet ik doen als ik geen verificatie-e-mail voor het domein heb ontvangen van DigiCert?*

    Als u beschikt over een CNAME-vermelding voor uw aangepaste domein die rechtstreeks verwijst naar de hostnaam van uw eindpunt (en u niet gebruikmaakt van de subdomeinnaam cdnverify), ontvangt u een dergelijke verificatie-e-mail niet. Validatie wordt dan automatisch uitgevoerd. In andere gevallen waarbij u geen CNAME-vermelding hebt en binnen 24 uur geen e-mail hebt ontvangen, neemt u contact op met Microsoft Ondersteuning.

4. *Is het gebruik van een SAN-certificaat minder veilig dan wanneer ik een toegewezen certificaat gebruik?*
    
    Voor een SAN-certificaat gelden dezelfde standaarden voor versleuteling en beveiliging als voor een toegewezen certificaat. Alle verleende SSL-certificaten maken gebruik van SHA-256 voor verbeterde serverbeveiliging.

5. *Kan ik een aangepaste domein-HTTPS gebruiken met Azure CDN van Akamai?*

    Deze functie is momenteel alleen beschikbaar met Azure CDN van Verizon. Microsoft werkt eraan om ergens in de komende maanden ondersteuning te gaan bieden voor deze functie met Azure CDN van Akamai.

6. *Heb ik een CAA-record nodig bij mijn DNS-provider?*

    Nee, een CAA-record is momenteel niet vereist. Als u er echter wel een hebt, moet deze DigiCert bevatten als een geldige CA.


## <a name="next-steps"></a>Volgende stappen

Wat u hebt geleerd:

> [!div class="checklist"]
> - Het HTTPS-protocol is ingeschakeld in uw aangepast domein
> - Het domein valideren
> - Het HTTPS-protocol is uitgeschakeld in uw aangepast domein

Ga naar de volgende zelfstudie voor meer informatie over het configureren van caching op het CDN-eindpunt.

> [!div class="nextstepaction"]
> [Cachinggedrag in Azure CDN beheren met cachingregels](cdn-caching-rules.md)

