---
title: Quickstart voor verloopbeleid voor Office 365-groepen in Azure Active Directory Domain Services | Microsoft Docs
description: Verloop voor Office 365-groepen - Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 05/06/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01cedadc115496fcf00df986b4ad4b9c5aab5139
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65606202"
---
# <a name="quickstart-set-office-365-groups-to-expire-in-azure-active-directory"></a>Quickstart: Office 365-groepen voor verloop instellen in Azure Active Directory

In deze snelstart stelt u het verloopbeleid in voor uw Office 365-groepen. Wanneer gebruikers hun eigen groepen kunnen instellen, kunnen niet-gebruikte groepen zich vermenigvuldigen. Eén manier om niet-gebruikte groepen te beheren, is door verloopbeleid voor deze groepen in te stellen om het handmatig verwijderen van groepen te verminderen.

Verloopbeleid maken is eenvoudig:

* Groepseigenaren worden op de hoogte gesteld als een groep moet worden vernieuwd die op het punt staat te verlopen
* Een groep die niet wordt vernieuwd, wordt verwijderd
* Een verwijderde Office 365-groep kan binnen dertig dagen door een groepseigenaar of een Azure AD-beheerder worden hersteld

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisite"></a>Vereiste

 De rol van de minste bevoegdheden voor het instellen van de vervaldatum van de groep is beheerder van de gebruiker in de organisatie.

## <a name="turn-on-user-creation-for-groups"></a>Maken van gebruikers voor groepen inschakelen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een administrator-account van gebruiker.

2. Selecteer **Groepen** en vervolgens **Algemeen**.
  
   ![Pagina voor groepsbeheer met Self-Service-instellingen](./media/groups-quickstart-expiration/self-service-settings.png)

3. Stel **Users can create Office 365 groups** in op **Ja**.

4. Selecteer **Opslaan** om de groepsinstellingen op te slaan als u klaar bent.

## <a name="set-group-expiration"></a>Verloopdatum van de groep instellen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), selecteer **Azure Active Directory** > **groepen** > **verlopen** naar Open de instellingen voor verlooptijd.
  
   ![Pagina met instellingen voor groep verlopen](./media/groups-quickstart-expiration/expiration-settings.png)

2. Stel het verloopinterval in. Selecteer een vooraf ingestelde waarde of voer een aangepaste waarde van meer dan 31 dagen in. 

3. Geef een e-mailadres op waar meldingen over verlooptijden naartoe moeten worden gestuurd als een groep geen eigenaar heeft.

4. Stel voor deze snelstart **Enable expiration for these Office 365 groups** in op **Alle**.

5. Selecteer **Opslaan** om de instellingen voor het verloop op te slaan als u klaar bent.

Dat is alles. In deze snelstart hebt u het verloopbeleid ingesteld voor de geselecteerde Office 365-groepen.

## <a name="clean-up-resources"></a>Resources opschonen

### <a name="to-remove-the-expiration-policy"></a>Het vervalbeleid verwijderen

1. Zorg dat u bent aangemeld bij de [Azure-portal](https://portal.azure.com) met een account van een globale beheerder voor de tenant.
2. Selecteer **Azure Active Directory** > **Groepen** > **Verlooptijd**.
3. Stel **Enable expiration for these Office 365 groups** in op **Geen**.

### <a name="to-turn-off-user-creation-for-groups"></a>Gebruiker maken voor groepen uitschakelen

1. Selecteer **Azure Active Directory** > **Groepen** > **Algemeen**. 
2. Stel **Users can create Office 365 groups in Azure portals** in op **Nee**.

## <a name="next-steps"></a>Volgende stappen

Zie het volgende artikel voor meer informatie over verlopen, met inbegrip van PowerShell-instructies en technische beperkingen:

> [!div class="nextstepaction"]
> [Vervaldatum van het beleid PowerShell](groups-lifecycle.md)
