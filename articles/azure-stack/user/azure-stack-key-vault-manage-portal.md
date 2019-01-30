---
title: Key Vault in Azure Stack met behulp van de portal beheren | Microsoft Docs
description: Informatie over het beheren van Key Vault in Azure Stack met behulp van de portal
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: D4300668-461F-45F6-BF3B-33B502C39D17
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: bc73e80acddaac8676b4cb47c0b9ced2467cdc44
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245453"
---
# <a name="manage-key-vault-in-azure-stack-by-using-the-portal"></a>Key Vault in Azure Stack met behulp van de portal beheren

U kunt Key Vault in Azure Stack kunt beheren met behulp van de Azure Stack-portal. In dit artikel wordt beschreven hoe u maken en beheren van een key vault in Azure Stack.

## <a name="prerequisites"></a>Vereisten

U moet zich abonneren op een aanbieding met inbegrip van de Azure Key Vault-service.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

1. Aanmelden bij de [gebruikersportal](https://portal.local.azurestack.external).

2. Selecteer in het dashboard, **+ een resource maken**, klikt u vervolgens **beveiliging en identiteit**, klikt u vervolgens **Key Vault**.

    ![Key Vault-scherm](media/azure-stack-key-vault-manage-portal/image1.png)

3. In de **Key Vault maken** deelvenster toewijzen een **naam** voor uw kluis. Namen van de kluis kunnen alleen alfanumerieke tekens en het koppelteken (-) bevatten. Deze mag niet beginnen met een cijfer.

4. Kies een **abonnement** uit de lijst met beschikbare abonnementen. Alle abonnementen die worden geboden door de service Key Vault worden weergegeven in de vervolgkeuzelijst.

5. Selecteer een bestaande **resourcegroep**, of maak een nieuwe.

6. Selecteer de **prijscategorie**. In de Azure Stack Development Kit (ASDK), ondersteuning voor sleutelkluizen **Standard** alleen SKU's.

7. Kies een van de bestaande **toegangsbeleid** of maak een nieuwe. Een toegangsbeleid kunt u machtigingen voor een gebruiker, toepassing of een beveiligingsgroep bewerkingen bij deze kluis uit te voeren.

8. (Optioneel) Kies een **geavanceerde toegangsbeleid** voor toegang tot functies. Bijvoorbeeld: virtuele machines (VM's) voor de implementatie van Resource Manager voor de sjabloonimplementatie en toegang tot Azure Disk Encryption voor versleuteling van het volume.

9. Nadat u de instellingen configureren, selecteert u **OK**, en selecteer vervolgens **maken**. Hiermee start u de sleutelkluis-implementatie.

## <a name="manage-keys-and-secrets"></a>Sleutels en geheimen beheren

Nadat u een kluis hebt gemaakt, gebruikt u de volgende procedure voor het maken en beheren van sleutels en geheimen in de kluis.

### <a name="create-a-key"></a>Een sleutel maken

1. Aanmelden bij de [gebruikersportal](https://portal.local.azurestack.external).

2. Selecteer in het dashboard, **alle resources**, selecteert u de sleutelkluis die u eerder hebt gemaakt en selecteer vervolgens de **sleutels** tegel.

3. In de **sleutels** venster **toevoegen**.

4. In de **maakt u een sleutel** deelvenster in de lijst van **opties**, kiest u de methode die u wilt gebruiken om een sleutel te maken. U kunt **genereren** een nieuwe sleutel **uploaden** een bestaande sleutel, of gebruik **back-up herstellen** om te selecteren van een back-up van een sleutel.

5. Voer een **naam** voor uw sleutel. Naam van de sleutel mag alleen alfanumerieke tekens en het koppelteken (-) bevatten.

6. Configureer desgewenst het **activeringsdatum instellen** en **vervaldatum instellen** waarden voor uw sleutel.

7. Selecteer **maken** implementatie te starten.

Nadat de sleutel is gemaakt, kunt u het selecteren onder **sleutels** en weergeven of wijzigen van de eigenschappen ervan. De sectie met eigenschappen bevat de **sleutel-id**, dit is een Uniform Resource Identifier (URI) dat externe toepassingen gebruiken voor toegang tot deze sleutel. Als u wilt beperken van bewerkingen op deze sleutel, configureert u de instellingen onder **operations toegestaan**.

![URI-sleutel](media/azure-stack-key-vault-manage-portal/image4.png)

### <a name="create-a-secret"></a>Een geheim maken

1. Aanmelden bij de [gebruikersportal](https://portal.local.azurestack.external).

2. Selecteer in het dashboard, **alle resources**, selecteert u de sleutelkluis die u eerder hebt gemaakt en selecteer vervolgens de **geheimen** tegel.

3. Onder **geheimen**, selecteer **toevoegen**.

4. Onder **een geheim maken**, in de lijst van **uploadopties**, kies een optie waarmee u wilt maken van een geheim. U kunt een geheim maken **handmatig** als u een waarde voor het geheim of upload opgeven een **certificaat** van uw lokale computer.

5. Voer een **naam** voor de geheime sleutel. De geheime naam mag alleen alfanumerieke tekens en het koppelteken (-) bevatten.

6. (Optioneel) Geef de **inhoudstype**, en configureer de waarden voor **activeringsdatum instellen** en **vervaldatum instellen** voor de geheime sleutel.

7. Selecteer **maken** implementatie te starten.

Nadat de geheime sleutel is gemaakt, kunt u het selecteren onder **geheimen** en weergeven of wijzigen van de eigenschappen ervan. De **geheim id** is een URI die externe toepassingen gebruiken kunnen voor toegang tot dit geheim.

![URI-geheim](media/azure-stack-key-vault-manage-portal/image5.png)

## <a name="next-steps"></a>Volgende stappen

* [Een virtuele machine implementeren met het ophalen van het wachtwoord die zijn opgeslagen in Key Vault](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Een virtuele machine implementeren met een certificaat dat is opgeslagen in Key Vault](azure-stack-key-vault-push-secret-into-vm.md)
