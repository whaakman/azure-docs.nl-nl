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
ms.openlocfilehash: 59a9011edef49494288716ab16f30e28e440293b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195179"
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-web-apps"></a>Problemen met domein- en SSL-certificaat oplossen in Azure-web-apps

Dit artikel worden veelvoorkomende problemen die u tegenkomen kunt wanneer u een domein of de SSL-certificaat voor uw Azure-web-apps configureren. Hierin worden ook mogelijke oorzaken en oplossingen voor deze problemen.

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **Get Support**.

## <a name="certificate-problems"></a>Certificaatproblemen

### <a name="you-cant-add-an-ssl-certificate-binding-to-a-web-app"></a>U kunt een SSL-certificaat-binding niet toevoegen aan een web-app 

#### <a name="symptom"></a>Symptoom

Wanneer u een SSL-binding toevoegt, wordt het volgende foutbericht weergegeven:

'Het toevoegen van SSL-binding is mislukt. Kan certificaat voor instellen bestaande VIP omdat een andere VIP wordt al gebruikt voor dat certificaat."

#### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als er meerdere IP-gebaseerde SSL-bindingen voor hetzelfde IP-adres voor meerdere Webapps. Web-app A heeft bijvoorbeeld een IP-gebaseerde SSL met een oud certificaat. Web-app B is een op IP gebaseerd SSL met een nieuw certificaat voor hetzelfde IP-adres. Wanneer u het SSL-binding van de web-app met het nieuwe certificaat bijwerken, mislukt dit bij deze fout omdat hetzelfde IP-adres wordt gebruikt voor een andere app. 

#### <a name="solution"></a>Oplossing 

U kunt dit probleem oplossen door een van de volgende methoden te gebruiken:

- Verwijder de IP-gebaseerde SSL-binding op de web-app die gebruikmaakt van het oude certificaat. 
- Maak een nieuwe op basis van IP-SSL-binding die gebruikmaakt van het nieuwe certificaat.

### <a name="you-cant-delete-a-certificate"></a>U kunt een certificaat niet verwijderen 

#### <a name="symptom"></a>Symptoom

Wanneer u probeert om een certificaat te verwijderen, wordt het volgende foutbericht weergegeven:

'Kan niet worden verwijderd van het certificaat omdat deze momenteel wordt gebruikt in een SSL-binding. De SSL-binding moet worden verwijderd voordat u het certificaat kunt verwijderen."

#### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als het certificaat wordt gebruikt door een andere web-app.

#### <a name="solution"></a>Oplossing

De SSL-binding voor dat certificaat verwijderen uit de web-apps. Probeer te verwijderen van het certificaat. Als u het certificaat niet verwijderen, schakelt u de internet-browser-cache en opnieuw openen van de Azure-portal in een nieuw browservenster. Probeer te verwijderen van het certificaat.

### <a name="you-cant-purchase-an-app-service-certificate"></a>U kunt geen een App Service-certificaat kopen 

#### <a name="symptom"></a>Symptoom
U kunt geen kopen een [Azure App Service-certificaat](./web-sites-purchase-ssl-web-site.md) vanuit de Azure-portal.

#### <a name="cause-and-solution"></a>Oorzaak en oplossing
Dit probleem kan optreden voor een van de volgende redenen:

- De App Service-abonnement is Free of Shared. Deze prijscategorie ondersteunen niet SSL. 

    **Oplossing**: de App Service-plan voor web-app bijwerken naar Standard.

- Het abonnement beschikt niet over een geldige creditcard.

    **Oplossing**: een geldige creditcard aan uw abonnement toevoegt. 

- De aanbieding voor abonnement biedt geen ondersteuning voor een App Service-certificaat, zoals Microsoft Student aanschaffen.  

    **Oplossing**: uw abonnement. 

