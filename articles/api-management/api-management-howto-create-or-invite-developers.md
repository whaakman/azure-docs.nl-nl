---
title: Hoe beheert u gebruikersaccounts in Azure API Management | Microsoft Docs
description: Meer informatie over het maken of uitnodigen gebruikers in Azure API Management
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: 6f2fd5e4c1a51fe9d1652c9970bcd8d76b25ab60
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Het beheren van gebruikersaccounts in Azure API Management
In API Management zijn ontwikkelaars de gebruikers van de API's die u met behulp van API Management. Deze handleiding wordt beschreven voor het maken en uit te nodigen ontwikkelaars met de API's en -producten te maken die voor hen beschikbaar met uw API Management-exemplaar. Zie voor informatie over het beheren van gebruikersaccounts via een programma, het [entiteit gebruiker](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentatie in de [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) verwijzing.

## <a name="create-developer"></a>Maken van een nieuwe ontwikkelaar
Klik op om een nieuwe ontwikkelaar **publicatieportal** in de Azure-Portal voor uw API Management-service. Hiermee gaat u naar de publicatieportal van API Management. Als u nog geen exemplaar van API Management-service hebt gemaakt, raadpleegt u [API Management service-exemplaar maken][Create an API Management service instance].

![Publicatieportal][api-management-management-console]

Klik op **gebruikers** van de **API Management** menu aan de linkerkant en klik vervolgens op **gebruiker toevoegen**.

![Ontwikkelaars maken][api-management-create-developer]

Voer de **e**, **wachtwoord**, en **naam** voor de nieuwe ontwikkelaars en klik op **opslaan**.

![Ontwikkelaars maken][api-management-add-new-user]

De van de zojuist gemaakte ontwikkelaarsaccounts zijn standaard **Active**, en die zijn gekoppeld aan de **ontwikkelaars** groep.

![Nieuwe ontwikkelaars][api-management-new-developer]

Ontwikkelaarsaccounts die zich in een **active** status kan worden gebruikt voor toegang tot alle van de API's waarvoor ze abonnementen hebben. De ontwikkelaar van de zojuist gemaakte koppelt u extra groepen, Zie [groepen koppelen aan ontwikkelaars][How to associate groups with developers].

## <a name="invite-developer"></a>Een ontwikkelaar uitnodigen
Als u wilt een ontwikkelaar uitnodigen, klikt u op **gebruikers** van de **API Management** menu aan de linkerkant en klik vervolgens op **gebruiker uit te nodigen**.

![Developer uit te nodigen][api-management-invite-developer]

Voer de naam en e-mailadres van de ontwikkelaar, en klik op **uitnodigen**.

![Developer uit te nodigen][api-management-invite-developer-window]

Een bevestigingsbericht wordt weergegeven, maar de ontwikkelaar van de nieuwe uitgenodigde niet wordt weergegeven in de lijst pas nadat ze de uitnodiging accepteren. 

![Bevestiging uitnodigen][api-management-invite-developer-confirmation]

Wanneer een ontwikkelaar wordt verzocht, wordt een e-mailbericht verzonden naar de ontwikkelaar. Dit e-mailbericht met een sjabloon wordt gegenereerd en kan worden aangepast. Zie voor meer informatie [e-mailsjablonen configureren][Configure email templates].

Zodra de uitnodiging is geaccepteerd, wordt het account actief.

## <a name="block-developer"></a> Deactiveren of opnieuw activeren een ontwikkelaarsaccount
Gemaakte of uitgenodigde ontwikkelaarsaccounts zijn standaard **Active**. U kunt een ontwikkelaarsaccount, klikt u op **blok**. Als u wilt een geblokkeerde ontwikkelaarsaccount activeren, klikt u op **activeren**. Een geblokkeerde ontwikkelaarsaccount kan geen toegang tot de portal voor ontwikkelaars of alle API's aanroepen. Als u wilt verwijderen van een gebruikersaccount, klikt u op **verwijderen**.

![Blok-ontwikkelaars][api-management-new-developer]

## <a name="reset-a-user-password"></a>Een gebruikerswachtwoord opnieuw instellen
Als u het wachtwoord voor een gebruikersaccount herstellen, klikt u op de naam van het account.

![Wachtwoord opnieuw instellen][api-management-view-developer]

Klik op **wachtwoord opnieuw instellen** een koppeling naar de gebruiker om hun wachtwoord opnieuw in te verzenden.

![Wachtwoord opnieuw instellen][api-management-reset-password]

Om te werken via een programma met een gebruikersaccount, Zie de [entiteit gebruiker](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentatie in de [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) verwijzing. Als u het wachtwoord van een gebruiker op een specifieke waarde herstellen, kunt u de [bijwerken van een gebruiker](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) bewerking en geef het gewenste wachtwoord.

## <a name="pending-verification"></a>Verificatie van in behandeling
![Verificatie van in behandeling][api-management-pending-verification]

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
