---
title: Tenant-brede inzicht voor Azure Security Center | Microsoft Docs
description: Meer informatie over het verkrijgen van de zichtbaarheid van de tenant-brede in Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: b85c0e93-9982-48ad-b23f-53b367f22b10
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2018
ms.author: terrylan
ms.openlocfilehash: 91d1be062dbf05f4c7c9c5c4a1eb3dfcfdb001af
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42441691"
---
# <a name="gain-tenant-wide-visibility-for-azure-security-center"></a>Tenant-brede inzicht voor Azure Security Center
Dit artikel helpt u aan de slag met het uitvoeren van verschillende acties die de Azure Security Center biedt voordelen maximaliseren. Deze acties worden uitgevoerd, kunt u inzicht op alle Azure-abonnementen die zijn gekoppeld aan uw Azure Active Directory-tenant en effectief beheren van de beveiligingsstatus van uw organisatie op schaal door het toepassen van beveiligingsbeleid voor meerdere krijgen abonnementen op een aggregative manier.

## <a name="management-groups"></a>Beheergroepen
Azure-beheergroepen bieden de mogelijkheid om efficiënt te beheren van toegang, beleidsregels en rapporteren van groepen van abonnementen, evenals de gehele Azure-omgeving effectief te beheren door het uitvoeren van acties op de root management-groep. Elke Azure AD-tenant krijgt een enkele op het hoogste niveau beheergroep met de naam van de root management-groep. Deze hoofdbeheergroep is zo in de hiërarchie ingebouwd dat alle beheergroepen en abonnementen hierin zijn opgevouwen. Deze groep kan globaal beleid en RBAC-toewijzingen moet worden toegepast op het niveau van de map. 

