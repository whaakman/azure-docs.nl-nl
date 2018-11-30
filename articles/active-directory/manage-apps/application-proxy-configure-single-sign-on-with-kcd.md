---
title: Eenmalige aanmelding met Application Proxy | Microsoft Docs
description: Bevat informatie over het bieden van eenmalige aanmelding met Azure AD-toepassingsproxy.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: dbb7fca08e8d5c59842f7ddfb02b463a42efadb7
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422280"
---
# <a name="kerberos-constrained-delegation-for-single-sign-on-to-your-apps-with-application-proxy"></a>Beperkte Kerberos-delegering voor single sign-on bij uw apps met Application Proxy

U kunt eenmalige aanmelding bieden voor on-premises toepassingen worden gepubliceerd via toepassingsproxy die zijn beveiligd met geïntegreerde Windows-verificatie. Deze toepassingen vereisen een Kerberos-ticket voor toegang. Application Proxy maakt gebruik van Kerberos-beperkte delegatie (KCD) om deze toepassingen te ondersteunen. 

U kunt eenmalige aanmelding inschakelen voor uw toepassingen met behulp van geïntegreerde Windows-verificatie (IWA) door middel van een Application Proxy connectors machtiging in Active Directory gebruikers te imiteren. De connectors gebruiken deze machtiging om te verzenden en ontvangen van tokens in hun naam.

## <a name="how-single-sign-on-with-kcd-works"></a>Hoe eenmalige aanmelding met KCD werkt
In dit diagram wordt de stroom uitgelegd wanneer een gebruiker probeert te krijgen tot een on-premises toepassing die gebruikmaakt van IWA.

![Stroomdiagram van Microsoft AAD-verificatie](./media/application-proxy-configure-single-sign-on-with-kcd/AuthDiagram.png)

1. De gebruiker voert de URL voor toegang tot de on-premises toepassing via Application Proxy.
2. Application Proxy stuurt de aanvraag naar Azure AD authentication-services kunnen worden. Azure AD van toepassing op dit moment voor alle van toepassing verificatie en autorisatiebeleid, zoals meervoudige verificatie. Als de gebruiker is gevalideerd, wordt Azure AD wordt een token gemaakt en verzendt ze naar de gebruiker.
3. De gebruiker wordt het token doorgegeven aan Application Proxy.
4. Valideert het token Application Proxy en de User Principal Name (UPN) opgehaald uit het en stuurt de aanvraag, de UPN en de Service Principal Name (SPN) naar de Connector via een beveiligd kanaal voor zowel geverifieerde.
5. De Connector voert Kerberos-beperkte delegatie (KCD)-onderhandeling met de on-premises AD, het imiteren van de gebruiker om op te halen van een Kerberos-token naar de toepassing.
6. Active Directory, verzendt de Kerberos-token voor de toepassing naar de Connector.
7. De Connector verzendt de oorspronkelijke aanvraag naar de application server, met behulp van het Kerberos-token dat het ontvangen van AD.
8. De toepassing stuurt het antwoord op de Connector, die vervolgens wordt geretourneerd naar de Application Proxy-service en ten slotte naar de gebruiker.

## <a name="prerequisites"></a>Vereisten
Voordat u aan de slag met eenmalige aanmelding voor geïntegreerde Windows-toepassingen, zorg er dan voor dat uw omgeving klaar is met de volgende instellingen en configuraties is:

