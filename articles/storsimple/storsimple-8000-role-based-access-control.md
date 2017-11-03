---
title: Toegangsbeheer op basis van rollen voor StorSimple gebruiken | Microsoft Docs
description: Beschrijft hoe u Azure rollen gebaseerd toegangsbeheer (RBAC) in de context van StorSimple.
services: storsimple
documentationcenter: 
author: alkohli
manager: jconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2017
ms.author: alkohli
ms.openlocfilehash: d040849360a47c611d44b3a5d7649c685dcc8068
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2017
---
# <a name="role-based-access-control-for-storsimple"></a>Toegangsbeheer op basis van rollen voor StorSimple

Dit artikel bevat een korte beschrijving van hoe gebaseerd toegangsbeheer (RBAC) voor uw StorSimple-apparaat kan worden gebruikt. RBAC biedt Geavanceerd toegangsbeheer voor Azure. Met RBAC kunt alleen de juiste hoeveelheid toegang tot de StorSimple-gebruikers om hun werk in plaats van zorgen dat iedereen onbeperkte toegang verlenen. Zie voor meer informatie over de basisprincipes van toegangsbeheer in Azure, [aan de slag met toegangsbeheer op basis van rollen in Azure portal](../active-directory/role-based-access-control-what-is.md).

In dit artikel is van toepassing op StorSimple 8000 series apparaten met Update 3.0 of hoger in de Azure portal.

## <a name="rbac-roles-for-storsimple"></a>RBAC-rollen voor StorSimple

RBAC kan worden toegewezen op basis van de rollen. De rollen ervoor zorgen dat bepaalde machtigingsniveaus op basis van de beschikbare bronnen in de omgeving. Er zijn twee typen van functies die StorSimple gebruikers kunnen kiezen uit: ingebouwde of aangepaste.

