---
title: Problemen met Kerberos-beperkte overdracht configuraties voor toepassingsproxy | Microsoft Docs
description: Kerberos-beperkte overdracht configuraties voor toepassingsproxy oplossen
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: f318c53de073c8f1fa6c3ae11cb335a4a91e137d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36334958"
---
# <a name="troubleshoot-kerberos-constrained-delegation-configurations-for-application-proxy"></a>Kerberos-beperkte overdracht configuraties voor toepassingsproxy oplossen

De methoden die beschikbaar zijn voor het bereiken van eenmalige aanmelding met gepubliceerde toepassingen kunnen variëren van één toepassing naar een andere. Een optie die toepassingsproxy van Azure Active Directory (Azure AD) standaard biedt is Kerberos-beperkte delegatie (KCD). U kunt een connector voor uw gebruikers, beperkte Kerberos-verificatie uitvoert op back-end-toepassingen kunt configureren.

De procedure voor het inschakelen van KCD is eenvoudig. Is er niet meer dan een algemeen begrip van de verschillende onderdelen en authenticatiestroom die ondersteuning bieden voor eenmalige aanmelding. Maar soms KCD SSO niet werkt zoals verwacht. U moet een goede bronnen van informatie voor het oplossen van deze scenario's.

In dit artikel biedt een centraal punt van verwijzing die helpt oplossen en enkele van de meest voorkomende problemen automatisch te herstellen. Het omvat tevens diagnose van problemen met complexere implementatie.

In dit artikel maakt de volgende veronderstellingen:

-   Implementatie van Azure AD-toepassingsproxy per [aan de slag met Application Proxy](manage-apps/application-proxy-enable.md) en algemene toegang tot toepassingen niet KCD werkt zoals verwacht.

-   De gepubliceerde doeltoepassing is gebaseerd op het Internet Information Services (IIS) en Microsoft-implementatie van Kerberos.

-   De hosts servers en toepassingen zich in één Azure Active Directory-domein bevinden. Zie voor gedetailleerde informatie over scenario's met verschillende domeinen en forest de [KCD witboek](https://aka.ms/KCDPaper).

-   De desbetreffende toepassing is gepubliceerd in een Azure-tenant met vooraf-verificatie ingeschakeld. Gebruikers worden voor verificatie op Azure via authenticatie op basis van formulieren verwacht. Rich client verificatie scenario's worden niet gedekt door dit artikel. Ze kunnen worden toegevoegd op een bepaald moment in de toekomst.

## <a name="prerequisites"></a>Vereisten

Azure AD-toepassingsproxy kan worden geïmplementeerd in veel soorten infrastructuren of omgevingen. De architecturen variëren organisatie. De meest voorkomende oorzaken van problemen met KCD zijn niet de omgevingen. Eenvoudige configuratiefouten of algemene fouten kunt u de meeste problemen veroorzaken.

Daarom moet het is raadzaam om ervoor te zorgen dat u hebt voldaan aan de vereisten in [met behulp van KCD eenmalige aanmelding bij de toepassingsproxy](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md) voordat u begint met het oplossen van problemen. Let op de sectie over het configureren van Kerberos-beperkte overdracht op 2012R2. Dit proces maakt gebruik van een andere benadering voor de configuratie van KCD in eerdere versies van Windows. Bovendien worden zich ervan bewust van deze overwegingen:

-   Het is niet ongewoon is voor een domeinlidserver voor het openen van een beveiligd kanaal-dialoogvenster met een specifieke domeincontroller (DC). De server mogelijk vervolgens verplaatsen naar een ander dialoogvenster op elk moment. Connector-hosts zijn dus niet beperkt tot communicatie met alleen specifieke lokale site DC's.

-   Verschillende domeinen scenario's zijn afhankelijk van verwijzingen die rechtstreeks van een connector-host op DC's die mogelijk buiten de lokale netwerkverbinding. In dergelijke gevallen is het belangrijk ook om verkeer te verzenden bewerken op DC's waarbij andere domeinen. Als dat niet het geval is, overdracht is mislukt.

-   Waar mogelijk, plaats u eventuele actieve apparaten voor IP-Adressen of -id's tussen connector hosts en de DC's. Deze apparaten worden soms overintrusive en core RPC-verkeer verstoren.

Delegering in het eenvoudige scenario's testen. Meer variabelen introduceren, hoe meer u wellicht concurreren met. Beperk de test voor één connector tijd besparen. Aanvullende connectors toevoegen nadat het probleem is opgelost.

