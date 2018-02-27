---
title: Problemen met Kerberos-beperkte overdracht configuraties voor toepassingsproxy | Microsoft Docs
description: Problemen met Kerberos-beperkte overdracht configuraties voor toepassingsproxy.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2018
ms.author: markvi
ms.reviewer: harshja
ms.openlocfilehash: a580b0afbd34623986ea8a3f60147a937c423e5e
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="troubleshoot-kerberos-constrained-delegation-configurations-for-application-proxy"></a>Problemen met Kerberos-beperkte overdracht configuraties voor toepassingsproxy

De methoden die beschikbaar zijn voor het bereiken van eenmalige aanmelding met gepubliceerde toepassingen kunnen enigszins verschillen van de ene toepassing naar een andere. Een van de opties die Azure-toepassingsproxy standaard biedt is Kerberos-beperkt delegatie (KCD). U kunt een connector om uit te voeren op back-end-toepassingen, namens gebruikers beperkte Kerberos-verificatie configureren.

De werkelijke procedure voor het inschakelen van KCD is betrekkelijk eenvoudig en is doorgaans niet meer dan een algemeen begrip van de verschillende onderdelen en authenticatiestroom voor eenmalige aanmelding. Goede bronnen van informatie voor het oplossen van scenario's waarbij KCD SSO niet werkt zoals verwacht, zoeken kan worden verspreid.

Als zodanig probeert in dit artikel toegang te bieden een potentieel verwijzing die u helpen kan oplossen en zelf herstellen van de meest voorkomende problemen gezien. Op hetzelfde moment bieden aanvullende richtlijnen bij het vaststellen van de complexere en moeilijkheden van implementatie.

In dit artikel maakt de volgende veronderstellingen:

-   De implementatie van Azure-toepassingsproxy per [documentatie](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable) en algemene toegang tot toepassingen niet KCD werkt zoals verwacht.

-   De gepubliceerde doeltoepassing is gebaseerd op IIS en Microsoft implementatie van Kerberos.

-   De hosts servers en toepassingen zich in één Active Directory-domein bevinden. Gedetailleerde informatie over cross-domein en forest-scenario's vindt u in de [KCD technisch document](https://aka.ms/KCDPaper).

-   De desbetreffende toepassing is gepubliceerd in een Azure-tenant met vooraf-verificatie ingeschakeld en gebruikers te verifiëren bij Azure via authenticatie op basis van formulieren worden verwacht. Rich client verificatie scenario's worden niet behandeld in dit artikel wordt beschreven, maar worden toegevoegd op een bepaald moment in de toekomst.

## <a name="prerequisites"></a>Vereisten

Azure Application Proxy kan worden geïmplementeerd in veel soorten infrastructuren of omgevingen en de architecturen geen twijfel bestaat organisatie variëren. Een van de meest voorkomende oorzaken van problemen met KCD zijn niet de omgevingen zelf, maar in plaats daarvan eenvoudige onjuiste configuraties of algemene toezicht.

Daarom moet het is verstandig om te starten door ervoor te zorgen dat aan alle lay-out in vereisten wordt voldaan [KCD eenmalige aanmelding met de toepassingsproxy artikel met behulp van](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) starten voor het oplossen van problemen.

Met name de sectie over het configureren van KCD op 2012R2, omdat dit een fundamenteel andere benadering voor de configuratie van KCD in eerdere versies van Windows, maar houd ook rekening met verschillende andere overwegingen terwijl de veiligheidsmaatregelen:

-   Het is niet ongewoon is voor een domeinlidserver voor het openen van een beveiligd kanaal-dialoogvenster met een specifieke domeincontroller. Vervolgens verplaatsen naar een ander dialoogvenster op elk gewenst zodat connector hosts niet kunnen communiceren met alleen specifieke lokale site DC's moeten worden beperkt.

-   Vergelijkbaar met het voorgaande tussen domeinen scenario's zijn afhankelijk van de referenties die een host connector op DC's die zich buiten het lokale netwerk perimeternetwerk directe-punt. In dit scenario is het belangrijk om te controleren of u ook toestaat dat verkeer naar DC's waarbij andere domeinen en hoger, of anders overdracht mislukken.

-   Waar mogelijk, moet u niet alle actieve apparaten in de IP-Adressen/id's tussen connector hosts en de DC's, plaatsen wanneer deze soms via Tussenkomende worden en core RPC-verkeer verstoren

In het eenvoudigste scenario's moet u de overdracht testen. Meer variabelen introduceren, hoe meer u wellicht concurreren met. Bijvoorbeeld: beperken van de test voor één connector bespaart kostbare tijd en aanvullende connectors kunnen worden toegevoegd nadat de problemen zijn opgelost.

