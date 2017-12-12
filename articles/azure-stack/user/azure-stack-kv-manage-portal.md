---
title: Sleutelkluis in Azure-Stack beheren via de portal | Microsoft Docs
description: Informatie over het beheren van Sleutelkluis in Azure-Stack met behulp van de portal
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: D4300668-461F-45F6-BF3B-33B502C39D17
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: mabrigg
ms.openlocfilehash: d76a1e188c5a5bf008ac2fba9b43741a6a8d97b1
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="manage-key-vault-in-azure-stack-by-using-the-portal"></a>Sleutelkluis in Azure-Stack beheren via de portal

U kunt de Sleutelkluis in Azure-Stack beheren met behulp van de Stack van Azure-portal. In dit artikel helpt u aan de slag maken en beheren van een sleutelkluis in Azure-Stack. 

## <a name="prerequisites"></a>Vereisten  

U moet zich abonneren op een aanbieding met de Azure Sleutelkluis-service.
 
## <a name="create-a-key-vault"></a>Een sleutelkluis maken 

1. Aanmelden bij de [gebruikersportal](https://portal.local.azurestack.external).  

2. Selecteer in het dashboard **nieuw** > **beveiliging en identiteit** > **Sleutelkluis**.  

    ![Sleutelkluis-scherm](media/azure-stack-kv-manage-portal/image1.png)  

3. In de **Sleutelkluis maken** deelvenster toewijzen een **naam** voor uw kluis. Namen van de kluis kunnen alleen alfanumerieke tekens en het afbreekstreepje speciaal teken (-) bevatten. Zij mag niet beginnen met een cijfer.  

4. Kies een **abonnement** uit de lijst met beschikbare abonnementen. Alle abonnementen die worden geboden door de Sleutelkluis-service worden weergegeven in de vervolgkeuzelijst.  

5. Selecteer een bestaande **resourcegroep** of maak een nieuwe.  

6. Selecteer de **prijscategorie**.  
    >[!NOTE]
    > Sleutel kluizen in de ondersteuning van Azure Stack Development Kit **standaard** alleen SKU's.

7. Kies een van de bestaande **toegangsbeleid** of maak een nieuwe. Een toegangsbeleid kunt u machtigingen voor een gebruiker, toepassing of een beveiligingsgroep bewerkingen aan deze kluis uit te voeren.  

8. U kunt eventueel een **geavanceerde toegangsbeleid** om functies, zoals toegang tot virtuele machines (VM's) voor de implementatie, toegang krijgen tot naar Resource Manager voor de sjabloonimplementatie van de en de toegang tot Azure Disk Encryption voor versleuteling voor volumes. 
  
9.  Nadat u de instellingen configureren, selecteert u **OK**, en selecteer vervolgens **maken**. Hiermee start u de sleutelkluis-implementatie. 

## <a name="manage-keys-and-secrets"></a>Sleutels en geheimen beheren

Nadat u een kluis hebt gemaakt, gebruik de volgende stappen voor het maken en beheren van sleutels en geheimen in de kluis.

### <a name="create-a-key"></a>Een sleutel maken

1. Aanmelden bij de [gebruikersportal](https://portal.local.azurestack.external).  

2. Selecteer in het dashboard **alle resources**, selecteert u de sleutelkluis die u eerder hebt gemaakt en selecteer vervolgens de **sleutels** tegel.  

3. In de **sleutels** deelvenster **toevoegen**. 

4. In de **maakt u een sleutel** deelvenster uit de lijst met **opties**, kiest u de methode die u gebruiken wilt voor het maken van een sleutel. U kunt **genereren** een nieuwe sleutel **uploaden** een bestaande sleutel, of gebruik **back-up herstellen** een back-up van een sleutel te selecteren.  

5. Voer een **naam** voor uw sleutel. Naam van de sleutel mag alleen alfanumerieke tekens en het afbreekstreepje speciaal teken (-) bevatten.  

6. Configureer desgewenst de **activeringsdatum ingesteld** en **verloopdatum instellen** waarden voor de sleutel.  

7. Selecteer **maken** implementatie te starten.  

Nadat de sleutel is gemaakt, kunt u het selecteren onder **sleutels** en weergeven of wijzigen van de eigenschappen ervan. De sectie eigenschappen bevat de **sleutel-id**, die een URI Uniform Resource Identifier () waarmee externe toepassingen toegang deze sleutel tot hebben is. Om te beperken van bewerkingen voor deze sleutel, configureer de instellingen onder **operations toegestaan**.

![URI-sleutel](media/azure-stack-kv-manage-portal/image4.png)  

### <a name="create-a-secret"></a>Een geheim maken 

1. Aanmelden bij de [gebruikersportal](https://portal.local.azurestack.external).  
2. Selecteer in het dashboard **alle resources**, selecteert u de sleutelkluis die u eerder hebt gemaakt en selecteer vervolgens de **geheimen** tegel.  

3. Onder **geheimen**, selecteer **toevoegen**.  

4. Onder **maken van een geheim**, uit de lijst met **uploaden opties**, kiest u een optie waarmee u wilt maken van een geheim. U kunt een geheim maken **handmatig** als u een waarde voor de geheim of uploaden van opgeeft een **certificaat** van uw lokale computer.  

5. Voer een **naam** voor het geheim. De geheime naam mag alleen alfanumerieke tekens en het afbreekstreepje speciaal teken (-) bevatten.  

6. Geef eventueel de **inhoudstype**, en configureer de waarden voor **activeringsdatum ingesteld** en **verloopdatum instellen** voor het geheim.  

7. Selecteer **maken** implementatie te starten.  

Nadat het geheim is gemaakt, kunt u het selecteren onder **geheimen** en weergeven of wijzigen van de eigenschappen ervan. De sectie eigenschappen bevat een **geheim id**, die een URI die externe toepassingen toegang dit geheim tot is. 

![URI-geheim](media/azure-stack-kv-manage-portal/image5.png) 


## <a name="next-steps"></a>Volgende stappen
* [Een virtuele machine implementeren door op te halen van het wachtwoord dat is opgeslagen in de Sleutelkluis](azure-stack-kv-deploy-vm-with-secret.md) 
* [Een virtuele machine implementeren met Sleutelkluis opgeslagen certificaat](azure-stack-kv-push-secret-into-vm.md)     


