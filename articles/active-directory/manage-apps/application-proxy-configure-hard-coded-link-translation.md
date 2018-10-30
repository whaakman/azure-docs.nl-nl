---
title: Koppelingen en URL's Azure AD App Proxy vertalen | Microsoft Docs
description: Bevat informatie over de basisbeginselen van Azure AD Application Proxy connectors.
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
ms.date: 05/04/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 73854cba151dfbebe53862a39fbe980502192c2d
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230060"
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Koppelingen naar de vastgelegd voor apps die zijn gepubliceerd met Azure AD Application Proxy omleiden

Azure AD-toepassingsproxy stelt uw on-premises toepassingen beschikbaar voor gebruikers die extern zijn, of op hun eigen apparaten. Sommige apps, maar zijn ontwikkeld met lokale koppelingen die zijn ingesloten in de HTML-code. Deze koppelingen werken niet goed als de app op afstand wordt gebruikt. Wanneer u verschillende on-premises toepassingen naar elkaar verwijzen hebt, verwachten uw gebruikers de koppelingen blijven werken wanneer ze niet op kantoor. 

De beste manier om ervoor te zorgen dat koppelingen werken op dezelfde manier binnen en buiten uw bedrijfsnetwerk, is het configureren van de externe URL's van uw apps om niet dezelfde zijn als de interne URL's. Gebruik [aangepaste domeinen](application-proxy-configure-custom-domain.md) het configureren van de externe URL's om uw zakelijke domeinnaam in plaats van het standaarddomein voor de proxy van toepassing.


Als u aangepaste domeinen niet in uw tenant gebruiken, zijn er verschillende opties om deze functionaliteit te bieden. Al deze waarden zijn ook compatibel met aangepaste domeinen en elkaar worden verbonden, zodat u aangepaste domeinen en andere oplossingen indien nodig kunt configureren. 

**Optie 1: De Managed Browser gebruiken** – deze oplossing is alleen van toepassing als u van plan bent om te aan te bevelen of vereisen dat gebruikers toegang krijgen de toepassing via de Intune Managed Browser tot. Verwerkt alle gepubliceerde URL's. 

**Optie 2: De MyApps-extensie gebruiken** : deze oplossing vereist dat gebruikers een van client-side-browserextensie installeren, maar dit wordt verwerkt alle gepubliceerde URL's en werkt met de meest populaire browsers. 

**Optie 3: De instelling van de vertaling koppeling gebruiken** : dit is een beheerder side-instelling die is zichtbaar voor gebruikers. Dit wordt echter alleen URL's in HTML en CSS verwerkt. Vastgelegde interne URL's die worden gegenereerd via Javascript werkt (bijvoorbeeld) niet.  

Deze drie functies houdt uw koppelingen werken ongeacht waar uw gebruikers zich bevinden. Wanneer u apps die rechtstreeks naar interne eindpunten of -poorten verwijzen hebt, kunt u deze interne URL's toewijzen aan de gepubliceerde externe Application Proxy-URL's. 

 
> [!NOTE]
> De laatste optie is alleen voor tenants die voor welke reden dan ook aangepaste domeinen niet kunnen gebruiken om de dezelfde interne en externe URL's voor hun apps. Voordat u deze functie inschakelt, zien als [aangepaste domeinen in Azure AD-toepassingsproxy](application-proxy-configure-custom-domain.md) voor u kan betekenen. 

