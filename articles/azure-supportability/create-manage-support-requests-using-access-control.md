---
title: Azure op rollen gebaseerde toegangsbeheer (RBAC) om toegangsrechten te maken en beheren van ondersteuningsaanvragen | Microsoft Docs
description: Azure op rollen gebaseerde toegangsbeheer (RBAC) om toegangsrechten te maken en beheren van aanvragen voor ondersteuning
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: 20ebd324cbf379980b43d255d468673de2b6d950
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-role-based-access-control-rbac-to-control-access-rights-to-create-and-manage-support-requests"></a>Azure op rollen gebaseerde toegangsbeheer (RBAC) om toegangsrechten te maken en beheren van aanvragen voor ondersteuning

[Op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) kunt Geavanceerd toegangsbeheer voor Azure.
Ondersteuning voor het maken van de aanvraag in de Azure portal [portal.azure.com](https://portal.azure.com), maakt gebruik van Azure RBAC-model om te definiëren die kunt maken en beheren van aanvragen voor ondersteuning.
Toegang wordt verleend door de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen op een bepaalde scope, kan dit een abonnement, resourcegroep of een resource.

Voorbeeld: als een eigenaar van de groep resource met leesmachtigingen voor de scope abonnement kunt u alle resources onder de resourcegroep, zoals websites, virtuele machines en subnetten beheren.
Echter, wanneer u probeert om een ondersteuningsaanvraag op basis van de bron van de virtuele machine te maken, de volgende fout optreedt

![Abonnement-fout](./media/create-manage-support-requests-using-access-control/subscription-error.png)

U moet in het beheer van de aanvraag van ondersteuning machtiging of een functie die de actie ondersteuning Microsoft.Support/* bij het abonnementsbereik heeft te kunnen maken en beheren van ondersteuningsaanvragen schrijven.

Het volgende artikel wordt uitgelegd hoe u Azure aangepaste op rollen gebaseerde toegangsbeheer (RBAC) maken en beheren van aanvragen voor ondersteuning in de Azure portal kunt gebruiken.

## <a name="getting-started"></a>Aan de slag

In het bovenstaande voorbeeld zou u een ondersteuningsaanvraag wilt indienen voor uw resource maken als u een aangepaste RBAC-rol op het abonnement zijn toegewezen door de eigenaar van het abonnement zijn.
[Aangepaste RBAC-rollen](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) kunnen worden gemaakt met Azure PowerShell, Azure-opdrachtregelinterface (CLI) en de REST-API.

De eigenschap acties van een aangepaste beveiligingsrol Hiermee geeft u de Azure-bewerkingen waarvoor de rol toegang verleent.
Voor het maken van een aangepaste rol voor het beheer van de aanvraag ondersteuning, moet de rol de actie Microsoft.Support/* hebben.

Hier volgt een voorbeeld van een aangepaste rol die u kunt maken en beheren van aanvragen voor ondersteuning.
We deze rol 'Ondersteuning vragen Inzender' hebt genoemd en hoe we verwijzen naar de aangepaste rol die in dit artikel is.

``` Json
{
    "Name":  "Support Request Contributor",
    "Id":  "1f2aad59-39b0-41da-b052-2fb070bd7942",
    "IsCustom":  true,
    "Description":  "Lets you create and manage support tickets.",
    "Actions":  [
                    "Microsoft.Support/*"
                ],
    "NotActions":  [
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Volg de stappen die worden beschreven in [in deze video](https://www.youtube.com/watch?v=-PaBaDmfwKI) voor informatie over het maken van een aangepaste beveiligingsrol voor uw abonnement.

## <a name="create-and-manage-support-requests-in-the-azure-portal"></a>Maken en beheren van aanvragen voor ondersteuning in de Azure portal

Voorbeeld: u bent de eigenaar van het abonnement "Visual Studio MSDN-abonnement."
Jan is uw peer wie de eigenaar van een resource op sommige van de resourcegroepen in dit abonnement is en leesrechten heeft voor het abonnement.
U wilt toegang geven tot uw peer, Jan, de mogelijkheid om te maken en beheren van ondersteuningstickets voor de resources onder dit abonnement.

1. De eerste stap is om naar het abonnement te gaan en onder 'Instellingen' ziet u een lijst met gebruikers. Klik op de gebruiker Jan die lezerstoegang heeft tot het abonnement en gaan we een nieuwe aangepaste beveiligingsrol aan hem toewijzen.

    ![Functie toevoegen](./media/create-manage-support-requests-using-access-control/add-role.png)

2. Klik op "Toevoegen" onder de blade 'Gebruikers'. Selecteer de aangepaste rol 'Ondersteuning vragen Inzender' uit de lijst met rollen

    ![Ondersteuning voor de rol van Inzender toevoegen](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. Klik op 'Gebruikers toevoegen' en voer de Jan e referenties na het selecteren van de naam van de rol. Klik op 'Selecteren'

    ![Gebruikers toevoegen](./media/create-manage-support-requests-using-access-control/add-users.png)

4. Klik op 'Ok' om door te gaan

    ![Toegang toevoegen](./media/create-manage-support-requests-using-access-control/add-access.png)

5. U ziet nu de gebruiker met de zojuist toegevoegde aangepaste rol 'Ondersteuning vragen Inzender' onder het abonnement waarvoor u de eigenaar bent

    ![Toegevoegde gebruiker](./media/create-manage-support-requests-using-access-control/user-added.png)

    Wanneer Jan zich in de portal aanmeldt, ziet hij het abonnement waaraan hij is toegevoegd.

7. Jan klikt op 'Nieuw ondersteuningsverzoek' op de blade 'Help en ondersteuning voor' en ondersteuningsaanvragen kunt maken voor 'Visual Studio Ultimate met MSDN'

    ![Nieuw ondersteuningsverzoek](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. Te klikken op 'Alle ondersteunen aanvragen' Jan bevat een overzicht van ondersteuningsaanvragen gemaakt voor dit abonnement ![Aanvraagdetails weergeven](./media/create-manage-support-requests-using-access-control/case-details-view.png)

## <a name="remove-support-request-access-in-the-azure-portal"></a>Toegang voor ondersteuning aanvragen in de Azure portal verwijderen

Net zoals het is mogelijk om toegang te verlenen aan een gebruiker maken en beheren van ondersteuningsaanvragen, is het mogelijk om toegang voor de gebruiker ook te verwijderen.
Om te kunnen maken en beheren van ondersteuningsaanvragen, gaat u naar het abonnement verwijderen, klik op 'Instellingen' en klik op de gebruiker (in dit geval Jan).
Met de rechtermuisknop op de naam van de rol, 'Ondersteuning vragen Inzender' en klik op 'Verwijderen'

![Ondersteuning aanvragen voor toegang verwijderen](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

Wanneer Jan zich bij de portal aanmeldt en probeert om een ondersteuningsaanvraag te maken, wordt hij de volgende fout aangetroffen

![Abonnement fout-2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

Jan niet zien alle aanvragen ondersteunen wanneer hij klikt op "Alle ondersteuning aanvragen"

![Aanvraagdetails weergave-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
