---
title: Hoe veilig om gebruikers te migreren tussen productlicenties met behulp van Groepsbeleid licentieverlening in Azure Active Directory | Microsoft Docs
description: Beschrijft de aanbevolen procedure voor het migreren van gebruikers tussen verschillende productlicenties (zoals Office 365 E1 en E3) met behulp van Groepsbeleid-licentieverlening
services: active-directory
keywords: Azure AD-licentieverlening
documentationcenter: 
author: piotrci
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/07/2018
ms.author: piotrci
ms.openlocfilehash: 97654673b395fd5b8cb41afdcdeaa21aba44f61d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-safely-migrate-users-between-product-licenses-using-group-based-licensing"></a>Hoe veilig om gebruikers te migreren tussen productlicenties met behulp van Groepsbeleid-licentieverlening

In dit artikel beschrijft de aanbevolen methode voor het verplaatsen van gebruikers tussen productlicenties wanneer u op basis van een groep licenties. Het doel van deze benadering is ervoor zorgen dat er geen verlies van service of gegevens tijdens de migratie: gebruikers moeten schakelen tussen producten naadloos. Twee varianten van het migratieproces worden behandeld:

-   Eenvoudige: tussen de productlicenties die conflicterende serviceplannen niet bijvoorbeeld bevatten: *Office 365 Enterprise E3* en *Office 365 Enterprise E5*

