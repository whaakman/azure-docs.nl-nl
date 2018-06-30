---
title: Hoe beheert u gebruikersaccounts in Azure API Management | Microsoft Docs
description: Meer informatie over het maken of uitnodigen gebruikers in Azure API Management
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
ms.openlocfilehash: 3d9a4454a1b3f65b42a46a26e8d483fad83f65f6
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111266"
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Het beheren van gebruikersaccounts in Azure API Management
In API Management zijn ontwikkelaars de gebruikers van de API's die u met behulp van API Management. Deze handleiding wordt beschreven voor het maken en uit te nodigen ontwikkelaars met de API's en -producten te maken die voor hen beschikbaar met uw API Management-exemplaar. Zie voor informatie over het beheren van gebruikersaccounts via een programma, het [entiteit gebruiker](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-user-entity) documentatie in de [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) verwijzing.

## <a name="prerequisites"></a>Vereisten

Taken in dit artikel: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-developer"> </a>Maak een nieuwe ontwikkelaar

Een nieuwe gebruiker toevoegen, de stappen in deze sectie uit te voeren:

1. Selecteer de **gebruikers** tabblad aan de linkerkant van het scherm.
2. Druk op **+ toevoegen**.
3. Voer de juiste informatie voor de gebruiker.
4. Klik op **Toevoegen**.

    ![Een nieuwe gebruiker toevoegen](./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png)

De van de zojuist gemaakte ontwikkelaarsaccounts zijn standaard **Active**, en die zijn gekoppeld aan de **ontwikkelaars** groep. Ontwikkelaarsaccounts die zich in een **active** status kan worden gebruikt voor toegang tot alle van de API's waarvoor ze abonnementen hebben. De ontwikkelaar van de zojuist gemaakte koppelt u extra groepen, Zie [groepen koppelen aan ontwikkelaars][How to associate groups with developers].

## <a name="invite-developer"> </a>Een ontwikkelaar uitnodigen
Als u wilt een ontwikkelaar uitnodigen, volg de stappen in deze sectie:

1. Selecteer de **gebruikers** tabblad aan de linkerkant van het scherm.
2. Druk op **+ uitnodigen**.

Een bevestigingsbericht wordt weergegeven, maar de ontwikkelaar van de nieuwe uitgenodigde niet wordt weergegeven in de lijst pas nadat ze de uitnodiging accepteren. 

Wanneer een ontwikkelaar wordt verzocht, wordt een e-mailbericht verzonden naar de ontwikkelaar. Dit e-mailbericht met een sjabloon wordt gegenereerd en kan worden aangepast. Zie voor meer informatie [e-mailsjablonen configureren][Configure email templates].

Zodra de uitnodiging is geaccepteerd, wordt het account actief.

## <a name="block-developer"> </a> Een ontwikkelaarsaccount activeren of deactiveren

Gemaakte of uitgenodigde ontwikkelaarsaccounts zijn standaard **Active**. U kunt een ontwikkelaarsaccount, klikt u op **blok**. Als u wilt een geblokkeerde ontwikkelaarsaccount activeren, klikt u op **activeren**. Een geblokkeerde developer-account geen toegang tot de portal voor ontwikkelaars of alle API's aanroepen. Als u wilt verwijderen van een gebruikersaccount, klikt u op **verwijderen**.

Volg de volgende stappen voor het blokkeren van een gebruiker.

1. Selecteer de **gebruikers** tabblad aan de linkerkant van het scherm.
2. Klik op de gebruiker die u wilt blokkeren.
3. Druk op **blok**.

## <a name="reset-a-user-password"></a>Een gebruikerswachtwoord opnieuw instellen

Om te werken via een programma met een gebruikersaccount, Zie de [entiteit gebruiker](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-user-entity) documentatie in de [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) verwijzing. Als u het wachtwoord van een gebruiker op een specifieke waarde herstellen, kunt u de [bijwerken van een gebruiker](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-user-entity#UpdateUser) bewerking en geef het gewenste wachtwoord.

## <a name="next-steps"> </a>Volgende stappen
Zodra een ontwikkelaarsaccount is gemaakt, kunt u aan functies koppelen en deze zich te abonneren op producten en -API's. Zie voor meer informatie [maken en gebruiken van groepen][How to create and use groups].

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
