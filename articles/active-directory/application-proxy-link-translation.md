---
title: Omzetten van koppelingen en URL's Azure AD-toepassingsproxy | Microsoft Docs
description: Bevat informatie over de basisbeginselen van Azure AD-toepassingsproxy connectors.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: e2057241065ecefbbcd398ddf482961359f51de7
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Koppelingen voor apps die zijn gepubliceerd met Azure AD-toepassingsproxy hardcoded omleiden

Azure AD-toepassingsproxy stelt uw lokale apps beschikbaar voor gebruikers die extern zijn of op hun eigen apparaten. Sommige apps echter zijn ontwikkeld met lokale koppelingen ingesloten in de HTML-code. Deze koppelingen werken niet goed wanneer de app op afstand wordt gebruikt. Wanneer er meerdere on-premises toepassingen naar elkaar verwijzen, geeft uw gebruikers verwachten dat de koppelingen blijven werken wanneer ze dat niet op kantoor zijn. 

De beste manier om ervoor te zorgen dat koppelingen werken op dezelfde manier zowel binnen als buiten uw bedrijfsnetwerk noodzakelijk is voor het configureren van de externe URL's van uw apps voor dezelfde zijn als de interne URL's. Gebruik [aangepaste domeinen](active-directory-application-proxy-custom-domains.md) voor het configureren van de externe URL's om uw zakelijke domeinnaam in plaats van de proxy van het standaardtoepassingsdomein.


Als u aangepaste domeinen niet in uw tenant gebruiken, zijn er verschillende opties voor het ontwikkelen van deze functionaliteit. Al deze waarden zijn ook compatibel met aangepaste domeinen en elkaar, zodat u aangepaste domeinen en andere oplossingen indien nodig kunt configureren. 

**Optie 1: De Managed Browser gebruiken** – deze oplossing is alleen van toepassing als u van plan bent te raden of vereisen dat gebruikers toegang krijgen de toepassing via de Intune Managed Browser tot. Verwerkt alle gepubliceerde URL's. 

**Optie 2: De extensie MyApps gebruiken** : deze oplossing vereist gebruikers voor het installeren van een client-side Browseruitbreiding maar verwerkt alle gepubliceerde URL's en werkt met de meest populaire browsers. 

**Optie 3: De instelling van de vertaling koppeling gebruiken** – dit is een beheerder side-instelling die niet wordt weergegeven aan gebruikers. Dit wordt echter alleen URL's in HTML en CSS verwerkt. Vastgelegde interne URL's gegenereerd via Javascript werkt (bijvoorbeeld) niet.  

Deze drie functies houdt uw koppelingen werken ongeacht waar uw gebruikers zijn. Wanneer u apps die rechtstreeks op de interne eindpunten of poorten verwijzen hebt, kunt u deze interne URL's toewijzen aan de gepubliceerde externe toepassing Proxy URL's. 

 
> [!NOTE]
> De laatste optie is alleen voor tenants die voor welke reden dan ook niet met aangepaste domeinen hebben de dezelfde interne en externe URL's voor hun apps. Voordat u deze functie inschakelt, zien als [aangepaste domeinen in Azure AD-toepassingsproxy](active-directory-application-proxy-custom-domains.md) voor u kan doen. 

