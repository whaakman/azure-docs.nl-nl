---
title: Verifiëren met beheerde identiteiten - Azure Logic Apps | Microsoft Docs
description: Als u wilt verifiëren zonder dat u aangemeld, kunt u een beheerde identiteit (voorheen beheerde Service-identiteit of MSI genoemd) zodat uw logische app toegang heeft tot resources in andere Azure Active Directory (Azure AD) zonder referenties of geheimen voor tenants
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 10/05/2018
ms.openlocfilehash: 19e6693de673eae6fe0b885580975c4cefc35d60
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52725145"
---
# <a name="authenticate-and-access-resources-with-managed-identities-in-azure-logic-apps"></a>Verifiëren en toegang krijgen tot bronnen met beheerde identiteiten in Azure Logic Apps

Voor toegang tot resources in andere tenants Azure Active Directory (Azure AD) en uw identiteit verifiëren zonder dat u aangemeld, uw logische app kunt gebruiken een [beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) (voorheen bekend als beheerde Service-identiteit of MSI-bestand), in plaats van referenties of geheimen. Azure beheert deze identiteiten voor u en helpt uw referenties beveiligen, omdat u hoeft te bieden of geheimen draaien. Dit artikel wordt beschreven hoe u kunt maken en gebruiken van een systeem toegewezen beheerde identiteit voor uw logische app. Zie voor meer informatie over beheerde identiteiten [wat is beheerde identiteiten voor Azure-resources?](../active-directory/managed-identities-azure-resources/overview.md)

> [!NOTE]
> U kunt op dit moment hebben maximaal 10 logic app workflows met het systeem toegewezen beheerd identiteiten in elk Azure-abonnement.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement, of als u een abonnement hebt <a href="https://azure.microsoft.com/free/" target="_blank">zich aanmelden voor een gratis Azure-account</a>.