De root management-groep wordt automatisch gemaakt wanneer u een van de volgende acties: 
1. Registreren voor het gebruik van Azure-beheergroepen door te navigeren naar **beheergroepen** in de [Azure-portal](https://portal.azure.com).
2. Maak een beheergroep via een API-aanroep.
3. Maak een beheergroep met PowerShell.

Zie voor een gedetailleerd overzicht van beheergroepen, de [organiseren van uw resources met Azure-beheergroepen](../azure-resource-manager/management-groups-overview.md) artikel.

## <a name="create-a-management-group-in-the-azure-portal"></a>Een beheergroep maken in Azure portal
U kunt abonnementen in beheergroepen ordenen en uw governancebeleid toepassen op de beheergroepen. Alle abonnementen in een beheergroep nemen automatisch het beleid over dat op de beheergroep is toegepast. Terwijl de beheergroepen niet vereist voor het vrijgeven van Security Center, wordt ten zeerste aanbevolen dat u ten minste één beheergroep maken, zodat de root management-groep is gemaakt. Nadat de groep is gemaakt, wordt alle abonnementen voor uw Azure AD-tenant zijn gekoppeld aan deze. Zie voor instructies voor PowerShell en meer informatie, [beheergroepen voor het beheer van resource- en organisatie maken](../azure-resource-manager/management-groups-create.md).

 
1. Meld u aan bij [Azure Portal](http://portal.azure.com).
2. Selecteer **alle services** > **beheergroepen**.
3. Selecteer op de hoofdpagina van **nieuwe beheergroep.** 

    ![Main-groep](./media/security-center-management-groups/main.png) 
4.  Vul in het veld management groep-ID. 
    - De **beheergroep-ID** is de unieke id van map die wordt gebruikt voor het verzenden van opdrachten in deze beheergroep. Deze id is niet bewerkbaar na het maken van zoals het in het hele systeem Azure wordt gebruikt voor het identificeren van deze groep. 
    - Het weergavenaamveld is de naam die wordt weergegeven in Azure portal. Een afzonderlijke weergavenaam is een optioneel veld bij het maken van het beheer te groeperen en op elk gewenst moment kan worden gewijzigd.  

      ![Maken](./media/security-center-management-groups/create_context_menu.png)  
5.  Selecteer **opslaan**

### <a name="view-management-groups-in-the-azure-portal"></a>Beheergroepen weergeven in Azure portal
1. Aanmelden bij de [Azure-portal](http://portal.azure.com).
2. Als u beheergroepen, selecteer **alle services** onder het hoofdmenu van Azure.
3. Onder **algemene**, selecteer **beheergroepen**.

    ![Een beheergroep maken](./media/security-center-management-groups/all-services.png)

## <a name="grant-tenant-level-visibility-and-the-ability-to-assign-policies"></a>Verleen op tenantniveau zichtbaarheid en de mogelijkheid om beleid toe te wijzen

Om inzicht te krijgen in de beveiligingsstatus van alle abonnementen die zijn geregistreerd in de Azure AD-tenant, is een RBAC-rol met voldoende machtigingen voor lezen vereist moeten worden toegewezen aan de root management-groep.

### <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Toegangsrechten voor een globale beheerder in Azure Active Directory
Een tenantbeheerder van Azure Active Directory heeft geen directe toegang tot Azure-abonnementen. Als de directorybeheerder van een hebben ze echter het recht op zichzelf aan een rol die is gemachtigd te verhogen. Een Azure AD-tenant-beheerder moet zelf uitbreiden naar de beheerder van gebruikerstoegang op het hoofdniveau van de management-groep, zodat ze kunnen RBAC-rollen toewijzen. Zie voor meer informatie en instructies voor PowerShell, [toegangsrechten voor een globale beheerder in Azure Active Directory](../role-based-access-control/elevate-access-global-admin.md). 


1. Aanmelden bij de [Azure-portal](https://portal.azure.com) of de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com).

2. Klik in de navigatielijst **Azure Active Directory** en klik vervolgens op **eigenschappen**.

   ![Eigenschappen van Azure AD - schermafbeelding](./media/security-center-management-groups/aad-properties.png)

3. Onder **globale beheerder kan beheren Azure-abonnementen en beheergroepen**, instellen van de switch op **Ja**.

   ![Globale beheerder kan beheren Azure-abonnementen en beheergroepen - schermafbeelding](./media/security-center-management-groups/aad-properties-global-admin-setting.png)

   - Als u de switch instelt op **Ja**, uw account voor globale beheerders (momenteel aangemelde gebruiker) wordt toegevoegd aan de rol Administrator voor gebruikerstoegang in Azure RBAC bij het root-bereik (`/`), hebt u toegang tot het weergeven en rapporteren op alle Azure-abonnementen die zijn gekoppeld aan uw Azure AD-tenant.

   - Als u de switch instelt op **Nee**, uw account voor globale beheerders (momenteel aangemelde gebruiker) wordt verwijderd uit de rol Administrator voor gebruikerstoegang in Azure RBAC. Alle Azure-abonnementen die gekoppeld aan de Azure AD-tenant zijn niet wordt weergegeven, en u kunt weergeven en beheren van alleen de Azure-abonnementen waartoe u toegang is verleend.

4. Klik op **opslaan** om op te slaan van de instelling.

    - Deze instelling is niet een globale eigenschap en geldt alleen voor de momenteel aangemelde gebruiker.

5. Voer de taken die u wilt maken op de uitgebreide toegangsrechten. Wanneer u klaar bent, stelt u de switch terug naar **Nee**.


### <a name="assign-rbac-roles-to-users"></a>RBAC-rollen toewijzen aan gebruikers
Voor meer zichtbaarheid voor alle abonnementen, moeten tenantbeheerders de juiste RBAC-rol toewijzen aan gebruikers voor het verlenen van tenant-brede zichtbaarheid, zelf, met inbegrip van op het hoofdniveau van de management-groep. De aanbevolen rollen om toe te wijzen zijn **beveiligingsbeheerder** of **Beveiligingslezer**. De rol Beveiligingsbeheerder is over het algemeen moet beleidsregels toepassen op het hoogste niveau doen, terwijl Beveiligingslezer worden volstaan om aan te bieden zichtbaarheid op tenantniveau. Zie voor meer informatie over de machtigingen van deze rollen, de [beschrijving van de ingebouwde rol Beveiligingsbeheerder](../role-based-access-control/built-in-roles.md#security-admin) of de [beschrijving van de ingebouwde rol Beveiligingslezer](../role-based-access-control/built-in-roles.md#security-reader).


#### <a name="assign-rbac-roles-to-users-through-the-azure-portal"></a>RBAC-rollen toewijzen aan gebruikers via de Azure-portal: 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Als u beheergroepen, selecteer **alle services** onder het Azure-hoofdmenu Selecteer **beheergroepen**.
3.  Selecteer een beheergroep en klik op **details**.

    ![Beheer van groepen details-schermafbeelding](./media/security-center-management-groups/management-group-details.PNG)
 
4. Klik op **toegangsbeheer (IAM)** vervolgens **toevoegen**.
5. Selecteer de rol om toe te wijzen en de gebruiker en klik vervolgens op **opslaan**.  
   
   ![Schermafbeelding van de rol van Beveiligingslezer toevoegen](./media/security-center-management-groups/asc-security-reader.png)


#### <a name="assign-rbac-roles-to-users-with-powershell"></a>RBAC-rollen toewijzen aan gebruikers met PowerShell: 
1. Installeer [Azure PowerShell](/powershell/azure/install-azurerm-ps).
2. Voer de volgende opdrachten uit: 

    ```azurepowershell
    # Install Management Groups Powershell module
    Install-Module AzureRM.Resources
    
    # Login to Azure as a Global Administrator user
    Login-AzureRmAccount
    ```

3. Wanneer u hierom wordt gevraagd, meldt u zich aan met de referenties van de globale beheerder. 

    ![Meld u aan vragen schermafbeelding](./media/security-center-management-groups/azurerm-sign-in.PNG)

4. Verlenen lezer rolmachtigingen door het uitvoeren van de volgende opdracht uit:

    ```azurepowershell
    # Add Reader role to the required user on the Root Management Group
    # Replace "user@domian.com” with the user to grant access to
    New-AzureRmRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/"
    ```
5. Als u wilt verwijderen van de rol, gebruik de volgende opdracht: 

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/" 
    ```

### <a name="open-or-refresh-security-center"></a>Open of vernieuwen van Security Center
Als u toegang hebt uitgebreid, openen of vernieuwen van Azure Security Center om te controleren of dat u hebt inzicht in alle abonnementen voor uw Azure AD-tenant. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Zorg ervoor dat u alle abonnementen in de lijst met abonnementen die u wilt weergeven in Security Center.
    ![Schermafbeelding van de abonnement-selector](./media/security-center-management-groups/subscription-selector.png)
1. Selecteer **alle services** onder het Azure-hoofdmenu Selecteer **Security Center**.
2. In de **overzicht**, bevindt zich een diagram van de dekking voor abonnement. 
    ![Schermafbeelding van abonnement dekking grafiek](./media/security-center-management-groups/security-center-subscription-coverage.png)
3. Klik op **dekking** om te zien van de lijst met abonnementen gedekt. 
    ![Schermafbeelding van de lijst met abonnement dekking](./media/security-center-management-groups/security-center-coverage.png)

### <a name="remove-elevated-access"></a>Toegang met verhoogde bevoegdheid verwijderen 
Zodra de RBAC-rollen zijn toegewezen aan de gebruikers, moet de tenantbeheerder zelf verwijderen uit de rol Beheerder gebruikerstoegang.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) of de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com).

2. Klik in de navigatielijst **Azure Active Directory** en klik vervolgens op **eigenschappen**.

3. Onder **globale beheerder kan beheren Azure-abonnementen en beheergroepen**, instellen van de switch op **Nee**.

4. Klik op **opslaan** om op te slaan van de instelling.



## <a name="adding-subscriptions-to-a-management-groups"></a>Abonnementen toevoegen aan een beheergroepen
U kunt abonnementen toevoegen aan de beheergroep die u hebt gemaakt. Deze stappen zijn niet verplicht voor het verkrijgen van zichtbaarheid van de tenant-brede en globaal beleid en toegangsbeheer.

1. Onder **beheergroepen**, selecteert u een beheergroep die u wilt toevoegen van uw abonnement op.

    ![Selecteer een beheergroep die u wilt het abonnement dat u wilt toevoegen](./media/security-center-management-groups/management-group-subscriptions.png)

2. Selecteer **bestaande toevoegen**.

    ![Bestaand item toevoegen](./media/security-center-management-groups/add-existing.png)

3. Voer abonnement onder **bestaande resource toevoegen** en klikt u op **opslaan**.

4. Herhaal stappen 1 t/m 3 totdat u alle abonnementen hebt toegevoegd in het bereik.

 > [!NOTE]
 > Beheergroepen kunnen zowel abonnementen en onderliggende beheergroepen bevatten. Wanneer u een gebruiker een RBAC-rol aan de bovenliggende beheergroep toewijzen, wordt de toegang is overgenomen door de onderliggende beheergroep abonnementen. Beleid dat is ingesteld op de bovenliggende beheergroep worden ook overgenomen door de onderliggende objecten. 

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u inzicht met tenant-brede voor Azure Security Center. Zie de volgende artikelen voor meer informatie over Security Center:

> [!div class="nextstepaction"]
> [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md)

> [!div class="nextstepaction"]
> [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md)