-   Complexere: tussen producten die conflicterende serviceplannen bijvoorbeeld bevatten: *Office 365 Enterprise E1* en *Office 365 Enterprise E3*. Meer informatie met betrekking tot conflicten [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal#conflicting-service-plans) en [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-product-and-service-plan-reference#service-plans-that-cannot-be-assigned-at-the-same-time).

Dit artikel bevat voorbeeldcode PowerShell waarmee u de stappen voor migratie- en verificatiestappen uit te voeren. Dit is vooral nuttig voor grootschalige bewerkingen waarbij is het niet mogelijk de stappen handmatig uitvoeren.

## <a name="before-you-begin"></a>Voordat u begint
Voordat u het migratieproces uitvoert, is het belangrijk om te controleren of de volgende veronderstellingen geldt voor alle gebruikers wordt gemigreerd. Als dat niet het geval is, de migratie mislukken voor enkele van de gebruikers en daardoor ze kunnen geen toegang meer tot services of gegevens:

-   Gebruikers hebben de *bron licentie* toegewezen met behulp van groep op basis van licenties. Licenties voor het product te verlaten worden overgenomen van een groep één bron en niet rechtstreeks worden toegewezen. Opmerking: als licenties ook direct toegewezen zijn, ze kunnen voorkomen dat de toepassing van de *doel licentie*. Meer informatie over directe en groep licentietoewijzing [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-advanced#direct-licenses-coexist-with-group-licenses). U kunt een PowerShell-script zoals gebruiken [deze](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-ps-examples#check-if-user-license-is-assigned-directly-or-inherited-from-a-group) om te controleren of gebruikers directe licenties hebt.

-   U hebt onvoldoende beschikbare licenties voor het doelproduct. Als u niet sommige gebruikers mogelijk om op te halen de *doel licentie*. U kunt het aantal beschikbare licenties controleren [hier](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products).

-   Gebruikers hebben geen andere toegewezen productlicenties die u mogelijk strijdig is met de *doel licentie* of kan voorkomen dat de *bron licentie* tegen verwijderen (bijvoorbeeld invoegtoepassing producten, zoals werkplek Analytics of Project Online met een afhankelijkheid van andere producten).

-   Begrijpen hoe groepen worden beheerd in uw omgeving. Bijvoorbeeld, als u groepen lokaal beheren en deze te synchroniseren met Azure AD met AAD Connect, moet u voor gebruikers in uw on-premises systeem toevoegen/verwijderen en het duurt enige tijd om de wijzigingen te synchroniseren in AAD en door op basis van een groep licentieverlening ophalen opgehaald. Als u dynamische groepslidmaatschappen van Azure AD gebruikt, u wordt gebruikers toevoegen/verwijderen door het wijzigen van hun kenmerken in plaats daarvan. Het proces van de algehele migratie blijft hetzelfde, het enige verschil wordt hoe gebruikers worden toegevoegd/verwijderd uit groepen.

## <a name="migrating-users-between-products-without-conflicting-service-plans"></a>De migratie van gebruikers tussen producten zonder de conflicterende service-abonnementen
Het doel is te gebruiken op basis van een groep licenties wijzigen gebruikerslicenties van *bron licentie* (in dit voorbeeld: *Office 365 Enterprise E3*) naar *doel licentie* (in dit voorbeeld: *Office 365 Enterprise E5*). De twee producten bevatten geen conflicterende serviceplannen, zodat ze volledig op hetzelfde moment, zonder een conflict kunnen worden toegewezen. Op geen enkel tijdens de migratie moeten gebruikers toegang tot services of gegevens verliezen. Bovendien worden de migratie wordt uitgevoerd in kleine 'batches', dus er is een kans om te valideren van de uitkomst voor elke batch en beperken van het bereik van eventuele problemen die tijdens het proces optreden kunnen. Over het algemeen is het proces als volgt:
1.  Gebruikers lid zijn van de brongroep en nemen de *bron licentie* uit die groep.
2.  Maken van een doelgroep met de *doel licentie* , maar zonder leden.
3.  Een batch gebruikers toevoegen aan de doelgroep. Dit zorgt ervoor dat op basis van een groep licentieverlening (GBL) om te kunnen worden opgepikt de wijziging en wijs de *doel licentie*. Houd er rekening mee dat dit even, afhankelijk van de grootte van de batch en andere activiteiten in de tenant duren kan.
4.  Controleer of de batch gebruikers volledig is verwerkt door GBL en elke gebruiker daadwerkelijk is de *doel licentie* toegewezen. Controleer of gebruikers is niet in een foutstatus zoals veroorzaakt een conflict met andere producten of gebrek aan voldoende licenties terechtkomen. Meer informatie over fouten [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal).
5.  Op dit moment gebruikers hebben voor zowel de bron en *doel licenties* toegewezen.
6.  Dezelfde gebruiker batch verwijderen uit de bronnengroep. GBL reageert op de wijziging en de *bron licenties* wordt verwijderd uit de gebruikers.
7.  Het proces herhaalt voor latere batches van gebruikers.

### <a name="migrating-a-single-user-using-azure-portal"></a>Migreren van een enkele gebruiker met Azure portal
Dit is een eenvoudig overzicht voor één gebruiker wordt gemigreerd.

- **Stap 1:** gebruiker heeft de *bron licentie* overgenomen van een groep en geen directe toewijzingen voor de licentie.
![Gebruiker met een bron-licentie is overgenomen van groep](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseInherited.png)

- **Stap 2:** gebruiker is toegevoegd aan de doelgroep en de wijziging is verwerkt door GBL; de gebruiker nu zowel de *bron* en *doel* licenties die zijn overgenomen van beide groepen.
![Gebruiker met zowel de bron en doel-licentie is overgenomen van groepen](media/active-directory-licensing-group-product-migration/UserWithBothSourceAndTargetLicense.png)

- **Stap 3:** gebruiker is verwijderd uit de brongroep en de wijziging is verwerkt door GBL; de gebruiker nu alleen de *doel licentie*
![gebruiker met een doel-licentie is overgenomen van groep](media/active-directory-licensing-group-product-migration/UserWithTargetLicenseAssigned.png)

### <a name="automating-migration-using-powershell"></a>Migratie met behulp van PowerShell automatiseren
> [!NOTE]
> Deze voorbeeldcode maakt gebruik van PowerShell-functies die zijn opgenomen in [in de laatste sectie](#powershell-automation-of-migration-and-verification-steps) van dit document.

Dit fragment toont hoe het migratieproces op grotere schaal kan worden geautomatiseerd.
```
#A batch of users that we want to migrate in this iteration. This can be an array of User Principal Names (string) or ObjectIds (Guid)
#Note: this could be loaded from a text file that represents a larger batch of users we want to migrate
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com','MigrationUser2@tailspinonline.com'

###############NON-CONFLICTING LICENSES SCENARIO################
#The group and license that we are moving from
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:ENTERPRISEPACK'      #<- this is the O365 E3 product
#The group and license that we are moving to
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPREMIUM'   #<- this is the O365 E5 product

if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will assign the target license $targetSkuId to all users"
AddUsersToGroup $usersToMigrate $targetGroupId

#Verify that the target license shows up in conflict state for each user on the list. This step will run in a loop, forever, until all users are in the expected state.
#Since GBL may take some time to reflect the changes on users, this loop should terminate after some time dependent on the size of the user collection.
#Note: If the loop has not terminated for a long time, stop the script and inspect the users reported as not yet in the expected state and verify that they are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceandTargetLicensePresent} 'STEP 2: Checking if all users still have the source license and now also have the target license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#Now it is safe to remove the users from the source group
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

#Verify that target license is now active on each user and the source license has been removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId
```

Voorbeeld van uitvoer (migreren 2 gebruikers):
```
Verifying initial assumptions:
Enough TailspinOnline:ENTERPRISEPREMIUM licenses available (13) for users: 2.
migrationuser@tailspinonline.com                OK
migrationuser2@tailspinonline.com               OK
Checks passed for all 2 users.

STEP 1: Adding users to the target group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe. This will assign the target license TailspinOnline:ENTERPRISEPREMIUM to all users
Adding user MigrationUser@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe
Adding user MigrationUser2@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe

STEP 2: Checking if all users still have the source license and now also have the target license from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 2: Checking if all users still have the source license and now also have the target license from target group. Check iteration: 2

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.

STEP 3: Removing users from the source group b82c04f0-ce30-4ff1-bac7-735d92d83036. This will remove the source license TailspinOnline:ENTERPRISEPACK.
Removing user MigrationUser@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036
Removing user MigrationUser2@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 2

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.
```

## <a name="migrating-users-between-products-with-conflicting-service-plans"></a>De migratie van gebruikers tussen producten met conflicterende service-abonnementen
Het doel is te gebruiken op basis van een groep licenties wijzigen gebruikerslicenties van *bron licentie* (in dit voorbeeld: *Office 365 Enterprise E1*) naar *doel licentie* (in dit voorbeeld: *Office 365 Enterprise E3*). De twee producten bevatten conflicterende service-abonnementen (voor meer informatie over conflicten [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal#conflicting-service-plans)), zodat we moet omzeilen die naadloos om gebruikers te migreren. Op geen enkel tijdens de migratie moeten gebruikers toegang tot services of gegevens verliezen. Bovendien worden de migratie wordt uitgevoerd in kleine 'batches', dus er is een kans om te valideren van de uitkomst voor elke batch en beperken van het bereik van eventuele problemen die tijdens het proces optreden kunnen. Over het algemeen is het proces als volgt:
1.  Gebruikers lid zijn van de brongroep en nemen de *bron licentie* uit die groep.
2.  Maken van een doelgroep met de *doel licentie* , maar zonder leden.
3.  Een batch gebruikers toevoegen aan de doelgroep. Hiermee wordt op basis van een groep licentieverlening (GBL) om op te halen de wijziging veroorzaken en probeert toe te wijzen de *doel licentie*. De toewijzing mislukt als gevolg van conflicten tussen services in de twee producten en in plaats daarvan GBL een fout op elke gebruiker wordt opgenomen.
Houd er rekening mee dat dit even, afhankelijk van de grootte van de batch en andere activiteiten in de tenant duren kan.
4.  Controleer of de batch gebruikers volledig is verwerkt door GBL en elke gebruiker heeft het conflict fout die is opgenomen. Controleer of sommige gebruikers niet met een onverwachte foutstatus eindigen kon. Meer informatie over fouten [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal).
5.  Op dit moment gebruikers nog steeds de *bron licentie* en een conflict fout die bij de *doel licentie* – hebben geen de *doel licentie* daadwerkelijk, nog toegewezen.
6.  Dezelfde gebruiker batch verwijderen uit de bronnengroep. GBL reageert op de wijziging en de *bron licentie* wordt verwijderd van elke gebruiker en op hetzelfde moment, de fout conflict ook worden verwijderd (ervan uitgaande dat de fout wordt veroorzaakt door andere productlicentie) en de *doel licenties* wordt toegewezen. Dit zorgt ervoor zonder verlies van services of gegevens tijdens de overgang.
7.  Het proces herhaalt voor latere batches van gebruikers.

### <a name="migrating-a-single-user-using-azure-portal"></a>Migreren van een enkele gebruiker met Azure portal
Dit is een eenvoudig overzicht voor één gebruiker wordt gemigreerd.

- **Stap 1:** gebruiker heeft de *bron licentie* overgenomen van een groep en geen directe toewijzingen voor de licentie.
![Gebruiker met een bron-licentie is overgenomen van groep](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseInheritedConflictScenario.png)

- **Stap 2:** gebruiker is toegevoegd aan de doelgroep en GBL is de wijziging verwerkt; de gebruiker nog steeds de *bron licentie* en hebben nu de *doel licentie* foutstatus als gevolg van het conflict .
![Gebruiker met een bron-licentie is overgenomen van groep en doel licentie foutstatus](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseAndTargetLicenseInConflict.png)

- **Stap 3:** gebruiker is verwijderd uit de bronnengroep en GBL is de wijziging verwerkt; het *doel licentie* is nu aan de gebruiker toegepast ![gebruiker met een doel-licentie is overgenomen van groep](media/active-directory-licensing-group-product-migration/UserWithTargetLicenseAssignedConflictScenario.png)

### <a name="automating-migration-using-powershell"></a>Migratie met behulp van PowerShell automatiseren
> [!NOTE]
> Deze voorbeeldcode maakt gebruik van PowerShell-functies die zijn opgenomen in [in de laatste sectie](#powershell-automation-of-migration-and-verification-steps) van dit document.

Dit fragment toont hoe het migratieproces op grotere schaal kan worden geautomatiseerd.
```
#A batch of users that we want to migrate in this iteration. This can be an array of User Principal Names (string) or ObjectIds (Guid)
#Note: this could be loaded from a text file that represents a larger batch of users we want to migrate
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com', 'MigrationUser2@tailspinonline.com'

###############CONFLICTING LICENSES SCENARIO################
#The group and license that we are moving from
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:STANDARDPACK'             #<- this is the O365 E1 product
#The group and license that we are moving to
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPACK'           #<- this is the O365 E3 product

#Assumptions before migration:
#1. Users are already in the source group and they have the source license assigned from that group
#2. Users do not have the same source license assigned from another group at the same time and they do not have the source license assigned directly
#This is important - if not true, removing users from the source group in Step 3 is not going to result in the target license getting applied correctly
#3. There are enough available licenses for the target license to assign to the users we are migrating.
if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will put target license $targetSkuId in conflict state with the source license $sourceSkuId"
AddUsersToGroup $usersToMigrate $targetGroupId

#Verify that the target license shows up in conflict state for each user on the list. This step will run in a loop, forever, until all users are in the expected state.
#Since GBL may take some time to reflect the changes on users, this loop should terminate after some time dependent on the size of the user collection.
#Note: If the loop has not terminated for a long time, stop the script and inspect the users reported as not yet in the expected state and verify that they are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceLicensePresentAndTargetLicenseInConflictState} 'STEP 2: Checking if all users still have the source license and are in conflict state for license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#Now it is safe to remove the users from the source group
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId and remove the conflict on target license $targetSkuId which will become assigned."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

#Verify that target license is now active on each user and the source license has been removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId
```

Voorbeeld van uitvoer (migreren 2 gebruikers):
```
Verifying initial assumptions:
Enough TailspinOnline:ENTERPRISEPACK licenses available (61) for users: 2.
migrationuser@tailspinonline.com                OK
migrationuser2@tailspinonline.com               OK
Checks passed for all 2 users.
STEP 1: Adding users to the target group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe. This will put target license TailspinOnline:ENTERPRISEPACK in conflict state with the source license TailspinOnline:STANDARDPACK
Adding user MigrationUser@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe
Adding user MigrationUser2@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe

STEP 2: Checking if all users still have the source license and are in conflict state for license from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 2: Checking if all users still have the source license and are in conflict state for license from target group. Check iteration: 3

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.

STEP 3: Removing users from the source group b82c04f0-ce30-4ff1-bac7-735d92d83036. This will remove the source license TailspinOnline:STANDARDPACK and remove the conflict on target license TailspinOnline:ENTERPRISEPACK which will become assigned.
Removing user MigrationUser@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036
Removing user MigrationUser2@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 3

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.
```

## <a name="powershell-automation-of-migration-and-verification-steps"></a>PowerShell-automatisering van migratie-en verificatiestappen
Deze sectie bevat de PowerShell-code vereist voor het uitvoeren van de scripts die eerder in dit artikel wordt gebruikt.

>[!WARNING]
>Deze code is bedoeld als voorbeeld voor demonstratiedoeleinden. Als u van plan bent om deze te gebruiken in uw omgeving, kunt u eerst testen op kleine schaal of in een afzonderlijke testtenant. U moet de code om te voldoen aan de specifieke behoeften van uw omgeving aanpassen.

Om de code uitvoeren, moet u gebruiken de [Azure AD PowerShell v1.0 bibliotheken](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0] for instructions). Uitvoeren van de *verbinding msolservice* cmdlet eerst aan te melden bij de tenant voordat het script wordt uitgevoerd.
```
#BEGIN: Helper functions used in the script

#Retrieves user object based on ObjectId or UserPrincipalName
function GetUserObject
{
    [OutputType([Microsoft.Online.Administration.User])]
    Param([object]$userId)

    $userIdType = $userId.GetType()

    if($userIdType -eq [Guid])
    {
        return Get-MsolUser -ObjectId $userId
    }
    elseif($userIdType -eq [string])
    {
        return Get-MsolUser -UserPrincipalName $userId
    }
    else
    {
        throw "User Id type must be a Guid or a string (UserPrincipalName). The user id type was: $($userIdType.Name)"
    }
}

#Gets a Guid objectId for a user, even if a UserPrincipal string was passed in. Guid ids are needed for group membership manipulation, where UPNs cannot be used
function GetGuidUserId
{
    [OutputType([Guid])]
    Param([object]$userId)

    [Guid]$guidId = [Guid]::Empty
    if($userId.GetType() -eq [String])
    {
        $user = GetUserObject $userId
        return $user.ObjectId
    }
    elseif($userId.GetType() -eq [Guid])
    {
        return $userId
    }
    else
    {
        throw "UserId type must be a Guid or a string (UserPrincipalName). The user id type was: $($userId.GetType().Name)"
    }
}

#Adds a collection of users to a group. Note: this fails if a user is already a member of the group
function AddUsersToGroup
{
    Param([object[]]$userIds, [Guid]$groupId)

    foreach($userId in $userIds)
    {
        $fetchId = GetGuidUserId $userId
        Write-Host "Adding user $userId to group $groupId"
        Add-MsolGroupMember -GroupObjectId $groupId -GroupMemberType User -GroupMemberObjectId $fetchId
    }
}

#Removes a collection of users from a group. Note: this fails if a user is not a member of the group
function RemoveUsersFromGroup
{
    Param([object[]]$userIds, [Guid]$groupId)

    foreach($userId in $userIds)
    {
        $fetchId = GetGuidUserId $userId
        Write-Host "Removing user $userId from group $groupId"
        Remove-MsolGroupMember -GroupObjectId $groupId -GroupMemberType User -GroupMemberObjectId $fetchId
    }
}

#Returns the license object corresponding to the skuId. Returns NULL if not found
function GetUserLicense
{
    [OutputType([Microsoft.Online.Administration.UserLicense])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    #we look for the specific license SKU in all licenses assigned to the user
    foreach($license in $user.Licenses)
    {
        if ($license.AccountSkuId -ieq $skuId)
        {
            return $license
        }
    }
    return $null
}

#Checks if the specific SKU license is assigned to the user, regardless of how it may be assigned (directly or via GBL)
function IsLicenseAssignedToUser
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [string]$skuId)

    $license = GetUserLicense $user $skuId

    return ($license -ne $null)
}

function GetObjectIdsAssigningLicense
{
    [OutputType([Guid[]])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    $license = GetUserLicense $user $skuId

    if ($license -ne $null)
    {
        return [Guid[]]$license.GroupsAssigningLicense
    }
    return [Array]::CreateInstance([Guid],0)
}

#Returns TRUE if the user is inheriting the license from the specific group.
#Note: this returns true only if the license is successfully assigned from the group. If the license is in error state, this return false
function UserHasLicenseAssignedFromThisGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId, [Guid]$groupId)

    [Guid[]]$objectsAssigningLicense = GetObjectIdsAssigningLicense $user $skuId

    #GroupsAssigningLicense contains a collection of IDs of objects assigning the license
    #This could be a group object or a user object (contrary to what the name suggests)
    foreach ($assignmentSource in $objectsAssigningLicense)
    {
        #If the collection contains at least one ID not matching the user ID this means that the license is inherited from a group.
        #Note: the license may also be assigned directly in addition to being inherited
        if ($assignmentSource -ieq $groupId)
        {
            return $true
        }
    }
    return $false
}

#Returns error objects for a specific license
function GetErrorsForLicense
{
    [OutputType([Microsoft.Online.Administration.IndirectLicenseError[]])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    [Microsoft.Online.Administration.IndirectLicenseError[]] $errorObjects = $user.IndirectLicenseErrors | Where {"$($_.AccountSku.AccountName):$($_.AccountSku.SkuPartNumber)" -ieq $skuId}

    if($errorObjects -eq $null)
    {
        #there are no errors at all
        return [Array]::CreateInstance([Microsoft.Online.Administration.IndirectLicenseError],0)
    }

    return $errorObjects
}
#Returns an error label associated with a specific license inherited from a specific group. return $null if there is no error
function GetErrorForLicenseFromGroup
{
    [OutputType([string])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$groupId, [string]$skuId)


    #There are some errors. Check if any of them is associated with the group
    foreach($licenseError in GetErrorsForLicense $user $skuId)
    {
        if($licenseError.ReferencedObjectId -eq $groupId)
        {
            return $licenseError.Error
        }
    }
    return $null
}

#Checks if the license is in an expected state for a given group.
#If expectedError is set to a value, this looks if the license is in that specific error state from the group
#If expectedError is NULL, this checks if the license is successfully assigned from the group
function IsExpectedLicenseStateForGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$groupId, [string]$skuId, [string]$expectedError)

    #we expect the license to be fully assigned from the group and not in error state
    if([string]::IsNullOrEmpty($expectedError))
    {
        #check if the assigned license is inherted from the expected group, without an error on it
        return (UserHasLicenseAssignedFromThisGroup $user $skuId $groupId)
    }
    #we expect the license to be in the specific error state on the specific group
    else
    {
        $error = GetErrorForLicenseFromGroup $user $groupId $skuId
        return ($error -ieq $expectedError)
    }
}

#Detects if the licenses are in the specific state where the source license is still assigned, but the target license is in conflict state
#Note: if the source license is not present, this throws an exception to abort the script, because that state may signify something went wrong with the migration steps and user lost access to services
function VerifySourceLicensePresentAndTargetLicenseInConflictState
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #check user still has source license - if not, abort because something is seriously wrong
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $sourceSkuId from source group $sourceGroupId, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    #check if the target license is in conflict, as expected
    $conflictError = 'MutuallyExclusiveViolation'
    return (IsExpectedLicenseStateForGroup $user $targetGroupId $targetSkuId $conflictError)
}

#Detects if the licenses are in the specific state where the source license is no longer present, but the target license is correctly assigned
#Note: if the source license is gone, but target license is not present, this throws an exception to abort the script, because something went wrong and the user may have lost access to services
function VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #check if user has the source license completely removed, which is a prerequisite to the target license eventually kicking in
    if(IsLicenseAssignedToUser $user $sourceSkuId)
    {
        return $false
    }

    #check user has the target license at all - if not, abort because something is seriously wrong
    if(-Not (IsLicenseAssignedToUser $user $targetSkuId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $targetSkuId assigned, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    #check if the target license is assigned from the expected target group, and not in error state anymore
    return (IsExpectedLicenseStateForGroup $user $targetGroupId $targetSkuId $null)
}

#Detects if the licenses are in the specific state where the source license is still assigned and the target license is assigned as well
#Note: if the source license is not present, this throws an exception to abort the script, because that state may signify something went wrong with the migration steps and user lost access to services
function VerifySourceandTargetLicensePresent
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #check user still has source license - if not, abort because something is seriously wrong
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $sourceSkuId from source group $sourceGroupId, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    #check if the target license is also assigned from the target group
    return (UserHasLicenseAssignedFromThisGroup $user $targetSkuId $targetGroupId)
}


#Verifies basic assumptions that should be true for a user before we execute the migration process.
#Returns TRUE if all assumptions are true. Prints details
function VerifyAssumptionsForUser
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    $userName = $user.UserPrincipalName
    #1. User has the source license assigned from the source group.
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        Write-Host "$userName does not have source license $sourceSkuId assigned from source group $sourceGroupId."
        return $false
    }

    #2. User does not have the same source license assigned from another group at the same time and they do not have the source license assigned directly
    [Guid[]]$otherObjectsAssigningLicense = GetObjectIdsAssigningLicense $user $sourceSkuId | Where {$_ -ne $sourceGroupId}
    foreach($otherObject in $otherObjectsAssigningLicense)
    {
        if($otherObject -eq $user.ObjectId)
        {
            Write-Host "$userName has source license assigned directly to the user."
        }
        else
        {
            Write-Host "$username has source license assigned from an additional unexpected group $otherObject."
        }
    }
    if($otherObjectsAssigningLicense -and $otherObjectsAssigningLicense.Count -gt 0)
    {
        return $false
    }

    #3. User does not have the target license assigned
    if(IsLicenseAssignedToUser $user $targetSkuId)
    {
        Write-Host "$userName already has target license assigned."
        return $false
    }

    #4. User does not have the target license in error state from some groups
    [Microsoft.Online.Administration.IndirectLicenseError[]]$licenseErrors = GetErrorsForLicense $user $targetSkuId
    foreach($licenseError in $licenseErrors)
    {
        Write-Host "$userName has target license in error state $($licenseError.Error) from unexpected group $($licenseError.ReferencedObjectId)."
    }
    if($licenseErrors -and $licenseErrors.Count -gt 0)
    {
        return $false
    }

    Write-Host "$userName`t`tOK"
    return $true
}

#Checks if all users to be migrated are in correct state
function VerifyAssumptions
{
    [OutputType([bool])]
    Param([object[]]$userIds, [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    Write-Host "Verifying initial assumptions:"

    #Check if there are enough target licenses for all users
    $skuState = Get-MsolAccountSku | Where {$_.AccountSkuId -ieq $targetSkuId}

    if($skuState -eq $null)
    {
        Write-Host "Target license SKU $targetSkuId does not exist in the tenant at all."
        return $false
    }

    $availableLicenses = $skuState.ActiveUnits - $skuState.ConsumedUnits

    if($userIds.Count -gt $availableLicenses)
    {
        Write-Host "Not enough licenses for all users. User count: $($userIds.Count), licenses available: $availableLicenses"
        return $false
    }
    else
    {
        Write-Host "Enough $targetSkuId licenses available ($availableLicenses) for users: $($userIds.Count)."
    }

    #Check if each user to be migrated is in expected state
    $usersOK = 0
    $usersNotOK = 0
    foreach($userId in $userIds)
    {
        $user = GetUserObject $userId
        if(VerifyAssumptionsForUser $user $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId)
        {
            ++$usersOK
        }
        else
        {
            ++$usersNotOK
        }
    }
    if($usersNotOK -gt 0)
    {
        Write-Host "Checks passed for $usersOK users, but failed for $usersNotOK users."
        return $false
    }
    Write-Host "Checks passed for all $usersOK users."
    return $true
}

#Helper function: it executes one of the verification functions (passed in as a delegate using $checkFunction) for each user, keeps track of how many users passed/failed verification
#and repeats the loop until all users have passed the check. The loop may never terminate if some users never reach the expected state, which should be investigated.
#Note: if the verification function fails with an exception (e.g. because it detected unexpected user state) this loop will terminate and investigation into user state is needed
function ExecuteVerificationLoop
{
    Param([System.Management.Automation.ScriptBlock]$checkFunction, [string]$consoleMessage, [object[]]$userIds,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #how long to wait until the loop is retried
    $sleepIntervalSeconds = 60
    $retryIteration = 1

    While($true)
    {
        Write-Host "`n$consoleMessage. Check iteration: $retryIteration`n"

        $usersInExpectedState = 0
        $usersNotYet = 0

        foreach ($userId in $userIds)
        {
            $user = GetUserObject $userId
            if($checkFunction.InvokeReturnAsIs($user, $sourceGroupId, $sourceSkuId, $targetGroupId, $targetSkuId))
            {
                Write-Host "$userId`t`tExpected state: YES"
                ++$usersInExpectedState
            }
            else
            {
                Write-Host "$userId`t`tExpected state: NO"
                ++$usersNotYet
            }
        }

        Write-Host "`nTotal users checked: $($userIds.Count). In expected state: $usersInExpectedState. Not yet: $usersNotYet"

        if($usersNotYet -eq 0)
        {
            Write-Host "Check passed for all users. Exiting check loop."
            return
        }

        ++$retryIteration
        Write-Host "Not all users are in expected state. Waiting $sleepIntervalSeconds seconds to try again."
        Start-Sleep -Seconds $sleepIntervalSeconds
    }
}
#END: Helper functions used in the script

#BEGIN: Execute script

#enable strict mode
Set-StrictMode -Version latest
#stop on first exception thrown
$ErrorActionPreference = "Stop"

#A batch of users that we want to migrate in this iteration. This can be an array of User Principal Names (string) or ObjectIds (Guid)
#Note: this could be loaded from a text file that represents a larger batch of users we want to migrate
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com', 'MigrationUser2@tailspinonline.com'

###############CONFLICTING LICENSES SCENARIO################
#The group and license that we are moving from
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:STANDARDPACK'             #<- this is the O365 E1 product
#The group and license that we are moving to
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPACK'           #<- this is the O365 E3 product

#Assumptions before migration:
#1. Users are already in the source group and they have the source license assigned from that group
#2. Users do not have the same source license assigned from another group at the same time and they do not have the source license assigned directly
#This is important - if not true, removing users from the source group in Step 3 is not going to result in the target license getting applied correctly
#3. There are enough available licenses for the target license to assign to the users we are migrating.
if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will put target license $targetSkuId in conflict state with the source license $sourceSkuId"
AddUsersToGroup $usersToMigrate $targetGroupId

#Verify that the target license shows up in conflict state for each user on the list. This step will run in a loop, forever, until all users are in the expected state.
#Since GBL may take some time to reflect the changes on users, this loop should terminate after some time dependent on the size of the user collection.
#Note: If the loop has not terminated for a long time, stop the script and inspect the users reported as not yet in the expected state and verify that they are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceLicensePresentAndTargetLicenseInConflictState} 'STEP 2: Checking if all users still have the source license and are in conflict state for license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#Now it is safe to remove the users from the source group
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId and remove the conflict on target license $targetSkuId which will become assigned."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

#Verify that target license is now active on each user and the source license has been removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#END: Execute script
```

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over andere scenario's voor Licentiebeheer via groepen.

* [Opsporen en oplossen van licentieproblemen met een voor een groep in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [PowerShell-voorbeelden voor op basis van een groep licentieverlening in Azure AD](active-directory-licensing-ps-examples.md)
* [Azure Active Directory-licentieverlening voor aanvullende scenario's op basis van groep](active-directory-licensing-group-advanced.md)
