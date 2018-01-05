---
title: Eenmalige aanmelding met toepassingsproxy | Microsoft Docs
description: Bevat informatie over het verstrekken van eenmalige aanmelding met Azure AD-toepassingsproxy.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: ded0d9c9-45f6-47d7-bd0f-3f7fd99ab621
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: 170498b2876947a45128377fa6cecb3931784237
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="kerberos-constrained-delegation-for-single-sign-on-to-your-apps-with-application-proxy"></a>Kerberos-beperkte delegatie voor eenmalige aanmelding tot uw apps met toepassingsproxy

U kunt eenmalige aanmelding bieden voor on-premises toepassingen zijn gepubliceerd via toepassingsproxy die zijn beveiligd met geïntegreerde Windows-verificatie. Deze toepassingen vereisen een Kerberos-ticket voor toegang. Toepassingsproxy maakt gebruik van Kerberos-beperkt delegatie (KCD) om deze toepassingen te ondersteunen. 

U kunt eenmalige aanmelding inschakelen voor uw toepassingen met behulp van geïntegreerde Windows-verificatie (IWA) door de toepassingsproxy connectors toestemming geven in Active Directory om gebruikers te imiteren. De connectors gebruiken deze machtiging te verzenden en ontvangen van tokens in hun naam.

## <a name="how-single-sign-on-with-kcd-works"></a>Hoe eenmalige aanmelding met KCD werkt
Dit diagram wordt de stroom uitgelegd wanneer een gebruiker probeert te krijgen tot een on-premises toepassing die gebruikmaakt van IWA.

