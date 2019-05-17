---
title: De naam of het logo van een zakelijke toepassing in Azure Active Directory wijzigen | Microsoft Docs
description: Het wijzigen van de naam of het logo voor een aangepaste zakelijke toepassing in Azure Active Directory
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 30da8d6843c27c42d4d99adef50b9ad98a131c95
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65780926"
---
# <a name="change-the-name-or-logo-of-an-enterprise-application-in-azure-active-directory"></a>De naam of het logo van een zakelijke toepassing in Azure Active Directory wijzigen

Het is gemakkelijk om te wijzigen van de naam of het logo voor een aangepaste zakelijke toepassing in Azure Active Directory (Azure AD). Hebt u de juiste machtigingen voor deze wijzigingen aanbrengt en u moet de maker van de aangepaste toepassing.

## <a name="how-do-i-change-an-enterprise-applications-name-or-logo"></a>Hoe wijzig ik de naam of het logo van een enterprise-toepassing?

1. Aanmelden bij de [Azure Active Directory-portal](https://aad.portal.azure.com/) met een account dat een globale beheerder voor de map. De **Azure Active Directory-beheercentrum** pagina wordt weergegeven.
2. Selecteer in het linkerdeelvenster **Enterprise-toepassingen**. De lijst met uw zakelijke toepassingen wordt weergegeven.
3. Selecteer een toepassing. De overzichtspagina van de toepassing wordt weergegeven.
4. In het overzichtsvenster van de van toepassing onder de **beheren** kop, selecteer **eigenschappen**. De **eigenschappen** pagina wordt weergegeven.
5. Als u de naam wijzigen wilt, selecteert u de **naam** vak, typ de nieuwe naam en druk op **Enter**.
6. Als u wijzigen van het logo wilt, vinden de **Logo** veld en selecteer het mappictogram naast de **selecteert u een bestand** vak is lager dan de huidige logoafbeelding van de toepassing.

   ![De opdracht Eigenschappen selecteren](./media/change-name-or-logo-portal/change-logo.png)

   Anders als u het logo worden niet wijzigen, gaat u naar stap 8.
7. Selecteer het gewenste bestand als het nieuwe logo in de bestandenkiezer. De naam van het bestand weergegeven in het vak onder de huidige logoafbeelding.

   > [!NOTE]
   > Azure vereist dat de installatiekopie van het logo moet een PNG-bestand en dit limieten van toepassing is in breedte, hoogte en bestandsgrootte.
8. Selecteer **Opslaan**. Als u ervoor een nieuw logo kiest, de **Logo** van veld-installatiekopie wordt gewijzigd in overeenstemming met het nieuwe logobestand.

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: De groepen en leden van uw organisatie weergeven in Azure Active Directory](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Een gebruiker of groep toewijzen aan een enterprise-app](assign-user-or-group-access-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](remove-user-or-group-access-portal.md)
* [Gebruikersaanmeldingen voor een zakelijke app uitschakelen](disable-user-sign-in-portal.md)
