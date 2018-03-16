---
title: "Microsoft Azure Active Directory één aanmelding invoegtoepassing Admin handleiding | Microsoft Docs"
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Microsoft Azure Active Directory eenmalige aanmelding voor JIRA.
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
ms.openlocfilehash: af949d1db8af37a534a16364f9f0763479c436e4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-azure-active-directory-single-sign-on-plugin-admin-guide"></a>Microsoft Azure Active Directory één aanmelding invoegtoepassing Admin handleiding

## <a name="table-of-contents"></a>Inhoudsopgave

1. **[OVERVIEW](#overview)**
2. **[HOE HET WERKT](#how-it-works)**
3. **[AUDIENCE](#audience)**
4. **[VERONDERSTELLINGEN](#assumptions)**
5. **[PREREQUISITES](#prerequisites)**
6. **[ONDERSTEUNDE VERSIES VAN JIRA EN SAMENLOOP](#supported-versions-of-jira-and-confluence)**
7. **[INSTALLATIE](#installation)**
8. **[CONFIGURATIE VAN DE INVOEGTOEPASSING](#plugin-configuration)**
9. **[VELD UITLEG VOOR EXTRA CONFIGURATIESCHERM:](#field-explanation-for-add---on-configuration-screen:)**
10. **[HET OPLOSSEN VAN PROBLEMEN](#troubleshooting)**

## <a name="overview"></a>Overzicht

Deze invoegtoepassingen kunnen Microsoft Azure AD-klanten hun organisatie gebruikersnaam en wachtwoord gebruiken voor aanmelding bij de Atlassian Jira en producten op basis van samenloop-Server. Het SAML 2.0 implementeert op basis van eenmalige aanmelding.

## <a name="how-it-works"></a>Hoe werkt het?

Wanneer gebruikers zich aanmelden bij Atlassian Jira of samenloop-toepassing willen, zien ze de **aanmelding met Azure AD** knop op de aanmeldingspagina. Wanneer ze op het klikt, zijn ze vereist voor aanmelding bij de aanmeldingspagina van Azure AD-organisatie.

Zodra de gebruikers zijn geverifieerd, is ze moeten kunnen aanmelden bij de toepassing. Als ze al met organisatie-ID en wachtwoord zijn geverifieerd, klikt u vervolgens meld zij rechtstreeks u aan bij de toepassing. Houd er ook rekening mee dat aanmelding voor JIRA en samenloop werkt. Als gebruikers zijn aangemeld in JIRA toepassing en samenloop ook openen in hetzelfde browservenster, moeten ze zich één keer aanmeldt bij en niet de referenties opnieuw opgeven voor andere app. De gebruikers ook toegang krijgen tot het product Atlassian via myapps onder Azure-account en moet zijn aangemeld zonder referenties wordt gevraagd.

> [!NOTE]
> Gebruikers inrichten is niet uitgevoerd met behulp van deze invoegtoepassing.

## <a name="audience"></a>Doelgroep

JIRA en samenloop beheerders die u van plan bent te gebruiken van deze invoegtoepassing SSO gebruikmaken van Azure AD inschakelen.

## <a name="assumptions"></a>Veronderstellingen

* JIRA/samenloop exemplaar is HTTPS-functionaliteit
* Gebruikers zijn reeds aangemaakt in JIRA/samenloop
* Gebruikers hebben in JIRA/samenloop toegewezen rol
* Beheerders hebben toegang tot gegevens die zijn vereist voor het configureren van de invoegtoepassing
* JIRA/samenloop zijn buiten het bedrijfsnetwerk ook beschikbaar
* Op deze optie werkt met alleen On-premises versie van JIRA en samenloop toevoegen

## <a name="prerequisites"></a>Vereisten

Opmerking vereisten voordat u doorgaan met installatie van de invoegtoepassing:

* JIRA/samenloop zijn geïnstalleerd op een Windows 64-bits versie
* JIRA/samenloop versies zijn HTTPS-functionaliteit
* Noteer de ondersteunde versie voor de invoegtoepassing in 'Ondersteunde versies' onderstaande sectie.
* JIRA/samenloop is beschikbaar op internet.
* Beheerreferenties voor JIRA/samenloop
* Beheerreferenties voor Azure AD
* WebSudo moet worden uitgeschakeld in JIRA en samenloop

## <a name="supported-versions-of-jira-and-confluence"></a>Ondersteunde versies van JIRA en samenloop

Vanaf nu de volgende versies van JIRA en samenloop worden ondersteund:

* JIRA Core en Software: 6.0-7.2.0
* JIRA servicedesk: 3.0 en 3.2
* Samenloop: 5.0 naar versie 5.10

## <a name="installation"></a>Installatie

Beheerder moet de stappen die hieronder worden vermeld voor het installeren van de invoegtoepassing:

1. Meld u aan bij uw JIRA/samenloop-exemplaar als een beheerder
    
2. Ga naar JIRA/samenloop beheer en klik op invoegtoepassingen.
    
3. Zoeken van Atlassian-marktplaats naar **SSO-invoegtoepassing voor Microsoft SAML**
 
4. Juiste versie van de invoegtoepassing wordt weergegeven in zoekresultaten
 
5. Selecteer de invoegtoepassing en UPM dezelfde installeert.
 
6. Nadat de invoegtoepassing is geïnstalleerd, wordt deze weergegeven in de gebruiker geïnstalleerd invoegtoepassingen sectie invoegtoepassing beheren
 
7. U moet de invoegtoepassing configureren voordat u deze gaan gebruiken.
 
8. Klik op de invoegtoepassing en ziet u een knop configureren.
 
9. Klik op deze informatie invoeren voor configuratie
    
## <a name="plugin-configuration"></a>Configuratie van de invoegtoepassing

Volgende afbeelding ziet u het Configuratiescherm voor de invoegtoepassing in zowel JIRA en samenloop
    
![configuratie van de invoegtoepassing](./media/ms-confluence-jira-plugin-adminguide/jira.png)

### <a name="field-explanation-for-add-on-configuration-screen"></a>Veld uitleg voor extra Configuratiescherm:

1.   Metagegevens-URL: URL van federatiemetagegevens ophalen van Azure AD
 
2.   ID: Wordt gebruikt door Azure AD voor het valideren van de bron van de aanvraag. Dit wordt toegewezen aan de ID-element in Azure AD. Dit is automatische afgeleid door invoegtoepassing als https://<domain:port>/
 
3.   URL van antwoord: Gebruik de URL van het antwoord in de IdP initiëren de SAML-aanmelding. Dit wordt toegewezen aan de antwoord-URL-element in Azure AD. Dit is automatische afgeleid door invoegtoepassing als https://<domain:port>/plugins/servlet/saml/auth
 
4.   Aanmelding URL: Gebruik aanmelding op de URL in de IdP initiëren de SAML-aanmelding. Dit wordt toegewezen aan het element aanmelding in Azure AD. Dit is automatische afgeleid door invoegtoepassing als https://<domain:port>/plugins/servlet/saml/auth
 
5.   IdP entiteit-ID: De entiteit-ID die gebruikmaakt van uw IdP. Dit wordt gevuld wanneer metagegevens-URL is opgelost.
 
6.   Aanmeldings-URL: De aanmeldings-URL van uw IdP. Dit wordt gevuld van Azure AD wanneer metagegevens-URL opgelost is.
 
7.   Afmeldings-URL: de URL voor afmelden van uw IdP. Dit wordt gevuld van Azure AD wanneer metagegevens-URL opgelost is.
 
8.   X.509-certificaat: Uw IdP x.509-certificaat. Dit wordt gevuld van Azure AD wanneer metagegevens-URL opgelost is.
 
9.   Naam van de knop aanmelden: De naam van de knop aanmelden van uw organisatie wil bekijken. Deze tekst wordt weergegeven op de knop aanmelden op het aanmeldingsscherm van gebruikers.
 
10.   SAML-ID gebruikerslocaties: Waar de gebruikers-id wordt verwacht in SAML-reactie. Het kan zijn in NameID of in de naam van een aangepast kenmerk. Deze ID is de JIRA/samenloop gebruikers-id.
 
11.   Kenmerknaam: naam van het kenmerk waar gebruikers-Id kan worden verwacht.
 
12.   Enable Home Realm Discovery: Schakel deze markering als het bedrijf met behulp van de aanmelding op basis van AD FS.
 
13.   Domeinnaam: Geef de domeinnaam hier in geval van de aanmelding op basis van AD FS
 
14.   Eenmalige aanmelding uit inschakelen: deze office controleren als u afmelden bij Azure AD wanneer een gebruiker zich wilt afmeldt van JIRA/samenloop.

### <a name="troubleshooting"></a>Problemen oplossen

* Als u meerdere certificaten fouten
    
    * Meld u aan bij Azure AD en de meerdere certificaten beschikbaar zijn op basis van de app verwijderen. Zorg ervoor dat er slechts één certificaat aanwezig zijn.

* Certificaat is verlopen in Azure AD.
    
    * Invoegtoepassingen zorgt voor automatische rollover van het certificaat. Wanneer een certificaat verlopen is, nieuw certificaat moet als actief gemarkeerd en ongebruikte certificaat moet worden verwijderd. Wanneer een gebruiker wil aanmelden bij JIRA in dit scenario, worden de invoegtoepassing haalt het nieuwe certificaat en opslaan in de invoegtoepassing.

* Het uitschakelen van WebSudo (de beheerder van de beveiligde sessie uitschakelen)
    
    * JIRA: Beveilig beheerder sessies (dat wil zeggen wachtwoordbevestiging voordat toegang tot de functies voor beheer) zijn standaard ingeschakeld. Als u dit in uw exemplaar JIRA uitschakelen wilt, kunt u deze functie uitschakelen door op te geven van de volgende regel in het bestand jira config.properties: ' ira.websudo.is.disabled = true '
    
    * Samenloop: Volg de stappen vermeld in de volgende URL https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html

* Velden die moeten worden gevuld met URL-metagegevens zijn niet ophalen van gevuld.
    
    * Controleer of de URL juist is. Controleer als u de juiste tenant en app-id hebt toegewezen.
    
    * Klik op de URL van de browser en zien als u de XML met federatiemetagegevens ontvangt.

* Interne serverfout:
    
    * Doorloop de logboeken die aanwezig zijn in de directory logs van de installatie. Als u de volgende fout krijgt wanneer de gebruiker probeert aan te melden met behulp van Azure AD SSO, kunt u de logboeken met de informatie over ondersteuning hieronder vindt u in dit document delen.

* Gebruikers-ID niet gevonden fout wanneer de gebruiker zich aanmeldt
    
    * Gebruiker is niet gemaakt in JIRA/samenloop, dus maak dezelfde.

* App fout niet vinden in Azure AD
    
    * Zie als de juiste URL is toegewezen aan de app in Azure AD.

* Ondersteuningsgegevens: bereiken aan ons op: [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). We reageren binnen 24-48 kantooruren.
    
    * U kunt ook een ondersteuningsticket met Microsoft via het Azure-portal kanaal verhogen.