Sommige omgevingsfactoren kunnen ook een bijdrage leveren aan een probleem. Tijdens het testen, de architectuur tot een absoluut minimum om te voorkomen dat deze omgevingsfactoren te minimaliseren. Bijvoorbeeld, onjuist geconfigureerde interne firewall ACL's zijn niet ongewoon, dus indien mogelijk hebben al het verkeer van een connector kunnen rechtstreeks via de DC's en de back-end-toepassing. 

Daadwerkelijk is de absolute beste plaats voor het plaatsen van connectors zo dicht mogelijk bij hun doelen omdat kan zijn. Met een firewall zat inline wordt aangeroepen terwijl er gewoon testen onnodige complexiteit toegevoegd en kan alleen verlengen van uw onderzoek.

Ja, wat wordt verstaan onder een probleem KCD toch? Er zijn verschillende klassieke aanduidingen van KCD SSO mislukt en de eerste tekenen van een probleem meestal manifest zelf in de browser.

   ![Onjuiste KCD configuratiefout](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

   ![Autorisatie is mislukt vanwege ontbrekende machtigingen](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

alle voorzien zijn van de dezelfde symptoom van een failover naar SSO worden uitgevoerd en de gebruikerstoegang tot de toepassing als gevolg daarvan te weigeren.

## <a name="troubleshooting"></a>Problemen oplossen

Hoe u vervolgens oplossen, afhankelijk van het probleem en symptomen waargenomen. Bekijk voordat u doorgaat een, de volgende koppelingen als ze nuttige informatie die u niet nog afkomstig bevatten via zijn mogelijk:

-   [Oplossen van problemen met toepassingsproxy en foutberichten](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot)

-   [Kerberos-fouten en problemen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#kerberos-errors)

-   [Werken met eenmalige aanmelding bij het on-premises en cloud identiteiten zijn niet identiek](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd#working-with-sso-when-on-premises-and-cloud-identities-are-not-identical)

Als u deze helemaal, vervolgens het belangrijkste probleem zeker hebt bestaat. Start het scheiden van de stroom in drie verschillende fasen die u kunt oplossen.

**Clientverificatie vooraf van** : de externe gebruiker verificatie met Azure via een browser.

Wordt vooraf verifiëren naar Azure is van cruciaal belang voor KCD SSO functie. U moet testen en dit adres als er problemen zijn. De verificatie vooraf fase heeft geen relatie met KCD of de gepubliceerde toepassing. Het moet redelijk eenvoudig naar eventuele afwijkingen corrigeren door sanity controleren van het onderwerp account bestaat in Azure, en dat deze is niet uitgeschakeld/geblokkeerd. Het foutbericht in de browser is meestal beschrijvende genoeg om te begrijpen van de oorzaak. U kunt ook onze andere problemen met documenten om te controleren als u niet zeker weet controleren.

**Overdracht service** : de Azure-Proxy connector ophalen van een Kerberos-serviceticket uit een KDC (Kerberos Distribution Center) namens gebruikers.

De externe communicatie tussen de client en de Azure-front-end moet geen gevolgen hebben voor KCD dan ook, andere dan ervoor zorgen dat deze werkt. Dit is de Azure-Proxy-service kan worden opgegeven met een geldige gebruikers-ID die wordt gebruikt om een Kerberos-ticket te verkrijgen. Zonder deze KCD is niet mogelijk en mislukken.

Zoals eerder vermeld, leveren de browser foutberichten sommige goede aanwijzingen gewoonlijk op waarom de dingen zijn mislukt. Zorg ervoor dat de activiteits-ID en een tijdstempel in het antwoord Noteer omdat Hiermee kunt u correlaties zichtbaar maken tussen het gedrag op werkelijke gebeurtenissen in het gebeurtenislogboek van de Azure-Proxy.

   ![Onjuiste KCD configuratiefout](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

En de overeenkomende vermeldingen gezien dat het gebeurtenislogboek zou worden gezien als gebeurtenissen 13019 of 12027. U vindt de gebeurtenislogboeken van de connector in **logboeken toepassingen en Services** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt; **Connector**&gt;**Admin**.

   ![Gebeurtenis 13019 uit Application Proxy-gebeurtenislogboek](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

   ![Gebeurtenis 12027 uit Application Proxy-gebeurtenislogboek](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

-   Een A-record gebruiken in uw interne DNS voor de toepassing adres en niet een CName

-   Bevestig dat de host van de connector de rechten voor het delegeren aan de aangewezen doelaccount van SPN en dat is verleend **elk verificatieprotocol voor gebruiken** is geselecteerd. Zie voor meer informatie over dit onderwerp [SSO configuratie artikel](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd)

-   Controleer of er slechts één exemplaar van de SPN-naam bestaat in AD door uitgifte van een `setspn -x` vanaf een opdrachtprompt op de host van een domein lid

-   Controleer of het domeinbeleid wordt afgedwongen beperken de [de maximale grootte van de uitgegeven tokens voor Kerberos](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/), zoals dit voorkomt dat de connector het verkrijgen van een token als overmatige worden gevonden

Een nieuwe netwerktracering vastleggen van de uitwisseling tussen de connector-host en een domein KDC worden dan de volgende aanbevolen stap bij het verkrijgen van meer op laag niveau informatie over de problemen. Voor meer informatie ziet, [diepgaand oplossen papier](https://aka.ms/proxytshootpaper).

Als tickets er goed uitziet, ziet u een gebeurtenis in de logboeken met de mededeling dat de verificatie is mislukt vanwege de toepassing een 401 retourneren. Dit geeft doorgaans aan dat de doeltoepassing uw ticket weigert, dus gaan met de volgende fase van de volgende:

**Doeltoepassing** -de consument van het Kerberos-ticket geleverd door de connector

In deze fase wordt de connector naar verwachting een Kerberos-serviceticket hebt verzonden naar de back-end als een koptekst binnen de eerste toepassingsaanvraag.

-   Met behulp van de toepassing interne URL die is gedefinieerd in de portal valideren dat de toepassing toegankelijk rechtstreeks vanuit de browser op de host van de connector is. U kunt Meld u vervolgens aan met succes. Details hierover vindt u op de pagina van de connector oplossen.

-   Nog steeds op de host connector bevestigen dat de verificatie tussen de browser en de toepassing is Kerberos, door een van de volgende manieren:

1.  Dev-hulpprogramma's uitvoeren (**F12**) in Internet Explorer of gebruik [Fiddler](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) van de connector host. Ga naar de toepassing die gebruikmaakt van de interne URL en controleer het aangeboden www-autorisatie-header in het antwoord van de toepassing geretourneerd om ervoor te zorgen dat onderhandelen of Kerberos aanwezig is. Een daaropvolgende Kerberos blob geretourneerd in de reactie van de browser naar de toepassing doorgaans begint met **YII**, zodat dit is een goede indicatie van Kerberos in play. NTLM aan de andere kant begint altijd met **TlRMTVNTUAAB**, welke luidt NTLMSSP bij het decoderen van Base64. Als u ziet **TlRMTVNTUAAB** aan het begin van de blob, betekent dit dat Kerberos **niet** beschikbaar. Als u dit niet ziet, is het Kerberos waarschijnlijk beschikbaar.
    > [!NOTE]
    > Als u Fiddler, zou deze methode nodig tijdelijk uitschakelen van de uitgebreide beveiliging op de configuratie van de toepassing in IIS.

     ![Venster van de controle van de browser](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

    *Afbeelding:* omdat deze niet met TIRMTVNTUAAB begint, dit is een voorbeeld is dat Kerberos beschikbaar is. Dit is een voorbeeld van een Kerberos-Blob die niet met YII begint.

2.  Tijdelijk NTLM uit de lijst met providers op de IIS-website in en toegang app rechtstreeks vanuit Internet Explorer op de host van de connector verwijderen. Met NTLM niet langer in de lijst met providers moet u toegang tot de toepassing alleen Kerberos gebruiken. Als dit niet lukt, vervolgens die wijst erop dat er een probleem met de configuratie van de toepassing en Kerberos-verificatie niet werkt.

Als Kerberos niet beschikbaar is, is controle van de verificatie-instellingen van de toepassing in IIS om ervoor te zorgen, te onderhandelen over bovenste met NTLM onder het vermeld. (Geen onderhandelen: Kerberos of onderhandelingsbericht: PKU2U). Alleen doorgaan als Kerberos functioneel is.

   ![Windows-verificatieproviders](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)
   
-   Met Kerberos en NTLM voldaan, kunt vooraf-verificatie voor de toepassing in de portal nu tijdelijk uitschakelen. Zie als u deze kunt openen vanaf het internet met behulp van de externe URL. U moet worden gevraagd om te verifiëren en moet kunnen doen met hetzelfde account gebruikt in de vorige stap. Als dat niet het geval is, wordt dit duidt op een probleem met de back-end-toepassing en geen KCD helemaal.

-   Nu opnieuw inschakelen van vooraf-verificatie in de portal en via Azure door de App via de externe URL verbinding proberen te verifiëren. Als u eenmalige aanmelding is mislukt, ziet u een niet-toegestane foutbericht weergegeven in de browser, plus de gebeurtenis 13022 in het logboek:

    *Microsoft AAD Application Proxy Connector kan de gebruiker kan niet verifiëren omdat de back-endserver op pogingen tot Kerberos-verificatie met een 401 HTTP-fout reageert.*

    ![HTTTP 401 fout is niet toegestaan](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

-   Controleer de IIS-toepassing om te controleren of dat de geconfigureerde groep van toepassingen is geconfigureerd voor gebruik van hetzelfde account dat de SPN-naam is geconfigureerd op basis van AD, door te navigeren in IIS, zoals in de volgende afbeelding

    ![Venster voor configuratie van IIS-toepassing](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

    Zodra u weet dat de identiteit, geven het volgende vanaf een opdrachtprompt om ervoor te zorgen dat dit account definitief is geconfigureerd met de desbetreffende SPN. Bijvoorbeeld:  `setspn –q http/spn.wacketywack.com`

    ![SetSPN-opdrachtvenster](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

-   Controleer of de SPN-naam is gedefinieerd voor de instellingen van de toepassing in de portal is de dezelfde SPN geconfigureerd op basis van het AD-doelaccount dat door de toepassingsgroep app gebruikt

   ![SPN-configuratie in Azure-portal](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)
   
-   Ga naar de IIS- en selecteer de **configuratie-Editor** optie voor de toepassing en navigeer naar **system.webServer/security/authentication/windowsAuthentication** om te controleren of de waarde **UseAppPoolCredentials** is **True**

   ![App-groepen van IIS-configuratie referentie-optie](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

Na het wijzigen van deze waarde **True**, alle Kerberos-tickets moeten worden verwijderd uit de back-endserver in cache opgeslagen. U kunt dit doen door de volgende opdracht uit te voeren:

```powershell
Get-WmiObject Win32_LogonSession | Where-Object {$_.AuthenticationPackage -ne 'NTLM'} | ForEach-Object {klist.exe purge -li ([Convert]::ToString($_.LogonId, 16))}
``` 

Zie voor meer informatie [opschonen van de Kerberos-ticket clientcache voor alle sessies](https://gallery.technet.microsoft.com/scriptcenter/Purge-the-Kerberos-client-b56987bf).



Terwijl wordt nuttig zijn bij het verbeteren van Kerberos-bewerkingen, verlaten kernelmodus ingeschakeld ook zorgt ervoor het ticket voor de aangevraagde service om te worden ontsleuteld met behulp van computeraccount dat. Dit wordt ook het lokale systeem, dus met dit instellen op true onderbreken KCD wanneer de toepassing wordt gehost op meerdere servers in een farm genoemd.

-   Als een aanvullende controle u mogelijk ook wilt uitschakelen de **Extended** beveiliging te. Er zijn aangetroffen scenario's waar dit is gebleken opsplitsen KCD wanneer ingeschakeld in specifieke configuraties waarin een toepassing is gepubliceerd als een submap van de standaardwebsite. Deze zelf is geconfigureerd voor anonieme verificatie, zodat de gehele dialoogvensters grijs voorstellen onderliggende objecten wordt niet overgenomen actieve instellingen. Maar waar mogelijk zou altijd aan te raden dit ingeschakeld, dus in alle geval test, maar vergeet niet om terug te zetten dit ingeschakeld.

Deze extra controles moeten hebben plaats in bijhouden aan de slag met uw gepubliceerde toepassing. U kunt opwekken en draaien van aanvullende connectors die zijn geconfigureerd voor het overdragen, maar als dingen niet verder zijn vervolgens zou het is raadzaam het lezen van onze uitgebreidere technische scenario [volledige handleiding voor het oplossen van Azure AD-toepassingsproxy](https://aka.ms/proxytshootpaper)

Als u nog steeds niet de voortgang van uw probleem, kan ondersteuning zou zijn meer dan u kunnen helpen en gaan. Maak een ondersteuningsticket rechtstreeks vanuit de portal (een technicus zal bereiken u).

## <a name="other-scenarios"></a>Andere scenario's

-   Een Kerberos-ticket aanvragen Azure toepassingsproxy voordat de aanvraag wordt verzonden naar een toepassing. Sommige toepassingen van derden zoals Tableau Server niet tevreden bent met deze methode voor het authenticeren en verwacht in plaats daarvan de meer traditionele onderhandelingen te kunnen uitvoeren. De eerste aanvraag is anonieme, zodat de toepassing reageren met de verificatietypen dat wordt ondersteund door een 401.

-   Dubbele hop verificatie - meestal gebruikt in scenario's waarbij lagen van een toepassing, met een back-end als front-end, beide verificatie te vereisen, zoals SQL Reporting Services.

## <a name="next-steps"></a>Volgende stappen
[Kerberos-beperkte delegatie (KCD) configureren op een beheerd domein](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-enable-kcd)