- Het abonnement bereikt de limiet van aankopen die zijn toegestaan voor een abonnement.

    **Oplossing**: App Service-certificaten hebben een limiet van 10 certificaat aankopen voor betalen per gebruik en EA abonnement. De limiet is voor andere typen abonnement 3. U kunt de limiet verhogen door contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Het App Service-certificaat is gemarkeerd als fraude. U hebt ontvangen dat het volgende foutbericht weergegeven: 'uw certificaat is gemarkeerd om mogelijke fraude. De aanvraag wordt momenteel onderzocht. Als het certificaat niet gebruikt binnen 24 uur wordt, neem contact op met ondersteuning van Azure."

    **Oplossing**: als het certificaat is gemarkeerd als fraude na 24 uur niet is opgelost, als volgt te werk:

    1. Meld u aan bij [Azure Portal](https://portal.azure.com).
    2. Ga naar **App Service Certificate**, en selecteer het certificaat.
    3. Selecteer **configuratie van het certificaat** > **stap 2: Controleer of** > **domeinverificatie**. Deze stap verzendt een e-mailbericht naar de Azure certificaatprovider het probleem op te lossen.

## <a name="domain-problems"></a>Domein-problemen

### <a name="you-purchased-an-ssl-certificate-for-the-wrong-domain"></a>U hebt aangeschaft van een SSL-certificaat voor het verkeerde domein

#### <a name="symptom"></a>Symptoom

U een App Service-certificaat van het verkeerde domein hebt aangeschaft. U kunt het certificaat voor het gebruik van het juiste domein niet bijwerken.

#### <a name="solution"></a>Oplossing

Certificaat verwijderen en vervolgens een nieuw certificaat kopen.

Als het huidige certificaat die gebruikmaakt van het verkeerde domein in de status 'Verleend' is, moet u ook worden gefactureerd voor dat certificaat. Er zijn geen App Service-certificaten worden terugbetaald, maar u kunt contact opnemen met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om te zien of er andere opties. 

### <a name="an-app-service-certificate-was-renewed-but-the-web-app-shows-the-old-certificate"></a>Een App Service-certificaat is vernieuwd, maar de web-app toont het oude certificaat 

#### <a name="symptom"></a>Symptoom

Het App Service-certificaat is vernieuwd, maar de web-app die gebruikmaakt van het App Service-certificaat is nog steeds met behulp van het oude certificaat. U hebt ontvangen ook een waarschuwing dat het HTTPS-protocol vereist is.

#### <a name="cause"></a>Oorzaak 
De functie Web Apps van Azure App Service een achtergrondtaak elke acht uur wordt uitgevoerd en de bron van het certificaat wordt gesynchroniseerd als er wijzigingen zijn. Wanneer u draaien of een certificaat bijwerken, soms de toepassing is nog steeds bij het ophalen van het oude certificaat en niet het bijgewerkte certificaat. De reden is dat de taak voor het synchroniseren van de bron van het certificaat nog niet is uitgevoerd. 
 
#### <a name="solution"></a>Oplossing

U kunt een synchronisatie van het certificaat afdwingen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **App Service Certificate**, en selecteer vervolgens het certificaat.
2. Selecteer **sleutel opnieuw genereren en synchroniseren**, en selecteer vervolgens **Sync**. De synchronisatie neemt enige tijd voltooien. 
3. Wanneer de synchronisatie is voltooid, ziet u de volgende melding: "Bijgewerkt alle resources met de meest recente certificaat."

### <a name="domain-verification-is-not-working"></a>Verificatie van domein werkt niet 

#### <a name="symptom"></a>Symptoom 
Het App Service-certificaat vereist verificatie van domein voordat het certificaat is gereed om te gebruiken. Wanneer u selecteert **controleren**, mislukt het proces.

#### <a name="solution"></a>Oplossing
Handmatig controleren of uw domein door een TXT-record toe te voegen:
 
1.  Ga naar de Service DNS (Domain Name)-provider die als host fungeert voor de domeinnaam van uw.
2.  Toevoegen van een TXT-record voor uw domein dat gebruikmaakt van de waarde van het domein-token dat wordt weergegeven in de Azure-portal. 

Wacht enkele minuten duren voordat DNS-servers moet worden uitgevoerd en selecteer vervolgens de **vernieuwen** knop voor het activeren van de verificatie. 

Als alternatief kunt u de HTML-webpagina methode handmatig controleren of uw domein. Deze methode kunt om te bevestigen dat het eigendom van het domein van het domein dat het certificaat is uitgegeven voor de certificeringsinstantie.

1.  Maak een HTML-bestand met de naam {verificatietoken domein} .html. De inhoud van dit bestand moet de waarde van het domein verificatietoken.
3.  Dit bestand in de hoofdmap van de webserver die als host voor uw domein fungeert niet uploaden.
4.  Selecteer **vernieuwen** om de certificaatstatus te controleren. Het kan enkele minuten duren voordat de verificatie te voltooien.

Bijvoorbeeld, als u een standard-certificaat voor azure.com met het domein verificatie token 1234abcd koopt, een webaanvraag uitgaan naar http://azure.com/1234abcd.html 1234abcd moet retourneren. 

> [!IMPORTANT]
> De volgorde van een certificaat heeft slechts 15 dagen bewerking voor de verificatie is voltooid. Na 15 dagen dat de certificeringsinstantie het certificaat geweigerd en u niet weet in rekening gebracht voor het certificaat. In dit geval kan dit certificaat te verwijderen en probeer het opnieuw.
>
> 

### <a name="you-cant-purchase-a-domain"></a>U kunt geen een domein kopen

#### <a name="symptom"></a>Symptoom
U kunt een domein via het Web-Apps of -App Service-domein in de Azure portal kan niet kopen.

#### <a name="cause-and-solution"></a>Oorzaak en oplossing

Dit probleem doet zich voor een van de volgende redenen:

- Er is geen creditcard op het Azure-abonnement of de creditcard is ongeldig.

    **Oplossing**: een geldige creditcard aan uw abonnement toevoegt.

- Bent u niet de eigenaar van het abonnement, zodat u bent niet gemachtigd om aan te schaffen van een domein.

    **Oplossing**: [toevoegen van de rol van eigenaar](../billing/billing-add-change-azure-subscription-administrator.md) aan uw account. Of neem contact op met de abonnementsbeheerder als u gemachtigd wilt aanschaffen van een domein.
- U hebt de limiet voor het aanschaffen van domeinen in uw abonnement bereikt. De huidige limiet is 20.

    **Oplossing**: om aan te vragen een toename van de limiet, neem contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Het type van uw Azure-abonnement biedt geen ondersteuning voor de aankoop van een App Service-domein.

    **Oplossing**: uw Azure-abonnement upgraden naar een ander abonnementstype, zoals een abonnement met betalen naar gebruik.

### <a name="you-cant-add-a-host-name-to-a-web-app"></a>U kunt een host-naam niet toevoegen aan een web-app 

#### <a name="symptom"></a>Symptoom

Wanneer u de naam van een host toevoegt, mislukt het proces om te valideren en controleer het domein.

#### <a name="cause"></a>Oorzaak 

Dit probleem doet zich voor een van de volgende redenen:

- U bent niet gemachtigd om toe te voegen een hostnaam.

    **Oplossing**: vraag de abonnementsbeheerder te machtigen om toe te voegen een hostnaam.
- De eigenaar van uw domein kan niet worden geverifieerd.

    **Oplossing**: controleren of uw CNAME of een record correct is geconfigureerd. Als u wilt een aangepast domein toewijzen aan web-app, een CNAME-record of een A-record te maken. Als u een hoofddomein gebruiken wilt, moet u de A- en TXT-records gebruiken:

    |Recordtype|Host|Wijs|
    |------|------|-----|
    |A|@|IP-adres voor een web-app|
    |TXT|@|< app-naam >. azurewebsites.net|
    |CNAME|www|< app-naam >. azurewebsites.net|

### <a name="dns-cant-be-resolved"></a>DNS kan niet worden omgezet

#### <a name="symptom"></a>Symptoom

U hebt ontvangen dat het volgende foutbericht weergegeven:

"De DNS-record kan niet worden gevonden."

#### <a name="cause"></a>Oorzaak
Dit probleem doet zich voor een van de volgende redenen:

- De tijd voor live (TTL)-periode niet is verlopen. Controleer de DNS-configuratie voor uw domein om te bepalen van de TTL-waarde en wacht tot de periode verloopt.
- De DNS-configuratie is onjuist.

#### <a name="solution"></a>Oplossing
- Wacht totdat dit probleem op te lossen zelf 48 uur.
- Als u de TTL-instelling in de DNS-configuratie wijzigen kunt, wijzigt u de waarde in vijf minuten om te zien of hiermee het probleem is opgelost.
- Gebruik [WhatsmyDNS.net](https://www.whatsmydns.net/) om te controleren of uw domein naar de web-app IP-adres verwijst. Als dit niet het geval is, configureert u de A-record in de juiste IP-adres van de web-app.

### <a name="you-need-to-restore-a-deleted-domain"></a>U moet een verwijderde domein herstellen 

#### <a name="symptom"></a>Symptoom
Uw domein is niet meer zichtbaar in de Azure-portal.

#### <a name="cause"></a>Oorzaak 
De eigenaar van het abonnement heeft mogelijk per ongeluk het domein verwijderd.

#### <a name="solution"></a>Oplossing
Als uw domein is minder dan zeven dagen geleden verwijderd, is het domein nog niet begonnen de verwijdering. In dit geval kunt u hetzelfde domein opnieuw op de Azure-portal onder hetzelfde abonnement kopen. (Zorg ervoor dat de exacte domeinnaam in het zoekvak typt.) U geen afgeschreven opnieuw voor dit domein. Als het domein is meer dan zeven dagen geleden verwijderd, neem dan contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor hulp bij het herstellen van het domein.

### <a name="a-custom-domain-returns-a-404-error"></a>Een aangepast domein retourneert een 404-fout 

#### <a name="symptom"></a>Symptoom

Wanneer u de site met behulp van de aangepaste domeinnaam bladert, wordt het volgende foutbericht weergegeven:

"Fout 404-Web-app niet gevonden."


#### <a name="cause-and-solution"></a>Oorzaak en oplossing

**Oorzaak 1** 

Er ontbreekt een CNAME- of A-record in het aangepaste domein die u hebt geconfigureerd. 

**Oplossing voor oorzaak 1**

- Als u een A-record hebt toegevoegd, moet u een TXT-record wordt ook toegevoegd. Zie voor meer informatie [de A-record maken](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Als u geen gebruik van het hoofddomein voor uw web-app, raden wij u een CNAME-record gebruiken in plaats van een A-record.
- Een CNAME-record en een A-record niet gebruiken voor hetzelfde domein. Dit kan een conflict veroorzaken en voorkomt dat het domein worden omgezet. 

**Oorzaak 2** 

De internetbrowser mogelijk nog steeds de oude IP-adres voor uw domein cachen. 

**Oplossing voor oorzaak 2**

Schakel de browser. U kunt de opdracht uitvoeren voor Windows-apparaten `ipconfig /flushdns`. Gebruik [WhatsmyDNS.net](https://www.whatsmydns.net/) om te controleren of uw domein naar de web-app IP-adres verwijst. 

### <a name="you-cant-add-a-subdomain"></a>U kunt een subdomein niet toevoegen 

#### <a name="symptom"></a>Symptoom

U kunt een nieuwe hostnaam niet toevoegen aan een web-app een subdomein toewijzen.

#### <a name="solution"></a>Oplossing

- Neem contact op met de abonnementsbeheerder om ervoor te zorgen dat u toegangsmachtigingen hebt voor een hostnaam toevoegen aan de web-app.
- Als u meer subdomeinen nodig hebt, raden wij aan dat u wijzigt de domeinen naar Azure DNS te hosten. U kunt met behulp van Azure DNS, 500 hostnamen toevoegen aan uw web-app. Zie voor meer informatie [toevoegen van een subdomein](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).











 


