* De logische app waar u wilt gebruiken door het systeem toegewezen identiteit beheerd. Als u een logische app niet hebt, raadpleegt u [maken van uw eerste werkstroom voor logische Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="create-identity"></a>

## <a name="create-managed-identity"></a>Beheerde identiteit maken

U kunt maken of een door het systeem toegewezen beheerde identiteit inschakelen voor uw logische app via de Azure portal, Azure Resource Manager-sjablonen of Azure PowerShell. 

### <a name="azure-portal"></a>Azure Portal

Inschakelen om in te schakelen op een systeem toegewezen beheerde identiteit voor uw logische app via de Azure-portal, de **registreren bij Azure Active Directory** instellen in instellingen voor de werkstroom van uw logische app.

1. In de [Azure-portal](https://portal.azure.com), opent u uw logische app in Logic App Designer.

1. Volg deze stappen: 

   1. Op het menu van de logische app, onder **instellingen**, selecteer **Werkstroominstellingen**. 

   1. Onder **beheerde service-identiteit** > 
    **registreren bij Azure Active Directory**, kiest u **op**.

   1. Wanneer u klaar bent, kiest u **opslaan** op de werkbalk.

      ![Schakel de instelling beheerde identiteit](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

      Uw logische app heeft nu een systeem toegewezen beheerde identiteit in Azure Active Directory worden geregistreerd met deze eigenschappen en waarden:

      ![GUID's voor de principal-ID en tenant-ID](./media/create-managed-service-identity/principal-tenant-id.png)

      | Eigenschap | Waarde | Beschrijving | 
      |----------|-------|-------------| 
      | **Principal-ID** | <*Principal-ID*> | Een Globally Unique Identifier (GUID) die aangeeft van de logische app in een Azure AD-tenant | 
      | **Tenant ID** | <*Azure-AD-tenant-ID*> | Een Globally Unique Identifier (GUID) die aangeeft van de Azure AD-tenant waar uw logische app nu lid is. In de Azure AD-tenant is de service-principal dezelfde naam als het logische app-exemplaar. | 
      ||| 

### <a name="deployment-template"></a>Sjabloon voor de implementatie

Als u maken en distribueren Azure-resources zoals logic apps automatiseren wilt, kunt u [Azure Resource Manager-sjablonen](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md). Voor het maken van een systeem toegewezen beheerde identiteit voor uw logische app via een sjabloon, voeg de `"identity"` element en `"type"` eigenschap in op de werkstroomdefinitie van uw logische app in uw sjabloon voor de implementatie: 

```json
"identity": {
   "type": "SystemAssigned"
}
```

Bijvoorbeeld:

```json
{
   "apiVersion": "2016-06-01", 
   "type": "Microsoft.logic/workflows", 
   "name": "[variables('logicappName')]", 
   "location": "[resourceGroup().location]", 
   "identity": { 
      "type": "SystemAssigned" 
   }, 
   "properties": { 
      "definition": { 
         "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#", 
         "actions": {}, 
         "parameters": {}, 
         "triggers": {}, 
         "contentVersion": "1.0.0.0", 
         "outputs": {} 
   }, 
   "parameters": {}, 
   "dependsOn": [] 
}
```

Als Azure uw logische app maakt, bevat de werkstroomdefinitie die logica-app de aanvullende eigenschappen:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Eigenschap | Waarde | Beschrijving | 
|----------|-------|-------------|
| **principalId** | <*Principal-ID*> | Een Globally Unique Identifier (GUID) die aangeeft van de logische app in de Azure AD-tenant | 
| **tenantId** | <*Azure-AD-tenant-ID*> | Een Globally Unique Identifier (GUID) die aangeeft van de Azure AD-tenant waar de logische app nu lid is. In de Azure AD-tenant is de service-principal dezelfde naam als het logische app-exemplaar. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Toegang tot bronnen met beheerde identiteit

Nadat u een systeem toegewezen beheerde identiteit voor uw logische app maakt, kunt u [die identiteit toegang geven tot andere Azure-resources](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Vervolgens kunt u die identiteit gebruiken voor verificatie, net als elk ander [service-principal](../active-directory/develop/app-objects-and-service-principals.md). 

> [!NOTE]
> Zowel de door het systeem toegewezen beheerde identiteit als de bron waar u toegang wilt toewijzen, moeten hetzelfde Azure-abonnement hebben.

### <a name="assign-access-to-managed-identity"></a>Toegang tot beheerde identiteit toewijzen

Om toegang te verlenen aan een andere Azure-resource voor beheerde identiteit van uw logische app het systeem is toegewezen, de volgende stappen uit:

1. Ga naar de Azure-resource waar u toegang wilt toewijzen voor uw beheerde identiteit in de Azure-portal. 

1. Selecteer in het resourcemenu **toegangsbeheer (IAM)**, en kies **roltoewijzing toevoegen**. 

   ![Roltoewijzing toevoegen](./media/create-managed-service-identity/add-permissions-logic-app.png)

1. Onder **roltoewijzing toevoegen**, selecteer de **rol** u wilt gebruiken voor de identiteit. 

1. In de **toegang toewijzen aan** eigenschap, selecteer **Azure AD-gebruiker, groep of service-principal**, als u nog niet is geselecteerd.

1. In de **Selecteer** , beginnen met het eerste teken in naam van uw logische app, voer de naam van uw logische app. Wanneer uw logische app wordt weergegeven, selecteert u de logische app.

   ![Selecteer de logische app met beheerde identiteit](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

1. Als u bent klaar, kiest u **Opslaan**.

### <a name="authenticate-with-managed-identity-in-logic-app"></a>Verifiëren met beheerde identiteit in de logische app

Na het instellen van uw logische app met een door het systeem toegewezen beheerde identiteit en toegang tot de resource die u wilt gebruiken voor die identiteit toegewezen, kunt u nu die identiteit gebruiken voor verificatie. U kunt bijvoorbeeld een HTTP-actie gebruiken zodat uw logische app kunt verzenden van een HTTP-aanvraag of aanroepen van die resource. 

1. Voeg in uw logische app, de **HTTP** actie. 

1. Geef de benodigde informatie voor de actie, zoals de aanvraag **methode** en **URI** locatie voor de resource die u wilt aanroepen.

1. Kies in de HTTP-actie **geavanceerde opties weergeven**. 

1. Uit de **verificatie** in de lijst met **beheerde Service-identiteit**, deze vervolgens toont de **doelgroep** eigenschap voor het instellen:

   ![Selecteer "Beheerde Service-identiteit"](./media/create-managed-service-identity/select-managed-service-identity.png)

1. Doorgaan met het ontwikkelen van de logische app zoals u dat wilt.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Verwijder de beheerde identiteit

Als u wilt een systeem toegewezen beheerde identiteit in uw logische app uitschakelen, kunt u Volg de stappen vergelijkbaar met hoe u de identiteit via de Azure portal, Azure Resource Manager-sjablonen voor implementatie of Azure PowerShell hebt gemaakt. 

Wanneer u uw logische app verwijdert, verwijderd Azure uw logische app het systeem toegewezen identiteit automatisch uit Azure AD.

### <a name="azure-portal"></a>Azure Portal

1. Open uw logische app in Logic App Designer.

1. Volg deze stappen: 

   1. Op het menu van de logische app, onder **instellingen**, selecteer **Werkstroominstellingen**. 
   
   1. Onder **beheerde service-identiteit**, kiest u **uit** voor de **registreren bij Azure Active Directory** eigenschap.

   1. Wanneer u klaar bent, kiest u **opslaan** op de werkbalk.

      ![Beheerde identiteit uitschakelen](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Sjabloon voor de implementatie

Als u beheerde identiteit van de logische app het systeem toegewezen met een Azure Resource Manager-implementatiesjabloon gemaakt, stelt u de `"identity"` van element `"type"` eigenschap `"None"`. Deze actie verwijdert u ook de principal-ID van Azure AD. 

```json
"identity": {
   "type": "None"
}
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

