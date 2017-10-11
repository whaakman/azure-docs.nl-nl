---
title: Het gebruik van toegangsbeheer (Java) | Microsoft Docs
description: Informatie over het ontwikkelen en toegangsbeheer gebruiken met Java in Azure.
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 247dfd59-0221-4193-97ec-4f3ebe01d3c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: 698403d181e1fee09bb4692290c92203ded97ba4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Hoe verifieert webgebruikers met Azure Access Control-Service met behulp van Eclipse
Deze handleiding wordt beschreven hoe u met de Azure Access Control Service (ACS) binnen de Azure-Toolkit voor Eclipse. Zie voor meer informatie over ACS de [Vervolgstappen](#next_steps) sectie.

> [!NOTE]
> Het Azure Access Services besturingselement Filter is een community technology preview. Als de bètasoftware, wordt deze niet formeel ondersteund door Microsoft.
> 
> 

## <a name="what-is-acs"></a>Wat is ACS?
De meeste ontwikkelaars zijn niet identiteit deskundigen en over het algemeen niet wilt besteden aan de tijd ontwikkelen verificatie en autorisatie mechanismen voor hun toepassingen en services. ACS is een Azure-service waarmee u een eenvoudige manier voor het authenticeren van gebruikers die toegang krijgen tot uw webtoepassingen en services moeten zonder factor authentication complexe logica in uw code.

De volgende functies zijn beschikbaar in ACS:

* Integratie met Windows Identity Foundation (WIF).
* Ondersteuning voor populaire web id-providers (IP's) dat met inbegrip van Windows Live ID, Google, Yahoo! en Facebook.
* Ondersteuning voor Active Directory Federatieservices (AD FS) 2.0.
* Een Open Data Protocol (OData)-management-service die programmatisch toegang tot ACS-instellingen biedt.
* Een beheerportal waarmee beheerderstoegang tot de ACS-instellingen.

Zie voor meer informatie over ACS [Access Control Service 2.0][Access Control Service 2.0].

## <a name="concepts"></a>Concepten
Azure ACS is gebaseerd op de principals van op claims gebaseerde identiteit - een consistente benadering voor het maken van verificatiemechanismen voor toepassingen die lokaal wordt uitgevoerd of in de cloud. Op claims gebaseerde identiteit biedt een veelgebruikte manier voor toepassingen en services te verkrijgen van de identiteit van de benodigde informatie over gebruikers binnen hun organisatie in andere organisaties en op het Internet.

Als u de taken in deze handleiding, moet u de volgende concepten begrijpen:

**Client** -In de context van deze handleiding instructies, is dit een browser die u probeert toegang te krijgen tot uw webtoepassing.

**Relying party (RP)-toepassing** -een RP-toepassing is een website of de service die verificatie van één externe autoriteit heeft. Identiteit jargon spreken we dat de RP die instantie wordt vertrouwd. Deze handleiding wordt uitgelegd hoe uw toepassing configureren voor de vertrouwensrelatie ACS.

**Token** -token is een verzameling van beveiligingsgegevens die meestal na een geslaagde verificatie van een gebruiker wordt verleend. Deze bevat een reeks *claims*, kenmerken van de geverifieerde gebruiker. Een claim kan vertegenwoordigen een gebruikersnaam, een id voor een functie van een gebruiker behoort, van een gebruiker leeftijd, enzovoort. Een token wordt meestal digitaal zijn ondertekend, wat betekent dat kunnen altijd afkomstig terug naar de certificaatverlener en de inhoud ervan kan worden geknoeid. Een gebruiker krijgt toegang tot een toepassing RP in de vorm van een geldig token dat is uitgegeven door een instantie die de RP-toepassing vertrouwt.

**ID-Provider (IP)** -een IP-adres is een instantie die verifieert gebruikers-id's en verstrekt beveiligingstokens. Het eigenlijke werk van de uitgifte van tokens wordt geïmplementeerd als een speciale service Security Token Service (STS) genoemd. Typische voorbeelden van IP-adressen zijn Windows Live ID, Facebook en zakelijke gebruiker opslagplaatsen (zoals Active Directory).
Als ACS is geconfigureerd voor het vertrouwen van een IP-adres, wordt het systeem accepteren en valideren van tokens die zijn uitgegeven door die IP. ACS kan meerdere IP-adressen in één keer, wat betekent dat als uw toepassing ACS vertrouwt, u direct uw toepassing aan alle geverifieerde gebruikers van de IP's aanbieden kunt dat wordt vertrouwd door ACS namens jou worden vertrouwd.

**Federation-Provider (EP)** -IP-adressen direct kennis hebben van gebruikers, en ze met hun referenties worden geverifieerd en claims over wat ze over deze weten uitgeven. Een Federation-Provider (EP) is een ander soort autoriteit: in plaats van rechtstreeks verifiëren van gebruikers die deze fungeert als een tussenkomst en beleggingsmakelaars verificatie tussen één RP en een of meer IP-adressen. Zowel de IP-adressen en de FPs beveiligingstokens, daarom ze gebruiken allebei Security Token Services (STS). ACS is één FrontPage.

**ACS regel Engine** -de logica die wordt gebruikt voor het transformeren van binnenkomende tokens van goedgekeurde IP-adressen voor tokens die zijn bedoeld om te worden gebruikt door de RP is overgegaan in de vorm van regels voor eenvoudige claims-transformatie. ACS is uitgerust met een regel-engine die al worden verzorgd door gelijk welke logica transformatie is opgegeven voor uw RP toe te passen.

**ACS-Namespace** -een-naamruimte is een partitie van het hoogste niveau van ACS die u gebruikt voor het ordenen van uw instellingen. Een naamruimte bevat een lijst met IP-adressen u vertrouwt, de RP-toepassingen die u wilt gebruiken, de regels die u verwacht dat de regel in de engine voor het verwerken van binnenkomende tokens met, enzovoort. Een naamruimte beschrijft de verschillende eindpunten die wordt gebruikt door de toepassing en de ontwikkelaar van de ACS om uit te voeren van de functie ophalen.

De volgende afbeelding ziet hoe de ACS-verificatie werkt met een webtoepassing:

![Stroomdiagram van ACS][acs_flow]

1. De client (in dit geval een browser) wordt een pagina opvraagt bij de Resourceprovider.
2. Omdat de aanvraag is nog niet geverifieerd, de RP wordt de gebruiker omgeleid naar de instantie die wordt vertrouwd, namelijk ACS. De ACS biedt de gebruiker de keuze van IP-adressen die zijn opgegeven voor deze RP. De gebruiker selecteert het juiste IP-adres.
3. De client naar het IP-adres verificatiepagina bladert en vraagt de gebruiker zich aanmeldt.
4. Nadat de client is geverifieerd (bijvoorbeeld de identiteit van de referenties zijn ingevoerd), wordt het IP-adres een beveiligingstoken.
5. Het IP-adres de client doorstuurt naar ACS en verzendt het beveiligingstoken dat is uitgegeven door het IP-adres in ACS op de client na uitgifte van een beveiligingstoken.
6. ACS valideert het beveiligingstoken dat is uitgegeven door het IP-adres invoer de identiteit van de claims in dit token in de ACS-regels-engine, berekent de identiteitsclaims uitvoer en geeft een nieuw beveiligingstoken dat deze uitvoer claims bevat.
7. De client doorstuurt ACS naar de Resourceprovider. De client verzendt de nieuw beveiligingstoken door ACS uitgegeven aan de Resourceprovider. Het RP valideert de handtekening van het beveiligingstoken dat is uitgegeven door ACS, valideert de claims in dit token en wordt de pagina die oorspronkelijk zijn aangevraagd.

## <a name="prerequisites"></a>Vereisten
Als u de taken in deze handleiding, moet u het volgende:

* Een Java Developer Kit (JDK), v 1.6 of hoger.
* Eclipse IDE voor Java EE-ontwikkelaars Indigo of hoger. Dit kan worden gedownload vanaf <http://www.eclipse.org/downloads/>. 
* Een distributiepunt van een Java gebaseerde webserver of toepassingsserver, zoals Apache Tomcat, GlassFish, JBoss-toepassingsserver of Jetty.
* een Azure-abonnement, die kan worden opgehaald uit <http://www.microsoft.com/windowsazure/offers/>.
* De Azure-werkset voor Eclipse release van April 2014 of hoger. Zie voor meer informatie [installeren van de Azure-werkset voor Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
* Een X.509-certificaat voor gebruik met uw toepassing. U moet dit certificaat in openbaar certificaat (.cer) en Personal Information Exchange (. (PFX)-indeling. (Opties voor het maken van dit certificaat wordt verderop in deze zelfstudie worden beschreven).
* Bekend bent met de Azure compute-emulator en implementatie technieken besproken op [maken van een Hallo wereld-toepassing voor Azure in Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Maken van een ACS-Namespace
Om te beginnen met Access Control Service (ACS) in Azure, moet u een ACS-naamruimte maken. De naamruimte biedt een unieke bereik op voor het verwerken van ACS-resources van uw toepassing.

1. Meld u aan bij de [Azure-beheerportal][Azure Management Portal].
2. Klik op **Active Directory**. 
3. Klik op om een nieuwe Access Control-naamruimte **nieuw**, klikt u op **App Services**, klikt u op **toegangsbeheer**, en klik vervolgens op **snelle invoer**. 
4. Voer een naam voor de naamruimte. Azure wordt gecontroleerd of de naam uniek is.
5. Selecteer de regio waarin de naamruimte wordt gebruikt. Gebruik de regio waarin u uw toepassing implementeren voor de beste prestaties.
6. Als u meer dan één abonnement hebt, selecteert u het abonnement dat u wilt gebruiken voor de ACS-naamruimte.
7. Klik op **Create**.

Azure maakt en de naamruimte wordt geactiveerd. Wacht totdat de status van de nieuwe naamruimte **Active** voordat u doorgaat. 

## <a name="add-identity-providers"></a>Id-providers toevoegen
In deze taak is voegt u IP-adressen wilt gebruiken met uw toepassing RP voor verificatie. Deze taak wordt beschreven hoe Windows Live toevoegen als een IP-adres voor demonstratiedoeleinden, maar u kunt een van de IP-adressen die worden vermeld in de ACS-beheerportal.

1. In de [Azure Management Portal][Azure Management Portal], klikt u op **Active Directory**, selecteert u een Access Control-naamruimte en klik vervolgens op **beheren**. Hiermee opent u de ACS-beheerportal.
2. Klik in het linkernavigatievenster van de ACS-Management-Portal op **identiteitsproviders**.
3. Windows Live ID is standaard ingeschakeld en kan niet worden verwijderd. Voor deze zelfstudie wordt alleen Windows Live ID gebruikt. Dit scherm is echter waar u andere IP-adressen kunnen toevoegen door te klikken op de **toevoegen** knop.

Windows Live ID is nu ingeschakeld als een IP-adres voor de ACS-naamruimte. Vervolgens moet u uw Java-webtoepassing (om te worden gemaakt met later) opgeven als een RP.

## <a name="add-a-relying-party-application"></a>Toevoegen van een relying party-toepassing
In deze taak configureert u ACS herkent uw Java-webtoepassing als een geldige RP-toepassing.

1. Klik op de ACS-beheerportal, **Relying party-toepassingen**.
2. Op de **Relying Party-toepassingen** pagina, klikt u op **toevoegen**.
3. Op de **Relying Party-toepassing toevoegen** pagina, de volgende handelingen uit:
   
   1. In **naam**, typ de naam van de Resourceprovider. Typ voor deze zelfstudie **Azure-Web-App**.
   2. In **modus**, selecteer **Voer instellingen handmatig**.
   3. In **Realm**, typt u de URI die het beveiligingstoken dat is uitgegeven door ACS van toepassing is. Typ voor deze taak **http://localhost: 8080 /**.
      ![Realm voor de relying party voor gebruik in rekenemulator][relying_party_realm_emulator]
   4. In **retour-URL** typt u de URL waarnaar ACS het beveiligingstoken retourneert. Typ voor deze taak **http://localhost:8080/MyACSHelloWorld/index.jsp**
      ![Relying party retour-URL voor gebruik in rekenemulator][relying_party_return_url_emulator]
   5. Accepteer de standaardwaarden in de rest van de velden.
4. Klik op **Opslaan**.

U hebt nu geconfigureerd uw Java-webtoepassing wanneer deze wordt uitgevoerd in de Azure-rekenemulator (op http://localhost: 8080 /) moet een RP in uw ACS-naamruimte. Maak vervolgens de regels die ACS gebruikt voor het verwerken van claims voor de Resourceprovider.

## <a name="create-rules"></a>Regels maken
In deze taak definieert u de regels die hoe claims van IP-adressen worden doorgegeven aan uw RP aansturen. Omwille van deze handleiding configureert we gewoon ACS voor het kopiëren van de invoer claimtypen en claimwaarden rechtstreeks in het uitvoertoken zonder deze wijzigen of filteren.

1. Klik op de hoofdpagina van de ACS-beheerportal **regel groepen**.
2. Op de **regelgroepen** pagina, klikt u op **regelgroep standaard voor Azure-Web-App**.
3. Op de **regelgroep bewerken** pagina, klikt u op **genereren**.
4. Op de **-regels genereren: standaard regelgroep voor Azure-Web-App** pagina, zorg ervoor dat Windows Live ID is ingeschakeld en klik vervolgens op **genereren**.    
5. Op de **regelgroep bewerken** pagina, klikt u op **opslaan**.

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Een certificaat uploaden naar de ACS-naamruimte
In deze taak die u uploadt een. PFX-certificaat dat wordt gebruikt voor het ondertekenen van aanvragen voor beveiligingstokens die door de ACS-naamruimte gemaakt.

1. Klik op de hoofdpagina van de ACS-beheerportal **certificaten en sleutels**.
2. Op de **certificaten en sleutels** pagina, klikt u op **toevoegen** hierboven **Token-ondertekening**.
3. Op de **certificaat toevoegen Token-ondertekening of sleutel** pagina:
   1. In de **gebruikt voor** sectie, klikt u op **Relying Party-toepassing** en selecteer **Azure-Web-App** (die u eerder hebt ingesteld als de naam van uw relying party-toepassing).
   2. In de **Type** sectie **X.509-certificaat**.
   3. In de **certificaat** sectie, klik op de bladerknop en navigeer naar het x.509-certificaat-bestand dat u wilt gebruiken. Dit is een. PFX-bestand. Selecteer het bestand, klikt u op **Open**, en voer het wachtwoord voor het certificaat in de **wachtwoord** in het tekstvak. Houd er rekening mee dat een zelfstandige-signed-certificaat voor testdoeleinden kan gebruiken. Voor het maken van een zelfondertekend certificaat gebruikt de **nieuw** knop in de **ACS Filter bibliotheek** dialoogvenster (later beschreven), of gebruik de **encutil.exe** hulpprogramma van de [project website] [ project website] van de Azure Starter Kit voor Java.
   4. Zorg ervoor dat **primair maken** is ingeschakeld. Uw **certificaat toevoegen Token-ondertekening of sleutel** pagina ziet er ongeveer als volgt.
       ![Certificaat voor token-ondertekening toevoegen][add_token_signing_cert]
   5. Klik op **opslaan** uw instellingen opslaan en sluiten de **certificaat toevoegen Token-ondertekening of sleutel** pagina.

Vervolgens Lees de informatie in de pagina van de integratie van toepassingen en kopieer de URI die u moet voor het configureren van uw Java-webtoepassing ACS gebruiken.

## <a name="review-the-application-integration-page"></a>Lees de pagina van de integratie van toepassingen
U vindt alle informatie en de code nodig voor het configureren van uw Java-webtoepassing (de RP-toepassing) om te werken met ACS op de pagina van de integratie van toepassingen van de ACS-beheerportal. U moet deze informatie bij het configureren van uw Java-webtoepassing voor federatieve verificatie.

1. Klik op de ACS-beheerportal, **toepassingsintegratie**.  
2. In de **toepassingsintegratie** pagina, klikt u op **aanmeldingspagina's**.
3. In de **aanmelding pagina integratie** pagina, klikt u op **Azure-Web-App**.

In de **aanmelding pagina integratie: Azure-Web-App** pagina, de URL in **optie 1: de koppeling naar een aanmeldingspagina met ACS worden gehost** wordt gebruikt in uw Java-webtoepassing. U moet deze waarde als u de bibliotheek Azure Access Control-servicefilter aan uw Java-toepassing toevoegt.

## <a name="create-a-java-web-application"></a>Een Java-webtoepassing maken
1. Klik in Eclipse op het menu op **bestand**, klikt u op **nieuw**, en klik vervolgens op **dynamisch webproject**. (Als er geen **dynamisch webproject** vermeld als een project beschikbaar wanneer u op **bestand**, **nieuw**, doet u het volgende: klik op **bestand**, klikt u op **nieuw**, klikt u op **Project**, vouw **Web**, klikt u op **dynamisch webproject**, en klik op  **Naast**.) Noem het project voor deze zelfstudie **MyACSHelloWorld**. (Zorg ervoor dat u gebruikt deze naam, volgende stappen in deze zelfstudie verwacht dat uw naam MyACSHelloWorld WAR-bestand). Uw scherm ziet er ongeveer als volgt:
   
    ![Maak een project Hallo wereld voor ACS exampple][create_acs_hello_world]
   
    Klik op **Voltooien**.
2. Vouw in de weergave Project Explorer van Eclipse **MyACSHelloWorld**. Klik met de rechtermuisknop op **WebContent**(Webinhoud) en klik vervolgens op **New** (Nieuw) en **JSP File** (JSP-bestand).
3. In de **nieuw JSP-bestand** dialoogvenster, de naam van het bestand **index.jsp**. Bewaar de bovenliggende map als MyACSHelloWorld/WebContent, zoals wordt weergegeven in het volgende:
   
    ![Een JSP-bestand bijvoorbeeld ACS toevoegen][add_jsp_file_acs]
   
    Klik op **Volgende**.
4. In de **JSP-sjabloon selecteren** dialoogvenster **nieuw JSP-bestand (html)** en klik op **voltooien**.
5. Wanneer het bestand index.jsp wordt geopend in Eclipse, toevoegen in weer te geven tekst **ACS HelloWorld!** in het bestaande `<body>`-element. Uw bijgewerkte `<body>` inhoud moet worden weergegeven als het volgende:
   
        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
   
    Index.jsp opslaan.

## <a name="add-the-acs-filter-library-to-your-application"></a>De ACS-Filter-bibliotheek aan uw toepassing toevoegen
1. In de Projectverkenner van Eclipse met de rechtermuisknop op **MyACSHelloWorld**, klikt u op **pad**, en klik vervolgens op **configureren pad**.
2. In de **Javabuild-pad** dialoogvenster, klikt u op de **bibliotheken** tabblad.
3. Klik op **bibliotheek toevoegen**.
4. Klik op **Azure Access Control servicefilter (door MS Open Tech)** en klik vervolgens op **volgende**. De **Azure Access Control-servicefilter** dialoogvenster wordt weergegeven.  (De **locatie** veld heeft misschien een ander pad, afhankelijk van waar u Eclipse hebt geïnstalleerd en het versienummer kan verschillen, afhankelijk van de software-updates.)
   
    ![Bibliotheek van ACS-Filter toevoegen][add_acs_filter_lib]
5. Met behulp van een browser geopend de **aanmelding pagina integratie** pagina van de beheerportal, Kopieer de URL die worden vermeld in de **optie 1: de koppeling naar een aanmeldingspagina met ACS worden gehost** veld en plak deze in de **ACS Eindpunt voor authenticatie** veld van het dialoogvenster Eclipse.
6. Met behulp van een browser geopend de **Relying Party-toepassing bewerken** pagina van de beheerportal, Kopieer de URL die worden vermeld in de **Realm** veld en plak deze in de **Relying Party Realm**veld van het dialoogvenster Eclipse.
7. Binnen de **beveiliging** sectie van het dialoogvenster Eclipse, als u wilt een bestaand certificaat gebruiken, klikt u op **Bladeren**, gaat u naar het certificaat dat u wilt gebruiken, selecteert u deze en klikt u op **openen**. Als u wilt een nieuw certificaat maken, klikt u op **nieuw** om weer te geven de **nieuw certificaat** dialoogvenster Geef vervolgens het wachtwoord, de naam van het cer-bestand en de naam van het pfx-bestand voor het nieuwe certificaat.
8. Controleer **insluiten van het certificaat in het WAR-bestand**. Het certificaat op deze manier insluiten opgenomen in de in uw implementatie zonder dat u deze handmatig toevoegen als een onderdeel is vereist. (Als in plaats daarvan moet u uw certificaat voor extern opslaan van het WAR-bestand, kan u het certificaat niet toevoegen als een onderdeel van de rol en schakel het selectievakje **insluiten van het certificaat in het WAR-bestand**.)
9. [Optioneel] Houd **vereisen HTTPS-verbindingen** gecontroleerd. Als u deze optie instelt, moet u toegang tot uw toepassing met behulp van het HTTPS-protocol. Als u niet dat HTTPS-verbindingen vereisen wilt, schakelt u deze optie.
10. Voor een implementatie van de rekenemulator uw **Azure ACS Filter** instellingen ziet er ongeveer als volgt.
    
    ![Azure ACS filterinstellingen voor een implementatie met de rekenemulator][add_acs_filter_lib_emulator]
11. Klik op **Voltooien**.
12. Klik op **Ja** wanneer krijgt met een dialoogvenster waarin wordt vermeld dat een web.xml-bestand wordt gemaakt.
13. Klik op **OK** sluiten de **Javabuild-pad** dialoogvenster.

## <a name="deploy-to-the-compute-emulator"></a>Op de rekenemulator implementeren
1. In de Projectverkenner van Eclipse met de rechtermuisknop op **MyACSHelloWorld**, klikt u op **Azure**, en klik vervolgens op **-pakket voor Azure**.
2. Voor **projectnaam**, type **MyAzureACSProject** en klik op **volgende**.
3. Selecteer een JDK en toepassingsservers. (Deze stappen worden behandeld in detail toe aan de [maken van een Hallo wereld-toepassing voor Azure in Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) zelfstudie).
4. Klik op **Voltooien**.
5. Klik op de **uitvoeren in Azure-Emulator** knop.
6. Nadat uw Java-webtoepassing is gestart in de rekenemulator, sluit u alle exemplaren van uw browser (zodat alle huidige browsersessies veroorzaken geen met ACS aanmelding testen conflicten).
7. Voer uw toepassing via <http://localhost: 8080/MyACSHelloWorld/> in uw browser (of <https://localhost:8080/MyACSHelloWorld/> als u dit selectievakje inschakelt **vereisen HTTPS-verbindingen** ). U moet worden gevraagd om een Windows Live ID-aanmelding en u moet worden uitgevoerd om de retour-URL opgegeven voor de relying party-toepassing.
8. Wanneer u klaar bent met het weergeven van uw toepassing, klikt u op de **opnieuw instellen van de Azure-Emulator** knop.

## <a name="deploy-to-azure"></a>Implementeren in Azure
Als u wilt implementeren in Azure, moet u de realm voor de relying party wijzigen en retour-URL voor de ACS-naamruimte.

1. In de Azure-beheerportal in de **Relying Party-toepassing bewerken** pagina, wijzigt u **Realm** moet de URL van uw geïmplementeerde site. Vervang **voorbeeld** met de DNS-naam die u hebt opgegeven voor uw implementatie.
   
    ![Realm voor de relying party voor gebruik in productie][relying_party_realm_production]
2. Wijzig **retour-URL** moet de URL van uw toepassing. Vervang **voorbeeld** met de DNS-naam die u hebt opgegeven voor uw implementatie.
   
    ![Relying party retour-URL voor gebruik in productie][relying_party_return_url_production]
3. Klik op **opslaan** uw bijgewerkte beantwoorden partij realm opslaan en wijzigingen van de URL te retourneren.
4. Houd de **aanmelding pagina integratie** pagina openen in uw browser, moet u enkele ogenblikken kopiëren.
5. In de Projectverkenner van Eclipse met de rechtermuisknop op **MyACSHelloWorld**, klikt u op **pad**, en klik vervolgens op **configureren pad**.
6. Klik op de **bibliotheken** tabblad **Azure Access Control-servicefilter**, en klik vervolgens op **bewerken**.
7. Met behulp van een browser geopend de **aanmelding pagina integratie** pagina van de beheerportal, Kopieer de URL die worden vermeld in de **optie 1: de koppeling naar een aanmeldingspagina met ACS worden gehost** veld en plak deze in de **ACS Eindpunt voor authenticatie** veld van het dialoogvenster Eclipse.
8. Met behulp van een browser geopend de **Relying Party-toepassing bewerken** pagina van de beheerportal, Kopieer de URL die worden vermeld in de **Realm** veld en plak deze in de **Relying Party Realm**veld van het dialoogvenster Eclipse.
9. Binnen de **beveiliging** sectie van het dialoogvenster Eclipse, als u wilt een bestaand certificaat gebruiken, klikt u op **Bladeren**, gaat u naar het certificaat dat u wilt gebruiken, selecteert u deze en klikt u op **openen**. Als u wilt een nieuw certificaat maken, klikt u op **nieuw** om weer te geven de **nieuw certificaat** dialoogvenster Geef vervolgens het wachtwoord, de naam van het cer-bestand en de naam van het pfx-bestand voor het nieuwe certificaat.
10. Houd **insluiten van het certificaat in het WAR-bestand** dit selectievakje inschakelt, ervan uitgaande dat u wilt insluiten van het certificaat in het WAR-bestand.
11. [Optioneel] Houd **vereisen HTTPS-verbindingen** gecontroleerd. Als u deze optie instelt, moet u toegang tot uw toepassing met behulp van het HTTPS-protocol. Als u niet dat HTTPS-verbindingen vereisen wilt, schakelt u deze optie.
12. Voor een implementatie naar Azure ziet uw Azure-ACS filterinstellingen vergelijkbaar met de volgende.
    
    ![Azure ACS filterinstellingen voor een productie-implementatie][add_acs_filter_lib_production]
13. Klik op **voltooien** sluiten de **bibliotheek bewerken** dialoogvenster.
14. Klik op **OK** sluiten de **eigenschappen voor MyACSHelloWorld** dialoogvenster.
15. Klik in Eclipse op de **publiceren naar Azure-Cloud** knop. Reageren op de vragen vergelijkbare zoals uitgevoerd de **voor het implementeren van uw toepassing in Azure** sectie van de [maken van een Hallo wereld-toepassing voor Azure in Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) onderwerp. 

Nadat u uw webtoepassing is geïmplementeerd, sluit eventuele geopende browsersessies, uitvoeren van uw webtoepassing en wordt u gevraagd zich aanmelden met een Windows Live ID-referenties, gevolgd door de retour-URL van uw relying party-toepassing worden verzonden.

Wanneer u klaar bent met behulp van uw toepassing ACS Hallo wereld onthouden verwijderen van de implementatie (u kunt informatie over het verwijderen van een implementatie in de [maken van een Hallo wereld-toepassing voor Azure in Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) onderwerp).

## <a name="next_steps"></a>Volgende stappen
Zie voor een onderzoek van de Security Assertion Markup Language (SAML) geretourneerd door ACS voor uw toepassing, [SAML weergeven die zijn geretourneerd door de Azure Access Control Service][How to view SAML returned by the Azure Access Control Service]. Verder verkennen van ACS-functionaliteit en om te experimenteren met meer geavanceerde scenario's, Zie [Access Control Service 2.0][Access Control Service 2.0].

Ook in dit voorbeeld gebruikt de **insluiten van het certificaat in het WAR-bestand** optie. Deze optie kunt u eenvoudig om het certificaat te implementeren. Als u in plaats daarvan Bewaar het certificaat voor ondertekening van het WAR-bestand wilt, kunt u de volgende techniek gebruiken:

1. Binnen de **beveiliging** sectie van de **Azure Access Control-servicefilter** dialoogvenster, type **${env. JAVA_HOME}/mycert.cer** en schakelt u **insluiten van het certificaat in het WAR-bestand**. (Mijncert.cer aanpassen als de naam van uw certificaat verschilt.) Klik op **voltooien** om het dialoogvenster te sluiten.
2. Kopieer het certificaat als onderdeel van uw implementatie: vouw In Projectverkenner van Eclipse **MyAzureACSProject**, met de rechtermuisknop op **WorkerRole1**, klikt u op **eigenschappen**, Vouw **Azure rol**, en klik op **onderdelen**.
3. Klik op **Add**.
4. Binnen de **onderdeel toevoegen** dialoogvenster:
   
   1. In de **importeren** sectie:
      1. Gebruik de **bestand** knop om te navigeren naar het certificaat dat u wilt gebruiken. 
      2. Voor **methode**, selecteer **kopie**.
   2. Voor **als naam**, klik op het tekstvak en accepteer de standaardnaam.
   3. In de **implementeren** sectie:
      1. Voor **methode**, selecteer **kopie**.
      2. Voor **naar map**, type **% JAVA_HOME %**.
   4. Uw **onderdeel toevoegen** dialoogvenster ziet er ongeveer als volgt.
      
       ![Certificaatonderdeel toevoegen][add_cert_component]
   5. Klik op **OK**.

Op dit moment zou uw certificaat worden opgenomen in uw implementatie. Ongeacht of u het certificaat in het WAR-bestand insluiten of als een onderdeel toevoegen aan uw implementatie, moet u het certificaat uploaden naar uw naamruimte, zoals beschreven in de [Upload een certificaat naar de ACS-naamruimte] [ Upload a certificate to your ACS namespace] sectie.

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Upload a certificate to your ACS namespace]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[project website]: http://wastarterkit4java.codeplex.com/releases/view/61026
[How to view SAML returned by the Azure Access Control Service]: active-directory-java-view-saml-returned-by-access-control.md
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure Management Portal]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png

