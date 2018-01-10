---
title: Aan de slag met Azure Active Directory | Microsoft Docs
description: Ophalen van licenties, domeinnaam toevoegen, aangepaste aanmeldingspagina maken en selfservice wachtwoordherstel in Azure Active Directory toevoegen
keywords: 
author: curtand
manager: mtillman
ms.author: curtand
ms.reviewer: jsnow
ms.date: 11/14/2017
ms.topic: article
ms.prod: 
ms.service: active-directory
ms.workload: identity
ms.technology: 
ms.assetid: 
services: active-directory
ms.custom: it-pro
ms.openlocfilehash: 9e1a7337c2477455aa0b56f2147f46f3bf5293a8
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="get-started-with-azure-ad"></a>Aan de slag met Azure AD
Moderne identiteitsbeheer vereist een schaalbare, consistente reliablity om te controleren of de beschikbaarheid van toepassingen en services aan alleen geverifieerde gebruikers. Om voldoende ondersteunen de identity management-behoeften van gebruikers, IT-afdeling moet een manier om toegang te bieden tot erkende, openbare software als een service (SaaS)-apps, kunt u interne line-host of-business-apps, en zelfs manieren voor het verbeteren van on-premises ontwikkeling van Apps en het gebruik. Al deze vereisten voldoen verwijzen aan de behoeften van een oplossing voor identiteitsbeheer cloud-gebaseerde.      

Azure Active Directory (Azure AD) is de cloud gebaseerde directory en identity management-service, Microsoft van meerdere tenants. Azure AD combineert core directoryservices, governance geavanceerde identiteit en toegang Toepassingsbeheer. Het ontwerp van multitenant, geografisch verspreide, hoge beschikbaarheid van Azure AD-betekent dat u erop voor uw meest kritieke zakelijke behoeften vertrouwen kunt.

Azure AD bevat een volledige reeks mogelijkheden voor identiteitsbeheer inclusief de mogelijkheid om te synchroniseren van lokale resourcegegevens, aanpasbare huisstijl, eenvoudige Licentiebeheer en beheer van zelfs selfservice voor wachtwoordherstel. Deze mogelijkheden eenvoudig te implementeren kunnen u helpen aan de slag met Azure AD cloud-gebaseerde toepassingen beveiligen, IT-processen te stroomlijnen, kosten knippen en ervoor te zorgen dat de doelstellingen van de naleving wordt voldaan.

![Azure AD ](./media/get-started-azure-ad/Azure_Active_Directory.png)

De rest van dit artikel wordt uitgelegd hoe u aan de slag met Azure AD. 

## <a name="sign-up-for-azure-active-directory-premium"></a>Registreren voor Azure Active Directory Premium
Naar [aan de slag met Azure Active Directory (Azure AD) Premium](active-directory-get-started-premium.md), kunt u licenties koopt en koppel deze aan uw Azure-abonnement. Als u een nieuwe Azure-abonnement maakt, moet u ook uw licentieabonnement en de toegang van Azure AD-service te activeren. 

U kunt zich op verschillende manieren registreren voor Active Directory Premium: 

- Met uw Azure- of Office 365-abonnement
- Met een Enterprise Mobility + Security-licentieabonnement
- Met een Microsoft Volume-licentieabonnement

### <a name="verification-step"></a>Verificatiestap
Zorg ervoor dat u met de service aanmelden kunt na het activeren van het abonnement.

## <a name="add-a-custom-domain-name"></a>Een aangepaste domeinnaam toevoegen
Elke Azure AD-directory wordt geleverd met een initiële domeinnaam in de vorm van *domainname*. onmicrosoft.com. De initiële domeinnaam kan niet worden gewijzigd of verwijderd, maar u kunt ook [uw zakelijke domeinnaam toevoegen aan Azure AD](add-custom-domain.md). Uw organisatie heeft bijvoorbeeld waarschijnlijk andere domeinnamen gebruikt voor bedrijven en gebruikers die zich aanmeldt met uw zakelijke domeinnaam. Het toevoegen van aangepaste domeinnamen naar Azure AD, kunt u gebruikersnamen toewijzen in de directory die bekend aan uw gebruikers, zoals zijn 'alice@contoso.com.' in plaats van 'alice@.onmicrosoft.com'. Het proces is eenvoudig:

