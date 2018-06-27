---
title: Tenant-wide inzicht voor Azure Security Center krijgen | Microsoft Docs
description: Meer informatie over het inzicht krijgen in Azure Security Center tenant wide.
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
ms.date: 06/22/2018
ms.author: terrylan
ms.openlocfilehash: 2c95b06ce34b850d1bfaf60e47d6e5fede148a38
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025739"
---
# <a name="gain-tenant-wide-visibility-for-azure-security-center"></a>Tenant-wide inzicht voor Azure Security Center krijgen
In dit artikel helpt u als volgt verschillende acties die de voordelen Azure Security Center biedt aan de slag. Deze acties worden uitgevoerd, kunt u inzicht op alle Azure-abonnementen die zijn gekoppeld aan uw Azure Active Directory-tenant en effectief beheren van uw organisatie beveiligingspostuur op grote schaal krijgen door het toepassen van beveiligingsbeleid over meerdere abonnementen op een aggregative manier.

## <a name="management-groups"></a>Beheergroepen
Azure-beheergroepen bieden de mogelijkheid op efficiënte wijze toegang, beleid en rapportage over groepen van abonnementen beheren, evenals effectief beheer van de volledige Azure goed door het uitvoeren van handelingen op de basis-beheergroep. Elke Azure AD-tenant krijgt een enkel op het hoogste niveau beheergroep met de naam van de root management-groep. Deze hoofdmap beheergroep is ingebouwd in de hiërarchie hebben alle beheergroepen en abonnementen vouw aan. Deze groep kan globaal beleid en toewijzingen moeten worden toegepast op het niveau van het directory RBAC. 

