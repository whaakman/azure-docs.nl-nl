<properties
    pageTitle="Azure AD Connect in Microsoft Cloud Duitsland"
    description="Azure AD Connect integreert uw on-premises directory's met Azure Active Directory. Hiermee kunt u een algemene identiteit bieden voor Office 365, Azure en SaaS toepassingen die zijn geïntegreerd met Azure AD."
    keywords="inleiding tot Azure AD Connect, overzicht Azure AD Connect, wat is Azure AD Connect, Active Directory installeren, Duitsland, Zwarte Woud"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>


#Azure AD Connect in Microsoft Cloud Duitsland - Openbare preview

## Inleiding
Azure AD Connect voorziet in synchronisatie tussen uw on-premises Active Directory en Azure Active Directory.
Op dit moment moeten veel van de scenario's in [Microsoft Cloud Duitsland](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) worden uitgevoerd door de operator. Wanneer u Microsoft Cloud Duitsland gebruikt, moet u zich van de volgende zaken bewust zijn:


- De volgende URL's moeten via een proxyserver worden geopend als u wilt dat de synchronisatie succesvol verloopt:
    - *.microsoftonline.de
    - *.windows.net
    - + Certificaatintrekkingslijsten

- Wanneer u zich bij uw Azure AD-directory aanmeldt, moet u een account uit het domein onmicrosoft.de gebruiken.
- De volgende functies zijn niet beschikbaar:
    - Azure AD Connect Health (Engelstalig)
    - Automatische updates
    - Wachtwoord terugschrijven

## Download
U kunt Azure AD Connect downloaden via de Azure AD Connect-blade in de portal.  Volg de onderstaande instructies om de Azure AD Connect-blade te vinden.

### De Azure AD Connect-blade

Wanneer u zich bij Azure Portal hebt aangemeld, doet u het volgende:

1. Ga naar Bladeren
2.  Selecteer Azure Active Directory
3.  Selecteer vervolgens Azure AD Connect

U ziet nu het volgende:

![De Azure AD Connect-blade](media\active-directory-aadconnect-germany\germany1.png)

 
In de volgende tabel staan welke functies worden weergegeven in de blade.


Titel|Beschrijving|
----- | ----- |
SYNCHRONISATIESTATUS|Hier ziet u of synchronisatie is in- of uitgeschakeld.|
LAATSTE SYNCHRONISATIE|De laatste keer dat een synchronisatie is voltooid.|
FEDERATIEVE DOMEINEN|Hier ziet u hoeveel federatieve domeinen er momenteel zijn geconfigureerd.|


## Installatie
Als u Azure AD Connect wilt installeren, kunt u [deze](active-directory-aadconnect.md#install-azure-ad-connect) documentatie gebruiken.

## Geavanceerde functies en aanvullende informatie
Zie [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md) voor meer informatie en tips over aangepaste instellingen en geavanceerde configuraties.  Deze pagina bevat informatie over en koppelingen naar aanvullende richtlijnen.



<!--HONumber=Sep16_HO3-->


