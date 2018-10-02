---
title: Beheerdershandleiding voor Atlassian Jira/samenloop - Azure Active Directory | Microsoft Docs
description: Beheerdershandleiding voor de Atlassian Jira en samenloop gebruiken met Azure Active Directory (Azure AD)...
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2018
ms.author: jeedes
ms.openlocfilehash: 49516523abdd927c3ae60235fcd74473689c6856
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019767"
---
# <a name="atlassian-jira-and-confluence-admin-guide-for-azure-active-directory"></a>Atlassian Jira en samenloop beheerdershandleiding voor Azure Active Directory

## <a name="overview"></a>Overzicht

De Azure Active Directory (Azure AD) eenmalige aanmelding (SSO) invoegtoepassing kunt Microsoft Azure AD-klanten hun werk- of schoolaccount om account te gebruiken voor aanmelding bij Atlassian Jira en producten op basis van een samenloop-Server. Deze implementeert op basis van SAML 2.0 SSO.

## <a name="how-it-works"></a>Hoe werkt het?

Wanneer gebruikers zich aanmelden bij de toepassing Atlassian Jira of samenloop willen, zien ze de **Meld u aan met Azure AD** knop op de pagina aanmelden. Wanneer ze deze selecteert, moeten ze zich aanmelden met behulp van de Azure AD-aanmelden pagina organisatie (dat wil zeggen, hun werk- of school-account).

Nadat de gebruikers zijn geverifieerd, moeten ze mogelijk zijn om aan te melden bij de toepassing. Als ze al zijn geverifieerd met de ID en het wachtwoord voor hun werk- of schoolaccount, klikt u vervolgens aanmelden ze rechtstreeks bij de toepassing. 

Aanmelding bij de geschikt voor Jira en samenloop. Als gebruikers zijn aangemeld bij de toepassing Jira en samenloop in hetzelfde browservenster wordt geopend, wordt ze geen voor de referenties voor de andere app. 

Gebruikers kunnen ook krijgen tot het product Atlassian via Mijn Apps onder het account werk- of schoolaccount. Ze moeten worden aangemeld zonder dat om referenties wordt gevraagd.

> [!NOTE]
> Inrichten van gebruikers loopt niet via de invoegtoepassing.

## <a name="audience"></a>Doelgroep

Jira en samenloop beheerders kunt de invoegtoepassing eenmalige aanmelding inschakelen met behulp van Azure AD.

## <a name="assumptions"></a>Veronderstellingen

* Jira en samenloop instanties zijn HTTPS-functionaliteit.
* Gebruikers zijn al gemaakt in Jira of samenloop.
* Gebruikers hebben de rol is toegewezen in Jira of samenloop.
* Beheerders hebben toegang tot de gegevens die zijn vereist voor het configureren van de invoegtoepassing.
* Jira of samenloop is beschikbaar buiten het bedrijfsnetwerk bevindt.
* De invoegtoepassing werkt met alleen de on-premises versie van Jira en samenloop.

## <a name="prerequisites"></a>Vereisten

Houd rekening met de volgende informatie voordat u de invoegtoepassing installeren:

* Jira en samenloop zijn geïnstalleerd op een Windows 64-bits versie.
* Jira en samenloop versies zijn HTTPS-functionaliteit.
* Jira en samenloop zijn beschikbaar op het internet.
* Referenties van een beheerder zijn ingesteld voor Jira en samenloop.
* Beheerdersreferenties zijn aanwezig voor Azure AD.
* WebSudo is uitgeschakeld in Jira en samenloop.

## <a name="supported-versions-of-jira-and-confluence"></a>Ondersteunde versies van Jira en samenloop

De invoegtoepassing biedt ondersteuning voor de volgende versies van Jira en samenloop:

* Jira-Core- en Software: 6.0-7.8
* Jira-servicedesk: 3.0 en 3.2
* Samenloop: 5.0 naar versie 5.10

## <a name="installation"></a>Installatie

Volg deze stappen voor het installeren van de invoegtoepassing:

1. Aanmelden bij uw Jira of samenloop-exemplaar als een beheerder.

2. Ga naar de Jira/samenloop administration console en selecteer **invoegtoepassingen**.

3. Zoek op Atlassian Marketplace **Microsoft SAML SSO-invoegtoepassing**.

   De juiste versie van de invoegtoepassing wordt weergegeven in de lijst met zoekresultaten.

4. Selecteer de invoegtoepassing en de universele invoegtoepassing (UPM) geïnstalleerd.

Nadat de invoegtoepassing is geïnstalleerd, wordt deze weergegeven in de **gebruiker geïnstalleerd invoegtoepassingen** sectie van **invoegtoepassingen beheren**.

## <a name="plug-in-configuration"></a>De configuratie van de invoegtoepassing

