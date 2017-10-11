---
title: Gebruik van certificaten met Enterprise Integration Pack | Microsoft Docs
description: Informatie over het gebruik van certificaten met de Enterprise Integration Pack | Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 0570aab14283b38f9efcc50636f0c0c1c8e3ed13
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Meer informatie over certificaten en het Enterprise Integration Pack
## <a name="overview"></a>Overzicht
Enterprise-integratie gebruikt certificaten voor het beveiligen van B2B-communicatie. U kunt twee typen certificaten gebruiken in uw onderneming integratie-apps:

* Openbare certificaten, moeten worden aangeschaft van een certificeringsinstantie (CA).
* Persoonlijke certificaten, kunt u zelf uitgeven. Deze certificaten worden soms aangeduid als zelfondertekende certificaten.

## <a name="what-are-certificates"></a>Wat zijn certificaten?
Certificaten worden digitale documenten die de identiteit van de deelnemers aan elektronische communicatie verifiëren en die ook elektronische communicatie beveiligen.

## <a name="why-use-certificates"></a>Waarom certificaten gebruiken?
Soms moet B2B-communicatie vertrouwelijk worden behandeld. Enterprise integration maakt gebruik van certificaten voor het beveiligen van deze communicatie op twee manieren:

* Door de inhoud van berichten te versleutelen
* Door berichten digitaal te ondertekenen  

## <a name="upload-a-public-certificate"></a>Een openbaar certificaat uploaden

Gebruik een *openbaar certificaat* in logic apps met B2B-mogelijkheden, moet u eerst het certificaat te uploaden naar uw integratie-account.  

Nadat u een certificaat hebt geüpload, wordt het beschikbaar om te helpen beveiligen van uw B2B-berichten bij het definiëren van de eigenschappen in de [overeenkomsten](logic-apps-enterprise-integration-agreements.md) die u maakt.  

Hier worden de gedetailleerde stappen voor het uploaden van uw certificaten voor openbare toegang tot je account integratie nadat u zich aanmeldt bij de Azure-portal:

1. Selecteer **meer services** en voer **integratie** in het zoekvak van het filter. Selecteer **Integratieaccounts** uit de lijst met resultaten     
![Selecteer Bladeren](media/logic-apps-enterprise-integration-certificates/overview-1.png)  
2. Selecteer de integratie-account waarnaar u wilt het certificaat niet toevoegen.  
![Selecteer de integratie-account waarnaar u wilt het certificaat toevoegen](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. Selecteer de **certificaten** tegel.  
![Selecteer de tegel certificaten](media/logic-apps-enterprise-integration-certificates/certificate-1.png)
4. In de **certificaten** blade die wordt geopend, selecteert de **toevoegen** knop.   
![Klik op de knop toevoegen](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
5. Voer een **naam** voor uw certificaat en selecteer vervolgens typt u het certificaat als **openbare** uit de vervolgkeuzelijst.  
6. Selecteer het mappictogram aan de rechterkant van de **certificaat** in het tekstvak. Wanneer de bestandskiezer wordt geopend, zoek en selecteer het certificaatbestand dat u wilt uploaden naar uw integratie-account.
7. Selecteer het certificaat en selecteer vervolgens **OK** in de bestandskiezer. Dit valideert en uploadt het certificaat aan uw account integratie.
8. Back-ten slotte op de **certificaat toevoegen** blade, selecteer de **OK** knop.  
![Selecteer de knop OK](media/logic-apps-enterprise-integration-certificates/certificate-3.png)  
9. Selecteer de **certificaten** tegel. U ziet het zojuist toegevoegde certificaat.  
![Zie het nieuwe certificaat](media/logic-apps-enterprise-integration-certificates/certificate-4.png)  

## <a name="upload-a-private-certificate"></a>Een persoonlijk certificaat uploaden

Gebruik een *persoonlijk certificaat* in logic apps met B2B-mogelijkheden, kunt u een persoonlijk certificaat uploaden naar uw account integratie met de volgende stappen

1. [Uw persoonlijke sleutel te uploaden naar Sleutelkluis](../key-vault/key-vault-get-started.md "meer informatie over Sleutelkluis") en geef een **sleutelnaam** 
   
   > [!TIP]
   > U kunt logische Apps op bewerkingen uitvoeren op de Sleutelkluis moet autoriseren. U kunt toegang verlenen tot de service-principal van Logic Apps met behulp van de volgende PowerShell-opdracht:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  
   > 
   > 

Nadat u de vorige stap hebt gemaakt, moet u een persoonlijk certificaat toevoegen aan integratie-account.

Hier volgen gedetailleerde stappen voor het uploaden van uw persoonlijke certificaten bij uw account integratie nadat u zich aanmeldt bij de Azure-portal:  
 
1. Selecteer de integratie-account die u wilt het certificaat toevoegen en selecteer de **certificaten** tegel.  
![Selecteer de tegel certificaten](media/logic-apps-enterprise-integration-certificates/certificate-1.png)  
2. In de **certificaten** blade die wordt geopend, selecteert de **toevoegen** knop.   
![Klik op de knop toevoegen](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
3. Voer een **naam** voor het certificaat en selecteer het certificaat typt als **persoonlijke** uit de vervolgkeuzelijst.   
4. Selecteer het mappictogram aan de rechterkant van de **certificaat** in het tekstvak. Wanneer de bestandskiezer wordt geopend, moet u het bijbehorende openbare certificaat dat u wilt uploaden naar uw account integratie vinden.   
   
   > [!Note]
   > Tijdens het toevoegen van een persoonlijk certificaat is het belangrijk om het openbare certificaat van de bijbehorende om weer te geven toevoegen [AS2-overeenkomst](logic-apps-enterprise-integration-as2.md) ontvangen en verzenden van instellingen voor het ondertekenen en versleutelen van berichten.
   > 
   >   

5. Selecteer de **resourcegroep**, **Sleutelkluis**, **sleutelnaam** en selecteer de **OK** knop.  
![Certificaat toevoegen](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)  
6. Selecteer de **certificaten** tegel. U ziet het zojuist toegevoegde certificaat.
![Zie het nieuwe certificaat](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)  



* [Maken van een B2B-overeenkomst](logic-apps-enterprise-integration-agreements.md)  
* [Meer informatie over Sleutelkluis](../key-vault/key-vault-get-started.md "meer informatie over Sleutelkluis")  