* Uw apps, zoals SharePoint Web-apps, zijn ingesteld op het gebruik van geïntegreerde Windows-verificatie. Zie voor meer informatie, [ondersteuning voor Kerberos-verificatie inschakelen](https://technet.microsoft.com/library/dd759186.aspx), of voor SharePoint Zie [plannen voor Kerberos-verificatie in SharePoint 2013](https://technet.microsoft.com/library/ee806870.aspx).
* Al uw apps hebt [Service Principal Names](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx).
* De server waarop de Connector wordt uitgevoerd en de server met de app zijn toegevoegd aan een domein en deel uitmaken van hetzelfde domein of vertrouwde domeinen. Zie voor meer informatie over het koppelen van domeinen, [een Computer toevoegen aan een domein](https://technet.microsoft.com/library/dd807102.aspx).
* De server waarop de Connector wordt uitgevoerd heeft toegang tot het lezen van het kenmerk TokenGroupsGlobalAndUniversal voor gebruikers. Deze standaardinstelling is mogelijk beïnvloed door het beperken van de omgeving.

### <a name="configure-active-directory"></a>Active Directory configureren
De Active Directory-configuratie varieert, afhankelijk van of de toepassingsserver en de Application Proxy-connector zich in hetzelfde domein of niet.

#### <a name="connector-and-application-server-in-the-same-domain"></a>Connector en toepassingsserver in hetzelfde domein
1. In Active Directory, gaat u naar **extra** > **: gebruikers en Computers**.
2. Selecteer de server waarop de connector wordt uitgevoerd.
3. Met de rechtermuisknop op en selecteer **eigenschappen** > **delegering**.
4. Selecteer **deze computer mag alleen aan opgegeven services delegeren**. 
5. Onder **Services waarop dit account gedelegeerde referenties kan presenteren** de waarde voor de identiteit van de SPN-naam van de toepassingsserver toevoegen. Hierdoor kunnen de Connector voor toepassingsproxy te imiteren van gebruikers in AD op basis van de toepassingen die zijn gedefinieerd in de lijst.

   ![Schermafbeelding van de eigenschappen van de connector-Server-venster](./media/application-proxy-configure-single-sign-on-with-kcd/Properties.jpg)

#### <a name="connector-and-application-server-in-different-domains"></a>Connector en toepassingsserver in verschillende domeinen bevinden
1. Zie voor een lijst met vereisten voor het werken met KCD in meerdere domeinen, [Kerberos-beperkte delegatie in domeinen](https://technet.microsoft.com/library/hh831477.aspx).
2. Gebruik de `principalsallowedtodelegateto` eigenschap op de Connector-server de Proxy-toepassing om te delegeren voor de Connector-server in te schakelen. De toepassingsserver wordt `sharepointserviceaccount` en de delegatieverlenend server `connectormachineaccount`. Gebruik deze code als voorbeeld voor Windows 2012 R2:

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount

Sharepointserviceaccount mag het computeraccount SP's of een service-account waarmee de app-pool van SP's wordt uitgevoerd.

## <a name="configure-single-sign-on"></a>Eenmalige aanmelding configureren 
1. Publiceer de toepassing op basis van de instructies die worden beschreven [publiceren van toepassingen met toepassingsproxy](application-proxy-publish-azure-portal.md). Zorg ervoor dat u selecteert **Azure Active Directory** als de **vooraf-verificatie methode**.
2. Nadat uw toepassing wordt weergegeven in de lijst van zakelijke toepassingen, selecteert u deze en klikt u op **eenmalige aanmelding**.
3. De modus voor één aanmelding instellen op **geïntegreerde Windows-verificatie**.  
4. Voer de **SPN voor interne toepassing** van de toepassingsserver. In dit voorbeeld is de SPN voor de gepubliceerde toepassing http/www.contoso.com. Deze SPN moet zich in de lijst met services die de connector gedelegeerde referenties kan presenteren. 
5. Kies de **gedelegeerde Aanmeldingsidentiteit** voor de connector te gebruiken namens uw gebruikers. Zie voor meer informatie, [werken met verschillende on-premises en cloud-identiteiten](#Working-with-different-on-premises-and-cloud-identities)

   ![Geavanceerde configuratie](./media/application-proxy-configure-single-sign-on-with-kcd/cwap_auth2.png)  


## <a name="sso-for-non-windows-apps"></a>SSO voor niet-Windows-apps

De stroom van de Kerberos-delegering in Azure AD-toepassingsproxy wordt gestart wanneer Azure AD de gebruiker in de cloud verifieert. Zodra de aanvraag is ontvangen on-premises, wordt in de Azure AD Application Proxy-connector een Kerberos-ticket namens de gebruiker door interactie met de lokale Active Directory uitgeeft. Dit proces wordt aangeduid als Kerberos-beperkte delegatie (KCD). In de volgende fase wordt een aanvraag verzonden naar de back-endtoepassing met dit Kerberos-ticket. 

Er zijn verschillende protocollen die definiëren hoe om dergelijke aanvragen te verzenden. De meeste niet-Windows-servers verwacht om te onderhandelen over met SPNEGO. Dit protocol wordt ondersteund op Azure AD-toepassingsproxy, maar is standaard uitgeschakeld. Een server kan worden geconfigureerd voor het SPNEGO- of standard KCD, maar niet allebei.

Als u een connector machine voor SPNEGO configureert, zorg ervoor dat alle andere connectors in die groep Connector ook worden geconfigureerd met SPNEGO. Standard KCD verwacht toepassingen moeten worden gerouteerd via andere connectors die niet zijn geconfigureerd voor SPNEGO.
 

SPNEGO inschakelen:

1. Open een opdrachtprompt die wordt uitgevoerd als beheerder.
2. Voer de volgende opdrachten op de Connectorservers waarvoor SPNEGO vanaf de opdrachtprompt.

    ```
    REG ADD "HKLM\SOFTWARE\Microsoft\Microsoft AAD App Proxy Connector" /v UseSpnegoAuthentication /t REG_DWORD /d 1
    net stop WAPCSvc & net start WAPCSvc
    ```

Zie voor meer informatie over Kerberos, [alle u wilt weten over Kerberos-beperkte delegatie (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

Niet-Windows-apps doorgaans gebruiker gebruikersnamen of namen van de SAM-account in plaats van dat e-mailadressen. Als deze situatie is van toepassing op uw toepassingen, moet u de gedelegeerde login ID-veld voor de verbinding van uw cloud-identiteiten met de identiteiten van uw toepassing configureren. 

## <a name="working-with-different-on-premises-and-cloud-identities"></a>Werken met verschillende on-premises en cloud-identiteiten
De toepassingsproxy wordt ervan uitgegaan dat gebruikers dezelfde identiteit in de cloud en on-premises. Als dat niet het geval is, kunt u nog steeds KCD gebruiken voor eenmalige aanmelding. Configureer een **gedelegeerde Aanmeldingsidentiteit** voor elke toepassing om op te geven welke identiteit moet worden gebruikt bij het uitvoeren van eenmalige aanmelding.  

Op deze manier kunt veel organisaties die beschikken over verschillende on-premises en cloud-identiteiten voor eenmalige aanmelding vanuit de cloud naar on-premises toepassingen zonder de gebruikers verschillende gebruikersnamen en wachtwoorden in te voeren. Dit omvat organisaties die:

* Hebt u meerdere domeinen intern (joe@us.contoso.com, joe@eu.contoso.com) en één domein in de cloud (joe@contoso.com).
* Niet-routeerbare domeinnaam intern hebben (joe@contoso.usa) en een juridische in de cloud.
* Domeinnamen niet intern gebruiken (Jaap)
* Gebruik verschillende aliassen on-premises en in de cloud. Bijvoorbeeld, joe-johns@contoso.com vs. joej@contoso.com  

Met Application Proxy, kunt u selecteren welke identiteit gebruiken om op te halen van het Kerberos-ticket. Deze instelling is per toepassing. Sommige van deze opties zijn geschikt voor systemen die e-mailindeling niet accepteert, anderen zijn ontworpen voor de alternatieve aanmelding.

![Schermafbeelding van de parameter gedelegeerde aanmelding identiteit](./media/application-proxy-configure-single-sign-on-with-kcd/app_proxy_sso_diff_id_upn.png)

Als gedelegeerde Aanmeldingsidentiteit wordt gebruikt, kan de waarde niet uniek zijn in alle domeinen of forests in uw organisatie. U kunt dit probleem voorkomen door het publiceren van deze twee keer met behulp van twee verschillende connectorgroepen toepassingen. Omdat elke toepassing een andere gebruiker publiek heeft, kunt u de Connectors toevoegen aan een ander domein.

### <a name="configure-sso-for-different-identities"></a>Eenmalige aanmelding configureren voor verschillende identiteiten
1. Azure AD Connect-instellingen configureren, zodat de identiteit van de belangrijkste het e-mailadres (e-mail is). Dit wordt gedaan als onderdeel van het proces aanpassen door het veranderen van de **User Principal Name** veld in de synchronisatie-instellingen. Deze instellingen ook bepalen hoe gebruikers aanmelden bij Office 365, Windows10 apparaten, en andere toepassingen die gebruikmaken van Azure AD als hun identiteitenarchief.  
   ![Schermafbeelding van de gebruikers - User Principal Name vervolgkeuzelijst identificeren](./media/application-proxy-configure-single-sign-on-with-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Selecteer in de toepassingsconfiguratie-instellingen voor de toepassing die u wilt wijzigen, de **gedelegeerde Aanmeldingsidentiteit** moet worden gebruikt:

   * User Principal Name (bijvoorbeeld joe@contoso.com)
   * Alternatieve UPN-naam (bijvoorbeeld joed@contoso.local)
   * Deel van de User Principal Name (bijvoorbeeld: Jaap) gebruikersnaam
   * Onderdeel van de gebruikersnaam van de alternatieve User Principal Name (bijvoorbeeld joed)
   * On-premises SAM-accountnaam is (afhankelijk van de configuratie van de domeincontroller)

### <a name="troubleshooting-sso-for-different-identities"></a>Oplossen van problemen met eenmalige aanmelding voor verschillende identiteiten
Als er een fout is opgetreden in het proces voor eenmalige aanmelding, deze wordt weergegeven in het gebeurtenislogboek van de connector-computer, zoals wordt beschreven [probleemoplossing](application-proxy-back-end-kerberos-constrained-delegation-how-to.md).
Maar in sommige gevallen kan de aanvraag is verzonden naar de back-endtoepassing terwijl antwoorden op deze toepassing in verschillende HTTP-antwoorden. Het oplossen van dergelijke gevallen moet beginnen door te controleren gebeurtenisnummer 24029 op de connector-machine in het gebeurtenislogboek van de Application Proxy-sessie. De identiteit van de gebruiker die is gebruikt voor overdracht wordt weergegeven in het veld 'gebruiker' in de details van de gebeurtenis. Als u wilt inschakelen sessielogboek, selecteer **logboeken en foutopsporing weergeven analytische** in het menu Beeld van event viewer.

## <a name="next-steps"></a>Volgende stappen

* [Een Application Proxy-toepassing voor het gebruik van Kerberos-beperkte overdracht configureren](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [Oplossen van problemen met Application Proxy](application-proxy-troubleshoot.md)


Ga naar het [blog over toepassingsproxy](https://blogs.technet.com/b/applicationproxyblog/) voor nieuws en updates.

