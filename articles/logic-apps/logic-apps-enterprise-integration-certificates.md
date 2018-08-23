---
title: Beveiligde B2B-berichten met certificaten - Azure Logic Apps | Microsoft Docs
description: Certificaten voor het beveiligen van B2B-berichten in Azure Logic Apps met het Enterprise Integration Pack toevoegen
services: logic-apps
ms.service: logic-apps
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, LADocs
manager: jeconnoc
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.suite: integration
ms.topic: article
ms.date: 08/17/2018
ms.openlocfilehash: 5ae69d365a183f7d2a219d853241e73c1e27212b
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42060582"
---
# <a name="secure-b2b-messages-with-certificates"></a>Beveiligde B2B-berichten met certificaten

Als u wilt de B2B-communicatie vertrouwelijk te houden, kunt u beveiligen B2B-communicatie voor uw onderneming integratieapps, specifiek logic apps, certificaten toe te voegen aan uw integratie-account. Certificaten worden digitale documenten die de identiteit voor de deelnemers in elektronische communicatie controleren en bij het beveiligen van communicatie op de volgende manieren:

* Codeer inhoud van het bericht.
* Digitaal ondertekenen van berichten. 

U kunt deze certificaten gebruiken in uw apps in enterprise integration:

* [Openbare certificaten](https://en.wikipedia.org/wiki/Public_key_certificate), die u moet aanschaffen bij een openbare internet [certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority) maar die niet vereisen geen sleutels. 

* Persoonlijke certificaten of [ *zelfondertekende certificaten*](https://en.wikipedia.org/wiki/Self-signed_certificate), waarbij u maken en uitgeven van uzelf, maar ook persoonlijke sleutels vereist. 

## <a name="upload-a-public-certificate"></a>Een openbaar certificaat uploaden

Gebruik een *openbaar certificaat* in logische apps met B2B-mogelijkheden, moet u eerst het certificaat uploaden naar uw integratie-account. Na het definiëren van de eigenschappen in de [overeenkomsten](logic-apps-enterprise-integration-agreements.md) dat u maakt, wordt het certificaat is beschikbaar voor het beveiligen van uw B2B-berichten.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer in het hoofdmenu van Azure **alle resources**. In het zoekvak, typ de naam van uw integratie-account en selecteer vervolgens de integratieaccount dat u wilt.

   ![Zoek en selecteer uw integratie-account](media/logic-apps-enterprise-integration-certificates/select-integration-account.png)  

2. Onder **onderdelen**, kiest u de **certificaten** tegel.

   ![Kies 'Certificaten'](media/logic-apps-enterprise-integration-certificates/add-certificates.png)

3. Onder **certificaten**, kiest u **toevoegen**. Onder **certificaat toevoegen**, geef de volgende gegevens voor uw certificaat. Wanneer u klaar bent, kiest u **OK**.

   | Eigenschap | Waarde | Beschrijving | 
   |----------|-------|-------------|
   | **Naam** | <*naam van certificaat*> | De naam van het certificaat, namelijk 'publicCert' in dit voorbeeld | 
   | **Certificaattype** | Public | Type van uw certificaat |
   | **Certificaat** | <*certificaat-bestandsnaam*> | Als u wilt zoeken en selecteren van het certificaatbestand dat u wilt uploaden, kies het pictogram van de map naast de **certificaat** vak. |
   ||||

   ![Kies 'Toevoegen', geeft u de details van certificaat](media/logic-apps-enterprise-integration-certificates/public-certificate-details.png)

   Nadat Azure uw selectie heeft gevalideerd, is Azure uploadt uw certificaat.

   ![Azure wordt het nieuwe certificaat weergegeven](media/logic-apps-enterprise-integration-certificates/new-public-certificate.png) 

## <a name="upload-a-private-certificate"></a>Een persoonlijk certificaat uploaden

Gebruik een *persoonlijk certificaat* in logische apps met B2B-mogelijkheden, moet u eerst het certificaat uploaden naar uw integratie-account. U moet ook een persoonlijke sleutel die u eerst toevoegen aan [Azure Key Vault](../key-vault/key-vault-get-started.md). 

Na het definiëren van de eigenschappen in de [overeenkomsten](logic-apps-enterprise-integration-agreements.md) dat u maakt, wordt het certificaat is beschikbaar voor het beveiligen van uw B2B-berichten.

> [!NOTE]
> Voor persoonlijke certificaten, zorg ervoor dat u een bijbehorende openbare certificaat dat wordt weergegeven toevoegt in de [AS2-overeenkomst](logic-apps-enterprise-integration-as2.md) **verzenden en ontvangen** instellingen voor het ondertekenen en versleutelen van berichten.

1. [Uw persoonlijke sleutel toevoegen aan Azure Key Vault](../key-vault/key-vault-get-started.md#add) en bieden een **sleutelnaam**.
   
2. Toestaan dat Azure Logic Apps voor het uitvoeren van bewerkingen op Azure Key Vault. Om toegang te verlenen aan de service-principal voor Logic Apps, gebruikt u de PowerShell-opdracht [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy), bijvoorbeeld:

   `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName 
   '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`
 
3. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer in het hoofdmenu van Azure **alle resources**. In het zoekvak, typ de naam van uw integratie-account en selecteer vervolgens de integratieaccount dat u wilt.

   ![Uw integratie-account zoeken](media/logic-apps-enterprise-integration-certificates/select-integration-account.png) 

4. Onder **onderdelen**, kiest u de **certificaten** tegel.  

   ![Kies de tegel certificaten](media/logic-apps-enterprise-integration-certificates/add-certificates.png)

5. Onder **certificaten**, kiest u **toevoegen**. Onder **certificaat toevoegen**, geef de volgende gegevens voor uw certificaat. Wanneer u klaar bent, kiest u **OK**.

   | Eigenschap | Waarde | Beschrijving | 
   |----------|-------|-------------|
   | **Naam** | <*naam van certificaat*> | De naam van het certificaat, namelijk 'privateCert' in dit voorbeeld | 
   | **Certificaattype** | Privé | Type van uw certificaat |
   | **Certificaat** | <*certificaat-bestandsnaam*> | Als u wilt zoeken en selecteren van het certificaatbestand dat u wilt uploaden, kies het pictogram van de map naast de **certificaat** vak. | 
   | **Resourcegroep** | <*integratie-account-resource-group*> | De resourcegroep van uw integratie-account, die 'MyResourceGroup' in dit voorbeeld is | 
   | **Key Vault** | <*naam van een Key vault*> | De naam van uw Azure key vault |
   | **Sleutelnaam** | <*naam van de sleutel*> | De naam van uw sleutel |
   ||||

   ![Kies 'Toevoegen', geeft u de details van certificaat](media/logic-apps-enterprise-integration-certificates/private-certificate-details.png)

   Nadat Azure uw selectie heeft gevalideerd, is Azure uploadt uw certificaat.

   ![Azure wordt het nieuwe certificaat weergegeven](media/logic-apps-enterprise-integration-certificates/new-private-certificate.png) 

## <a name="next-steps"></a>Volgende stappen

* [Een B2B-overeenkomst maken](logic-apps-enterprise-integration-agreements.md)
