---
title: Azure Role-Based toegangsbeheer (RBAC) om toegangsrechten te maken en beheren van ondersteuningsaanvragen | Microsoft Docs
description: Azure Role-Based toegangsbeheer (RBAC) om toegangsrechten te maken en beheren van aanvragen voor ondersteuning
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: azure
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: d98d0637c6d520193b11f4267c59016772ef063a
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792477"
---
# <a name="azure-role-based-access-control-rbac-to-control-access-rights-to-create-and-manage-support-requests"></a>Azure Role-Based toegangsbeheer (RBAC) om toegangsrechten te maken en beheren van aanvragen voor ondersteuning

[Rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) kunt u over Geavanceerd toegangsbeheer voor Azure.
Ondersteuning voor het maken van aanvraag in de Azure-portal [portal.azure.com](https://portal.azure.com), het model van Azure RBAC gebruikt om te definiëren die kunt maken en beheren van aanvragen voor ondersteuning.
Toegang wordt verleend door de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen met een bepaald bereik, dit kan een abonnement, resourcegroep of een resource.

We nemen een voorbeeld: U kunt alle resources onder de resourcegroep, zoals websites, virtuele machines en subnetten te beheren als een eigenaar van de groep resource met machtigingen voor lezen op het abonnementsbereik.
Echter, wanneer u probeert een ondersteuningsaanvraag op basis van de bron van de virtuele machine wilt maken, de volgende fout optreedt

![Abonnement-fout](./media/create-manage-support-requests-using-access-control/subscription-error.png)

Aanvraag in beheer van ondersteuning moet u toestemming of een functie die de actie ondersteuning Microsoft.Support/* op het abonnementsbereik heeft mogelijk te maken en beheren van ondersteuningsaanvragen schrijven.

Het volgende artikel wordt uitgelegd hoe u Azure van aangepaste rollen gebaseerd toegangsbeheer (RBAC) kunt gebruiken om te maken en beheren van aanvragen voor ondersteuning in Azure portal.

## <a name="getting-started"></a>Aan de slag

In het bovenstaande voorbeeld zou u een ondersteuningsaanvraag voor uw resource te maken als u een aangepaste RBAC-rol op het abonnement zijn toegewezen door de eigenaar van het abonnement zijn.
[Aangepaste RBAC-rollen](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) kunnen worden gemaakt met Azure PowerShell, Azure-opdrachtregelinterface (CLI) en de REST-API.

De eigenschap acties van een aangepaste rol Hiermee geeft u de Azure-bewerkingen waarmee de rol toegang verleent.
Voor het maken van een aangepaste rol voor het beheer van de aanvraag voor ondersteuning, moet de rol de actie Microsoft.Support/* hebben.

Hier volgt een voorbeeld van een aangepaste rol die u gebruiken kunt voor het maken en beheren van ondersteuningsaanvragen.
We hebben met de naam "Ondersteuning aanvragen bijdrager" van deze rol en dat is hoe verwijzen we naar de aangepaste rol die in dit artikel.

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

Volg de stappen in [in deze video](https://www.youtube.com/watch?v=-PaBaDmfwKI) voor informatie over het maken van een aangepaste rol voor uw abonnement.

## <a name="create-and-manage-support-requests-in-the-azure-portal"></a>Maken en beheren van aanvragen voor ondersteuning in Azure portal

We nemen een voorbeeld: u bent de eigenaar van het abonnement "Visual Studio MSDN-abonnement.
Jaap is die een resource-eigenaar tot een aantal van de resourcegroepen in dit abonnement is en leesrechten heeft voor het abonnement van uw peer.
Wilt u toegang geven tot uw peer, Jan, de mogelijkheid om te maken en beheren van ondersteuningstickets voor de resources in dit abonnement.

1. De eerste stap is om naar het abonnement te gaan en onder 'Instellingen' ziet u een lijst van gebruikers. Klik op de gebruiker Jan die lezerstoegang voor het abonnement heeft en laten we een nieuwe aangepaste rol toewijzen aan hem.

    ![Rol toevoegen](./media/create-manage-support-requests-using-access-control/add-role.png)

2. Klik op 'Toevoegen' op de blade 'Gebruikers'. Selecteer de aangepaste rol 'Ondersteuning aanvragen Inzender' uit de lijst met rollen

    ![Toevoegen van ondersteuning voor de rol van Inzender](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. Na het selecteren van de naam van de rol, klikt u op 'Gebruikers toevoegen' en voer de Jaaps e-referenties. Klik op Selecteren.

    ![Gebruikers toevoegen](./media/create-manage-support-requests-using-access-control/add-users.png)

4. Klik op "Ok" om door te gaan

    ![Toegang toevoegen](./media/create-manage-support-requests-using-access-control/add-access.png)

5. U ziet nu de gebruiker met de zojuist toegevoegde aangepaste rol 'Ondersteuning aanvragen Inzender' onder het abonnement waarvoor u de eigenaar bent

    ![Gebruiker is toegevoegd](./media/create-manage-support-requests-using-access-control/user-added.png)

    Wanneer de Jaap beschikbaar zijn in de portal, ziet hij het abonnement waaraan hij is toegevoegd.

7. Jaap klikt op 'Nieuwe ondersteuningsaanvraag' op de blade 'Help en ondersteuning voor' en aanvragen voor ondersteuning kunt maken voor "Visual Studio Ultimate met MSDN"

    ![Nieuwe ondersteuningsaanvraag](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. Te klikken op "Alle ondersteuningsaanvragen" Jaap ziet de lijst met aanvragen voor ondersteuning voor dit abonnement gemaakt ![Aanvraagdetails weergeven](./media/create-manage-support-requests-using-access-control/case-details-view.png)

## <a name="remove-support-request-access-in-the-azure-portal"></a>Ondersteuning aanvragen voor toegang in Azure portal verwijderen

Net zoals het is mogelijk om toegang te verlenen aan een gebruiker maken en beheren van ondersteuningsaanvragen, is het mogelijk toegang tot de voor de gebruiker ook verwijderen.
Als u wilt verwijderen van de mogelijkheid om te maken en beheren van ondersteuningsaanvragen, gaat u naar het abonnement, klikt u op 'Instellingen' en klik op de gebruiker (in dit geval Jaap).
Met de rechtermuisknop op de naam van de rol, "Ondersteuning aanvragen bijdrager" en klik op "Verwijderen"

![Ondersteuning aanvragen voor toegang verwijderen](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

Wanneer Jaap zich bij de portal aanmeldt en probeert te maken van een ondersteuningsaanvraag, optreedt hij de volgende fout

![Abonnement fout-2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

Jaap Zie niet alle ondersteuningsaanvragen wanneer hij klikt op "Alle ondersteuningsaanvragen"

![Aanvraagdetails weergeven-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
