---
title: Omzetten van koppelingen en URL's Azure AD-toepassingsproxy | Microsoft Docs
description: Bevat informatie over de basisbeginselen van Azure AD-toepassingsproxy connectors.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: e4d143b4937a1f6c1c21783ae357dbe617816e73
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Koppelingen voor apps die zijn gepubliceerd met Azure AD-toepassingsproxy hardcoded omleiden

Azure AD-toepassingsproxy stelt uw lokale apps beschikbaar voor gebruikers die extern zijn of op hun eigen apparaten. Sommige apps echter zijn ontwikkeld met lokale koppelingen ingesloten in de HTML-code. Deze koppelingen werken niet goed wanneer de app op afstand wordt gebruikt. Wanneer er meerdere on-premises toepassingen naar elkaar verwijzen, geeft uw gebruikers verwachten dat de koppelingen blijven werken wanneer ze dat niet op kantoor zijn. 

De beste manier om ervoor te zorgen dat koppelingen werken op dezelfde manier zowel binnen als buiten uw bedrijfsnetwerk noodzakelijk is voor het configureren van de externe URL's van uw apps voor dezelfde zijn als de interne URL's. Gebruik [aangepaste domeinen](active-directory-application-proxy-custom-domains.md) voor het configureren van de externe URL's om uw zakelijke domeinnaam in plaats van de proxy van het standaardtoepassingsdomein.

Als u aangepaste domeinen niet in uw tenant gebruiken, houdt uw koppelingen werken ongeacht waar uw gebruikers zich met de functie koppeling vertaling van Application Proxy. Wanneer u apps die rechtstreeks op de interne eindpunten of poorten verwijzen hebt, kunt u deze interne URL's toewijzen aan de gepubliceerde externe toepassing Proxy URL's. Wanneer de vertaling van koppelingen is ingeschakeld en toepassingsproxy doorzoekt HTML, CSS en JavaScript-select-tags voor gepubliceerde interne koppelingen. Vervolgens zet de service voor toepassingsproxy deze om zodat uw gebruikers beschikken over een ononderbroken ervaring.

>[!NOTE]
>De koppeling NAT-functie is voor tenants die voor welke reden dan ook niet met aangepaste domeinen hebben de dezelfde interne en externe URL's voor hun apps. Voordat u deze functie inschakelt, zien als [aangepaste domeinen in Azure AD-toepassingsproxy](active-directory-application-proxy-custom-domains.md) voor u kan doen.
>
>Of, als de toepassing die u wilt configureren met een koppeling vertaling SharePoint is, raadpleegt u [alternatieve toegangstoewijzingen voor SharePoint 2013 configureren](https://technet.microsoft.com/library/cc263208.aspx) voor een andere methode om de toewijzing van koppelingen.

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
- Koppelingen niet in HTML, CSS of JavaScript. 
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

We willen uw waarmee u deze functie werkt voor al uw apps. We zoeken naar meer dan 30 labels in HTML en CSS en overweegt die JavaScript aanvragen om te ondersteunen. Hebt u een voorbeeld van de gegenereerde koppelingen die niet zijn wordt vertaald, verzendt een codefragment aan [Application Proxy Feedback](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Volgende stappen
[Aangepaste domeinen gebruiken met Azure AD-toepassingsproxy](active-directory-application-proxy-custom-domains.md) in dezelfde interne en externe URL

[Alternatieve toegangstoewijzingen voor SharePoint 2013 configureren](https://technet.microsoft.com/library/cc263208.aspx)
