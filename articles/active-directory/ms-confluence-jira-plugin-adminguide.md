---
title: Handleiding voor de Azure Active Directory SSO invoegtoepassing Admin | Microsoft Docs
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Jira/samenloop.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: jeedes
ms.openlocfilehash: d34ff6021816c73fb064a3ce73b7fcf3ae22dbd1
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="admin-guide-for-the-azure-active-directory-sso-plug-in"></a>Admin-handleiding voor de Azure Active Directory SSO-invoegtoepassing

## <a name="overview"></a>Overzicht

De Azure Active Directory (Azure AD) eenmalige aanmelding (SSO) invoegtoepassing kunt Microsoft Azure AD-klanten kunnen hun werk of school-account gebruiken voor aanmelding bij Atlassian Jira en producten op basis van samenloop-Server. Het implementeert op basis van SAML 2.0 SSO.

## <a name="how-it-works"></a>Hoe werkt het?

Wanneer gebruikers zich aanmelden bij de toepassing Atlassian Jira of samenloop willen, zien ze de **aanmelding met Azure AD** knop op de aanmeldingspagina. Wanneer ze deze optie inschakelt, zijn ze aan te melden met behulp van de Azure AD organisatie aanmeldingspagina (dat wil zeggen, hun werk of school-account) vereist.

Nadat de gebruikers zijn geverifieerd, is ze moeten kunnen aanmelden bij de toepassing. Als ze al zijn geverifieerd met de ID en wachtwoord voor hun werk of school-account, klikt u vervolgens aanmelden zij rechtstreeks bij de toepassing. 

Aanmelden de werkt via Jira en samenloop. Als gebruikers zijn aangemeld bij de toepassing Jira en samenloop in hetzelfde browservenster wordt geopend, zijn ze om de referenties voor de andere app opgeven. 

Gebruikers kunnen ook krijgen tot het product Atlassian via Mijn Apps onder het account voor werk of school. Ze moeten worden aangemeld zonder referenties wordt gevraagd.

> [!NOTE]
> Gebruikers inrichten is via de invoegtoepassing niet uitgevoerd.

## <a name="audience"></a>Doelgroep

Jira en samenloop beheerders kunt de invoegtoepassing eenmalige aanmelding inschakelen met behulp van Azure AD gebruiken.

## <a name="assumptions"></a>Veronderstellingen

* Jira en samenloop instanties zijn HTTPS-functionaliteit.
* Gebruikers zijn reeds aangemaakt in Jira of samenloop.
* Gebruikers hebben rollen in Jira of samenloop is toegewezen.
* Beheerders hebben toegang tot informatie die vereist is voor het configureren van de invoegtoepassing.
* Jira of samenloop is buiten het bedrijfsnetwerk ook beschikbaar.
* De invoegtoepassing werkt met alleen de on-premises versie van Jira en samenloop.

## <a name="prerequisites"></a>Vereisten

Let op de volgende informatie voordat u de invoegtoepassing installeren:

* Jira en samenloop zijn geïnstalleerd op een Windows 64-bits versie.
* De versies Jira en samenloop zijn HTTPS-functionaliteit.
* Jira en samenloop zijn beschikbaar op internet.
* Er zijn beheerdersreferenties voor Jira en samenloop voor.
* Er zijn beheerdersreferenties in plaats voor Azure AD.
* WebSudo is in Jira en samenloop uitgeschakeld.

## <a name="supported-versions-of-jira-and-confluence"></a>Ondersteunde versies van Jira en samenloop

De invoegtoepassing biedt ondersteuning voor de volgende versies van Jira en samenloop:

* Jira Core en Software: 6.0-7.2.0
* Jira servicedesk: 3.0 en 3.2
* Samenloop: 5.0 naar versie 5.10

## <a name="installation"></a>Installatie

Volg deze stappen voor het installeren van de invoegtoepassing:

1. Meld u aan met uw exemplaar Jira of samenloop als een beheerder.
    
2. Ga naar de Jira/samenloop administration console en selecteer **invoegtoepassingen**.
    
3. Zoek in de Atlassian Marketplace **SSO-invoegtoepassing voor Microsoft SAML**.
 
   De juiste versie van de invoegtoepassing wordt weergegeven in de zoekresultaten.
 
5. Selecteer de invoegtoepassing en de universele invoegtoepassing (UPM) is geïnstalleerd.
 
Nadat de invoegtoepassing is geïnstalleerd, wordt deze weergegeven in de **gebruiker geïnstalleerd invoegtoepassingen** sectie van **invoegtoepassingen beheren**.
    
## <a name="plug-in-configuration"></a>Invoegtoepassing configuration

Voordat u met de invoegtoepassing begint, moet u deze configureren. De invoegtoepassing, selecteer de **configureren** knop en geef de configuratiedetails.

De volgende afbeelding toont het Configuratiescherm in Jira en samenloop:
    
![Scherm invoegtoepassing configuratie](./media/ms-confluence-jira-plugin-adminguide/jira.png)

*   **Metagegevens-URL**: de URL voor federatiemetagegevens ophalen van Azure AD.
 
