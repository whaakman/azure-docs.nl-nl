---
title: Toegang tot en verifiëren zonder dat u aangemeld - Azure Logic Apps | Microsoft Docs
description: Een beheerde identiteit maken, zodat uw logische app kunt verifiëren en toegang resources in andere tenants Azure Active Directory (Azure AD) zonder uw referenties tot
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: fb1c31e6e7c075e20191a4e51d7b1a9323f3b979
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973963"
---
# <a name="access-resources-and-authenticate-as-managed-identities-in-azure-logic-apps"></a>Toegang tot bronnen en verifiëren als beheerde identiteiten in Azure Logic Apps

Voor toegang tot resources in andere tenants Azure Active Directory (Azure AD) en uw identiteit verifiëren zonder dat u aangemeld, kunt u een [beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) die uw logische app wordt gebruikt in plaats van uw referenties. Azure beheert deze identiteiten voor u en helpt uw referenties beveiligen, omdat u niet hoeft te bieden of geheimen draaien. In dit artikel laat zien hoe het maken en gebruiken van een beheerde identiteit voor uw logische app. Zie voor meer informatie, [identiteiten voor een Azure-resources beheren](../app-service/app-service-managed-service-identity.md).

> [!NOTE]
> Beheerde identiteiten voor een Azure-resources is de naam van de vervanging voor de service die voorheen bekend als Managed Service Identity (MSI).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement, of als u een abonnement hebt <a href="https://azure.microsoft.com/free/" target="_blank">zich aanmelden voor een gratis Azure-account</a>.

* De logische app waar u wilt gebruikmaken van de beheerde identiteit. Als u een logische app niet hebt, raadpleegt u [maken van uw eerste werkstroom voor logische Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="create-identity"></a>

## <a name="create-managed-identity"></a>Beheerde identiteit maken

U kunt maken of een beheerde identiteit inschakelen voor uw logische app via de Azure portal, Azure Resource Manager-sjablonen of Azure PowerShell. 

### <a name="azure-portal"></a>Azure Portal

Inschakelen voor het maken van een beheerde identiteit voor uw logische app via de Azure-portal, de **registreren bij Azure Active Directory** instellen in instellingen voor de werkstroom van uw logische app.

1. In de [Azure-portal](https://portal.azure.com), opent u uw logische app in Logic App Designer.

1. Volg deze stappen: 

   1. Op het menu van de logische app, onder **instellingen**, selecteer **Werkstroominstellingen**. 

   1. Onder **beheerde service-identiteit** > 
    **registreren bij Azure Active Directory**, kiest u **op**.

   1. Wanneer u klaar bent, kiest u **opslaan** op de werkbalk.

      ![Schakel de instelling beheerde identiteit](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

      Azure geeft nu deze eigenschappen en waarden voor de beheerde identiteit van uw logische app:

      ![GUID's voor de principal-ID en tenant-ID](./media/create-managed-service-identity/principal-tenant-id.png)

      | Eigenschap | Waarde | Beschrijving | 
      |----------|-------|-------------| 
      | **Principal-ID** | <*Principal-ID-GUID*> | Een Globally Unique Identifier (GUID) die aangeeft van de logische app in een Azure AD-tenant | 
      | **Tenant-ID** | <*Azure-AD-tenant-ID-GUID*> | Een Globally Unique Identifier (GUID) die aangeeft van de Azure AD-tenant waar uw logische app nu lid is. In de Azure AD-tenant is de service-principal dezelfde naam als het logische app-exemplaar. | 
      ||| 

### <a name="deployment-template"></a>Sjabloon voor de implementatie

Als u wilt maken en distribueren Azure-resources zoals logic apps automatiseren, kunt u Azure Resource Manager-sjablonen instellen. Zie voor meer informatie, [maken en implementeren van logische apps met Azure Resource Manager-sjablonen](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md). 

Voor het maken van een beheerde identiteit voor uw logische app via een sjabloon, voeg de **identiteit** element en **type** eigenschap in op de werkstroomdefinitie van uw logische app in uw sjabloon voor de implementatie. Deze instellingen geven aan dat Azure maakt en deze identiteit voor uw logische app beheert:

```json
"identity": {
    "type": "SystemAssigned"
}
```

Bijvoorbeeld: uw logische app eruit kan deze versie:

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
    "principalId": "<principal-ID-GUID>",
    "tenantId": "<Azure-AD-tenant-ID>-GUID"
}
```

| Eigenschap | Waarde | Beschrijving | 
|----------|-------|-------------|
| **principalId** | <*Principal-ID-GUID*> | Een Globally Unique Identifier (GUID) die aangeeft van de logische app in de Azure AD-tenant | 
| **tenantId** | <*Azure-AD-tenant-ID-GUID*> | Een Globally Unique Identifier (GUID) die aangeeft van de Azure AD-tenant waar de logische app nu lid is. In de Azure AD-tenant is de service-principal dezelfde naam als het logische app-exemplaar. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Toegang tot bronnen met beheerde identiteit

Nadat u een beheerde identiteit voor uw logische app maakt, kunt u [die identiteit toegang geven tot andere bronnen](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Vervolgens kunt u die beheerde identiteit gebruiken voor verificatie, net als elk ander [service-principal](../active-directory/develop/app-objects-and-service-principals.md). 

Stel bijvoorbeeld dat u al een logische app met een beheerde identiteit die toegang tot een andere resource heeft hebt ingesteld. U kunt nu een HTTP-actie toevoegen zodat uw logische app kunt verzenden van een HTTP-aanvraag of aanroepen van die resource. 

1. Voeg in uw logische app, de **HTTP** actie. 

1. Geef de benodigde informatie voor de actie, zoals de aanvraag **methode** en **URI** locatie voor de resource die u wilt aanroepen.

1. Kies in de HTTP-actie **geavanceerde opties weergeven**. 

1. Uit de **verificatie** in de lijst met **beheerde Service-identiteit**, deze vervolgens toont de **doelgroep** eigenschap voor het instellen:

   ![Selecteer "Beheerde Service-identiteit"](./media/create-managed-service-identity/select-managed-service-identity.png)

1. Doorgaan met het ontwikkelen van de logische app zoals u dat wilt.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Verwijder de beheerde identiteit

Als u wilt een beheerde identiteit in uw logische app uitschakelen, kunt u Volg de stappen vergelijkbaar met hoe u de identiteit via de Azure portal, Azure Resource Manager-sjablonen voor implementatie of Azure PowerShell hebt gemaakt. 

Wanneer u uw logische app verwijdert, verwijderd Azure uw logische app het systeem toegewezen identiteit automatisch uit Azure AD.

### <a name="azure-portal"></a>Azure Portal

1. Open uw logische app in Logic App Designer.

1. Volg deze stappen: 

   1. Op het menu van de logische app, onder **instellingen**, selecteer **Werkstroominstellingen**. 
   
   1. Onder **beheerde service-identiteit**, kiest u **uit** voor de **registreren bij Azure Active Directory** eigenschap.

   1. Wanneer u klaar bent, kiest u **opslaan** op de werkbalk.

      ![Beheerde identiteit uitschakelen](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Sjabloon voor de implementatie

Als u beheerde identiteit van de logische app met een Azure Resource Manager-implementatiesjabloon gemaakt, stelt u de `"identity"` van element `"type"` eigenschap `"None"`. Deze actie verwijdert u ook de principal-ID van Azure AD. 

```json
"identity": {
    "type": "None"
}
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).
