---
title: Problemen met domein en SSL-certificaat oplossen in Azure-web-apps | Microsoft Docs
description: Problemen met domein en SSL-certificaat oplossen in Azure-web-apps
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
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 5c5bdb8fad60a2e4196c2c9f74764e27cec5ba62
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52970770"
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-web-apps"></a>Problemen met domein en SSL-certificaat oplossen in Azure-web-apps

Dit artikel worden veelvoorkomende problemen die u tegenkomen kunt wanneer u een domein of een SSL-certificaat voor uw Azure-web-apps configureren. Ook wordt beschreven mogelijke oorzaken en oplossingen voor deze problemen.

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning voor Azure site](https://azure.microsoft.com/support/options/) en selecteer **ontvang ondersteuning**.

## <a name="certificate-problems"></a>Certificaatproblemen

### <a name="you-cant-add-an-ssl-certificate-binding-to-a-web-app"></a>U kunt een SSL-certificaat-binding niet toevoegen aan een web-app 

#### <a name="symptom"></a>Symptoom

Wanneer u een SSL-binding toevoegt, wordt de volgende strekking weergegeven:

"Het SSL-binding toevoegen is mislukt. Kan geen certificaat instellen voor de bestaande VIP omdat een andere VIP wordt al gebruikt voor dat certificaat."

#### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als er meerdere IP-gebaseerd SSL-bindingen voor hetzelfde IP-adres voor meerdere Webapps. Web-app A heeft bijvoorbeeld een op IP gebaseerde SSL met een oud certificaat. Web-app B is een op IP gebaseerde SSL met een nieuw certificaat voor hetzelfde IP-adres. Wanneer u het SSL-binding van de web-app met het nieuwe certificaat bijwerken, is het vanwege de volgende fout mislukt omdat hetzelfde IP-adres wordt gebruikt voor een andere app. 

#### <a name="solution"></a>Oplossing 

U lost dit probleem, moet u een van de volgende methoden gebruiken:

- De IP-gebaseerd SSL-binding op de web-app die gebruikmaakt van het oude certificaat verwijderd. 
- Maak een nieuwe IP-gebaseerd SSL-binding die gebruikmaakt van het nieuwe certificaat.

### <a name="you-cant-delete-a-certificate"></a>U kunt een certificaat niet verwijderen 

#### <a name="symptom"></a>Symptoom

Wanneer u probeert om een certificaat te verwijderen, ontvangt u de volgende strekking weergegeven:

'Kan niet het certificaat niet verwijderen omdat deze momenteel wordt gebruikt in een SSL-binding. De SSL-binding moet worden verwijderd voordat u het certificaat kunt verwijderen."

#### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als het certificaat wordt gebruikt door een andere web-app.

#### <a name="solution"></a>Oplossing

De SSL-binding voor dat certificaat verwijderen uit de web-apps. Probeer te verwijderen van het certificaat. Als u het certificaat niet verwijderen, schakelt u de cache van de browser internet en opnieuw openen van de Azure portal in een nieuw browservenster. Probeer te verwijderen van het certificaat.

### <a name="you-cant-purchase-an-app-service-certificate"></a>Kunt u aanschaffen een App Service certificate niet 

#### <a name="symptom"></a>Symptoom
U kunt geen koopt een [Azure App Service certificate](./web-sites-purchase-ssl-web-site.md) vanuit Azure portal.

#### <a name="cause-and-solution"></a>Oorzaak en oplossing
Dit probleem kan optreden voor het gebruik van de volgende redenen:

- De App Service-plan is gratis of gedeeld. Deze Prijscategorieën ondersteuning geen voor SSL. 

    **Oplossing**: de App Service-plan voor web-app een upgrade uitvoert naar Standard.

- Het abonnement beschikt niet over een geldige creditcard.

    **Oplossing**: een geldige creditcard aan uw abonnement toevoegt. 

- Het abonnement biedt geen ondersteuning voor een App Service-certificaat, zoals Microsoft Student aanschaffen.  

    **Oplossing**: Upgrade van uw abonnement. 

- Het abonnement bereikt de limiet van aankopen die zijn toegestaan op een abonnement.

    **Oplossing**: App Service-certificaten hebben een limiet van 10 certificaat aankopen voor het abonnement betalen per gebruik en EA. Voor andere abonnementstypen is de limiet van 3. Als u wilt de limiet te verhogen, neem contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Het App Service-certificaat is gemarkeerd als fraude. U hebt ontvangen van de volgende strekking weergegeven: 'uw certificaat is gemarkeerd voor mogelijke fraude. De aanvraag wordt momenteel onderzocht. Als het certificaat niet gebruikt binnen 24 uur wordt, neem contact op met ondersteuning voor Azure."

    **Oplossing**: als het certificaat is gemarkeerd als fraude en na 24 uur niet is opgelost, volgt u deze stappen:

    1. Meld u aan bij [Azure Portal](https://portal.azure.com).
    2. Ga naar **App Service-certificaten**, en selecteer het certificaat.
    3. Selecteer **configuratie van het certificaat** > **stap 2: Controleer of** > **domeinverificatie**. Deze stap verzendt een e-mailbericht naar de provider van het certificaat van Azure om het probleem te verhelpen.

## <a name="domain-problems"></a>Problemen met domein

### <a name="you-purchased-an-ssl-certificate-for-the-wrong-domain"></a>U hebt aangeschaft met een SSL-certificaat voor het verkeerde domein

#### <a name="symptom"></a>Symptoom

U hebt aangeschaft voor een App Service-certificaat voor het verkeerde domein. U kunt het certificaat voor het gebruik van het juiste domein niet bijwerken.

#### <a name="solution"></a>Oplossing

Certificaat verwijderen en vervolgens een nieuw certificaat kopen.

Als het huidige certificaat die gebruikmaakt van het verkeerde domein zich in de status "Verleend", moet u ook worden gefactureerd voor dat certificaat. App Service-certificaten zijn niet worden terugbetaald, maar u kunt contact opnemen met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om te zien of er nog andere opties. 

### <a name="an-app-service-certificate-was-renewed-but-the-web-app-shows-the-old-certificate"></a>Een App Service-certificaat is vernieuwd, maar de web-app ziet u het oude certificaat 

#### <a name="symptom"></a>Symptoom

Het App Service-certificaat is vernieuwd, maar de web-app die gebruikmaakt van de App Service-certificaat is nog steeds met behulp van het oude certificaat. U hebt ontvangen ook een waarschuwing dat het HTTPS-protocol vereist is.

#### <a name="cause"></a>Oorzaak 
De functie Web Apps van Azure App Service wordt een achtergrondtaak om de acht uur wordt uitgevoerd en de certificaatresource wordt gesynchroniseerd als er wijzigingen zijn. Wanneer u draaien of bijwerken van een certificaat, soms de toepassing is nog steeds bezig met ophalen van het oude certificaat en niet het bijgewerkte certificaat. De reden is dat de taak voor het synchroniseren van de certificaatresource nog niet is uitgevoerd. 
 
#### <a name="solution"></a>Oplossing

U kunt afdwingen dat een synchronisatie van het certificaat:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **App Service-certificaten**, en selecteer vervolgens het certificaat.
2. Selecteer **opnieuw versleutelen en synchroniseren**, en selecteer vervolgens **synchronisatie**. De synchronisatie duurt enige tijd om te voltooien. 
3. Wanneer de synchronisatie is voltooid, ziet u de volgende melding: "Zijn bijgewerkt alle resources met het nieuwste certificaat."

### <a name="domain-verification-is-not-working"></a>Domeinverificatie werkt niet 

#### <a name="symptom"></a>Symptoom 
Het App Service-certificaat vereist domeinverificatie uit te voeren voordat het certificaat klaar is voor gebruik. Wanneer u selecteert **controleren**, het proces is mislukt.

#### <a name="solution"></a>Oplossing
Handmatig controleren of uw domein door een TXT-record toe te voegen:
 
1.  Ga naar de Domain Name Service (DNS)-provider die als host fungeert voor de domeinnaam van uw.
2.  Toevoegen van een TXT-record voor uw domein dat gebruikmaakt van de waarde van het domein-token dat wordt weergegeven in de Azure-portal. 

Wacht een paar minuten voor DNS-doorgifte uitvoeren en selecteer vervolgens de **vernieuwen** knop voor het activeren van de verificatie. 

Als alternatief kunt u de HTML-webpagina methode handmatig controleren of uw domein. Deze methode kunt de certificeringsinstantie te bevestigen dat het domeineigendom van het domein dat het certificaat is verleend.

1.  Maak een HTML-bestand met de naam {domeinverificatietoken} .html. De inhoud van dit bestand moet de waarde van het domeinverificatietoken zijn.
3.  Upload dit bestand in de hoofdmap van de webserver die als host voor uw domein fungeert.
4.  Selecteer **vernieuwen** om de certificaatstatus te controleren. Het duurt enkele minuten voor verificatie te voltooien.

Bijvoorbeeld, als u een standaard-certificaat voor azure.com met het domein verificatie token waarde 1234abcd, een webaanvraag gedaan bij https://azure.com/1234abcd.html moet de waarde 1234abcd retourneren. 

> [!IMPORTANT]
> De volgorde van een certificaat heeft slechts 15 dagen om het domein verificatie-bewerking te voltooien. Na 15 dagen, de certificeringsinstantie het certificaat geweigerd en u niet worden in rekening gebracht voor het certificaat. In dit geval verwijderen van dit certificaat en probeer het opnieuw.
>
> 

### <a name="you-cant-purchase-a-domain"></a>Kunt u aanschaffen niet een domein

#### <a name="symptom"></a>Symptoom
U kunt geen aanschaffen van een domein van de Web-Apps of App Service-domein in Azure portal.

#### <a name="cause-and-solution"></a>Oorzaak en oplossing

Dit probleem doet zich voor een van de volgende redenen:

- Er is geen creditcard is geregistreerd op het Azure-abonnement of de creditcard is ongeldig.

    **Oplossing**: een geldige creditcard aan uw abonnement toevoegt.

- U bent niet de eigenaar van het abonnement, zodat u bent niet gemachtigd om aan te schaffen van een domein.

    **Oplossing**: [toewijzen van de rol van eigenaar](../role-based-access-control/role-assignments-portal.md) aan uw account. Of neem contact op met de beheerder van het abonnement om machtiging voor het kopen van een domein.
- U hebt de limiet voor het aanschaffen van domeinen in uw abonnement bereikt. De huidige limiet is 20.

    **Oplossing**: voor het aanvragen van een uitbreiding van de limiet, neem contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Het type Azure-abonnement biedt geen ondersteuning voor de aanschaf van een App Service-domein.

    **Oplossing**: uw Azure-abonnement upgraden naar een ander abonnementstype, zoals een abonnement met betalen per gebruik.

### <a name="you-cant-add-a-host-name-to-a-web-app"></a>U kunt een hostnaam niet toevoegen aan een web-app 

#### <a name="symptom"></a>Symptoom

Wanneer u een hostnaam toevoegt, mislukt het proces om te valideren en controleer of het domein.

#### <a name="cause"></a>Oorzaak 

Dit probleem doet zich voor een van de volgende redenen:

- U bent niet gemachtigd om toe te voegen een hostnaam.

    **Oplossing**: vraagt u om toestemming te geven om toe te voegen een naam voor de beheerder van het abonnement.
- Uw domeineigendom kan niet worden geverifieerd.

    **Oplossing**: controleren of uw CNAME- of een record correct is geconfigureerd. Als u wilt een aangepast domein toewijzen aan web-app, een CNAME-record of een A-record te maken. Als u een hoofddomein gebruiken wilt, moet u een en TXT-records:

    |Recordtype|Host|Wijs|
    |------|------|-----|
    |A|@|IP-adres voor een web-app|
    |TXT|@|< app-naam >. azurewebsites.net|
    |CNAME|www|< app-naam >. azurewebsites.net|

### <a name="dns-cant-be-resolved"></a>DNS kan niet worden omgezet

#### <a name="symptom"></a>Symptoom

U hebt ontvangen het volgende foutbericht:

"De DNS-record kan niet worden gevonden."

#### <a name="cause"></a>Oorzaak
Dit probleem doet zich voor een van de volgende redenen:

- Time to live (TTL) periode niet is verlopen. Controleer de DNS-configuratie voor uw domein om te bepalen van de TTL-waarde, en wacht totdat de periode is verlopen.
- De DNS-configuratie is onjuist.

#### <a name="solution"></a>Oplossing
- Wacht tot 48 uur voor dit probleem op te lossen zelf.
- Als u de TTL-instelling in de DNS-configuratie wijzigen kunt, wijzigt u de waarde in vijf minuten om te zien of hiermee het probleem is opgelost.
- Gebruik [WhatsmyDNS.net](https://www.whatsmydns.net/) om te controleren of uw domein naar het IP-adres van de web-app verwijst. Als dat niet het geval is, configureert u de A-record naar het juiste IP-adres van de web-app.

### <a name="you-need-to-restore-a-deleted-domain"></a>U moet een verwijderde domein herstellen 

#### <a name="symptom"></a>Symptoom
Uw domein is niet meer zichtbaar in de Azure portal.

#### <a name="cause"></a>Oorzaak 
De eigenaar van het abonnement heeft mogelijk per ongeluk het domein verwijderd.

#### <a name="solution"></a>Oplossing
Als uw domein is minder dan zeven dagen geleden hebt verwijderd, is de verwijdering niet nog gestart door het domein. In dit geval kunt u hetzelfde domein opnieuw op de Azure-portal onder hetzelfde abonnement kopen. (Zorg ervoor dat de exacte domeinnaam in het zoekvak typt.) U wordt niet in rekening gebracht opnieuw voor dit domein. Als het domein is meer dan zeven dagen geleden hebt verwijderd, neem dan contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor hulp bij het herstellen van het domein.

### <a name="a-custom-domain-returns-a-404-error"></a>Een aangepast domein retourneert een 404-fout 

#### <a name="symptom"></a>Symptoom

Wanneer u naar de site bladert met behulp van de aangepaste domeinnaam, ontvangt u de volgende strekking weergegeven:

"Fout 404-Web-app niet vinden."


#### <a name="cause-and-solution"></a>Oorzaak en oplossing

**Oorzaak 1** 

Het aangepaste domein die u hebt geconfigureerd, een CNAME- of A-record ontbreekt. 

**Oplossing voor oorzaak 1**

- Als u een A-record hebt toegevoegd, zorg ervoor dat er ook een TXT-record wordt toegevoegd. Zie voor meer informatie, [A-record maken](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Als u geen gebruik van het hoofddomein voor uw web-app, wordt u aangeraden dat u een CNAME-record in plaats van een A-record gebruiken.
- Een CNAME-record en een A-record niet gebruiken voor hetzelfde domein bevinden. Dit kan een conflict veroorzaken en te voorkomen dat het domein worden omgezet. 

**Oorzaak 2** 

De browser internet mogelijk nog steeds de oude IP-adres voor uw domein caching. 

**Oplossing voor oorzaak 2**

Schakel de browser. Voor Windows-apparaten, kunt u de opdracht uitvoeren `ipconfig /flushdns`. Gebruik [WhatsmyDNS.net](https://www.whatsmydns.net/) om te controleren of uw domein naar het IP-adres van de web-app verwijst. 

### <a name="you-cant-add-a-subdomain"></a>U kunt geen een subdomein toevoegen 

#### <a name="symptom"></a>Symptoom

U kunt een nieuwe hostnaam niet toevoegen aan een web-app een subdomein toewijzen.

#### <a name="solution"></a>Oplossing

- Neem contact op met de beheerder van abonnement om ervoor te zorgen dat u machtigingen hebben voor een hostnaam toevoegen aan de web-app.
- Als u meer subdomeinen nodig hebt, raden wij u wijzigt de hosting van domein naar Azure DNS. Met behulp van Azure DNS kunt u 500 hostnamen toevoegen aan uw web-app. Zie voor meer informatie, [een subdomein toevoegen](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).











 


