![Stroomdiagram van Microsoft AAD-verificatie](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. De gebruiker voert de URL voor toegang tot de on-premises toepassing via toepassingsproxy.
2. Toepassingsproxy leidt de aanvraag om naar Azure AD authentication-services kunnen worden. Azure AD van toepassing op dit moment voor eventuele van toepassing verificatie en autorisatie-beleidsregels, zoals meervoudige verificatie. Als de gebruiker is gevalideerd, wordt Azure AD een token wordt gemaakt en verzendt het naar de gebruiker.
3. De gebruiker stuurt het token naar Application Proxy.
4. Toepassingsproxy valideert het token en haalt de User Principal Name (UPN) van deze en stuurt de aanvraag, de UPN en de Service Principal Name (SPN) naar de Connector via een zowel geverifieerde beveiligd kanaal.
5. De Connector voert Kerberos-beperkt delegatie (KCD)-onderhandeling met de on-premises AD, het imiteren van de gebruiker om een Kerberos-token naar de toepassing.
6. Active Directory, verzendt het Kerberos-token voor de toepassing naar de Connector.
7. De Connector verzendt de oorspronkelijke aanvraag naar de toepassingsserver, met behulp van het Kerberos-token dat wordt ontvangen van AD.
8. De toepassing verzendt het antwoord op de Connector die vervolgens wordt geretourneerd naar de service Application Proxy en ten slotte aan de gebruiker.

## <a name="prerequisites"></a>Vereisten
Voordat u aan de slag met eenmalige aanmelding voor geïntegreerde Windows-toepassingen, controleert u dat uw omgeving is klaar met de volgende instellingen en configuraties:

* Uw apps, zoals SharePoint Web-apps, worden ingesteld op het gebruik van geïntegreerde Windows-verificatie. Zie voor meer informatie [ondersteuning voor Kerberos-verificatie inschakelen](https://technet.microsoft.com/library/dd759186.aspx), of voor SharePoint-Zie [plannen voor Kerberos-verificatie in SharePoint 2013](https://technet.microsoft.com/library/ee806870.aspx).
* Alle apps hebben [Service Principal Names](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx).
* De server die de Connector en de server met de app zijn verbonden met het domein en deel uitmaken van hetzelfde domein of vertrouwde domeinen. Zie voor meer informatie over domein [een Computer toevoegen aan een domein](https://technet.microsoft.com/library/dd807102.aspx).
* De server waarop de Connector wordt uitgevoerd heeft toegang tot het lezen van het kenmerk TokenGroupsGlobalAndUniversal voor gebruikers. Deze instelling is mogelijk beïnvloed door beveiliging hardening van de omgeving.

### <a name="configure-active-directory"></a>Active Directory configureren
De Active Directory-configuratie varieert, afhankelijk van of uw Application Proxy connector en de toepassingsserver in hetzelfde domein of niet zijn.

#### <a name="connector-and-application-server-in-the-same-domain"></a>Connector en de toepassingsserver in hetzelfde domein
1. Ga in de Active Directory naar **extra** > **gebruikers en Computers**.
2. Selecteer de server waarop de connector wordt uitgevoerd.
3. Met de rechtermuisknop en selecteer **eigenschappen** > **delegering**.
4. Selecteer **deze computer mag alleen aan opgegeven services delegeren**. 
5. Onder **Services waarop dit account gedelegeerde referenties kan presenteren** de waarde voor de SPN-identiteit van de toepassingsserver toevoegen. Hierdoor kan de Connector voor toepassingsproxy om gebruikers te imiteren in AD op basis van de toepassingen die zijn gedefinieerd in de lijst.

   ![Schermopname connector SVR eigenschappen](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-application-server-in-different-domains"></a>Verbindingslijn en toepassingsservers in verschillende domeinen
1. Zie voor een lijst met vereisten voor het werken met KCD in meerdere domeinen, [Kerberos-beperkte delegatie in domeinen](https://technet.microsoft.com/library/hh831477.aspx).
2. Gebruik de `principalsallowedtodelegateto` eigenschap op de Connector-server de Proxy van de toepassing te delegeren voor de Connector-server in te schakelen. De toepassingsserver wordt `sharepointserviceaccount` en de delegatieverlenend server `connectormachineaccount`. Gebruik deze code als voorbeeld voor Windows 2012 R2:

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount

Sharepointserviceaccount mag de computeraccount SP's zijn of een serviceaccount waaronder de groep van toepassingen SP's wordt uitgevoerd.

## <a name="configure-single-sign-on"></a>Eenmalige aanmelding configureren 
1. Uw toepassing volgens de instructies die worden beschreven publiceren [publiceren van toepassingen met toepassingsproxy](application-proxy-publish-azure-portal.md). Zorg ervoor dat u selecteert **Azure Active Directory** als de **pre-authenticatie methode**.
2. Nadat uw toepassing wordt weergegeven in de lijst van zakelijke toepassingen, te selecteren en op **eenmalige aanmelding**.
3. De modus voor eenmalige aanmelding instellen op **geïntegreerde Windows-verificatie**.  
4. Voer de **interne toepassing SPN** van de toepassingsserver. In dit voorbeeld is de SPN voor de gepubliceerde toepassing http/www.contoso.com. Deze SPN moet zich in de lijst met services waarop de connector gedelegeerde referenties kan presenteren. 
5. Kies de **overgedragen aanmelding identiteit** voor de connector te gebruiken namens uw gebruikers. Zie voor meer informatie [werken met andere on-premises en cloud-identiteiten](#Working-with-different-on-premises-and-cloud-identities)

   ![Geavanceerde Toepassingsconfiguratie](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  


## <a name="sso-for-non-windows-apps"></a>Eenmalige aanmelding voor niet-Windows-apps
De Kerberos-delegatie-stroom in Azure AD-toepassingsproxy wordt gestart wanneer het Azure AD verifieert de gebruiker in de cloud. Zodra de aanvraag is ontvangen op de lokale, wordt in de Azure AD-toepassingsproxy-connector namens de gebruiker een Kerberos-ticket uitgeeft door interactie met de lokale Active Directory. Dit proces wordt aangeduid als Kerberos-beperkt delegatie (KCD). In de volgende fase wordt een aanvraag verzonden naar de back-end-toepassing met dit Kerberos-ticket. Er zijn verschillende protocollen die het verzenden van dergelijke aanvragen definiëren. De meeste niet-Windows-servers verwachten Negotiate/SPNego die nu wordt ondersteund op Azure AD-toepassingsproxy.

Zie voor meer informatie over Kerberos [alle u wilt weten over Kerberos-beperkt delegatie (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

Niet-Windows-apps doorgaans gebruiker gebruikersnamen of SAM-accountnamen in plaats van domein e-mailadressen. Als deze situatie voor uw toepassingen geldt, moet u het veld met de gedelegeerde aanmelding voor uw cloud-identiteiten verbinding met de identiteiten van uw toepassing configureren. 

## <a name="working-with-different-on-premises-and-cloud-identities"></a>Werken met andere on-premises en cloud-identiteiten
Toepassingsproxy wordt ervan uitgegaan dat gebruikers dezelfde identiteit in de cloud en on-premises hebben. Als dat niet het geval is, kunt u nog steeds KCD voor eenmalige aanmelding gebruiken. Configureer een **overgedragen aanmelding identiteit** voor elke toepassing om op te geven welke identiteit moet worden gebruikt bij het uitvoeren van eenmalige aanmelding.  

Op deze manier kunt veel organisaties die verschillende on-premises en cloud-identiteiten voor eenmalige aanmelding vanuit de cloud naar lokale apps zonder de gebruikers in te voeren voor verschillende gebruikersnamen en wachtwoorden hebben. Dit omvat organisaties die:

* Meerdere domeinen hebt intern (joe@us.contoso.com, joe@eu.contoso.com) en één domein in de cloud (joe@contoso.com).
* Niet-routeerbare domeinnaam intern hebt (joe@contoso.usa) en een geldige in de cloud.
* Domeinnamen niet intern gebruiken (Jan)
* Gebruik verschillende aliassen on-premises en in de cloud. Bijvoorbeeld: joe-johns@contoso.com vs.joej@contoso.com  

Met toepassingsproxy, kunt u selecteren welke identiteit moet worden gebruikt om de Kerberos-ticket te verkrijgen. Deze instelling is per toepassing. Sommige van deze opties zijn geschikt voor systemen die e-mailadres indeling niet accepteert, anderen zijn ontworpen voor alternatieve aanmelding.

![Schermafbeelding van de parameter gedelegeerd aanmelding identiteit](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Als gedelegeerde aanmeldings-id wordt gebruikt, de waarde mogelijk niet uniek zijn in alle domeinen of forests in uw organisatie. U kunt dit probleem voorkomen door het publiceren van deze toepassingen tweemaal met twee verschillende groepen van Connector. Omdat elke toepassing een andere gebruiker doelgroep heeft, kunt u de Connectors koppelen naar een ander domein.

### <a name="configure-sso-for-different-identities"></a>Eenmalige aanmelding configureren voor verschillende identiteiten
1. Azure AD Connect-instellingen configureren zodat de identiteit van de belangrijkste het e-mailadres (e-mail). Dit wordt gedaan als onderdeel van het proces aanpassen door het wijzigen van de **principalnaam van gebruiker** veld in de synchronisatie-instellingen. Deze instellingen bepalen ook hoe gebruikers zich aanmelden Office365, Windows10 apparaten, en andere toepassingen die gebruikmaken van Azure AD als hun identiteit-archief.  
   ![Schermafbeelding van de gebruikers - User Principal Name dropdown identificeren](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Selecteer in de toepassingsconfiguratie-instellingen voor de toepassing die u wilt wijzigen, de **overgedragen aanmelding identiteit** moet worden gebruikt:

   * User Principal Name (bijvoorbeeld joe@contoso.com)
   * Alternatieve UPN-naam (bijvoorbeeld joed@contoso.local)
   * Onderdeel van de gebruikersnaam van de User Principal Name (bijvoorbeeld Jan)
   * Onderdeel van de gebruikersnaam van alternatieve UPN-naam (bijvoorbeeld joed)
   * Lokale SAM-accountnaam (afhankelijk van de configuratie van de domeincontroller)

### <a name="troubleshooting-sso-for-different-identities"></a>Eenmalige aanmelding voor probleemoplossing voor verschillende identiteiten
Als er een fout opgetreden in het proces voor eenmalige aanmelding, deze wordt weergegeven in het gebeurtenislogboek van de connector-machine zoals toegelicht in [probleemoplossing](application-proxy-back-end-kerberos-constrained-delegation-how-to.md).
Maar in sommige gevallen kan de aanvraag is verzonden naar de back-end-toepassing terwijl deze toepassing in verschillende HTTP-antwoorden beantwoordt. Het oplossen van dergelijke gevallen moet worden gestart door te onderzoeken gebeurtenisnummer 24029 op de connector-machine in het gebeurtenislogboek voor toepassingsproxy-sessie. De identiteit van de gebruiker die is gebruikt voor de overdracht wordt weergegeven in het veld 'gebruiker' binnen de details van de gebeurtenis. Als u wilt inschakelen sessielogboek, selecteer **logboeken en foutopsporing weergeven analytische** in het menu event viewer-weergave.

## <a name="next-steps"></a>Volgende stappen

* [Het configureren van een toepassing toepassingsproxy voor het gebruik van Kerberos-beperkte overdracht](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [Oplossen van problemen met toepassingsproxy](active-directory-application-proxy-troubleshoot.md)


Ga naar het [blog over toepassingsproxy](http://blogs.technet.com/b/applicationproxyblog/) voor nieuws en updates.

