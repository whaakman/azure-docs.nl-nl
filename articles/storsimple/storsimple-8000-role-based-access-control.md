---
title: Role-based Access Control gebruikt voor StorSimple | Microsoft Docs
description: Beschrijft hoe u Azure Role-based Access Control (RBAC) in de context van StorSimple gebruiken.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2017
ms.author: alkohli
ms.openlocfilehash: be0c1611856a1fa68d20696c32b5fadcd8572004
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793608"
---
# <a name="role-based-access-control-for-storsimple"></a>Toegangsbeheer op basis van rollen voor StorSimple

Dit artikel bevat een korte beschrijving van hoe de op rollen gebaseerd toegangsbeheer (RBAC) voor uw StorSimple-apparaat kan worden gebruikt. RBAC biedt verfijnd toegangsbeheer voor Azure. Gebruik RBAC om precies het juiste aantal toegang tot de StorSimple-gebruikers kunnen hun werk in plaats van iedereen onbeperkte toegang verlenen. Zie voor meer informatie over de basisprincipes van beheer van toegang in Azure, [aan de slag met toegangsbeheer op basis van rollen in Azure portal](../role-based-access-control/overview.md).

In dit artikel is van toepassing op StorSimple 8000-serie apparaten met Update 3.0 of hoger in de Azure-portal.

## <a name="rbac-roles-for-storsimple"></a>RBAC-rollen voor StorSimple

RBAC kan worden toegewezen op basis van de rollen. De rollen die ervoor zorgen dat bepaalde machtigingsniveaus op basis van de beschikbare resources in de omgeving. Er zijn twee typen van die StorSimple-gebruikers kunnen kiezen uit: ingebouwde of aangepaste.

* **Ingebouwde rollen** -de ingebouwde rollen kunnen zijn eigenaar, bijdrager, lezer of administrator voor gebruikerstoegang. Zie voor meer informatie, [ingebouwde rollen voor Azure Role-based Access Control](../role-based-access-control/built-in-roles.md).

* **Aangepaste rollen** -als de ingebouwde rollen niet behoeften, kunt u aangepaste RBAC-rollen maken voor StorSimple. Voor het maken van een aangepaste RBAC-rol, beginnen met een ingebouwde rol, bewerken en vervolgens importeren in de omgeving. Het downloaden en het uploaden van de rol worden beheerd met behulp van Azure PowerShell of Azure CLI. Zie voor meer informatie, [aangepaste rollen maken voor Role-based Access Control](../role-based-access-control/custom-roles.md).

Als u wilt weergeven van de verschillende rollen die beschikbaar zijn voor de gebruiker van een StorSimple-apparaat in Azure portal, gaat u naar uw StorSimple Device Manager-service en ga vervolgens naar **toegangsbeheer (IAM) > rollen**.


## <a name="create-a-custom-role-for-storsimple-infrastructure-administrator"></a>Een aangepaste rol maken voor StorSimple infrastructuurbeheerder

In het volgende voorbeeld beginnen we met de ingebouwde rol **lezer** waarmee gebruikers om alle scopes van de resource weer te geven, maar niet bewerken of nieuwe labels maken. We deze rol voor het maken van een nieuwe aangepaste rol infrastructuur van StorSimple-beheerder. vervolgens kunnen worden uitgebreid Deze rol is toegewezen aan gebruikers die de infrastructuur voor de StorSimple-apparaten kunnen beheren.

1. Windows PowerShell ook uitvoeren als beheerder.

2. Aanmelden bij Azure.

    `Connect-AzureRmAccount`

3. De rol van lezer exporteren als een JSON-sjabloon op uw computer.

    ```powershell
    Get-AzureRMRoleDefinition -Name "Reader"

    Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
    ```

4. Open het JSON-bestand in Visual Studio. U ziet dat een typische RBAC-rol uit drie gedeelten bestaat **acties**, **NotActions**, en **AssignableScopes**.

    In de **actie** sectie, alle de toegestane bewerkingen voor deze rol worden vermeld. Elke actie is van een resourceprovider toegewezen. Voor de beheerder van een StorSimple-infrastructuur, gebruikt u de `Microsoft.StorSimple` resourceprovider.

    PowerShell gebruiken om te zien van alle resourceproviders beschikbaar zijn en geregistreerd in uw abonnement.

    `Get-AzureRMResourceProvider`

    U kunt ook controleren voor alle beschikbare PowerShell-cmdlets voor het beheren van de resourceproviders.

    In de **NotActions** secties, alle beperkte acties voor een bepaalde RBAC-rol worden vermeld. In dit voorbeeld zijn er geen acties beperkt.
    
    Onder de **AssignableScopes**, de abonnement-id's worden vermeld. Zorg ervoor dat de RBAC-rol bevat de expliciete abonnements-ID waar deze wordt gebruikt. Als de juiste abonnements-ID niet opgegeven is, wordt u niet toegestaan voor het importeren van de rol in uw abonnement.

    Bewerk het bestand te houden rekening met de voorgaande overwegingen.

    ```json
    {
        "Name":  "StorSimple Infrastructure Admin",
        "Id":  "<guid>",
        "IsCustom":  true,
        "Description":  "Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install, download etc.",
        "Actions":  [
                        "Microsoft.StorSimple/managers/alerts/read",
                        "Microsoft.StorSimple/managers/devices/volumeContainers/read",
                        "Microsoft.StorSimple/managers/devices/jobs/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/write",
                        "Microsoft.StorSimple/managers/clearAlerts/action",
                        "Microsoft.StorSimple/managers/devices/networkSettings/read",
                        "Microsoft.StorSimple/managers/devices/publishSupportPackage/action",
                        "Microsoft.StorSimple/managers/devices/scanForUpdates/action",
                        "Microsoft.StorSimple/managers/devices/metrics/read"

                    ],
        "NotActions":  [

                    ],
        "AssignableScopes":  [
                                "/subscriptions/<subscription_ID>/"
                            ]
    }
    ```

