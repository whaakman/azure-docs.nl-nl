---
title: Een Azure AD-tenant verkrijgen | Microsoft Docs
description: Een Azure Active Directory-tenant verkrijgen voor het registreren en maken van toepassingen.
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: fe33d490b754e2f793f5c7a13dc55ca038b1b71c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Een Azure Active Directory-tenant verkrijgen
In Azure Active Directory (Azure AD) is een [tenant](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) representatief voor een organisatie.  Het is een toegewezen exemplaar van de Azure AD-service die een organisatie ontvangt en waarvan de organisatie de eigenaar is wanneer deze zich registreert voor een Microsoft-cloudservice, zoals Azure, Microsoft Intune of Office 365.  Elke Azure AD-tenant is uniek en werkt afzonderlijk van andere Azure AD-tenants.  

Een tenant bevat alle gebruikers in een bedrijf en de bijbehorende informatie: hun wachtwoorden, gebruikersprofielgegevens, machtigingen enzovoort.  Het bevat ook groepen, toepassingen en andere informatie over een organisatie en de beveiliging.

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij uw toepassing, moet u uw toepassing registreren in een tenant van uzelf.  Het publiceren van een toepassing in een Azure AD-tenant is **helemaal gratis**.  De meeste ontwikkelaars maken verschillende tenants en toepassingen om te experimenten, te ontwikkelen, te faseren en te testen.  Organisaties die zich registreren voor uw toepassing en deze gebruiken, kunnen ervoor kiezen licenties aan te schaffen als ze gebruik willen maken van geavanceerde directoryfuncties.

Maar hoe kunt u een Azure AD-tenant verkrijgen?  De procedure kan enigszins afwijken als u:

* [Een bestaand Office 365-abonnement hebt](#use-an-existing-office-365-subscription)
* [Een bestaand Azure-abonnement hebt dat aan een Microsoft-Account is gekoppeld](#use-an-msa-azure-subscription)
* [Een bestaand Azure-abonnement hebt dat aan een organisatieaccount is gekoppeld](#use-an-organizational-azure-subscription)
* [Geen van de bovenstaande abonnementen hebt en de procedure vanaf het begin wilt uitvoeren](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Een bestaand Office 365-abonnement gebruiken
Als u een bestaand Office 365-abonnement hebt, beschikt u al over een Azure AD-tenant. U kunt u aanmelden bij [Azure Portal](https://portal.azure.com) met uw O365-account en Azure AD gebruiken.

## <a name="use-an-msa-azure-subscription"></a>Een MSA Azure-abonnement gebruiken
Als u zich eerder hebt geregistreerd voor een Azure-abonnement met uw afzonderlijke Microsoft-account, hebt u al een tenant.  Wanneer u zich bij [Azure Portal](https://portal.azure.com) aanmeldt, wordt u automatisch aangemeld bij uw standaardtenant. U kunt deze tenant naar wens gebruiken, maar mogelijk wilt u een beheerdersaccount voor de organisatie maken.

Voer hiervoor de volgende stappen uit.  U kunt ook een nieuwe tenant en een beheerder in deze tenant maken via een vergelijkbare procedure.

1. Meld u aan bij [Azure Portal](https://portal.azure.com) met uw afzonderlijke account
2. Ga naar de sectie Azure Active Directory van de portal (op de linkernavigatiebalk onder **Meer services**)
3. U moet automatisch worden aangemeld bij de standaarddirectory. Als dit niet het geval is, kunt u van directory wisselen door in de rechterbovenhoek op uw accountnaam te klikken.
4. Kies in de sectie **Snelle taken** de optie **Een gebruiker toevoegen**.
5. Geef in het formulier Gebruiker toevoegen de volgende informatie op:

   * Naam: (kies de gewenste waarde)
   * Gebruikersnaam: (kies een gebruikersnaam op voor deze beheerder)
   * Profiel: (vul de gewenste waarden in voor de voornaam, achternaam, functie en afdeling)
   * Rol: hoofdbeheerder
6. Wanneer u het formulier Gebruiker toevoegen hebt ingevuld en het tijdelijke wachtwoord voor de nieuwe gebruiker met beheerdersrechten hebt ontvangen, noteert u dit wachtwoord omdat u zich moet aanmelden met deze nieuwe gebruiker om het wachtwoord te wijzigen. U kunt het wachtwoord ook rechtstreeks naar de gebruiker verzenden via een alternatief e-mailbericht.
7. Klik op **Maken** om de nieuwe gebruiker te maken.
8. Als u het tijdelijke wachtwoord wilt wijzigen, meldt u zich aan bij [https://login.microsoftonline.com](https://login.microsoftonline.com) met dit nieuwe gebruikersaccount en wijzigt u het wachtwoord als u hierom wordt gevraagd.

## <a name="use-an-organizational-azure-subscription"></a>Een Azure-abonnement voor de organisatie gebruiken
Als u zich eerder hebt geregistreerd voor een Azure-abonnement met uw organisatieaccount, hebt u al een tenant.  In [Azure Portal](https://portal.azure.com) vindt u een tenant wanneer u naar Meer services en Azure Active Directory navigeert.  U kunt deze tenant naar wens gebruiken.

## <a name="start-from-scratch"></a>De procedure vanaf het begin uitvoeren
U hoeft zich geen zorgen te maken als u de bovenstaande informatie niet begrijpt.  Ga naar [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) om u aan te melden voor Azure met een nieuwe organisatie.  Wanneer u de procedure hebt voltooid, hebt u uw eigen Azure AD-tenant met de domeinnaam die u hebt gekozen tijdens het aanmelden.  In [Azure Portal](https://portal.azure.com) kunt u uw tenant zoeken door in het linkernavigatievenster naar Azure Active Directory te gaan.

Tijdens de aanmeldingsprocedure voor Azure moet u uw creditcardgegevens opgeven.  U kunt gerust doorgaan. Er worden namelijk geen kosten in rekening gebracht voor het publiceren van toepassingen in Azure AD of het maken van nieuwe tenants.
