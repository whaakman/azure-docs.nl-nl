---
title: B2B-berichten met de certificaten in Azure Logic Apps beveiligen | Microsoft Docs
description: Toevoegen van certificaten om B2B-berichten met de Enterprise-integratiepakket te beveiligen
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 708bdcddede71186c48ae7d4034cc9df0bd61391
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="secure-b2b-messages-with-certificates"></a>Beveiligde B2B-berichten met certificaten

Soms moet u B2B-communicatie vertrouwelijk houden. U kunt certificaten toevoegen aan uw account integratie om te helpen beveiligen B2B-communicatie voor uw onderneming integratie-apps, speciaal logic apps. Certificaten worden digitale documenten die de identiteit van de deelnemers aan elektronische communicatie.
Certificaten kunnen u beveiligde communicatie op de volgende manieren:

* Inhoud van het bericht te versleutelen
* Digitaal ondertekenen van berichten  

U kunt deze certificaten gebruiken in uw onderneming integratie-apps:

* Openbare certificaten, moeten worden aangeschaft van een certificeringsinstantie (CA).
* Persoonlijke certificaten, kunt u zelf uitgeven. Deze certificaten worden soms aangeduid als zelfondertekende certificaten.

## <a name="upload-a-public-certificate"></a>Een openbaar certificaat uploaden

Gebruik een *openbaar certificaat* in logic apps die mogelijkheden voor B2B hebt, moet u eerst het certificaat uploaden naar uw integratie-account. Nadat u de eigenschappen definiëren in de [overeenkomsten](logic-apps-enterprise-integration-agreements.md) dat u maakt, wordt het certificaat is beschikbaar om te helpen beveiligen van uw B2B-berichten.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer op het Azure hoofdmenu **alle services**. Voer in het zoekvak "-integratie" en selecteer vervolgens **integratieaccounts**.

   ![Uw account integratie vinden](media/logic-apps-enterprise-integration-certificates/overview-1.png)  

3. Onder **Integratieaccounts**, selecteert u de integratie account waar u het certificaat niet toevoegen.

   ![Selecteer de integratie-account waarnaar u wilt het certificaat toevoegen](media/logic-apps-enterprise-integration-certificates/overview-3.png)  

4. Kies de **certificaten** tegel.  

   ![Kiest u 'Certificaten'](media/logic-apps-enterprise-integration-certificates/certificate-1.png)

5. Onder **certificaten**, kies **toevoegen**.

   ![Kies 'Add'](media/logic-apps-enterprise-integration-certificates/certificate-2.png)

6. Onder **certificaat toevoegen**, geef de details voor het certificaat.
   
   1. Voer uw certificaat **naam**. Selecteer voor het certificaattype **openbare**.

   2. Aan de rechterkant van de **certificaat** pictogram van de map kiest. 
   Zoek en selecteer het certificaatbestand dat u wilt uploaden. 
   Als u bent klaar, kiest u **OK**.

      ![Openbaar certificaat uploaden](media/logic-apps-enterprise-integration-certificates/certificate-3.png)

   Azure uploadt u uw certificaat na de validatie van uw selectie.

   ![Zie het nieuwe certificaat](media/logic-apps-enterprise-integration-certificates/certificate-4.png) 

## <a name="upload-a-private-certificate"></a>Een persoonlijk certificaat uploaden

Gebruik een *persoonlijk certificaat* in logic apps die mogelijkheden voor B2B hebt, moet u eerst het certificaat uploaden naar uw integratie-account. U moet bovendien beschikken over een persoonlijke sleutel die u eerst toevoegen aan [Azure Key Vault](../key-vault/key-vault-get-started.md). 

Nadat u de eigenschappen definiëren in de [overeenkomsten](logic-apps-enterprise-integration-agreements.md) dat u maakt, wordt het certificaat is beschikbaar om te helpen beveiligen van uw B2B-berichten.

> [!NOTE]
> Persoonlijke certificaten, zorg ervoor dat u, een certificaat met de bijbehorende openbare moet worden weergegeven toevoegt de [AS2-overeenkomst](logic-apps-enterprise-integration-as2.md) verzenden en ontvangen van instellingen voor het ondertekenen en versleutelen van berichten.

1. [Uw persoonlijke sleutel toevoegen aan Azure Sleutelkluis](../key-vault/key-vault-get-started.md#add) en geef een **sleutelnaam**.
   
2. Toestaan dat Azure Logic Apps op bewerkingen uitvoeren op Azure Sleutelkluis. Om toegang te verlenen aan de service-principal van Logic Apps, gebruikt u de PowerShell-opdracht [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy), bijvoorbeeld:

   `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName 
   '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`
 
3. Meld u aan bij [Azure Portal](https://portal.azure.com).

4. Selecteer op het Azure hoofdmenu **alle services**. Voer in het zoekvak "-integratie" en selecteer vervolgens **integratieaccounts**.

   ![Uw account integratie vinden](media/logic-apps-enterprise-integration-certificates/overview-1.png) 

5. Onder **Integratieaccounts**, selecteert u de integratie account waar u het certificaat niet toevoegen.

6. Kies de **certificaten** tegel.  

   ![Kies de tegel certificaten](media/logic-apps-enterprise-integration-certificates/certificate-1.png)

7. Onder **certificaten**, kies **toevoegen**.   

   ![Kies de knop toevoegen](media/logic-apps-enterprise-integration-certificates/certificate-2.png)

8. Onder **certificaat toevoegen**, geef de details voor het certificaat.
   
   1. Voer uw certificaat **naam**. Selecteer voor het certificaattype **persoonlijke**.

   2. Aan de rechterkant van de **certificaat** pictogram van de map kiest. 
   Zoek en selecteer het certificaatbestand dat u wilt uploaden. 
   Schakel ook het **resourcegroep**, **Sleutelkluis**, en **sleutelnaam**. 
   Als u bent klaar, kiest u **OK**.

      ![Certificaat toevoegen](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)

   Azure uploadt u uw certificaat na de validatie van uw selectie.

   ![Zie het nieuwe certificaat](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)

## <a name="next-steps"></a>Volgende stappen

* [Maken van een B2B-overeenkomst](logic-apps-enterprise-integration-agreements.md)
