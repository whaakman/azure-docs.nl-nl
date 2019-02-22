---
title: Over het maken van een rol registratie voor Azure Stack
description: Het maken van een aangepaste rol om te voorkomen met behulp van globale beheerder voor de registratie.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2019
ms.author: patricka
ms.reviewer: rtiberiu
ms.lastreviewed: 02/13/2019
ms.openlocfilehash: 4858e33023ff0f94bb9fb879b01846184df2491e
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56652141"
---
# <a name="create-a-registration-role-for-azure-stack"></a>Een registratie-rol maken voor Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Voor scenario's waarin u eigenaarsmachtigingen geven in het Azure-abonnement niet wilt, kunt u een aangepaste rol machtigingen toewijzen aan een gebruikersaccount voor het registreren van uw Azure-Stack maken.

> [!WARNING]
> Dit is niet een beveiligingsfunctie verbeteren. Worden gebruikt in scenario's waar u beperkingen instellen om te voorkomen dat onbedoelde wijzigingen aan het Azure-abonnement. Wanneer een gebruiker overgedragen rechten aan deze aangepaste rol is, heeft de gebruiker rechten voor het bewerken van machtigingen en rechten, met verhoogde bevoegdheden. Alleen gebruikers die u op de aangepaste rol vertrouwt toewijzen.

Bij het registreren van Azure Stack, is het registratie-account vereist de volgende Azure Active Directory-machtigingen en de machtigingen van de Azure-abonnement:

* **Registratie-machtigingen van de toepassing in uw Azure Active Directory-tenant:** Beheerders hebben machtigingen van de registratie van de toepassing. De machtiging voor gebruikers is een algemene instelling voor alle gebruikers in de tenant. Als u wilt weergeven of wijzigen van de instelling, Zie [maken van een Azure AD-toepassing en service-principal die toegang hebben tot resources](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions).

    De *gebruikers kan toepassingen registreren* instelling moet worden ingesteld op **Ja** voor u om in te schakelen van een gebruikersaccount voor het registreren van Azure Stack. Als de instelling app-registraties is ingesteld op **Nee**, u een gebruikersaccount kan niet worden gebruikt en moet een globale beheerdersaccount gebruiken voor het registreren van Azure Stack.

* **Een set met voldoende machtigingen voor Azure-abonnement:** Gebruikers in de groep Eigenaren hebben voldoende machtigingen. Voor andere accounts, kunt u de machtiging is ingesteld door een aangepaste rol toewijzen, zoals wordt beschreven in de volgende secties toewijzen.

## <a name="create-a-custom-role-using-powershell"></a>Maak een aangepaste rol die met behulp van PowerShell

Voor het maken van een aangepaste rol, moet u de `Microsoft.Authorization/roleDefinitions/write` machtiging op alle `AssignableScopes`, zoals [eigenaar](../role-based-access-control/built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](../role-based-access-control/built-in-roles.md#user-access-administrator). Gebruik de volgende JSON-sjabloon voor het vereenvoudigen van de aangepaste rol definiëren. De sjabloon maakt u een aangepaste rol waarmee de vereiste lees- en schrijftoegang voor registratie in Azure Stack.

1. Maak een JSON-bestand. Bijvoorbeeld:  `C:\CustomRoles\registrationrole.json`
2. Voeg de volgende JSON-code toe aan het bestand. Vervang <SubscriptionID> door de id van uw Azure-abonnement.

    ```json
    {
      "Name": "Azure Stack registration role",
      "Id": null,
      "IsCustom": true,
      "Description": "Allows access to register Azure Stack",
      "Actions": [
        "Microsoft.Resources/subscriptions/resourceGroups/write",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.AzureStack/registrations/*",
        "Microsoft.AzureStack/register/action",
        "Microsoft.Authorization/roleAssignments/read",
        "Microsoft.Authorization/roleAssignments/write",
        "Microsoft.Authorization/roleAssignments/delete",
        "Microsoft.Authorization/permissions/read"
      ],
      "NotActions": [
      ],
      "AssignableScopes": [
        "/subscriptions/<SubscriptionID>"
      ]
    }
    ```

3. Verbinding maken met Azure aan het gebruik van Azure Resource Manager in PowerShell. Wanneer u hierom wordt gevraagd, moet u verifiëren met een account met de juiste machtigingen, zoals [eigenaar](../role-based-access-control/built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](../role-based-access-control/built-in-roles.md#user-access-administrator).

    ```azurepowershell
    Connect-AzureRmAccount
    ```

4. U kunt de functie toevoegen aan het abonnement met **New-AzureRmRoleDefinition** het JSON-sjabloonbestand op te geven.

    ``` azurepowershell
    New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\registrationrole.json"
    ```

## <a name="assign-a-user-to-registration-role"></a>Een gebruiker toewijzen aan de rol van de registratie

Nadat de registratie van de aangepaste rol is gemaakt, moet u de rol-gebruikers registreren van Azure Stack toewijzen.

1. Aanmelden met het account met voldoende machtigingen op het Azure-abonnement om te delegeren rechten - zoals [eigenaar](../role-based-access-control/built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](../role-based-access-control/built-in-roles.md#user-access-administrator) .
2. In **abonnementen**, selecteer **toegangsbeheer (IAM) > roltoewijzing toevoegen**.
3. In **rol**, kiest u de aangepaste rol die u hebt gemaakt *Azure Stack-registratie rol*.
4. Selecteer de gebruikers die u wilt toewijzen aan de rol.
5. Selecteer **opslaan** de geselecteerde gebruikers toewijzen aan de rol.

    ![Gebruikers toewijzen aan rol selecteren](media/azure-stack-registration-role/assign-role.png)

Zie voor meer informatie over het gebruik van aangepaste rollen [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Volgende stappen

[Azure Stack registreren bij Azure](azure-stack-registration.md)
