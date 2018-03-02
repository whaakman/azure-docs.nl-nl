---
title: Probleem administrator-referenties opslaan bij het configureren van gebruikers inrichten tot een galerie van Azure AD-toepassing | Microsoft Docs
description: Het oplossen van algemene problemen bij het configureren van gebruikers inrichten tot een toepassing die al in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: asmalser
ms.openlocfilehash: 6617345c8923b1fc8081b01ddfe8b4bedf10b6ea
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="problem-saving-administrator-credentials-while-configuring-user-provisioning-to-an-azure-active-directory-gallery-application"></a>Probleem bij het opslaan van referenties tijdens het configureren van gebruikers inrichten tot een galerie van Azure Active Directory-toepassing 

Wanneer u de Azure-portal voor het configureren van [automatisch gebruikers inrichten](active-directory-saas-app-provisioning.md) voor een zakelijke toepassing een situatie kunnen optreden wanneer:

* De **beheerdersreferenties** hebt ingevoerd voor de toepassing geldig zijn, en de **testverbinding** knop werkt. Echter, de referenties kunnen niet worden opgeslagen en de Azure-portal retourneert een algemene foutmelding.

Op basis van SAML eenmalige aanmelding ook is geconfigureerd voor dezelfde toepassing, de meest waarschijnlijke oorzaak van de fout is dat Azure AD interne, per toepassing opslaglimiet voor certificaten als referenties is overschreden.

Momenteel Azure AD heeft een maximale capaciteit van een kilobyte voor alle certificaten, geheime tokens, referenties en verwante configuratiegegevens die zijn gekoppeld aan één exemplaar van een toepassing (ook wel bekend als een service principal-record in Azure AD).

Wanneer op basis van SAML eenmalige aanmelding is geconfigureerd, wordt het certificaat gebruikt voor het ondertekenen van de SAML-tokens hier is opgeslagen en vaak verbruikt meer dan 50 procent van de ruimte.

Alle geheime tokens, URI's melding e-mailadressen, gebruikersnamen en wachtwoorden op dat ophalen ingevoerd tijdens de installatie van de gebruikersinrichting kunnen leiden tot de opslaglimiet wordt overschreden.

## <a name="how-to-work-around-this-issue"></a>Hoe u kunt dit probleem omzeilen 

Er zijn twee mogelijke manieren vandaag de dag dit probleem omzeilen:

1. **Galerie met twee exemplaren van een toepassing, één voor eenmalige aanmelding en één voor gebruikers inrichten gebruiken** -de galerie-toepassing duurt [LinkedIn bevoegdheden](active-directory-saas-linkedinelevate-tutorial.md) als voorbeeld, kunt u LinkedIn worden de bevoegdheden van de galerie toevoegen en configureren het is voor eenmalige aanmelding. Voor het inrichten, toevoegen van een ander exemplaar van het LinkedIn worden de bevoegdheden van de app-galerie van Azure AD en naam 'LinkedIn uitbreiden (inrichten)'. Voor deze tweede exemplaar configureren [inrichting](active-directory-saas-linkedinelevate-provisioning-tutorial.md), maar geen eenmalige aanmelding. Wanneer u deze tijdelijke oplossing, dezelfde gebruikers en groepen moeten [toegewezen](active-directory-coreapps-assign-user-azure-portal.md) voor beide toepassingen. 

2. **Verklein de hoeveelheid opgeslagen configuratiegegevens** -alle gegevens die zijn opgegeven in de [beheerdersreferenties](active-directory-saas-app-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) sectie van het tabblad voor inrichting is opgeslagen in dezelfde locatie als het SAML-certificaat. Hoewel het niet mogelijk om te beperken, de lengte van al deze gegevens, sommige velden optionele configuratie, zoals de **e-mailmelding** kan worden verwijderd.

## <a name="next-steps"></a>Volgende stappen
[Gebruiker inrichting en inrichten van de SaaS-toepassingen configureren](active-directory-saas-app-provisioning.md)
