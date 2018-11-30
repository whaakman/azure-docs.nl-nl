---
title: Hoe beheert gebruikersaccounts in Azure API Management | Microsoft Docs
description: Meer informatie over het maken of uitnodigen van gebruikers in Azure API Management
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 827f8eb7491c6f64b275dee83345c29d51c0933d
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442478"
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Over het beheren van gebruikersaccounts in Azure API Management

Ontwikkelaars zijn in API Management, de gebruikers van de API's die u beschikbaar maakt met behulp van API Management. Deze handleiding wordt beschreven hoe om te maken en ontwikkelaars uitnodigen met de API's en producten die u hen ter beschikking met uw exemplaar van API Management. Zie voor meer informatie over het programmatisch beheren van gebruikersaccounts, het [gebruikersentiteit](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-user-entity) documentatie in de [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) verwijzing.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>Vereisten

Taken in dit artikel uit te voeren: [maken van een Azure API Management-exemplaar](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-developer"> </a>Maak een nieuwe ontwikkelaar

Als u wilt toevoegen van een nieuwe gebruiker, de stappen in deze sectie uit te voeren:

1. Selecteer de **gebruikers** tabblad aan de linkerkant van het scherm.
2. Druk op **+ toevoegen**.
3. Voer de juiste informatie voor de gebruiker.
4. Klik op **Toevoegen**.

    ![Een nieuwe gebruiker toevoegen](./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png)

De van de zojuist gemaakte ontwikkelaarsaccounts zijn standaard **Active**, en die zijn gekoppeld aan de **ontwikkelaars** groep. Accounts voor ontwikkelaars die zich in een **active** status kan worden gebruikt voor toegang tot alle van de API's waarvoor ze abonnementen hebben. Als u wilt de ontwikkelaar van de zojuist gemaakte koppelen aan extra groepen, Zie [groepen koppelen aan ontwikkelaars][How to associate groups with developers].

## <a name="invite-developer"> </a>Een ontwikkelaar uitnodigen
Als u wilt uitnodigen een ontwikkelaar, de stappen in deze sectie uit te voeren:

1. Selecteer de **gebruikers** tabblad aan de linkerkant van het scherm.
2. Druk op **+ uitnodigen**.

Een bevestigingsbericht wordt weergegeven, maar de ontwikkelaar van de zojuist uitgenodigde wordt niet weergegeven in de lijst totdat nadat ze de uitnodiging accepteren. 

Wanneer een ontwikkelaar wordt uitgenodigd, wordt een e-mailbericht verzonden naar de ontwikkelaar. Dit e-mailbericht met een sjabloon wordt gegenereerd en kan worden aangepast. Zie voor meer informatie, [e-mailsjablonen configureren][Configure email templates].

Zodra de uitnodiging is geaccepteerd, wordt het account actief.

## <a name="block-developer"> </a> Een developer-account opnieuw activeren of deactiveren

Standaard de zojuist gemaakte of uitgenodigde ontwikkelaarsaccounts zijn **Active**. U kunt een developer-account, klikt u op **blok**. Als u wilt een geblokkeerde developer-account opnieuw activeren, klikt u op **activeren**. Een geblokkeerde developer-account kan toegang krijgen tot de portal voor ontwikkelaars of alle API's aanroepen. Als u wilt verwijderen van een gebruikersaccount, klikt u op **verwijderen**.

Volg de volgende stappen uit voor het blokkeren van een gebruiker.

1. Selecteer de **gebruikers** tabblad aan de linkerkant van het scherm.
2. Klik op de gebruiker die u wilt blokkeren.
3. Druk op **blok**.

## <a name="reset-a-user-password"></a>Een gebruikerswachtwoord opnieuw instellen

Als u wilt werken via een programma met behulp van gebruikersaccounts, Zie de [gebruikersentiteit](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-user-entity) documentatie in de [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) verwijzing. Als u wilt opnieuw instellen van wachtwoord voor een gebruikersaccount op een specifieke waarde, kunt u de [bijwerken van een gebruiker](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-user-entity#UpdateUser) bewerking en geef het gewenste wachtwoord.

## <a name="next-steps"> </a>Volgende stappen
Zodra een developer-account is gemaakt, kunt u deze koppelen aan rollen en het abonneren op producten en API's. Zie voor meer informatie, [hoe u groepen maken en gebruiken][How to create and use groups].

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