6. Importeer de aangepaste RBAC-rol terug in de omgeving.

    `New-AzureRMRoleDefinition -InputFile "C:\ssrbaccustom.json"`


Deze rol wordt nu weergegeven in de lijst met rollen in de **toegangsbeheer** blade.

![RBAC-rollen weergeven](./media/storsimple-8000-role-based-access-control/rbac-role-types.png)

Ga voor meer informatie naar [aangepaste rollen](../role-based-access-control/custom-roles.md).

### <a name="sample-output-for-custom-role-creation-via-the-powershell"></a>Voorbeeld van uitvoer voor de aangepaste rol maken via de PowerShell

```powershell
Connect-AzureRmAccount
```

```Output
Environment           : AzureCloud
Account               : john.doe@contoso.com
TenantId              : <tenant_ID>
SubscriptionId        : <subscription_ID>
SubscriptionName      : Internal Consumption
CurrentStorageAccount :
```

```powershell
Get-AzureRMRoleDefinition -Name "Reader"
```

```Output
Name             : Reader
Id               : <guid>
IsCustom         : False
Description      : Lets you view everything, but not make any changes.
Actions          : {*/read}
NotActions       : {}
AssignableScopes : {/}
```

```powershell
Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
New-AzureRMRoleDefinition -InputFile "C:\ssrbaccustom.json"
```

```Output
Name             : StorSimple Infrastructure Admin
Id               : <tenant_ID>
IsCustom         : True
Description      : Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install,
                   download etc.
Actions          : {Microsoft.StorSimple/managers/alerts/read,
                   Microsoft.StorSimple/managers/devices/volumeContainers/read,
                   Microsoft.StorSimple/managers/devices/jobs/read,
                   Microsoft.StorSimple/managers/devices/alertSettings/read...}
NotActions       : {}
AssignableScopes : {/subscriptions/<subscription_ID>/}
```

## <a name="add-users-to-the-custom-role"></a>Gebruikers aan de aangepaste rol toevoegen

U verleent toegang vanuit de resource en resourcegroep die of het abonnement dat het bereik van de roltoewijzing is. Bij toegang, draagt u er rekening mee dat de toegang is verleend op het bovenliggende knooppunt wordt overgenomen door het onderliggende object. Ga voor meer informatie naar [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md).

1. Ga naar **toegangsbeheer (IAM)**. Klik op **+ toevoegen** op de blade toegangsbeheer.

    ![Toegang tot met RBAC-rol toevoegen](./media/storsimple-8000-role-based-access-control/rbac-add-role.png)

2. Selecteer de rol die u wilt toewijzen, in dit geval is de **StorSimple infrastructuur beheerder**.

3. Selecteer de gebruiker, groep of toepassing in de directory aan wie/waaraan u toegang wilt verlenen. U kunt zoeken in de directory met weergavenamen, e-mailadressen en object-id's.

4. Selecteer **opslaan** om de toewijzing te maken.

    ![Voeg machtigingen toe aan de RBAC-rol](./media/storsimple-8000-role-based-access-control/rbac-create-role-infra-admin.png)

Een **gebruiker toevoegen** melding wordt de voortgang bijgehouden. Nadat de gebruiker is toegevoegd, wordt de lijst met gebruikers in de Access control wordt bijgewerkt.

## <a name="view-permissions-for-the-custom-role"></a>Machtigingen voor de aangepaste rol weergeven

Zodra deze rol is gemaakt, kunt u de machtigingen die zijn gekoppeld aan deze rol in Azure portal kunt weergeven.

1. Als u de machtigingen die zijn gekoppeld aan deze rol, gaat u naar **toegangsbeheer (IAM) > rollen > StorSimple infrastructuur beheerder**. De lijst met gebruikers in deze rol wordt weergegeven.

2. Selecteer een gebruiker met beheerdersrechten voor StorSimple-infrastructuur en klik op **machtigingen**.

    ![Machtigingen voor de rol Admin Infra StorSimple weergeven](./media/storsimple-8000-role-based-access-control/rbac-roles-view-permissions.png)

3. De machtigingen die zijn gekoppeld aan deze rol worden weergegeven.

    ![Gebruikers in rol Admin Infra StorSimple weergeven](./media/storsimple-8000-role-based-access-control/rbac-infra-admin-permissions1.png)


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [aangepaste rollen toewijzen voor interne en externe gebruikers](../role-based-access-control/role-assignments-external-users.md).