Sommige omgevingsfactoren mogelijk ook bijdragen aan een probleem. Om te voorkomen dat deze factoren, minimaliseert architectuur zo veel mogelijk tijdens het testen. Bijvoorbeeld, onjuist geconfigureerde interne firewall ACL's gelden. Indien mogelijk alle verkeer verzenden van een connector rechtstreeks naar de DC's en back-endtoepassing.

De beste plaats voor het plaatsen van connectors is zo dicht mogelijk bij hun doelen. Een firewall die inline bevindt zich bij het testen van onnodige complexiteit toegevoegd en uw onderzoek kan duren.

Wat er wordt een probleem KCD? Er zijn enkele algemene aanwijzingen waarvoor KCD SSO is mislukt. De eerste tekenen van een probleem weergegeven in de browser.

   ![Onjuiste KCD configuratiefout](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

   ![Autorisatie is mislukt vanwege ontbrekende machtigingen](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

Beide van deze installatiekopieën de dezelfde symptoom weergeven: fout van eenmalige aanmelding. Gebruikerstoegang tot de toepassing is geweigerd.

## <a name="troubleshooting"></a>Probleemoplossing

Hoe u oplossen, is afhankelijk van het probleem en de problemen die u wilt zien. Voordat u een verder gaat, gebruik de volgende artikelen. Ze bieden nuttige probleemoplossingsinformatie:

-   [Oplossen van problemen met toepassingsproxy en foutberichten](manage-apps/application-proxy-troubleshoot.md)

-   [Kerberos-fouten en problemen](manage-apps/application-proxy-troubleshoot.md#kerberos-errors)

-   [Werken met eenmalige aanmelding bij het on-premises en cloud identiteiten niet identiek zijn](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#working-with-different-on-premises-and-cloud-identities)

Als u op dit punt hebt verkregen, is uw belangrijkste probleem. Als u wilt starten, scheidt u de stroom in de volgende drie fasen die u kunt oplossen.

### <a name="client-pre-authentication"></a>Clientverificatie vooraf van 
De externe gebruiker verificatie met Azure via een browser. De mogelijkheid voor vooraf verificatie op Azure is nodig voor KCD SSO functie. Testen en oplossen van deze mogelijkheid als er problemen zijn. De fase vooraf-verificatie is niet gerelateerd aan KCD of de gepubliceerde toepassing. Het is gemakkelijk om op te lossen door het controleren van sanity eventuele verschillen die het onderwerp account in Azure bestaat. Ook controleren of deze niet is uitgeschakeld of geblokkeerd. Het foutbericht in de browser is beschrijvende om uit te leggen van de oorzaak. Als u niet zeker bent, controleert u andere probleemoplossing artikelen van Microsoft om te controleren.

### <a name="delegation-service"></a>Service voor overdracht 
De Azure-Proxy-connector die opgehaald van een Kerberos-serviceticket voor gebruikers van een Kerberos Key Distribution Center (KCD).

De externe communicatie tussen de client en de Azure-front-end hebben geen invloed op KCD. Deze communicatie alleen Zorg dat de KCD werkt. De Azure-Proxy-service krijgt u een geldige gebruikers-ID die wordt gebruikt voor het ophalen van een Kerberos-ticket. Zonder deze ID KCD is niet mogelijk en mislukt.

Zoals eerder vermeld, biedt de browser foutberichten sommige aflezen waarom dingen mislukken. Zorg ervoor dat Noteer de activiteits-ID en een tijdstempel in het antwoord. Deze informatie helpt u het gedrag op werkelijke gebeurtenissen in het gebeurtenislogboek van de Azure-Proxy correleren.

   ![Onjuiste KCD configuratiefout](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

De overeenkomende vermeldingen weergegeven in het gebeurtenislogboek weergeven als 13019 of 12027-gebeurtenissen. De gebeurtenislogboeken van de connector in vinden **logboeken toepassingen en Services** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt;  **Connector** &gt; **Admin**.

   ![Gebeurtenis 13019 uit Application Proxy-gebeurtenislogboek](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

   ![Gebeurtenis 12027 uit Application Proxy-gebeurtenislogboek](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

1.   Gebruik een **A** niet vastleggen in uw interne DNS voor de toepassing adres, een **CName**.

2.   Bevestig dat de host van de connector het recht om te delegeren aan de aangewezen doelaccount SPN is verleend. Bevestig dat **elk verificatieprotocol voor gebruiken** is geselecteerd. Zie voor meer informatie de [SSO configuratie artikel](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md).

3.   Controleer of er slechts één exemplaar van de SPN-naam bestaat in Azure AD. Probleem `setspn -x` vanaf een opdrachtprompt op de host van een domein lid.

4.   Controleer dat het domeinbeleid wordt afgedwongen dat beperkt de [maximale grootte van de uitgegeven tokens voor Kerberos](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/). Dit beleid wordt de connector van het ophalen van een token als wordt geconstateerd overmatige worden gestopt.

Een netwerktracering maken waarmee de uitwisseling tussen de connector-host en een domein KDC wordt vastgelegd is de volgende aanbevolen stap op laag niveau nader ophalen over de problemen. Zie voor meer informatie de [diepgaand oplossen papier](https://aka.ms/proxytshootpaper).

Als tickets er goed uitziet, ziet u een gebeurtenis in de logboeken met de mededeling dat de verificatie is mislukt, omdat de toepassing een 401 geretourneerd. Deze gebeurtenis geeft aan dat de doeltoepassing uw ticket geweigerd. Ga naar de volgende fase.

### <a name="target-application"></a>Doeltoepassing 
De consument van het Kerberos-ticket geleverd door de connector. In deze fase wordt verwacht dat de connector van een Kerberos-serviceticket hebt verzonden naar de back-end. Dit ticket is een kop in de eerste toepassingsaanvraag.

1.   Valideren dat de toepassing toegankelijk rechtstreeks vanuit de browser op de host van de connector is met behulp van de toepassing interne URL die is gedefinieerd in de portal. Vervolgens kunt u aanmelden met succes. Meer informatie vindt u op de connector **oplossen** pagina.

2.   Bevestig dat de verificatie tussen de browser en de toepassing gebruikmaakt van Kerberos nog steeds op de host connector. Voer een van de volgende acties:

3.  Voer DevTools (**F12**) in Internet Explorer of gebruik [Fiddler](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) van de connector host. Ga naar de toepassing met behulp van de interne URL. Inspecteer de aangeboden www-autorisatie-header geretourneerd in het antwoord van de toepassing om ervoor te zorgen dat een onderhandelen of Kerberos aanwezig is. 

    a. De volgende Kerberos blob die wordt geretourneerd in het antwoord van de browser naar de toepassing wordt gestart met **YII**. Deze letters laat u weten dat Kerberos wordt uitgevoerd. Microsoft NT LAN Manager (NTLM) aan de andere kant begint altijd met **TlRMTVNTUAAB**, welke luidt NTLM Security Support Provider (NTLMSSP) bij het decoderen van Base64. Als u ziet **TlRMTVNTUAAB** aan het begin van de blob wordt Kerberos is niet beschikbaar. Als er geen **TlRMTVNTUAAB**, Kerberos is waarschijnlijk beschikbaar.

       > [!NOTE]
       > Als u Fiddler, wordt deze methode moet u uitgebreide beveiliging van de configuratie van de toepassing in IIS tijdelijk uitschakelen.

       ![Venster van de controle van de browser](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

    b. De blob in deze afbeelding begint niet met **TIRMTVNTUAAB**. In dit voorbeeld Kerberos beschikbaar, en wordt de Kerberos-blob begint niet met **YII**.

4.  Tijdelijk NTLM uit de lijst met providers op de IIS-site verwijderen. Toegang tot de app rechtstreeks vanuit Internet Explorer op de host van de connector. NTLM is niet langer in de lijst met providers. U kunt toegang tot de toepassing alleen via Kerberos. Als de toegang is mislukt, is er mogelijk een probleem met de configuratie van de toepassing. Kerberos-verificatie is niet werkt.

    a. Als Kerberos niet beschikbaar is, controleert u de verificatie-instellingen van de toepassing in IIS. Zorg ervoor dat **Negotiate** aan de bovenkant met NTLM onder het wordt vermeld. Als u ziet **niet onderhandelen over**, **Kerberos of Negotiate**, of **PKU2U**, alleen doorgaan als Kerberos functioneel is.

       ![Windows-verificatieproviders](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)
   
    b. Met Kerberos en NTLM in plaats tijdelijk uitschakelen vooraf-verificatie voor de toepassing in de portal. Proberen deze te benaderen via internet met behulp van de externe URL. U wordt gevraagd om te verifiëren. U kunt doen dit met hetzelfde account gebruikt in de vorige stap. Als dat niet het geval is, is er een probleem met de back-endtoepassing, niet KCD.

    c. Vooraf-verificatie in de portal opnieuw inschakelen. Verifiëren via Azure probeert verbinding maken met de App via de externe URL. Als u eenmalige aanmelding is mislukt, ziet u een niet-toegestane foutbericht in de browser en de gebeurtenis 13022 in het logboek:

       *Microsoft AAD Application Proxy Connector kan de gebruiker kan niet verifiëren omdat de back-endserver op pogingen tot Kerberos-verificatie met een 401 HTTP-fout reageert.*

       ![HTTTP 401 fout is niet toegestaan](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

    d. Controleer de IIS-toepassing. Zorg ervoor dat de geconfigureerde groep van toepassingen en de SPN-naam zijn geconfigureerd voor het gebruik van hetzelfde account in Azure AD. Navigeer in IIS, zoals wordt weergegeven in de volgende afbeelding:

       ![Venster voor configuratie van IIS-toepassing](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

       Nadat u weet dat de identiteit, zorg er dan voor dat dit account is geconfigureerd met de desbetreffende SPN. Een voorbeeld is `setspn –q http/spn.wacketywack.com`. Voer de volgende tekst in een opdrachtprompt:

       ![SetSPN-opdrachtvenster](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

    e. Controleer de SPN gedefinieerd voor de instellingen van de toepassing in de portal. Zorg ervoor dat de dezelfde SPN geconfigureerd op de doel-Azure AD-account wordt gebruikt door de toepassingsgroep app.

       ![SPN-configuratie in de Azure-portal](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)
   
    f. Ga naar de IIS- en selecteer de **configuratie-Editor** optie voor de toepassing. Navigeer naar **system.webServer/security/authentication/windowsAuthentication**. Zorg ervoor dat de waarde **UseAppPoolCredentials** is **True**.

       ![App-groepen van IIS-configuratie referentie-optie](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

       Wijzig deze waarde in **True**. Alle in de cache Kerberos-tickets verwijderen uit de back-endserver door de volgende opdracht uit te voeren:

       ```powershell
       Get-WmiObject Win32_LogonSession | Where-Object {$_.AuthenticationPackage -ne 'NTLM'} | ForEach-Object {klist.exe purge -li ([Convert]::ToString($_.LogonId, 16))}
       ``` 

Zie voor meer informatie [opschonen van de Kerberos-ticket clientcache voor alle sessies](https://gallery.technet.microsoft.com/scriptcenter/Purge-the-Kerberos-client-b56987bf).



Als u niets Kernel-modus is ingeschakeld, worden de prestaties van Kerberos-bewerkingen verbeterd. Maar het ook ervoor zorgt dat het ticket voor de aangevraagde service om te worden ontsleuteld met behulp van de computeraccount. Dit account wordt ook het lokale systeem genoemd. Deze waarde instelt op **True** KCD verbreken wanneer de toepassing wordt gehost op meer dan één server in een farm.

-   Als een aanvullende controle uitschakelen **Extended** beveiliging te. In sommige gevallen **Extended** beveiliging heeft overschreden KCD als deze optie is ingeschakeld in specifieke configuraties. In deze gevallen kan is een toepassing gepubliceerd als een submap van de standaardwebsite. Deze toepassing is geconfigureerd voor anonieme verificatie alleen. De dialoogvensters zijn niet beschikbaar, die wordt voorgesteld wouldn't actieve instellingen worden overgenomen door onderliggende objecten. Het is raadzaam dat u testen, maar vergeet niet om te herstellen van deze waarde om te **ingeschakeld**indien mogelijk.

    Deze aanvullende controle worden geplaatst op volgen om uw gepubliceerde toepassing te gebruiken. U kunt u aanvullende connectors die zijn geconfigureerd voor het delegeren draaien. Lees voor meer informatie, de uitgebreidere technische procedure [probleemoplossing van de Azure AD-toepassingsproxy](https://aka.ms/proxytshootpaper).

Als u nog steeds kan niet uitgevoerd, kan Microsoft ondersteuning u helpen. Maak een ondersteuningsticket rechtstreeks vanuit de portal. Een technicus contact met u op.

## <a name="other-scenarios"></a>Andere scenario's

- Een Kerberos-ticket aanvragen Azure toepassingsproxy voordat de aanvraag wordt verzonden naar een toepassing. Sommige toepassingen van derden zoals Tableau Server als niet deze methode te verifiëren. Deze toepassingen verwachten dat de meer traditionele onderhandelingen te kunnen uitvoeren. De eerste aanvraag is anoniem, waardoor de toepassing om te reageren met de verificatietypen dat wordt ondersteund door een 401.

- Multihop-verificatie wordt meestal gebruikt in scenario's waarbij lagen van een toepassing, met een back-end en de front-end, waarbij zowel verificatie, zoals SQL Server Reporting Services is vereist. Zie het support-artikel voor het configureren van het scenario multihop [Kerberos-beperkte overdracht kan vereisen Protocol Transition in Multihop-scenario's](https://support.microsoft.com/help/2005838/kerberos-constrained-delegation-may-require-protocol-transition-in-mul).

## <a name="next-steps"></a>Volgende stappen
[KCD configureren op een beheerd domein](../active-directory-domain-services/active-directory-ds-enable-kcd.md).
