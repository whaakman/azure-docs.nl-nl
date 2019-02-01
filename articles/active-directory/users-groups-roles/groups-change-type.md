---
title: Lidmaatschapstype van statische groep naar een dynamische - Azure Active Directory wijzigen | Microsoft Docs
description: Het maken van regels voor groepslidmaatschap voor het automatisch vullen van groepen, en een verwijzing naar de regel.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 7f8c9298c1e74e87411386a1d8ae36a34d986065
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55508484"
---
# <a name="change-static-group-membership-to-dynamic-in-azure-active-directory"></a>Statische groepslidmaatschap wijzigen naar een dynamische in Azure Active Directory

U kunt het lidmaatschap van een groep wijzigen van statisch in dynamisch (of omgekeerd) In Azure Active Directory (Azure AD). Azure AD houdt u de dezelfde naam en de ID in het systeem, zodat alle bestaande verwijzingen naar de groep nog steeds geldig zijn. Als u een nieuwe groep in plaats daarvan maakt, moet u deze verwijzingen bijwerken. Dynamisch groepslidmaatschap elimineert management overhead toevoegen en verwijderen van gebruikers. In dit artikel wordt uitgelegd hoe u bestaande groepen converteren van statisch naar dynamisch lidmaatschap met behulp van Azure AD-beheercentrum of PowerShell-cmdlets.

> [!WARNING]
> Wanneer u een bestaande statische groep naar een dynamische groep, alle bestaande leden zijn verwijderd uit de groep en klikt u vervolgens het lidmaatschapsregel is verwerkt, om nieuwe leden toevoegen. Als de groep wordt gebruikt voor het beheren van toegang tot apps of resources, er rekening mee dat de oorspronkelijke leden mogelijk geen toegang meer tot het lidmaatschapsregel volledig wordt verwerkt.
>
> Het is raadzaam dat u de nieuwe lidmaatschapsregel tevoren testen om ervoor te zorgen dat het nieuwe lidmaatschap in de groep is zoals verwacht.

## <a name="change-the-membership-type-for-a-group"></a>Het lidmaatschapstype voor een groep wijzigen

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat een globale beheerder of een beheerder van gebruikersaccounts in uw tenant.
2. Selecteer **groepen**.
3. Uit de **alle groepen** weergeven, opent u de groep die u wilt wijzigen.
4. Selecteer **eigenschappen**.
5. Op de **eigenschappen** pagina voor de groep, selecteer een **lidmaatschapstype** van toegewezen (statisch), dynamische gebruiker of dynamische apparaat, afhankelijk van uw gewenste lidmaatschapstype. Voor dynamisch lidmaatschap, kunt u de opbouwfunctie voor regel selecteren opties voor een eenvoudige regel of een lidmaatschapsregel zelf schrijven. 

De volgende stappen zijn een voorbeeld van een groep wijzigen van statisch naar dynamisch lidmaatschap voor een groep gebruikers.

1. Op de **eigenschappen** pagina voor de geselecteerde groep, selecteer een **lidmaatschapstype** van **dynamische gebruiker**, selecteert u Ja in het dialoogvenster waarin wordt uitgelegd van de wijzigingen aan de groep lidmaatschap om door te gaan. 
  
   ![Selecteer het lidmaatschapstype van dynamische gebruiker](./media/groups-change-type/select-group-to-convert.png)
  
2. Selecteer **dynamische query toevoegen**, en geef vervolgens de regel.
  
   ![Geef de regel](./media/groups-change-type/enter-rule.png)
  
3. Na het maken van de regel, selecteer **query toevoegen** aan de onderkant van de pagina.
4. Selecteer **opslaan** op de **eigenschappen** pagina voor de groep uw wijzigingen op te slaan. De **lidmaatschapstype** van de groep is direct bijgewerkt in de lijst met groepen.

> [!TIP]
> Groepsconversie kan mislukken als het lidmaatschapsregel die u hebt ingevoerd onjuist is. Een melding wordt weergegeven in de rechterbovenhoek hoek rechtsboven van de portal die deze bevat een uitleg waarom de regel kan niet worden geaccepteerd door het systeem. Lees zorgvuldig om te begrijpen hoe u de regel zodat deze geldige kunt aanpassen. Zie voor voorbeelden van de regelsyntaxis van de en een volledige lijst van de ondersteunde eigenschappen, operators en waarden voor de regel voor een verzameling [dynamisch-lidmaatschapregels voor groepen in Azure Active Directory](groups-dynamic-membership.md).


## <a name="change-membership-type-for-a-group-powershell"></a>Wijzigen van het lidmaatschapstype voor een groep (PowerShell)

> [!NOTE]
> U moet het gebruik van cmdlets van eigenschappen voor dynamische groep wijzigen **de preview-versie van** [Azure AD PowerShell versie 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0). U kunt installeren de Preview-versie van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureADPreview).

Hier volgt een voorbeeld van de functies die het lidmaatschap management op een bestaande groep overschakelen. In dit voorbeeld is care uitgevoerd, correct manipuleren van de eigenschap GroupTypes en behouden van alle waarden die niet gerelateerd aan dynamisch lidmaatschap zijn.

```
#The moniker for dynamic groups as used in the GroupTypes property of a group object
$dynamicGroupTypeString = "DynamicMembership"

function ConvertDynamicGroupToStatic
{
    Param([string]$groupId)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -eq $null -or !$groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a static group. Aborting conversion.";
    }


    #remove the type for dynamic groups, but keep the other type values
    $groupTypes.Remove($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to remove the dynamic type, ii) pause execution of the current rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "Paused"
}

function ConvertStaticGroupToDynamic
{
    Param([string]$groupId, [string]$dynamicMembershipRule)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -ne $null -and $groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a dynamic group. Aborting conversion.";
    }
    #add the dynamic group type to existing types
    $groupTypes.Add($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to add the dynamic type, ii) start execution of the rule, iii) set the rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "On" -MembershipRule $dynamicMembershipRule
}
```
Een groep statische maken:

```
ConvertDynamicGroupToStatic "a58913b2-eee4-44f9-beb2-e381c375058f"
```

U een groep dynamisch:

```
ConvertStaticGroupToDynamic "a58913b2-eee4-44f9-beb2-e381c375058f" "user.displayName -startsWith ""Peter"""
```

## <a name="next-steps"></a>Volgende stappen

Deze artikelen bevatten aanvullende informatie over groepen in Azure Active Directory.

* [Bestaande groepen weergeven](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toevoegen](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Lidmaatschappen van een groep beheren](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](groups-dynamic-membership.md)
