---
title: Kenmerken met Azure AD voor de toewijzing van synchroniseren | Microsoft Docs
description: Informatie over het synchroniseren van de kenmerken van uw on-premises Active Directory naar Azure AD. Bij het configureren van gebruikersinrichting voor SaaS-apps, gebruikt u de functie voor directory-extensie toe te voegen bronkenmerken die standaard zijn niet gesynchroniseerd.
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
ms.date: 05/13/2019
ms.author: mimart
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9460bc924ea662646360d1a3f5087949a39a03f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65806114"
---
# <a name="sync-an-attribute-from-your-on-premises-active-directory-to-azure-ad-for-provisioning-to-an-application"></a>Synchroniseren van een kenmerk uit uw on-premises Active Directory naar Azure AD voor het inrichten van een toepassing

Bij het aanpassen van kenmerktoewijzingen voor het inrichten van gebruikers, wellicht u dat het kenmerk dat u wilt toewijzen, worden niet weergegeven in de **bronkenmerk** lijst. Dit artikel leest u hoe de ontbrekend kenmerk toevoegen met het synchroniseren van uw on-premises Active Directory (AD) naar Azure Active Directory (Azure AD).

Azure AD, moet de gegevens die zijn vereist voor het maken van een gebruikersprofiel bij het inrichten van gebruikersaccounts uit Azure AD om een SaaS-app te bevatten. In sommige gevallen kan de gegevens beschikbaar maken u mogelijk moet kenmerken synchroniseren vanuit uw on-premises AD met Azure AD. Azure AD Connect synchroniseert automatisch bepaalde kenmerken naar Azure AD, maar niet alle kenmerken. Bovendien kunnen enkele kenmerken (zoals SAMAccountName) die zijn gesynchroniseerd standaard niet worden blootgesteld via de Azure AD Graph API. In dergelijke gevallen kunt u de functie van Azure AD Connect directory-extensie om te synchroniseren van het kenmerk Azure AD. Op die manier het kenmerk zichtbaar zijn voor de Azure AD Graph-API en de Azure AD-inrichtingsservice.

Als de gegevens die u nodig hebt voor het inrichten in Active Directory is, maar is niet beschikbaar voor het inrichten van de oorzaken die hierboven worden beschreven, volg deze stappen.
 
## <a name="sync-an-attribute"></a>Synchroniseren van een kenmerk 

1. Open de wizard Azure AD Connect, kiest u taken en kies vervolgens **aanpassen Synchronisatieopties**.

   ![Azure Active Directory Connect extra taken wizardpagina](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-customize.png)
 
2. Meld u aan als globale beheerder Azure AD. 

3. Op de **optionele functies** weergeeft, schakelt **synchronisatie van Directory-extensiekenmerken**.
 
   ![Azure Active Directory Connect optionele functies wizardpagina](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extension-attribute-sync.png)

4. Selecteer de kenmerken die u wilt uitbreiden naar Azure AD.
   > [!NOTE]
   > De zoekopdracht onder **beschikbare kenmerken** is hoofdlettergevoelig.

   ![Azure Active Directory Connect Directory-extensies selectie wizardpagina](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extensions.png)

5. De Azure AD Connect-wizard hebt voltooid en maken van een volledige synchronisatiecyclus om uit te voeren. Wanneer de cyclus voltooid is, wordt het schema is uitgebreid en de nieuwe waarden worden gesynchroniseerd tussen uw on-premises AD en Azure AD.
 
6. In Azure portal, terwijl u bent [kenmerktoewijzingen gebruiker bewerken](customize-application-attributes.md), wordt de **bronkenmerk** lijst bevat nu het toegevoegde kenmerk in de indeling `<attributename> (extension_<appID>_<attributename>)`. Selecteer het kenmerk en wijs deze toe aan de doeltoepassing om in te richten.

   ![Azure Active Directory Connect Directory-extensies selectie wizardpagina](media/user-provisioning-sync-attributes-for-mapping/attribute-mapping-extensions.png)

> [!NOTE]
> De mogelijkheid om in te richten verwijzingskenmerken van on-premises AD, zoals **managedby** of **DN-naam/DistinguishedName**, is momenteel niet ondersteund. U kunt deze functie aanvragen op [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory). 

## <a name="next-steps"></a>Volgende stappen

* [Wie is binnen het bereik voor het inrichten van definiëren](define-conditional-rules-for-provisioning-user-accounts.md)