Voordat u met behulp van de invoegtoepassing begint, moet u deze configureren. De invoegtoepassing, selecteer de **configureren** knop en geef informatie over de configuratie.

De volgende afbeelding ziet u het Configuratiescherm in Jira en samenloop:

![Invoegtoepassing Configuratiescherm](./media/ms-confluence-jira-plugin-adminguide/jira.png)

*   **Metagegevens-URL**: de URL voor federatiemetagegevens ophalen uit Azure AD.

*   **Id's**: de URL die Azure AD wordt gebruikt voor het valideren van de bron van de aanvraag. Het wordt toegewezen aan de **id** -element in Azure AD. De invoegtoepassing automatisch afgeleid deze URL als https://*< domein: poort >*/.

*   **Antwoord-URL**: de antwoord-URL in uw id-provider (IdP) die de SAML-aanmelding initieert. Het wordt toegewezen aan de **antwoord-URL** -element in Azure AD. De invoegtoepassing automatisch afgeleid deze URL als https://*< domein: poort >*/plugins/servlet/saml/auth.

*   **Meld u URL**: de aanmeldings-URL in de IdP die de SAML-aanmelding initieert. Het wordt toegewezen aan de **aanmelding** -element in Azure AD. De invoegtoepassing automatisch afgeleid deze URL als https://*< domein: poort >*/plugins/servlet/saml/auth.

*   **IdP entiteit-ID**: de entiteit-ID die gebruikmaakt van uw id-provider. Dit vak wordt gevuld wanneer de metagegevens-URL opgelost is.

*   **Aanmeldings-URL**: de URL aanmelden vanaf uw id-provider. Dit vak wordt opgehaald uit Azure AD wanneer de metagegevens-URL opgelost is.

*   **Afmeldings-URL van**: de URL voor afmelden van uw id-provider. Dit vak wordt opgehaald uit Azure AD wanneer de metagegevens-URL opgelost is.

*   **X.509-certificaat**: X.509-certificaat van uw id-provider. Dit vak wordt opgehaald uit Azure AD wanneer de metagegevens-URL opgelost is.

*   **Aanmeldingsnaam van de knop**: de naam van de knop aanmelden die uw organisatie wil dat gebruikers om te zien op de pagina aanmelden.

*   **SAML-ID gebruikerslocaties**: de locatie waar de Jira of samenloop gebruikers-ID wordt verwacht in het SAML-antwoord. Kan **NameID** of in de naam van een aangepast kenmerk.

*   **De naam van het kenmerk**: de naam van het kenmerk waar de gebruikers-ID wordt verwacht.

*   **Inschakelen van start Realm detectie**: de selectie te maken als het bedrijf met behulp van Active Directory Federation Services (AD FS) - op basis van aanmelding - in.

*   **Domeinnaam**: de naam van het domein als de aanmelding is AD FS op basis van.

*   **Inschakelen van één die afmelden wordt weergegeven**: de selectie te maken als u zich wilt afmelden bij Azure AD wanneer een gebruiker zich afmeldt van Jira of samenloop.

## <a name="troubleshooting"></a>Problemen oplossen

* **U krijgt meerdere certificaatfouten**: aanmelden bij Azure AD en de meerdere certificaten die beschikbaar zijn op basis van de app verwijderen. Zorg ervoor dat slechts één certificaat aanwezig is.

* **Een certificaat is bijna verlopen in Azure AD**: invoegtoepassingen regelt automatisch rollover van het certificaat. Wanneer een certificaat bijna verlopen is, een nieuw certificaat moet als actief gemarkeerd en niet-gebruikte certificaten moeten worden verwijderd. Wanneer een gebruiker wil zich aanmelden bij Jira in dit scenario, de invoegtoepassing haalt en slaat het nieuwe certificaat.

* **U wilt uitschakelen WebSudo (de beheerder van de beveiligde sessie uitschakelen)**:

  * Beveilig voor Jira, beheerder van sessies (dat wil zeggen, wachtwoordbevestiging voordat u toegang tot functies voor beheer) zijn standaard ingeschakeld. Als u verwijderen van deze mogelijkheid in uw exemplaar Jira wilt, geeft u de volgende regel in het bestand jira-config.properties: `ira.websudo.is.disabled = true`

  * Voor samenloop, kunt u de stappen volgen op de [samenloop ondersteuningssite](https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html).

* **Velden die moeten worden gevuld met de metagegevens-URL zijn niet ophalen van gevuld**:

  * Controleer of de URL juist is. Controleer als u hebt toegewezen aan de juiste tenant- en app-ID.

  * Voer de URL in een browser en zien als u de federatiemetagegevens XML ontvangt.