>Of, als de toepassing die u wilt configureren met koppeling vertaling SharePoint is, raadpleegt u [alternatieve toegangstoewijzingen voor SharePoint 2013 configureren](https://technet.microsoft.com/library/cc263208.aspx) voor een andere benadering voor de toewijzing van koppelingen. 

 
### <a name="option-1-intune-managed-browser-integration"></a>Optie 1: Intune Managed Browser-integratie 

U kunt de Intune Managed Browser gebruiken verder beschermen van uw toepassing en de inhoud. Als u wilt deze oplossing gebruikt, moet u vereisen/raadzaam om gebruikers toegang tot de toepassing via de Intune Managed Browser. Alle interne URL's die zijn gepubliceerd met toepassingsproxy worden herkend door de Managed Browser en omgeleid naar de bijbehorende externe URL. Dit zorgt ervoor dat alle vastgelegde interne URL's werken, en als een gebruiker, wordt omgeleid naar de browser en de interne URL rechtstreeks van het type, het werkt, zelfs als de gebruiker extern is.  

Raadpleeg voor meer informatie, inclusief hoe u deze optie configureert de [Managed Browser](https://docs.microsoft.com/intune/app-configuration-managed-browser) documentatie.  

### <a name="option-2-myapps-browser-extension"></a>Optie 2: MyApps browserextensie 

Met de extensie van de Browser MyApps zijn alle interne URL's die zijn gepubliceerd met toepassingsproxy wordt herkend door de extensie en omgeleid naar de bijbehorende externe URL. Dit zorgt ervoor dat alle vastgelegde interne URL's werken, en als een gebruiker, wordt omgeleid naar de adresbalk van de browser en de interne URL rechtstreeks van het type, het werkt, zelfs als de gebruiker extern is.  

Deze functie wilt gebruiken, wordt de gebruiker moet voor het downloaden van de extensie en zijn aangemeld. Er is geen andere configuratie die nodig zijn voor beheerders of gebruikers. 

 

### <a name="option-3-link-translation-setting"></a>Optie 3: Koppeling NAT-instelling 

Als koppeling NAT is ingeschakeld, wordt de service voor toepassingsproxy HTML en CSS doorzocht voor gepubliceerde interne koppelingen en zet deze zodat uw gebruikers een ononderbroken ervaring krijgen. 



## <a name="how-link-translation-works"></a>Hoe werkt vertaling koppelen

Na verificatie als de proxyserver wordt de toepassingsgegevens aan de gebruiker doorgegeven, Application Proxy scant de aanvraag voor vastgelegde koppelingen en vervangen door hun respectieve externe URL's gepubliceerd.

De toepassingsproxy wordt ervan uitgegaan dat de toepassingen zijn gecodeerd in UTF-8. Als dat niet het geval is, geef het type codering in een http-antwoordheader zoals `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Welke koppelingen worden beïnvloed?

De functie van de vertaling koppeling zoekt alleen naar koppelingen die in de code-codes in de hoofdtekst van een app. Application Proxy heeft een afzonderlijke functie voor het omzetten van cookies of URL's in de headers. 

Er zijn twee algemene typen van interne koppelingen in on-premises toepassingen:

- **Relatieve interne koppelingen** dat punt met een gedeelde bron in de structuur van een lokaal bestand, zoals `/claims/claims.html`. Deze koppelingen worden automatisch in apps die worden gepubliceerd via toepassingsproxy en blijven werken met of zonder de vertaling van koppelingen werken. 
- **Interne koppelingen vastgelegd** naar andere on-premises toepassingen, zoals `http://expenses` of bestanden, zoals gepubliceerd `http://expenses/logo.jpg`. De functie van de vertaling koppeling werkt op interne koppelingen vastgelegd, en kunnen worden aangepast om te verwijzen naar de externe URL's die externe gebruikers moeten doorlopen.

### <a name="how-do-apps-link-to-each-other"></a>Hoe apps koppelen aan elkaar?

Koppeling vertaling is ingeschakeld voor elke toepassing die u controle over de gebruikerservaring op het niveau van de per-app hebt. Koppeling vertaling voor een app inschakelen als u wilt dat de koppelingen *van* die app worden omgezet, is niet gekoppeld *naar* die app. 

Stel bijvoorbeeld dat u hebt drie toepassingen die zijn gepubliceerd via toepassingsproxy dat alle aan elkaar koppelen: voordelen, kosten en reizen. Er is een vierde app, Feedback, die niet worden gepubliceerd via toepassingsproxy.

Wanneer u een koppeling vertaling voor de app voordelen inschakelt, wordt de koppeling naar de kosten en reizen worden omgeleid naar de externe URL's voor deze apps, maar de koppeling naar Feedback niet wordt omgeleid omdat er geen externe URL. Koppelingen van uitgaven en reizen naar voordelen werkt niet, omdat de koppeling vertaling is niet ingeschakeld voor deze twee apps.

![Koppelingen van voordelen naar andere apps als koppeling NAT is ingeschakeld](./media/application-proxy-configure-hard-coded-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Welke koppelingen worden niet vertaald?

Ter verbetering van prestaties en beveiliging, niet worden enkele koppelingen die geconverteerd:

- De koppelingen niet in de codetags. 
- De koppelingen niet in HTML of CSS. 
- Koppelingen in de URL-notatie.
- Interne koppelingen geopend vanuit andere programma's. Koppelingen verzonden via e-mail of chatbericht of opgenomen in andere documenten wordt niet worden vertaald. De gebruikers moeten weten om naar de externe URL te gaan.

Als u nodig hebt ter ondersteuning van een van deze twee scenario's, gebruikt u dezelfde interne en externe URL's in plaats van de vertaling van koppelingen.  

## <a name="enable-link-translation"></a>Link-omzetting inschakelen

Aan de slag met koppeling vertaling is net zo gemakkelijk als het op een knop te klikken:

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen** > Selecteer de app die u wilt beheren >  **Toepassingsproxy**.
3. Schakel **URL's vertalen in de hoofdtekst van de toepassing** naar **Ja**.

   ![Selecteer Ja om te vertalen van URL's in de hoofdtekst van de toepassing](./media/application-proxy-configure-hard-coded-link-translation/select_yes.png)
4. Selecteer **opslaan** de wijzigingen worden toegepast.

Nu, als uw gebruikers toegang hebben tot deze toepassing, wordt de proxy worden automatisch voor interne URL's die zijn gepubliceerd via toepassingsproxy op uw tenant gescand.

## <a name="send-feedback"></a>Feedback verzenden

We willen uw waarmee u deze functie werkt voor al uw apps. We zoeken naar meer dan 30 tags in HTML en CSS. Hebt u een voorbeeld van gegenereerde koppelingen die worden niet worden omgezet, verzendt u een codefragment aan [Proxy toepassingsfeedback](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Volgende stappen
[Aangepaste domeinen gebruiken met Azure AD-toepassingsproxy](application-proxy-configure-custom-domain.md) hebben dezelfde interne en externe URL

[Alternatieve toegangstoewijzingen voor SharePoint 2013 configureren](https://technet.microsoft.com/library/cc263208.aspx)
