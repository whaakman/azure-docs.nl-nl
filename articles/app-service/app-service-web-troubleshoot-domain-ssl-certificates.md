---
title: Problemen met domein- en SSL-certificaat oplossen in Azure-web-apps | Microsoft Docs
description: Problemen met domein- en SSL-certificaat oplossen in Azure-web-apps
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: genli
ms.openlocfilehash: ba21475b771f1688c0a3f2f34c8d961fba5cd182
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-web-apps"></a>Problemen met domein- en SSL-certificaat oplossen in Azure-web-apps

Dit artikel worden veelvoorkomende problemen die u tegenkomen kunt wanneer u een domein of de SSL-certificaat voor uw Azure-web-apps configureren. Hierin worden ook mogelijke oorzaken en oplossingen voor deze problemen.

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [de Azure MSDN en de Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en klik op **Get Support**.

## <a name="certificate-problems"></a>Certificaatproblemen

### <a name="unable-to-add-bind-ssl-certificate-to-a-web-app"></a>Kan geen binding SSL-certificaat toevoegen aan een Web-App 

### <a name="symptom"></a>Symptoom

Wanneer u een SSL-binding toevoegt, wordt het volgende foutbericht weergegeven:

**Toevoegen van SSL-binding is mislukt. Certificaat kan niet worden ingesteld voor bestaande VIP, omdat een andere VIP al deze certificaten worden gebruikt.**

### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als er meerdere IP-gebaseerde SSL-bindingen voor hetzelfde IP-adres voor meerdere Webapps. Web-app A heeft bijvoorbeeld IP-gebaseerde SSL met oude certificaat. Web-app B met SSL op basis van IP-beveiliging met nieuw certificaat voor hetzelfde IP-adres. Wanneer u SSL-binding voor web-app met het nieuwe certificaat bijwerkt, mislukken deze bij deze fout aangezien hetzelfde IP-adres wordt gebruikt voor een andere app. 

### <a name="solution"></a>Oplossing 

U kunt dit probleem oplossen door een van de volgende methoden te gebruiken:

- Verwijder de IP-gebaseerde SSL-binding op web-app die gebruikmaakt van het oude certificaat. 
- Maak een nieuwe op basis van IP-SSL-binding die gebruikmaakt van het nieuwe certificaat.

### <a name="unable-to-delete-a-certificate"></a>Kan niet worden verwijderd van een certificaat 

### <a name="symptom"></a>Symptoom

Wanneer u probeert om een certificaat te verwijderen, wordt het volgende foutbericht weergegeven:

**Kan het certificaat verwijderen omdat deze momenteel wordt gebruikt in een SSL-binding. De SSL-binding moet worden verwijderd voordat u het certificaat kunt verwijderen.**

### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als het certificaat wordt gebruikt door een andere web-app.

### <a name="solution"></a>Oplossing

SSL-binding voor dat certificaat verwijderen uit de web-apps. Probeer te verwijderen van het certificaat. Als u het certificaat niet verwijderen, schakelt u de Internet-browser-cache, opent u de Azure-portal in een nieuw browservenster opnieuw. En probeer het certificaat te verwijderen.

### <a name="unable-to-purchase-an-app-service-certificate"></a>Kan een App Service-certificaat kopen 

### <a name="symptom"></a>Symptoom
U kunt geen kopen een [App Service-certificaat](./web-sites-purchase-ssl-web-site.md) vanuit Azure-portal.

### <a name="cause-and-solution"></a>Oorzaak en oplossing
Dit probleem kan optreden voor een van de volgende redenen:

- De App Service-abonnement is 'Gratis' of 'Gedeeld'. Wij ondersteunen geen SSL voor deze Prijscategorieën. 

    **Oplossing**: upgraden van de App Service-plan voor web-app op 'Standaard'.

- Het abonnement beschikt niet over een geldige creditcard.

    **Oplossing**: een geldige creditcard aan uw abonnement toevoegt. 

- De aanbieding voor abonnement biedt geen ondersteuning voor het aanschaffen van een App Service-certificaat, zoals Microsoft Student.  

    **Oplossing**: uw abonnement. 

- Het abonnement heeft het maximum aantal aankopen die zijn toegestaan voor een abonnement bereikt.

    **Oplossing**: App Service-certificaten hebben een limiet van 10 certificaat aankopen voor Pay als Go en EA abonnementen typen. De limiet is voor andere typen abonnement 3. U kunt de limiet verhogen door contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Het App Service-certificaat is gemarkeerd als fraude. U ontvangt het volgende foutbericht weergegeven: 'uw certificaat is gemarkeerd om mogelijke fraude. De aanvraag wordt momenteel onderzocht. Als het certificaat niet wordt gebruikt binnen 24 uur'.

    **Oplossing**: als het certificaat is gemarkeerd als fraude niet na 24 uur is opgelost, klikt u vervolgens als volgt te werk:

    1. Meld u aan bij [Azure Portal](https://portal.azure.com).
    2. Ga naar **App Service Certificate**, selecteert u het certificaat.
    3. Selecteer **configuratie van het certificaat** > **stap 2: Controleer of** > **domeinverificatie**. Hierdoor wordt een e-mailbericht verzonden naar de Azure certificaatprovider het probleem op te lossen.

## <a name="domain-problems"></a>Domein-problemen

### <a name="purchased-ssl-certificate-for-wrong-domain"></a>SSL-certificaat voor de verkeerde domein aangeschaft

### <a name="symptom"></a>Symptoom

U hebt aangeschaft van een App Service-certificaat van het verkeerde domein en u kunt het certificaat voor het gebruik van het juiste domein niet bijwerken.

### <a name="solution"></a>Oplossing

- Certificaat verwijderen en vervolgens een nieuw certificaat kopen.
- Als het huidige certificaat die gebruikmaakt van het verkeerde domein in de status 'Verleend' is, worden klikt u ook gefactureerd voor dat certificaat. Er zijn geen App Service-certificaten worden terugbetaald, maar u kunt contact opnemen met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om te zien of er andere opties. 

### <a name="app-service-certificate-was-renewed-but-still-shows-the-old-certificate"></a>App Service-certificaat is vernieuwd, maar nog steeds weergegeven in het oude certificaat 

### <a name="symptom"></a>Symptoom

Het App Service-certificaat is vernieuwd, maar de web-app die gebruikmaakt van het App Service-certificaat is nog steeds met behulp van het oude certificaat. U hebt ontvangen ook een waarschuwing dat het HTTPS-protocol vereist is.

### <a name="cause"></a>Oorzaak 
De Web-app service een achtergrondtaak elke acht uur wordt uitgevoerd en de bron van het certificaat wordt gesynchroniseerd als er wijzigingen zijn. Daarom wanneer u draaien of een certificaat bijwerken, soms de toepassing is nog steeds bij het ophalen van het oude certificaat en niet het bijgewerkte certificaat. Dit is omdat de taak voor het synchroniseren van de bron van het certificaat is nog niet uitgevoerd. 
 
### <a name="solution"></a>Oplossing

U kunt een synchronisatie van het certificaat afdwingen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer de **App service Certificate**, en selecteer vervolgens het certificaat.
2. Klik op **sleutel opnieuw genereren en synchroniseren**, en klik vervolgens op **Sync**. Dit neemt enige tijd om te voltooien. 
3. Wanneer de synchronisatie is voltooid, ziet u de volgende melding: 'Is bijgewerkt op alle resources met de meest recente certificaat'.

### <a name="domain-verification-is-not-working"></a>Verificatie van domein werkt niet 

### <a name="symptom"></a>Symptoom 
Het App Service-certificaat vereist verificatie van domein voordat het certificaat is gereed om te gebruiken. Wanneer u klikt op **controleren**, mislukt het proces.

### <a name="solution"></a>Oplossing
Handmatig controleren of uw domein door een TXT-record toe te voegen:
 
1.  Ga naar de Service DNS (Domain Name)-provider die als host fungeert voor de domeinnaam van uw.
2.  Toevoegen van een TXT-record voor uw domein dat gebruikmaakt van de waarde van het domein-token dat wordt weergegeven in de Azure-portal. 

Wacht enkele minuten duren voordat DNS-servers wilt uitvoeren en klik vervolgens op **vernieuwen** knop voor het activeren van de verificatie. 

Alternatieve is handmatig controleren of de 'HTML-pagina methode' die kan worden gebruikt om toe te staan om te bevestigen dat het eigendom van het domein van het domein dat het certificaat is uitgegeven voor de certificeringsinstantie.

1.  Maak een HTML-bestand met de naam {domein verificatie Token} .html. 
2.  Inhoud van dit bestand moet de waarde van een domein verificatie Token.
3.  Dit bestand in de hoofdmap van de webserver die als host voor uw domein fungeert niet uploaden
4.  Klik op **vernieuwen** om de status van het certificaat te controleren. Het kan enkele minuten duren voordat de verificatie te voltooien.

Bijvoorbeeld, als u een standard-certificaat voor azure.com met domein verificatie Token '1234abcd' koopt vervolgens een webaanvraag gesteld aan http://azure.com/1234abcd.html 1234abcd moet retourneren. 

> [!IMPORTANT]
> De volgorde van een certificaat heeft slechts 15 dagen bewerking voor de verificatie is voltooid. Na 15 dagen het certificaat is geweigerd door de certificeringsinstantie en u niet weet in rekening gebracht voor het certificaat. In dit geval kan dit certificaat te verwijderen en probeer het opnieuw.
>
> 

### <a name="unable-to-purchase-a-domain"></a>Kan het aanschaffen van een domein

### <a name="symptom"></a>Symptoom
U kan niet kopen van domein van de Web-app of App-domein in de Azure portal.

### <a name="cause-and-solution"></a>Oorzaak en oplossing

Dit probleem doet zich voor een van de volgende redenen:

- Geen creditcard op de Azure-abonnement of creditcard is ongeldig.

    **Oplossing**: een geldige creditcard toevoegen aan uw abonnement als u niet hebt.

- Als u niet de eigenaar van het abonnement, kunt u niet gemachtigd domein kopen.

    **Oplossing**: [toevoegen van de rol van eigenaar](../billing/billing-add-change-azure-subscription-administrator.md) aan uw account of contact met de abonnementsbeheerder om machtigingen voor het aanschaffen van een domein.
- U hebt de limiet voor het aanschaffen van domeinen in uw abonnement bereikt. De huidige limiet is 20.

    **Oplossing**: op aanvraag Verhoog de limiet, neem contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Het type van uw Azure-abonnement biedt geen ondersteuning voor de aankoop van App Service-domein.

    **Oplossing**: uw Azure-abonnement upgraden naar andere typen abonnement zoals een abonnement met betalen naar gebruik.

### <a name="unable-to-add-a-hostname-to-web-app"></a>Kan een hostnaam toevoegen aan Web-app 

### <a name="symptom"></a>Symptoom

Wanneer u een hostnaam toevoegt, mislukt het proces om te valideren en controleer het domein.

### <a name="cause"></a>Oorzaak 

Dit probleem doet zich voor een van de volgende redenen:

- U bent niet gemachtigd om toe te voegen een hostnaam.

    **Oplossing**: abonnementsbeheerder om ervoor te zorgen dat u een machtiging voor het hebt toevoegen van een hostnaam te controleren.
- De eigenaar van uw domein kan niet worden geverifieerd.

    **Oplossing**: controleren of uw CNAME of een record correct zijn geconfigureerd. Voor het aangepaste domein toewijzen aan web-app, een CNAME- of A-Record te maken. Als u hoofddomein gebruiken wilt, moet u de A- en TXT-records gebruiken:

    |Recordtype|Host|Wijs|
    |------|------|-----|
    |A|@|IP-adres voor de web-app|
    |TXT|@|< app-naam >. azurewebsites.net|
    |CNAME|www|< app-naam >. azurewebsites.net|

### <a name="dns-cannot-be-resolved"></a>DNS kan niet worden omgezet.

### <a name="symptom"></a>Symptoom

U ontvangt een foutbericht 'de DNS-record kan niet worden gevonden'.

### <a name="cause"></a>Oorzaak
Dit probleem doet zich voor een van de volgende redenen:

- De tijd voor Live (TTL)-periode niet is verlopen. Controleer de DNS-configuratie voor uw domein om te bepalen van de TTL-waarde en wacht tot de periode verloopt.
- De DNS-configuratie is onjuist.

### <a name="solution"></a>Oplossing
- Wacht totdat dit probleem op te lossen zelf 48 uur.
- Als u de TTL-instelling in de DNS-configuratie wijzigen kunt, wijzigt u de waarde in vijf minuten om te zien of hiermee het probleem is opgelost.
- Gebruik [WhatsmyDNS.net](https://www.whatsmydns.net/) om te controleren of uw domein naar het IP-adres van de web-app verwijst. Als dit niet het geval is, configureert u de A-record in de juiste IP-adres van de web-app.

### <a name="restore-a-deleted-domain"></a>Herstellen van een verwijderde domein 

### <a name="symptom"></a>Symptoom
Uw domein is niet meer zichtbaar in de Azure-portal.

### <a name="cause"></a>Oorzaak 
Het domein mogelijk per ongeluk zijn verwijderd door de eigenaar van het abonnement.

### <a name="solution"></a>Oplossing
Als uw domein is minder dan zeven dagen geleden verwijderd, is het domein nog niet begonnen de verwijdering. In dit geval kunt u hetzelfde domein opnieuw op de Azure-portal onder hetzelfde abonnement (Zorg ervoor dat de exacte domeinnaam in het zoekvak typt) kopen. U wordt niet in rekening gebracht opnieuw voor dit domein. Als het domein is meer dan zeven dagen geleden verwijderd, contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor hulp bij het herstellen van het domein.

### <a name="custom-domain-returns-404-or-site-inaccessible"></a>Aangepast domein retourneert 404 of de site niet toegankelijk 

### <a name="symptom"></a>Symptoom

Wanneer u de site met behulp van de aangepaste domeinnaam bladert, wordt het volgende foutbericht weergegeven:

**Fout 404-Web-app is niet gevonden.**


### <a name="cause-and-solution"></a>Oorzaak en oplossing

**Oorzaak 1** 

Er ontbreekt een CNAME- of A-record in het aangepaste domein die u hebt geconfigureerd. 

**Oplossing voor oorzaak 1**

- Als u een A-record hebt toegevoegd, moet u dat een TXT-record wordt ook toegevoegd. Zie voor meer informatie [maken-de-a-record](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Als u geen hoofddomein gebruiken voor uw web-app, wordt het aanbevolen een CNAME-record gebruiken in plaats van een record.
- Gebruik geen een CNAME- en de A-record voor hetzelfde domein. Dit kan conflict veroorzaken en voorkomen dat het oplossen van het domein. 

**Oorzaak 2** 

De internetbrowser mogelijk nog steeds de oude IP-adres voor uw domein cachen. 

**Oplossing voor oorzaak 2**

Schakel de browser. U kunt de opdracht uitvoeren voor Windows-apparaten `ipconfig /flushdns`. Gebruik [WhatsmyDNS.net](https://www.whatsmydns.net/) om te controleren of uw domein naar het IP-adres van de web-app verwijst. 

### <a name="unable-to-add-subdomain"></a>Kan geen subdomein toevoegen 

### <a name="symptom"></a>Symptoom

U kunt een nieuwe hostnaam niet toevoegen aan een web-app een subdomein toewijzen.

### <a name="solution"></a>Oplossing

- Neem contact op met de abonnementsbeheerder om te controleren of dat u machtigingen hebt voor een hostnaam toevoegen aan de web-app.
- Als u meer subdomeinen nodig hebt, raden wij aan dat u wijzigt de domeinen naar Azure DNS te hosten. U kunt met behulp van Azure DNS, 500 hostnamen toevoegen aan uw web-app. Zie voor meer informatie [subdomein toevoegen](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).











 


