* **Ingebouwde rollen** -de ingebouwde rollen zijn eigenaar, bijdrager, lezer en beheerder voor gebruikerstoegang. Zie voor meer informatie [ingebouwde functies voor toegangsbeheer op basis van rollen in Azure](../active-directory/role-based-access-control-what-is.md#built-in-roles).

* **Aangepaste rollen** -als de ingebouwde rollen niet behoeften, kunt u aangepaste RBAC-rollen maken voor StorSimple. Beginnen met een ingebouwde rol voor het maken van een aangepaste RBAC-rol, bewerken en vervolgens importeren in de omgeving. Het downloaden en het uploaden van de rol worden beheerd met behulp van Azure PowerShell of Azure CLI. Zie voor meer informatie [aangepaste rollen maken voor toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-custom-roles.md).

Als u wilt weergeven van de verschillende functies die beschikbaar is voor de gebruiker van een StorSimple-apparaat in de Azure portal, gaat u naar de service voor uw StorSimple-Apparaatbeheer en gaat u naar **toegangsbeheer (IAM) > rollen**.


## <a name="create-a-custom-role-for-storsimple-infrastructure-administrator"></a>Maak een aangepaste beveiligingsrol infrastructuurbeheerder voor StorSimple

In het volgende voorbeeld we beginnen met de ingebouwde functie **lezer** waarmee gebruikers om alle scopes van de resource weer te geven, maar niet op deze bewerken of nieuwe maken. We uitbreiden deze rol voor het maken van een nieuwe aangepaste rol beheerder van de StorSimple-infrastructuur Deze rol wordt toegewezen aan gebruikers die de infrastructuur voor de StorSimple-apparaten kunnen beheren.

1. Voer Windows PowerShell als beheerder.

2. Aanmelden bij Azure.

    `Login-AzureRMAccount`

3. De rol Lezer exporteren als een JSON-sjabloon op uw computer.

    ```
    Get-AzureRMRoleDefinition -Name "Reader"

    Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
    ```

4. De JSON-bestand openen in Visual Studio. U ziet dat er een typische RBAC-rol uit drie gedeelten bestaat **acties**, **NotActions**, en **AssignableScopes**.

    In de **actie** sectie alle de toegestane bewerkingen voor deze rol worden vermeld. Elke actie is van een resourceprovider toegewezen. Voor de beheerder van een StorSimple-infrastructuur, gebruikt u de `Microsoft.StorSimple` resourceprovider.

    Gebruik PowerShell om alle resourceproviders beschikbaar is en geregistreerd in uw abonnement.

    `Get-AzureRMResourceProvider`

    U kunt ook controleren voor alle beschikbare PowerShell-cmdlets voor het beheren van de resourceproviders.

    In de **NotActions** secties alle de beperkte acties voor een bepaalde RBAC-rol worden vermeld. Er zijn geen acties in dit voorbeeld wordt beperkt.
    
    Onder de **AssignableScopes**, de abonnement-id's worden vermeld. Zorg ervoor dat de RBAC-rol bevat de expliciete abonnements-ID waar deze wordt gebruikt. Als de juiste abonnements-ID niet is opgegeven, wordt u niet toegestaan voor het importeren van de rol in uw abonnement.

    Bewerk het bestand Houd in gedachten de voorgaande overwegingen.

    ```
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

6. De aangepaste RBAC-rol opnieuw importeren in de omgeving.

    `New-AzureRMRoleDefinition -InputFile "C:\ssrbaccustom.json"`


Deze rol wordt nu weergegeven in de lijst met rollen in de **toegangsbeheer** blade.

![RBAC-rollen weergeven](./media/storsimple-8000-role-based-access-control/rbac-role-types.png)

Ga voor meer informatie naar [maakt u een aangepaste RBAC-rol met behulp van PowerShell](../active-directory/role-based-access-control-create-custom-roles-for-internal-external-users.md#create-a-custom-rbac-role-to-open-support-requests-using-powershell).

### <a name="sample-output-for-custom-role-creation-via-the-powershell"></a>Voorbeeld van uitvoer voor de aangepaste beveiligingsrol maken via de PowerShell

```
PS C:\WINDOWS\system32> Login-AzureRMAccount

Environment           : AzureCloud
Account               : john.doe@contoso.com
TenantId              : <tenant_ID>
SubscriptionId        : <subscription_ID>
SubscriptionName      : Internal Consumption
CurrentStorageAccount :

PS C:\WINDOWS\system32> Get-AzureRMRoleDefinition -Name "Reader"

Name             : Reader
Id               : <guid>
IsCustom         : False
Description      : Lets you view everything, but not make any changes.
Actions          : {*/read}
NotActions       : {}
AssignableScopes : {/}

PS C:\WINDOWS\system32> Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json

PS C:\WINDOWS\system32> New-AzureRMRoleDefinition -InputFile "C:\ssrbaccustom.json"

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

PS C:\WINDOWS\system32>
```

## <a name="add-users-to-the-custom-role"></a>Gebruikers toevoegen aan de aangepaste rol

U verleent toegang vanuit de resource en resourcegroep die of het abonnement dat het bereik van de roltoewijzing is. Wanneer ze toegang verlenen, draagt u er rekening mee dat de toegang verleend aan het bovenliggende knooppunt wordt overgenomen door de onderliggende. Ga voor meer informatie naar [Resource hiërarchie en toegang overname](../active-directory/role-based-access-control-what-is.md#resource-hierarchy-and-access-inheritance).

1. Ga naar **toegangsbeheer (IAM)**. Klik op **+ toevoegen** op de Access control-blade.

    ![Toegang tot met RBAC-rol toevoegen](./media/storsimple-8000-role-based-access-control/rbac-add-role.png)

2. Selecteer de rol die u wilt toewijzen, in dit geval is de **StorSimple infrastructuur Admin**.

3. Selecteer de gebruiker, groep of toepassing in de directory aan wie/waaraan u toegang wilt verlenen. U kunt zoeken in de directory met weergavenamen, e-mailadressen en object-id's.

4. Selecteer **opslaan** de toewijzing te maken.

    ![Voeg machtigingen toe aan de RBAC-rol](./media/storsimple-8000-role-based-access-control/rbac-create-role-infra-admin.png)

Een **gebruiker toevoegen** melding wordt de voortgang bijgehouden. Nadat de gebruiker is toegevoegd, wordt de lijst met gebruikers in de Access control bijgewerkt.

## <a name="view-permissions-for-the-custom-role"></a>Machtigingen voor de aangepaste rol weergeven

Zodra deze rol is gemaakt, kunt u de machtigingen die zijn gekoppeld aan deze rol in de Azure portal kunt weergeven.

1. De machtigingen die zijn gekoppeld aan deze rol, Ga naar **toegangsbeheer (IAM) > rollen > StorSimple infrastructuur Admin**. De lijst met gebruikers met deze rol wordt weergegeven.

2. Selecteer een gebruiker met beheerdersrechten van StorSimple-infrastructuur en klik op **machtigingen**.

    ![Machtigingen voor de rol StorSimple Infra Admin weergeven](./media/storsimple-8000-role-based-access-control/rbac-roles-view-permissions.png)

3. De machtigingen die zijn gekoppeld aan deze rol worden weergegeven.

    ![Gebruikers in rol StorSimple Infra Admin weergeven](./media/storsimple-8000-role-based-access-control/rbac-infra-admin-permissions1.png)


## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [aangepaste rollen voor interne en externe gebruikers toewijzen](../active-directory/role-based-access-control-create-custom-roles-for-internal-external-users.md).

