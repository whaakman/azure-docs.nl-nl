---
title: Bewaken van uw webtoepassing met webtests met meerdere stappen en Azure Application Insights | Microsoft Docs
description: WebTest met meerdere stappen-tests instellen voor het bewaken van uw webtoepassingen met Azure Application Insights
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: d8bfe92af4e8afc4edae76efb2e1cb7b287c7aa9
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304884"
---
# <a name="multi-step-web-tests"></a>Webtests met meerdere stappen

U kunt een geregistreerde reeks URL's en -interacties met een website via webtests met meerdere stappen bewaken. In dit artikel begeleidt u door het proces van het maken van een WebTest met meerdere stappen met Visual Studio Enterprise.

> [!NOTE]
> Webtests met meerdere stappen hebben extra kosten die zijn gekoppeld. Voor meer informatie over meer consult de [officiële prijzen handleiding](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="pre-requisites"></a>Vereisten

* Visual Studio 2017 Enterprise of hoger.
* Visual Studio web performance en load testprogramma's.

Op zoek naar de vereiste tests hulpprogramma's. Start de **Visual Studio Installer** > **afzonderlijke onderdelen** > **foutopsporing en testen**  >   **Web-prestaties en testhulpprogramma's laden**.

![Schermafbeelding van het installatieprogramma voor Visual Studio gebruikersinterface met afzonderlijke onderdelen geselecteerd met een selectievakje naast het item voor webprestaties en belastingstests voor de hulpprogramma 's](./media/availability-multistep/web-performance-load-testing.png)

## <a name="record-a-multi-step-web-test"></a>Neem een WebTest met meerdere stappen

Als u een test met meerdere stappen wilt maken, neemt u het scenario op met Visual Studio Enterprise en uploadt u vervolgens de opname naar Application Insights. Application Insights speelt het scenario opnieuw met ingestelde intervallen en controleert de reactie.

> [!IMPORTANT]
> * U kunt in uw tests geen gecodeerde functies of lussen gebruiken. De test moet volledig zijn opgenomen in het .webtest-script. U kunt echter wel standaard-invoegtoepassingen gebruiken.
> * Alleen Engelse tekens worden ondersteund in de webtest met meerdere stappen. Als u Visual Studio in andere talen gebruikt, werkt u het definitiebestand van de webtest bij om niet-Engelse tekens te vertalen of uit te sluiten.

Gebruik Visual Studio Enterprise om een websessie op te nemen.

1. Maak een Web performance en Load-testproject. **Bestand** > **nieuwe** > **Project** > **Visual C#**   >  **testen**

    ![Visual Studio-nieuw project UI](./media/availability-multistep/vs-web-performance-and-load-test.png)

2. Open de `.webtest` bestands- en begin met opnemen.

    ![Visual Studio test UI op te nemen](./media/availability-multistep/open-web-test.png)

3. Klik in de stappen die u wilt dat uw test om te simuleren als onderdeel van de opname.

    ![Browseropname UI](./media/availability-multistep/record.png)

4. Bewerk de test als volgt:

    * Voeg validaties toe om de ontvangen tekst en reactiecodes te controleren.
    * Verwijder alle onnodig interacties. U kan ook afhankelijke aanvragen voor afbeeldingen verwijderen of toevoegen van tracking-sites die niet relevant voor u overweegt uw test geslaagd.
    
    Houd er rekening mee dat u alleen het testscript bewerken kunt: u kunt aangepaste code toevoegen of andere webtests aanroepen. Voeg geen lussen toe aan de test. U kunt standaardinvoegtoepassingen voor webtest gebruiken.

5. De test uitvoeren in Visual Studio om te valideren en te controleren of dat deze werkt.

    De webtestrunner opent een webbrowser en herhaalt de acties die u hebt opgenomen. Zorg ervoor dat alles werkt zoals verwacht.

## <a name="upload-the-web-test"></a>De WebTest uploaden

1. Selecteer in de Application Insights-portal in het deelvenster beschikbaarheid **maken testen** > **testtype** > **WebTest met meerdere stappen**.

2. Stel de testlocaties, frequentie en Waarschuwingsparameters.

### <a name="frequency--location"></a>Frequentie en locatie

|Instelling| Uitleg
|----|----|----|
|**Testfrequentie**| Hiermee stelt u op hoe vaak de test wordt uitgevoerd vanaf elke testlocatie. Met een standaardfrequentie van vijf minuten en vijf testlocaties wordt uw site gemiddeld per minuut getest.|
|**Testlocaties**| Zijn de plaatsen van waar onze servers webaanvragen naar uw URL verzenden. **Onze minimum aantal aanbevolen testlocaties is vijf** om te zorgen dat u problemen in uw website van netwerkproblemen onderscheiden kunt. U kunt maximaal 16 locaties selecteren.

### <a name="success-criteria"></a>Criteria voor succes

|Instelling| Uitleg
|----|----|----|
| **Time-out voor testen** |Verlaag deze waarde om te worden gewaarschuwd over trage reacties. De test wordt als mislukt beschouwd als er binnen deze periode geen reactie van uw site is ontvangen. Als u **Parse onafhankelijke aanvragen** hebt geselecteerd, moeten alle afbeeldingen, stijlbestanden, scripts en andere afhankelijke resources binnen deze periode worden ontvangen.|
| **HTTP-antwoord** | De geretourneerde statuscode die voor een geslaagde test staat. 200 is de code die aangeeft dat er een normale webpagina is geretourneerd.|
| **Inhoudsovereenkomst** | Een tekenreeks, zoals 'Welkom!' Er wordt getest of er in elke respons een exacte (hoofdlettergevoelige) overeenkomst wordt gevonden. Het moet een eenvoudige tekenreeks zijn, zonder jokertekens. Als uw pagina-inhoud wordt gewijzigd, moet u deze tekenreeks mogelijk ook bijwerken. **Alleen Engelse tekens worden ondersteund met inhoudsovereenkomsten** |

### <a name="alerts"></a>Waarschuwingen

|Instelling| Uitleg
|----|----|----|
|**Near-realtime (Preview)** | Het is raadzaam om met behulp van de bijna realtime waarschuwingen. Configureren van dit type waarschuwing wordt uitgevoerd nadat de beschikbaarheidstest is gemaakt.  |
|**Klassiek** | We niet meer wordt aanbevolen met klassieke waarschuwingen voor nieuwe beschikbaarheidstests.|
|**Voor waarschuwingslocatie**|U wordt aangeraden een minimum van 3/5 locaties. De optimale relatie tussen waarschuwingslocatie en het aantal testlocaties **waarschuwingslocatie** = **aantal testlocaties - 2, met een minimum van vijf testlocaties.**|

## <a name="advanced-configuration"></a>Geavanceerde configuratie

### <a name="plugging-time-and-random-numbers-into-your-test"></a>Tijd en willekeurige cijfers invoegen in uw test

Stel dat u een hulpprogramma test dat tijdsafhankelijke gegevens ontvangt van een externe feed (bijvoorbeeld een feed met aandelenkoersen). Wanneer u uw webtest opneemt, moet u specifieke tijden gebruiken, maar u stelt deze in als testparameters: StartTime en EndTime.

![Mijn geweldige aandelen app-schermafbeelding](./media/availability-multistep/app-insights-72webtest-parameters.png)

Wanneer u de test uitvoert, moet EndTime altijd de huidige tijd zijn. StartTime moet een kwartier in het verleden liggen.

De datum tijd de Webtestinvoegtoepassing biedt u de manier om af te handelen parameteriseren tijden.

1. Voeg een webtestinvoegtoepassing toe voor elke gewenste variabele parameterwaarde. Kies in de werkbalk van de webtest de optie **Webtestinvoegtoepassing toevoegen**.
    
    ![Webtestinvoegtoepassing toevoegen](./media/availability-multistep/app-insights-72webtest-plugin-name.png)
    
    In dit voorbeeld gebruiken we twee exemplaren van de invoegtoepassing Date Time. Een exemplaar is voor "15 minuten geleden" en een ander voor “nu”.

2. Open de eigenschappen van elke invoegtoepassing. Geef de invoegtoepassing een naam en stel deze zodanig in dat de huidige tijd wordt gebruikt. Stel voor een van de toepassingen Minuten toevoegen in op -15.

    ![Context Parameters](./media/availability-multistep/app-insights-72webtest-plugin-parameters.png)

3. Gebruik in de webtestparameters {{plug-in name}} om te verwijzen naar de naam van de invoegtoepassing.

    ![StartTime](./media/availability-multistep/app-insights-72webtest-plugins.png)

Upload uw test nu naar de portal. Telkens wanneer de test wordt uitgevoerd, worden er dynamische waarden gebruikt.

### <a name="dealing-with-sign-in"></a>Omgaan met aanmelden

Als uw gebruikers zich aanmelden bij uw app, hebt u verschillende functies om de aanmelding te simuleren, zodat u pagina’s na het aanmelden kunt testen. Welke aanpak u gebruikt, hangt af van het type beveiliging van de app.

In alle gevallne moet u een account maken in uw toepassing voor testdoeleinden. Beperk indien mogelijk de machtigingen voor dit testaccount, zodat webtests echte gebruikers niet beïnvloeden.

**Eenvoudige gebruikersnaam en wachtwoord** een WebTest op de gebruikelijke manier. Verwijder eerst de cookies.

**SAML-verificatie** gebruikt u de SAML-invoegtoepassing die beschikbaar is voor webtests. Toegang tot de-invoegtoepassing door...

**Clientgeheim** als uw app een aanmeldroute die een klantgeheim omvat, gebruik dan deze route. Azure Active Directory (AAD) is een voorbeeld van een service die aanmelden met een clientgeheim bevat. In AAD is het klantgeheim de App Key.

Hier is een voorbeeldwebtest van een Azure web-app met een App Key:

![Voorbeeld-schermafbeelding](./media/availability-multistep/client-secret.png)

Token van AAD krijgen met klantgeheim (AppKey).
Bearer token uit antwoord halen.
API oproepen met bearer token in de autorisatie-header.
Zorg ervoor dat de WebTest een eigenlijke client is - dat wil zeggen, deze een eigen app in AAD heeft - en zijn clientId + app key. Uw service onder de test heeft ook een eigen app in AAD: de appID URI van deze app wordt weergegeven in de WebTest in het resourceveld.

### <a name="open-authentication"></a>Open verificatie
Een voorbeeld van open verificatie is het aanmelden met uw Microsoft- of Google-account. Veel apps die OAuth gebruiken, bieden een alternatief met clientgeheim, zodat uw eerste tactiek moet zijn deze mogelijkheid te onderzoeken.

Als uw test moet aanmelden met OAuth, is de algemene benadering:

Gebruik een hulpprogramma zoals Fiddler om het verkeer tussen de webbrowser, de verificatiesite en uw app te onderzoeken.
Voer twee of meer aanmeldingen uit via verschillende apparaten en browsers, of met lange periodes ertussen (zodat de tokens verlopen).
Vergelijk de verschillende sessies om te bepalen welk token is doorgegeven door de verificatiesite en daarna, na het aanmelden, wordt doorgegeven aan uw appserver.
Neem een webtest op met Visual Studio.
Maak parameters van de tokens. Stel de parameter in wanneer er een token wordt geretourneerd van de verificator en gebruik deze in de query voor de site. (Visual Studio probeert de testparameters toe te voegen, maar voegt de parameters voor de tokens niet correct toe.)

## <a name="troubleshooting"></a>Problemen oplossen

Toegewezen [probleemoplossingsartikel](troubleshoot-availability.md).

## <a name="next-steps"></a>Volgende stappen

* [Beschikbaarheid van waarschuwingen](availability-alerts.md)
* [URL-ping-webtests](monitor-web-app-availability.md)