* **Er is een interne serverfout**: Bekijk de logboeken in de logboekmap van de installatie. Als u krijgt de fout op wanneer de gebruiker wil zich aanmelden met behulp van Azure AD-eenmalige aanmelding, kunt u de logboeken delen met het ondersteuningsteam.

* **Er is een fout 'Gebruikers-ID niet gevonden' wanneer de gebruiker probeert aan te melden bij**: de gebruikers-ID in Jira of samenloop maken.

* **Er is een 'App is niet gevonden'-fout in Azure AD**: Zie als de juiste URL is toegewezen aan de app in Azure AD.

* **U ondersteuning nodig hebt**: contact opnemen met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). Het team reageert in 24 tot 48 tijdens kantooruren.

  U kunt ook een ondersteuningsticket met Microsoft via het Azure-portal kanaal verhogen.

## <a name="plug-in-faq"></a>Veelgestelde vragen over de invoegtoepassing

Raadpleeg onderstaande veelgestelde vragen over hebt u een query met betrekking tot deze invoegtoepassing.

### <a name="what-does-the-plug-in-do"></a>Wat doet de invoegtoepassing?

De invoegtoepassing biedt mogelijkheden voor eenmalige aanmelding (SSO) voor Atlassian Jira (met inbegrip van Jira-Core, Jira Software, Jira Service Desk) en samenloop on-premises software. De invoegtoepassing werkt met Azure Active Directory (Azure AD) als id-provider (IdP).

### <a name="which-atlassian-products-does-the-plug-in-work-with"></a>Welke producten Atlassian werkt de invoegtoepassing met?

De invoegtoepassing werkt met on-premises versies van Jira en samenloop.

### <a name="does-the-plug-in-work-on-cloud-versions"></a>De invoegtoepassing werkt op versies van de cloud?

Nee. De invoegtoepassing ondersteunt alleen on-premises versies van Jira en samenloop.

### <a name="which-versions-of-jira-and-confluence-does-the-plug-in-support"></a>Welke versies van Jira en samenloop heeft de invoegtoepassing ondersteuning?

De invoegtoepassing biedt ondersteuning voor deze versies:

* Jira-Core- en Software: 6.0-7.8
* Jira-servicedesk: 3.0 en 3.2
* Samenloop: 5.0 naar versie 5.10

### <a name="is-the-plug-in-free-or-paid"></a>De invoegtoepassing is gratis of betaald?

Het is een gratis invoegtoepassing.

### <a name="do-i-need-to-restart-jira-or-confluence-after-i-deploy-the-plug-in"></a>Moet ik Jira of samenloop opnieuw opstarten nadat ik de invoegtoepassing geïmplementeerd?

Opnieuw opstarten is niet vereist. U kunt starten met behulp van de invoegtoepassing onmiddellijk.

### <a name="how-do-i-get-support-for-the-plug-in"></a>Hoe krijg ik ondersteuning voor de invoegtoepassing?

U kunt contact op met de [Team van Azure AD-eenmalige aanmelding integratie](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) voor ondersteuning die nodig zijn voor deze invoegtoepassing. Het team reageert in 24 tot 48 tijdens kantooruren.

U kunt ook een ondersteuningsticket met Microsoft via het Azure-portal kanaal verhogen.

### <a name="would-the-plug-in-work-on-a-mac-or-ubuntu-installation-of-jira-and-confluence"></a>De invoegtoepassing werkzaamheden op een Mac- of Ubuntu-installatie van Jira en samenloop zou?

We hebben de invoegtoepassing alleen op 64-bits Windows Server-installaties van Jira en samenloop getest.

### <a name="does-the-plug-in-work-with-idps-other-than-azure-ad"></a>De invoegtoepassing werkt met de id-providers dan Azure AD?

Nee. Het werkt alleen met Azure AD.

### <a name="what-version-of-saml-does-the-plug-in-work-with"></a>Welke versie van SAML werkt de invoegtoepassing met?

Het werkt met SAML 2.0.

### <a name="does-the-plug-in-do-user-provisioning"></a>De invoegtoepassing doet inrichten van gebruikers?

Nee. De invoegtoepassing biedt alleen op basis van SAML 2.0 SSO. De gebruiker heeft om te worden ingericht in de toepassing voordat de aanmelding eenmalige aanmelding.

### <a name="does-the-plug-in-support-cluster-versions-of-jira-and-confluence"></a>De invoegtoepassing ondersteuning-clusterversies van Jira en samenloop gebruikt?

Nee. De invoegtoepassing werkt met on-premises versies van Jira en samenloop.

### <a name="does-the-plug-in-work-with-http-versions-of-jira-and-confluence"></a>De invoegtoepassing werkt met HTTP-versies van Jira en samenloop?

Nee. De invoegtoepassing werkt met installaties alleen HTTPS-functionaliteit.