De root management-groep wordt automatisch gemaakt wanneer u een van de volgende acties: 
1. U meldt zich aan het Azure-beheergroepen gebruiken door te navigeren naar **beheergroepen** in de [Azure-portal](https://portal.azure.com).
2. Maak een beheergroep via een API-aanroep.
3. Maken van een beheergroep met PowerShell.

Zie voor een gedetailleerd overzicht van beheergroepen de [ordenen van uw resources met Azure-beheergroepen](../azure-resource-manager/management-groups-overview.md) artikel.

## <a name="create-a-management-group-in-the-azure-portal"></a>Een beheergroep hebt gemaakt in de Azure portal
U kunt abonnementen in beheergroepen indelen en uw governance-beleid toepassen op de beheergroepen. Het beleid toegepast op de beheergroep wordt automatisch overgenomen door alle abonnementen binnen een beheergroep. Terwijl beheergroepen niet vereist voor on-board Security Center, het raadzaam dat u ten minste één beheergroep maken, zodat de root management-groep is gemaakt. Nadat de groep is gemaakt, wordt alle abonnementen onder uw Azure AD-tenant zijn gekoppeld aan deze. Zie voor instructies voor meer informatie en PowerShell [beheergroepen voor het beheer van de bron- en organisatie maken](../azure-resource-manager/management-groups-create.md).

 
1. Meld u aan bij [Azure Portal](http://portal.azure.com).
2. Selecteer **alle services** > **beheergroepen**.
3. Selecteer op de hoofdpagina **nieuwe beheergroep.** 

    ![Hoofdgroep](./media/security-center-management-groups/main.png) 
4.  Vul in het veld management groep-ID. 
    - De **beheergroep-ID** is de directory unieke id die wordt gebruikt voor het indienen van opdrachten in deze beheergroep. Deze id niet meer worden bewerkt nadat de gemaakt zoals deze in het hele systeem Azure wordt gebruikt om deze groep te identificeren. 
    - Veld met de weergavenaam is de naam die wordt weergegeven in de Azure-portal. Een afzonderlijke weergavenaam is een optioneel veld bij het maken van de management groep en op elk gewenst moment kan worden gewijzigd.  

      ![Maken](./media/security-center-management-groups/create_context_menu.png)  
5.  Selecteer **opslaan**

### <a name="view-management-groups-in-the-azure-portal"></a>Beheergroepen weergeven in de Azure portal
1. Aanmelden bij de [Azure-portal](http://portal.azure.com).
2. Als u wilt weergeven beheergroepen, selecteer **alle services** onder het hoofdmenu van Azure.
3. Onder **algemene**, selecteer **beheergroepen**.

    ![Maken van een beheergroep](./media/security-center-management-groups/all-services.png)

## <a name="grant-tenant-level-visibility-and-the-ability-to-assign-policies"></a>Verleen op tenantniveau zichtbaarheid en de mogelijkheid om beleid toe te wijzen

Om inzicht te krijgen in de beveiligingsstatus van alle abonnementen die zijn geregistreerd in de Azure AD-tenant, is een RBAC-rol met voldoende machtigingen voor lezen vereist op de root management-groep worden toegewezen.

### <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Toegangsrechten voor een globale beheerder in Azure Active Directory uitbreiden
Een Azure Active Directory-tenantbeheerder heeft geen directe toegang tot Azure-abonnementen. Als de beheerder van een directory hebben ze echter het recht om zichzelf aan een rol die toegang heeft de bevoegdheden. Een Azure AD-tenantbeheerder moet zichzelf uitbreiden naar de beheerder voor gebruikerstoegang op het hoofdniveau van de management-groep, zodat ze kunnen RBAC-rollen toewijzen. Zie voor meer informatie en instructies voor PowerShell [toegangsrechten voor een globale beheerder in Azure Active Directory uitbreiden](../role-based-access-control/elevate-access-global-admin.md). 


1. Aanmelden bij de [Azure-portal](https://portal.azure.com) of de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com).

2. Klik in de lijst navigatie op **Azure Active Directory** en klik vervolgens op **eigenschappen**.

   ![Eigenschappen van Azure AD - schermafbeelding](./media/security-center-management-groups/aad-properties.png)

3. Onder **globale beheerder kunnen worden beheerd Azure-abonnementen en beheergroepen**, instellen van de switch op **Ja**.

   ![Globale beheerder kunnen worden beheerd Azure-abonnementen en beheergroepen - schermafbeelding](./media/security-center-management-groups/aad-properties-global-admin-setting.png)

   - Als u de switch instelt op **Ja**, uw globale beheerdersaccount (momenteel aangemelde gebruiker) wordt toegevoegd aan de rol beheerder voor gebruikerstoegang in Azure RBAC voor het root-bereik (`/`), hebt u toegang tot de weergave en het rapport op alle Azure-abonnementen die zijn gekoppeld aan uw Azure AD-tenant.

   - Als u de switch instelt op **Nee**, uw globale beheerdersaccount (momenteel aangemelde gebruiker) is verwijderd uit de rol beheerder voor gebruikerstoegang in Azure RBAC. U kunt niet alle Azure-abonnementen die gekoppeld aan de Azure AD-tenant zijn bekijken en u kunt weergeven en beheren van alleen de Azure-abonnementen waartoe u toegang hebben gekregen.

4. Klik op **opslaan** om op te slaan van de instelling.

    - Deze instelling is niet van een algemene eigenschap en geldt alleen voor de momenteel aangemelde gebruiker.

5. Voer de taken die u wilt maken op de uitgebreide toegangsrechten. Als u bent klaar, stelt u de schakeloptie terug naar **Nee**.

### <a name="assign-rbac-roles-to-users"></a>RBAC-rollen toewijzen aan gebruikers
Als tenantbeheerder toegang verhoogde, kunnen ze RBAC-rol toewijzen aan de betreffende gebruikers op het hoofdniveau van de management-groep. De aanbevolen functie toewijzen is [ **lezer**](../role-based-access-control/built-in-roles.md#reader). Deze rol is vereist voor tenantniveau inzicht verschaffen. De toegewezen rol wordt automatisch doorgegeven aan alle beheergroepen en abonnementen onder de root management-groep. Zie voor meer informatie over RBAC-rollen [beschikbare rollen](../active-directory/active-directory-assign-admin-roles-azure-portal.md#available-roles).

1. Installeer [Azure PowerShell](/powershell/azure/install-azurerm-ps).
2. Voer de volgende opdrachten uit: 

    ```azurepowershell
    # Install Management Groups Powershell module
    Install-Module AzureRM.Resources
    
    # Login to Azure as a Global Administrator user
    Login-AzureRmAccount
    ```

3. Wanneer u wordt gevraagd, meld u aan met referenties van de globale beheerder. 

    ![Meld u aan vragen schermafbeelding](./media/security-center-management-groups/azurerm-sign-in.PNG)

4. Verlenen lezer rolmachtigingen met de volgende opdracht:

    ```azurepowershell
    # Add Reader role to the required user on the Root Management Group
    # Replace "user@domian.com” with the user to grant access to
    New-AzureRmRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/"
    ```
5. Gebruik de volgende opdracht voor het verwijderen van de rol: 

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/" 
    ```

<!-- Currently, PowerShell method only 6/26/18

1. Sign in to the [Azure portal](https://portal.azure.com). 
2. To view management groups, select **All services** under the Azure main menu then select **Management Groups**.
3.  Select a management group and click **details**.

    ![Management Groups details screenshot](./media/security-center-management-groups/management-group-details.PNG)
 
4. Click **Access control (IAM)** then **Add**.
5. Select the role to assign and the user, then click **Save**.  
   
   ![Add Security Reader role screenshot](./media/security-center-management-groups/asc-security-reader.png)
-->

### <a name="remove-elevated-access"></a>Verhoogde toegang verwijderen 
Zodra de RBAC-rollen zijn toegewezen aan de gebruikers, moet de tenantbeheerder zelf verwijderen uit de rol Beheerder gebruikerstoegang.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) of de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com).

2. Klik in de lijst navigatie op **Azure Active Directory** en klik vervolgens op **eigenschappen**.

3. Onder **globale beheerder kunnen worden beheerd Azure-abonnementen en beheergroepen**, instellen van de switch op **Nee**.

4. Klik op **opslaan** om op te slaan van de instelling.

### <a name="open-or-refresh-security-center"></a>Open of vernieuw Security Center
Als u de RBAC-rollen toegewezen hebt, openen of vernieuwen van Azure Security Center om te controleren of dat u inzicht in alle abonnementen hebt onder uw Azure AD-tenant. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Selecteer **alle services** onder het Azure hoofdmenu Selecteer **Security Center**.
3. In de **overzicht**, er is een grafiek van de dekking van uw abonnement. 
    ![Schermafbeelding van grafiek dekking van uw abonnement](./media/security-center-management-groups/security-center-subscription-coverage.png)
4. Klik op **dekking** om te zien van de lijst met abonnementen behandeld. 
    ![Abonnement dekking lijst-schermafbeelding](./media/security-center-management-groups/security-center-coverage.png)

## <a name="adding-subscriptions-to-a-management-groups"></a>Abonnementen toe te voegen aan een beheergroepen
U kunt abonnementen toevoegen aan de beheergroep die u hebt gemaakt. Deze stappen zijn niet verplicht voor tenant wide zichtbaarheid en globaal beleid en beheer van toegang te krijgen.

1. Onder **beheergroepen**, selecteer een beheergroep toevoegen van uw abonnement.

    ![Selecteer een beheergroep abonnement toevoegen](./media/security-center-management-groups/management-group-subscriptions.png)

2. Selecteer **bestaande toevoegen**.

    ![Bestaand item toevoegen](./media/security-center-management-groups/add-existing.png)

3. Voer abonnement onder **bestaande resource toevoegen** en klik op **opslaan**.

4. Herhaal stap 1 tot en met 3 totdat u alle abonnementen hebt toegevoegd in het bereik.

 > [!NOTE]
 > Beheergroepen kunnen zowel abonnementen en onderliggende beheergroepen bevatten. Wanneer u een gebruiker een RBAC-rol aan de bovenliggende groep toewijst, wordt de toegang wordt overgenomen door de onderliggende beheergroep abonnementen. Beleid dat is ingesteld op de bovenliggende groep worden ook overgenomen door de onderliggende elementen. 

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe tenant wide inzicht voor Azure Security Center krijgen. Zie de volgende artikelen voor meer informatie over Security Center:

> [!div class="nextstepaction"]
> [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md)

> [!div class="nextstepaction"]
> [Beheren van en reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md)

