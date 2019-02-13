---
title: Probleem bij het opslaan van administratorreferenties tijdens het inrichten van gebruikers naar een toepassing in Azure AD-galerie configureren | Microsoft Docs
description: Het oplossen van veelvoorkomende problemen met het configureren gebruikers in te richten al een toepassing die worden vermeld in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: celested
ms.reviewer: asmalser
ms.collection: M365-identity-device-management
ms.openlocfilehash: e74519bf2eeb31a700c2fb02cbf3b94702449427
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56218088"
---
# <a name="problem-saving-administrator-credentials-while-configuring-user-provisioning-to-an-azure-active-directory-gallery-application"></a>Problemen bij het opslaan van referenties tijdens het inrichten van gebruikers naar een galerie van Azure Active Directory-toepassing configureren 

Bij het gebruik van de Azure-portal configureren [automatisch gebruikers inrichten](user-provisioning.md) voor een bedrijfstoepassing, kan er een situatie waarbij:

* De **beheerdersreferenties** hebt ingevoerd voor de toepassing geldig zijn, en de **testverbinding** knop werkt. Echter, de referenties kunnen niet worden opgeslagen en de Azure-portal geeft als resultaat een algemeen foutbericht krijgen.

Als SAML gebaseerde eenmalige aanmelding ook is geconfigureerd voor dezelfde toepassing, de meest waarschijnlijke oorzaak van de fout is dat Azure AD-limiet voor interne, per toepassing opslag voor certificaten en referenties is overschreden.

Azure AD op dit moment heeft een maximale capaciteit van 1024 bytes voor alle certificaten, geheime tokens, referenties en verwante configuratiegegevens die zijn gekoppeld aan één exemplaar van een toepassing (ook wel bekend als een service principal-record in Azure AD).

Wanneer SAML gebaseerde eenmalige aanmelding is geconfigureerd, wordt het certificaat dat wordt gebruikt voor het ondertekenen van de SAML-tokens worden opgeslagen en vaak verbruikt meer dan 50 procent van de ruimte.

Een geheim tokens, URI's melding e-mailadressen, gebruikersnamen en wachtwoorden op dat u hebt opgegeven tijdens de installatie van het inrichten van gebruikers kunnen leiden tot de limiet voor opslag wordt overschreden.

## <a name="how-to-work-around-this-issue"></a>Hoe u dit probleem omzeilen 

Er zijn twee manieren om vandaag nog dit probleem omzeilen:

1. **Gebruik de galerie met twee exemplaren van een toepassing, één voor één aanmelding en één voor het inrichten van gebruikers** -houdend met de toepassing in de galerie [LinkedIn met verhoogde bevoegdheden](../saas-apps/linkedinelevate-tutorial.md) als voorbeeld, kunt u LinkedIn met verhoogde bevoegdheden uit de galerie toevoegen en configureren het is voor eenmalige aanmelding. Voor het inrichten, een ander exemplaar van LinkedIn met verhoogde bevoegdheden uit de galerie met Azure AD-app toevoegen en de naam "LinkedIn toestaan (inrichten)." Voor deze tweede exemplaar configureren [inrichting](../saas-apps/linkedinelevate-provisioning-tutorial.md), maar geen eenmalige aanmelding. Wanneer u deze tijdelijke oplossing, de dezelfde gebruikers en groepen moeten worden [toegewezen](assign-user-or-group-access-portal.md) voor beide toepassingen. 

2. **Verminder de hoeveelheid opgeslagen configuratiegegevens** -alle gegevens die zijn opgegeven in de [beheerdersreferenties](user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) sectie van het tabblad voor de inrichting is opgeslagen op dezelfde plaats als het SAML-certificaat. Hoewel het niet mogelijk om te beperken van de lengte van al deze gegevens, voor sommige velden optionele configuratie, zoals de **e-mailmelding** kan worden verwijderd.

## <a name="next-steps"></a>Volgende stappen
[Gebruiker inrichting en ongedaan maken inrichting voor SaaS-toepassingen configureren](user-provisioning.md)
