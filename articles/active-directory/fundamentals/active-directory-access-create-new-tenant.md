---
title: Snelstartgids over het maken en gebruiken van een nieuwe tenant - Azure Active Directory | Microsoft Docs
description: Instructies over het zoeken van Azure Active Directory en het maken van een nieuwe tenant voor uw organisatie.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f970e9d43bc6dd67fe55ca9cdcfcae716c9f166a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67110609"
---
# <a name="quickstart-create-a-new-tenant-in-azure-active-directory"></a>Snelstartgids: Een nieuwe tenant maken in Azure Active Directory
U kunt al uw administratieve taken uitvoeren met behulp van de portal van Azure Active Directory (Azure AD), met inbegrip van het maken van een nieuwe tenant voor uw organisatie. 

In deze Snelstartgids leert u hoe u bij de Azure Portal en Azure Active Directory komt en hoe u een eenvoudige tenant voor uw organisatie maakt.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij de [Azure Portal](https://portal.azure.com/) van uw organisatie met een Globale-administratoraccount.

![Azure portal scherm, klikt u met Azure AD-optie](media/active-directory-access-create-new-tenant/azure-ad-portal.png)

## <a name="create-a-new-tenant-for-your-organization"></a>Een nieuwe tenant maken voor uw organisatie
Nadat u zich bij Azure Portal aanmeldt, kunt u een nieuwe tenant maken voor uw organisatie. De nieuwe tenant vertegenwoordigt uw organisatie en helpt u bij het beheren van een specifiek exemplaar van Microsoft cloudservices voor uw interne en externe gebruikers.

### <a name="to-create-a-new-tenant"></a>Een nieuwe tenant maken
1. Selecteer **Een resource maken**, selecteer **Identiteit** en selecteer vervolgens **Azure Active Directory**.

    De pagina **Map maken** wordt weergegeven.

    ![De pagina Maken voor Azure Active Directory](media/active-directory-access-create-new-tenant/azure-ad-create-new-tenant.png)

2.  Op de pagina **Map maken**, voer de volgende informatie in:
    
    - Type _Contoso_ in het vak **Organisatienaam**.

    - Type _Contoso_ in het vak **Initiële domeinnaam**.

    - Laat de optie _Verenigde Staten_ in het vak **Land of regio**.

3. Selecteer **Maken**.

De nieuwe tenant wordt gemaakt met het domein contoso.onmicrosoft.com.

## <a name="clean-up-resources"></a>Resources opschonen
Als u deze toepassing niet verder gebruikt, kunt u de tenant met de volgende stappen verwijderen:

- Selecteer **Azure Active Directory**, en klik vervolgens op de pagina **Contoso - Overzicht** en selecteer **Map verwijderen**.

    De tenant en de bijbehorende informatie wordt verwijderd.

    ![De overzichtspagina met knop van de directory gemarkeerde verwijderen](media/active-directory-access-create-new-tenant/azure-ad-delete-new-tenant.png)

## <a name="next-steps"></a>Volgende stappen
- Zie [Een aangepaste domeinnaam toevoegen aan Azure Active Directory](add-custom-domain.md) voor het wijzigen of toevoegen van meer informatie over extra domeinnamen

- Gebruikers toevoegen, zie [Een nieuwe gebruiker toevoegen of verwijderen](add-users-azure-active-directory.md)

- Groepen en leden toevoegen, zie [Een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)

- Meer informatie over [rollen gebaseerde toegang met behulp van Privileged Identity Management](../../role-based-access-control/pim-azure-resource.md) en [voorwaardelijke toegang](../../role-based-access-control/conditional-access-azure-management.md) voor het beheer van toegang tot de toepassing en de resource van van uw organisatie.

- Informatie over Azure Active Directory, met inbegrip van [basisinformatie over licenties, terminologie en bijbehorende functies](active-directory-whatis.md).
