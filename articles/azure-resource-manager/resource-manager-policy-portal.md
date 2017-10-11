---
title: Azure-portal voor bronbeleid | Microsoft Docs
description: Beschrijft hoe Azure-portal gebruiken om te maken en beheren van Resource Manager-beleid. Beleidsregels kunnen worden toegepast op de groepen abonnement of resourcegroep.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: tomfitz
ms.openlocfilehash: 868b2cc1559053057d17b34c03e2e31347f399bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-portal-to-assign-and-manage-resource-policies"></a>Gebruik van Azure-portal toewijzen en beheren van bronbeleid
De Azure-portal kunt u bronbeleid toewijzen aan resourcegroepen en abonnementen. De gebruikersinterface, kunt u gemakkelijk Selecteer het beleid dat u wilt toewijzen en geef parameterwaarden voor dat beleid voor het aanpassen van de beleidsinstellingen. 

Als een beleid via de portal wilt toewijzen, moet de beleidsdefinitie al aanwezig in uw abonnement. Uw abonnement heeft diverse ingebouwde beleidsdefinities die gereed zijn voor u toewijzen aan resourcegroepen of abonnementen. Ziet u deze beleidsregels ingebouwde en aangepaste beleidsregels die u hebt gedefinieerd wanneer het beleid toe te wijzen met behulp van de portal. Zie voor een inleiding tot beleid en het aangepaste beleid definiëren, [Resource overzicht](resource-manager-policy.md).

Beleidsregels worden overgenomen door alle onderliggende resources. Dus als een beleid wordt toegepast op een resourcegroep, is van toepassing op alle resources in die resourcegroep. In dit artikel wordt de term **bereik** verwijst naar de resourcegroep of abonnement dat het beleid is toegewezen. 

Beleidsregels worden geëvalueerd wanneer maken en bijwerken van resources (plaatsen en PATCH operations).

## <a name="assign-a-policy"></a>Geen beleid toewijzen

1. Als u wilt een beleid toewijzen aan een resourcegroep of een abonnement, selecteert u de resourcegroep of abonnement. Selecteer in de instellingen voor **beleid**.

   ![beleid selecteren](./media/resource-manager-policy-portal/select-policies.png)

2. Selecteer voor het maken van de beleidstoewijzing van een voor deze scope **toewijzing toevoegen**.

   ![toewijzing toevoegen](./media/resource-manager-policy-portal/add-assignment.png)

3. Selecteer het beleid dat u wilt toewijzen. Zelfs als u hebt beleidsdefinities niet toegevoegd aan uw abonnement, ziet u de ingebouwde beleidsregels die beschikbaar voor toewijzing zijn. Deze ingebouwde beleidsregels algemene scenario's uitgelegd.

   ![definitie selecteren](./media/resource-manager-policy-portal/select-definition.png)

4. Na het selecteren van een beleid, raadpleegt u een beschrijving van het beleid en parameters voor dat beleid. Bijvoorbeeld de volgende afbeelding toont de **toegestaan locaties** parameter, die is vereist voor het beleid dat Hiermee beperkt u de beschikbare locaties.

   ![parameters weergeven](./media/resource-manager-policy-portal/show-parameters.png)

5. Selecteer de waarden op te geven voor de beleidsparameters (zoals de locaties die kunnen worden gebruikt voor implementatie) via de gebruikersinterface.

   ![Selecteer de parameterwaarden](./media/resource-manager-policy-portal/select-parameters.png)

6. Geef waarden op voor de andere parameters. De scope wordt automatisch toegewezen op basis van de blade die u hebt geselecteerd bij het starten van de beleidstoewijzing. Selecteer **OK** wanneer u gereed bent.

   ![parameters definiëren](./media/resource-manager-policy-portal/define-parameters.png)

  U kunt het beleid hebt toegewezen aan het opgegeven bereik.

## <a name="view-policy-assignments"></a>Beleidstoewijzingen weergeven

Na het toewijzen van een beleid voor bekijken u deze in de lijst met beleidsregels voor deze scope. De **Details** tabblad bevat een overzicht van de beleidstoewijzing.

![details weergeven](./media/resource-manager-policy-portal/show-details.png)

De **toewijzingsregel** tabblad ziet u de JSON van de beleidsdefinitie.

![regel voor agenttoewijzing weergeven](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a>Wijzigen van een bestaande beleidstoewijzing van

Als u een beleid, selecteert **toewijzing bewerken** of **verwijderen**

![bewerken of verwijderen van toewijzing](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a>Aangepast beleid toe te wijzen

Als u aangepaste beleidsregels hebt gedefinieerd in uw abonnement, zijn deze beleidsregels beschikbaar voor toewijzing via de portal. Deze beleidsregels worden voorafgegaan door **[Custom]**

![aangepast beleid](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de JSON-syntaxis voor het definiëren van beleid, [Resource overzicht](resource-manager-policy.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).
* Het schema van het beleid wordt gepubliceerd op [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

