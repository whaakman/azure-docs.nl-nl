---
title: Quickstart voor naambeleid voor Office 365-groepen in Microsoft Azure Active Directory | Microsoft Docs
description: Legt uit hoe u nieuwe gebruikers toevoegt of bestaande gebruikers verwijdert in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 04/24/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0e600204479bc54a590df6bf1bbcd634eaac7fc
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65605637"
---
# <a name="quickstart-naming-policy-for-groups-in-azure-active-directory"></a>Snelstartgids: Naamgevingsbeleid voor groepen in Azure Active Directory

In deze snelstart stelt u naambeleid in uw Azure Active Directory (Azure AD)-tenant in voor Office 365-groepen die door gebruikers zijn gemaakt. Hierdoor kunt u de groepen van de tenant sorteren en doorzoeken. U kunt het naambeleid onder meer gebruiken voor:

* Het communiceren van de functie van een groep, het lidmaatschap, de geografische regio of de maker van een groep.
* Het classificeren van groepen in het adresboek.
* Zorgen dat het gebruik van bepaalde woorden voor groepsnamen en -aliassen wordt geblokkeerd.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="configure-the-group-naming-policy-for-a-tenant-using-azure-portal-preview"></a>Configureer het beleid voor naamgeving voor een tenant met behulp van Azure portal (preview)

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een administrator-account van gebruiker.
1. Selecteer **groepen**en selecteer vervolgens **naamgevingsbeleid** de Naming beleid-pagina te openen.

    ![Open de pagina van de Naming-beleid in het beheercentrum](./media/groups-naming-policy/policy-preview.png)

### <a name="view-or-edit-the-prefix-suffix-naming-policy"></a>Weergeven of bewerken van het naamgevingsbeleid van voorvoegsel-achtervoegsel

1. Op de **naamgevingsbeleid** weergeeft, schakelt **naming groepsbeleid**.
1. U kunt weergeven of bewerken van het huidige voorvoegsel of achtervoegsel naming beleid afzonderlijk door de kenmerken of tekenreeksen die u wilt afdwingen als onderdeel van het naamgevingsbeleid te selecteren.
1. Als u wilt verwijderen een voorvoegsel of achtervoegsel in de lijst, selecteert u het voorvoegsel of achtervoegsel en vervolgens **verwijderen**. Meerdere items kunnen worden verwijderd op hetzelfde moment.
1. Selecteer **opslaan** voor uw wijzigingen aan het beleid van kracht.

### <a name="view-or-edit-the-custom-blocked-words"></a>Weergeven of bewerken van de aangepaste geblokkeerde woorden

1. Op de **naamgevingsbeleid** weergeeft, schakelt **woorden geblokkeerd**.

    ![bewerken en uploaden van de lijst met geblokkeerde woorden voor de naamgeving van beleid](./media/groups-naming-policy/blockedwords-preview.png)

1. Weergeven of bewerken van de huidige lijst met geblokkeerde dan speciale woorden hiervoor **downloaden**.
1. Upload een nieuwe lijst met geblokkeerde dan speciale woorden door het bestandspictogram te selecteren.
1. Selecteer **opslaan** voor uw wijzigingen aan het beleid van kracht.

Dat is alles. U hebt het naambeleid ingesteld en uw aangepaste, geblokkeerde woorden toegevoegd.

## <a name="clean-up-resources"></a>Resources opschonen

### <a name="remove-the-naming-policy-using-azure-portal-preview"></a>Verwijder het naming beleid met behulp van Azure portal (preview)

1. Op de **naamgevingsbeleid** weergeeft, schakelt **beleid verwijderen**.
1. Nadat u de verwijdering bevestigt, het naamgevingsbeleid wordt verwijderd, inclusief alle voorvoegsel-achtervoegsel naamgeving van beleid en eventuele aangepaste geblokkeerde woorden.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd over het instellen van het naamgevingsbeleid van uw organisatie Azure AD via Azure portal.

Ga naar het volgende artikel voor meer informatie, met inbegrip van de PowerShell-cmdlets voor het benoemen van beleid, technische beperkingen, een lijst met geblokkeerde dan speciale woorden en de ervaringen van eindgebruikers toe te voegen in Office 365-apps.
> [!div class="nextstepaction"]
> [Naamgevingsbeleid voor PowerShell](groups-naming-policy.md)