*   **Id's**: de URL die Azure AD wordt gebruikt voor het valideren van de bron van de aanvraag. Deze toegewezen aan de **id** element in Azure AD. De invoegtoepassing automatisch afgeleid deze URL als https://*< domein >: poort*/.
 
*   **Antwoord-URL**: de antwoord-URL in uw identiteitsprovider (IdP) die de aanmeldingspagina SAML start. Deze toegewezen aan de **antwoord-URL** element in Azure AD. De invoegtoepassing automatisch afgeleid deze URL als https://*< domein >: poort*/plugins/servlet/saml/auth.
 
*   **Meld u op URL**: de aanmeldings-URL in de IdP waarop u de SAML-aanmeldingspagina te starten. Deze toegewezen aan de **aanmelding** element in Azure AD. De invoegtoepassing automatisch afgeleid deze URL als https://*< domein >: poort*/plugins/servlet/saml/auth.
 
*   **Entiteit-ID IdP**: de entiteit-ID die gebruikmaakt van uw IdP. In dit vak wordt gevuld wanneer de metagegevens-URL opgelost is.
 
*   **Aanmeldings-URL**: de URL aanmelden in de IdP. In dit vak is ingevuld vanuit Azure AD wanneer de metagegevens-URL opgelost is.
 
*   **URL voor afmelden**: de URL voor afmelden van uw IdP. In dit vak is ingevuld vanuit Azure AD wanneer de metagegevens-URL opgelost is.
 
*   **X.509-certificaat**: uw IdP X.509-certificaat. In dit vak is ingevuld vanuit Azure AD wanneer de metagegevens-URL opgelost is.
 
*   **Aanmeldingsnaam van de knop**: de naam van de knop aanmelden die uw organisatie wil gebruikers dat te zien op de aanmeldingspagina.
 
*   **SAML-ID gebruikerslocaties**: de locatie waar de Jira of samenloop gebruikers-ID wordt verwacht in de SAML-reactie. Het kan zijn **NameID** of in de naam van een aangepast kenmerk.
 
*   **Naam van het kenmerk**: de naam van het kenmerk waar de gebruikers-ID wordt verwacht.
 
*   **Inschakelen van Thuisrealmdetectie**: de selectie te maken als het bedrijf van Active Directory Federation Services (AD FS gebruikmaakt) - gebaseerde sign - in.
 
*   **Domeinnaam**: de naam van het domein als aanmelden AD FS is gebaseerd.
 
*   **Inschakelen van één Signout**: de selectie te maken als u afmelden bij Azure AD wanneer een gebruiker zich wilt afmeldt van Jira of samenloop.

## <a name="troubleshooting"></a>Problemen oplossen

* **U krijgt meerdere certificaatfouten**: aanmelden bij Azure AD en de meerdere certificaten die beschikbaar zijn op basis van de app verwijderen. Zorg ervoor dat slechts één certificaat aanwezig is.

* **Een certificaat is bijna verlopen in Azure AD**: invoegtoepassingen zorgt voor automatische rollover van het certificaat. Een certificaat is verlopen, een nieuw certificaat moet als actief worden gemarkeerd als niet-gebruikte certificaten moeten worden verwijderd. Wanneer een gebruiker probeert aan te melden bij Jira in dit scenario wordt de invoegtoepassing op te halen en het nieuwe certificaat wordt opgeslagen.

* **U wilt uitschakelen (de beheerder van de beveiligde sessie uitschakelen) WebSudo**:
    
  * Beveilig voor Jira, beheerder sessies (dat wil zeggen wachtwoordbevestiging voordat toegang tot de functies voor beheer) zijn standaard ingeschakeld. Als u verwijderen van deze mogelijkheid in uw exemplaar Jira wilt, geeft u de volgende regel in het bestand jira config.properties: `ira.websudo.is.disabled = true`
    
  * Voor samenloop, volg de stappen op de [samenloop ondersteuningssite](https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html).

* **Zijn de velden die moeten worden gevuld met de metagegevens-URL niet ophalen van gevuld**:
    
  * Controleer of de URL juist is. Controleer of u hebt toegewezen aan de juiste tenant en app-ID.
    
  * Voer de URL in een browser en Ga na of de federatiemetagegevens-XML.

* **Er is een interne serverfout**: de logboeken in de logboekmap van de installatie. Als u de volgende fout krijgt wanneer de gebruiker probeert aan te melden met behulp van Azure AD SSO, kunt u de logboeken delen met het ondersteuningsteam.

* **Er is een fout 'Gebruiker-ID niet gevonden' wanneer de gebruiker zich probeert aan te melden**: de gebruikers-ID in Jira of samenloop maken.

* **Er is een fout 'De App is niet gevonden' in Azure AD**: Zie als de juiste URL is toegewezen aan de app in Azure AD.

* **U ondersteuning nodig**: bereiken de [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). Het team reageert in 24-48 kantooruren.
    
  U kunt ook een ondersteuningsticket met Microsoft via het Azure-portal kanaal verhogen.