1. Voeg de aangepaste domeinnaam toe aan uw directory.
2. Voeg een DNS-vermelding voor de domeinnaam toe aan de domeinnaamregistrar.
3. Verifieer de aangepaste domeinnaam in Azure AD.

### <a name="verification-step"></a>Verificatiestap
Zorg ervoor dat er na het toevoegen van een aangepast domein, de **gecontroleerd** status wordt weergegeven op de **aangepaste domeinnamen** blade van de Azure AD-portal.

## <a name="add-company-branding-to-your-sign-in-page"></a>Huisstijl aan uw aanmeldingspagina toevoegen 
Om verwarring te voorkomen, willen veel bedrijven een consistente look gebruiken voor alle websites en services die ze beheren. Azure Active Directory (Azure AD) biedt deze mogelijkheid doordat u [pas het uiterlijk van de aanmeldingspagina met uw bedrijfslogo en kleurenschema toepassen](customize-branding.md). De aanmeldingspagina is de pagina die wordt weergegeven wanneer u zich aanmeldt bij Office 365 of andere toepassingen op Internet die van Azure AD als hun id-provider gebruikmaken. U communiceert met deze pagina uw referenties in te voeren.

### <a name="verification-step"></a>Verificatiestap
Aanmelden bij de Azure-portal en zorg ervoor dat uw aangepaste aanmeldingspagina en eventuele aanpassingen extra taal juist hebt geconfigureerd. 

## <a name="add-new-users"></a>Nieuwe gebruikers toevoegen
U kunt [nieuwe gebruikers toevoegen aan uw organisatie Azure AD](add-users-azure-active-directory.md) één op een tijdstip met de Azure portal of door het synchroniseren van uw on-premises Windows Server AD-bron-gegevens. U kunt gebruikers cloud-gebaseerde toevoegen rechtstreeks vanuit de Azure AD-portal of lokale gebruikersinformatie te synchroniseren.

Als u on-premises identiteitssynchronisatie met Azure AD wilt inschakelen, moet u [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) installeren en configureren op een server in uw organisatie. Deze toepassing draagt zorg voor het synchroniseren van gebruikers en groepen uit uw bestaande identiteitsbron met uw Azure AD-tenant.

### <a name="verification-step"></a>Verificatiestap
Na het maken of de synchronisatie van nieuwe gebruikers, zorg zijn ze zichtbaar in Azure AD.

## <a name="assign-licenses"></a>Licenties toewijzen
Hoewel het verkrijgen van een abonnement hoeft u alleen betaald mogelijkheden configureren, moet u nog steeds [gebruikerslicenties toewijzen](license-users-groups.md) voor Azure AD Premium betaalde functies. Elke gebruiker die toegang moeten hebben tot of die wordt beheerd via een Azure AD betaald functie moet een licentie worden toegewezen. De licentietoewijzing is geen toewijzing tussen een gebruiker en een gekochte service, zoals Azure AD Premium, Basic of Enterprise Mobility + Security.

U kunt de licentietoewijzing op basis van een groep gebruiken voor het instellen van regels, zoals in de volgende voorbeelden:

- Alle gebruikers in uw directory ophalen automatisch een licentie
- Iedereen met de juiste functie een licentie opgehaald
- U kunt het besluit om andere beheerders in de organisatie te delegeren (via Self-service groepen)

### <a name="verification-step"></a>Verificatiestap
Controleer toegewezen en beschikbare licenties onder **Azure Active Directory** > **licenties** > **alle producten**.

## <a name="configure-self-service-password-reset"></a>De self-service voor wachtwoord opnieuw instellen configureren
[Selfservice voor wachtwoordherstel (SSPR)](active-directory-passwords-getting-started.md) biedt een eenvoudige methode voor IT-beheerders kunnen gebruikers opnieuw instellen of hun wachtwoorden of accounts ontgrendelen. Het systeem biedt gedetailleerde rapporten zodat u kunt volgen wanneer gebruikers het systeem gebruiken. U ontvangt ook meldingen om u te waarschuwen over misbruik.

### <a name="verification-step"></a>Verificatiestap
Controleren ingeschakeld SSPR eigenschappen onder **Azure Active Directory** > **wachtwoordherstel** om te controleren of de juiste gebruiker en groepstoewijzingen zijn aangebracht. 


## <a name="next-steps"></a>Volgende stappen
[Pagina van de service Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

[Azure Active Directory informatiepagina met prijzen](https://azure.microsoft.com/pricing/details/active-directory/)