>Of, als de toepassing die u wilt configureren met een koppeling vertaling SharePoint is, raadpleegt u [alternatieve toegangstoewijzingen voor SharePoint 2013 configureren](https://technet.microsoft.com/library/cc263208.aspx) voor een andere methode om de toewijzing van koppelingen. 

 
### <a name="option-1-intune-managed-browser-integration"></a>Optie 1: De Intune Managed Browser-integratie 

De Intune Managed Browser kunt u uw toepassing en de inhoud verder te beschermen. Voor het gebruik van deze oplossing, moet u vereisen/raadzaam om gebruikers toegang tot de toepassing via de Intune Managed Browser. Alle interne URL's gepubliceerd met toepassingsproxy wordt herkend door de Managed Browser en omgeleid naar de bijbehorende externe URL. Dit zorgt ervoor dat alle vastgelegde interne URL's werken en als een gebruiker, gaat u naar de browser en de interne URL rechtstreeks van het type, het werkt zelfs als de gebruiker extern is.  

Raadpleeg voor meer informatie, inclusief het configureren van deze optie de [Managed Browser](https://docs.microsoft.com/intune/app-configuration-managed-browser) documentatie.  

### <a name="option-2-myapps-browser-extension"></a>Optie 2: MyApps Browseruitbreiding 

Met de extensie van de Browser MyApps alle interne URL's gepubliceerd met toepassingsproxy herkend door de uitbreiding en omgeleid naar de bijbehorende externe URL. Dit zorgt ervoor dat alle vastgelegde interne URL's werken en als een gebruiker naar de adresbalk van de browser overschakelt en de interne URL rechtstreeks van het type, het werkt zelfs als de gebruiker extern is.  

Deze functie wilt gebruiken, moet de gebruiker downloaden van de extensie en zijn aangemeld. Er is geen andere configuratie die nodig zijn voor beheerders of gebruikers. 

 

### <a name="option-3-link-translation-setting"></a>Optie 3: Koppeling vertaling instelling 

Wanneer de vertaling van de koppeling is ingeschakeld, wordt de service voor toepassingsproxy HTML en CSS doorzocht voor gepubliceerde interne koppelingen en zet deze zodat uw gebruikers beschikken over een ononderbroken ervaring. 



## <a name="how-link-translation-works"></a>Hoe werkt de vertaling koppelen

Na verificatie als de proxyserver wordt de toepassingsgegevens aan de gebruiker doorgegeven toepassingsproxy scant de toepassing voor hardcoded koppelingen en vervangen door hun respectieve externe URL's gepubliceerd.

Toepassingsproxy wordt ervan uitgegaan dat de toepassingen zijn gecodeerd in UTF-8. Als dit niet het geval is, geef het type codering in een http-antwoordheader zoals `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Welke koppelingen worden beïnvloed?

De functie van de vertaling koppeling wordt alleen gezocht voor koppelingen die in de code-codes in de hoofdtekst van een app. Toepassingsproxy heeft een afzonderlijke functie voor het omzetten van cookies of URL's in de headers. 

Er zijn twee algemene typen interne koppelingen in on-premises toepassingen:

- **Relatieve interne koppelingen** dat punt met een gedeelde bron in de structuur van een lokaal bestand, zoals `/claims/claims.html`. Deze koppelingen werken automatisch in apps die zijn gepubliceerd via toepassingsproxy en blijven werken met of zonder de vertaling van koppelingen. 
- **Interne koppelingen hardcoded** naar andere apps lokale zoals `http://expenses` of bestanden zoals gepubliceerde `http://expenses/logo.jpg`. De functie van de vertaling koppeling werkt op interne koppelingen vastgelegd en kunnen worden aangepast om te verwijzen naar de externe URL's die externe gebruikers moeten doorlopen.

### <a name="how-do-apps-link-to-each-other"></a>Hoe kunnen apps met elkaar koppelen?

De vertaling van de koppeling is ingeschakeld voor elke toepassing, zodat u controle over de ervaring van de gebruiker op het niveau voor de per-app hebt. De vertaling van de koppeling voor een app inschakelen als u wilt dat de koppelingen *van* die app om te zetten, is niet gekoppeld *naar* die app. 

Stel bijvoorbeeld dat u hebt drie toepassingen die zijn gepubliceerd via toepassingsproxy alle aan elkaar koppelen: voordelen, kosten en reizen. Er is een vierde Feedback, die niet is gepubliceerd via toepassingsproxy-app.

Wanneer u de vertaling van koppelingen voor de app voordelen inschakelt, worden de koppelingen naar uitgaven en reizen worden omgeleid naar de externe URL's voor deze apps, maar de koppeling om Feedback omdat er geen externe URL niet wordt omgeleid. Koppelingen van kosten en reizen terug naar voordelen werken niet, omdat de vertaling van de koppeling is niet ingeschakeld voor deze twee apps.

![Voordelen koppelingen naar andere apps wanneer de vertaling van koppelingen is ingeschakeld](./media/application-proxy-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Welke koppelingen worden niet vertaald?

Ter verbetering van prestaties en beveiliging, is enkele koppelingen zijn niet geconverteerd:

- De koppelingen niet binnen de codetags. 
- Koppelingen niet in de HTML-indeling of CSS. 
- Interne koppelingen geopend vanuit andere programma's. Koppelingen verzonden via e-mail of chatbericht of in andere documenten opgenomen won't worden vertaald. De gebruikers moeten weten om naar de externe URL te gaan.

Als u moet voor de ondersteuning van een van deze twee scenario's, gebruikt u dezelfde interne en externe URL's in plaats van de vertaling van koppelingen.  

## <a name="enable-link-translation"></a>Link-omzetting inschakelen

Aan de slag met de vertaling van koppelingen is net zo eenvoudig als het klikken op een knop:

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen** > Selecteer de app die u wilt beheren >  **Toepassingsproxy**.
3. Schakel **vertalen URL's in de hoofdtekst van de toepassing** naar **Ja**.

   ![Selecteer Ja als u wilt converteren van URL's in de hoofdtekst van de toepassing](./media/application-proxy-link-translation/select_yes.png).
4. Selecteer **opslaan** uw wijzigingen toe te passen.

Nu wanneer uw gebruikers toegang krijgen deze toepassing tot, scant de proxy automatisch voor interne URL's die zijn gepubliceerd via toepassingsproxy van uw tenant.

## <a name="send-feedback"></a>Feedback verzenden

We willen uw waarmee u deze functie werkt voor al uw apps. We zoeken op meer dan 30 labels in HTML en CSS. Hebt u een voorbeeld van de gegenereerde koppelingen die niet zijn wordt vertaald, verzendt een codefragment aan [Application Proxy Feedback](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Volgende stappen
[Aangepaste domeinen gebruiken met Azure AD-toepassingsproxy](active-directory-application-proxy-custom-domains.md) in dezelfde interne en externe URL

[Alternatieve toegangstoewijzingen voor SharePoint 2013 configureren](https://technet.microsoft.com/library/cc263208.aspx)
