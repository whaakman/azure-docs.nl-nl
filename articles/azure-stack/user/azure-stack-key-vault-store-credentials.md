---
title: Azure Stack store Servicereferenties-principal in Key Vault | Microsoft Docs
description: Informatie over hoe Key Vault worden opgeslagen referenties voor service-principal in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: sethm
ms.openlocfilehash: 3fcbf2b3160d57e56a59ba9c374c9b1b2a75a159
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54330242"
---
# <a name="store-service-principal-credentials-in-key-vault"></a>Referenties voor service-principal Store in Key Vault

Doorgaans het ontwikkelen van toepassingen in Azure Stack, moet een service-principal maken en deze referenties gebruiken om te verifiëren voordat u implementeert. Echter vaak de opgeslagen referenties voor de service-principal zijn verkeerd wordt geplaatst. In dit artikel wordt beschreven hoe u een service-principal maken en de waarden in Azure Key Vault opslaan voor later gebruik.

Zie voor meer informatie over Key Vault [in dit artikel](azure-stack-key-vault-intro.md).

## <a name="prerequisites"></a>Vereisten

- Een abonnement op een aanbieding met de Azure Key Vault-service.
- PowerShell is geconfigureerd voor gebruik met Azure Stack.

## <a name="key-vault-in-azure-stack"></a>Key Vault in Azure Stack

Key Vault in Azure Stack helpt ter bescherming van cryptografische sleutels en geheimen die toepassingen en services in de cloud gebruiken. Met Key Vault, kunt u sleutels en geheimen versleutelen.

Volg deze stappen voor het maken van een key vault:

1. Aanmelden bij de Azure Stack-portal.

2. Selecteer in het dashboard, **+ een resource maken**, klikt u vervolgens **beveiliging en identiteit**en selecteer vervolgens **Key Vault.**

   ![Sleutelkluis maken](media/azure-stack-key-vault-store-credentials/create-key-vault.png)

3. In de **Key Vault maken** deelvenster toewijzen een **naam** voor uw kluis. Namen van de kluis kunnen alleen alfanumerieke tekens en het koppelteken (-) bevatten. Deze mag niet beginnen met een cijfer.

4. Kies een abonnement in de lijst met beschikbare abonnementen.

5. Selecteer een bestaande resourcegroep of maak een nieuwe.

6. Selecteer de prijscategorie.

7. Kies een van de bestaande beleidsregels voor toegang of een nieuwe maken. Een toegangsbeleid kunt u machtigingen voor een gebruiker, toepassing of een beveiligingsgroep bewerkingen bij deze kluis uit te voeren.

8. (Optioneel) Kies een geavanceerde toegangsbeleid voor toegang tot functies.

9. Nadat u de instellingen configureren, selecteert u **OK**, en selecteer vervolgens **maken**. De sleutelkluis-implementatie begint.

## <a name="create-a-service-principal"></a>Een service-principal maken

1. Aanmelden bij uw Azure-account via Azure portal.

2. Selecteer **Azure Active Directory**, klikt u vervolgens **App-registraties**, klikt u vervolgens **toevoegen**.

3. Geef een naam en URL op voor de toepassing. Selecteer een **Web-app / API** of **systeemeigen** voor het type van de toepassing die u wilt maken. Na het instellen van de waarden, selecteer **maken**.

4. Selecteer **Active Directory**, klikt u vervolgens **App-registraties**, en selecteer uw toepassing.

5. Kopieer de **Toepassings-id** en sla deze op in uw toepassingscode. De toepassingen in de voorbeeldtoepassingen **client-ID** met betrekking tot de **toepassings-ID**.

6. Selecteer **Sleutels** om een verificatiesleutel te genereren.

7. Geef een beschrijving voor de sleutel en een duur.

8. Selecteer **Opslaan**.

9. Kopieer de **sleutel** dat in de toekomst beschikbaar nadat u hebt geklikt **opslaan**.

## <a name="store-the-service-principal-inside-key-vault"></a>De service-principal in Key Vault Store

1. Aanmelden bij de gebruikersportal voor Azure Stack, en vervolgens selecteert u de sleutelkluis die u eerder hebt gemaakt en selecteer vervolgens de **geheim** tegel.

2. In de **geheim** venster **genereren/importeren**.

3. In de **een geheim maken** deelvenster in de lijst met opties selecteren **handmatig**. Als u de service principal hebt gemaakt met behulp van certificaten, selecteert u de certificaten in de vervolgkeuzelijst en vervolgens het bestand uploaden.

4. Voer **de toepassings-ID** gekopieerd uit de service-principal als de naam voor de sleutel. Naam van de sleutel mag alleen alfanumerieke tekens en het koppelteken (-) bevatten.

5. Plak de waarde van de sleutel van de service-principal in de **waarde** tabblad.

6. Selecteer **Service-Principal** voor de **inhoudstype**.

7. Stel de **activeringsdatum** en **vervaldatum** waarden voor uw sleutel.

8. Selecteer **maken** implementatie te starten.

Nadat de geheime sleutel is gemaakt, kan de gegevens voor de service-principal, er worden opgeslagen. U kunt dit op elk gewenst moment onder selecteren **geheimen**, en weergeven of wijzigen van de eigenschappen ervan. De sectie met eigenschappen bevat de geheime id, dit is een Uniform Resource Identifier (URI) dat externe toepassingen gebruiken voor toegang tot dit geheim.

## <a name="next-steps"></a>Volgende stappen

- [Service-principals gebruiken](azure-stack-create-service-principals.md)
- [Key Vault in Azure Stack door de portal beheren](azure-stack-key-vault-manage-portal.md)  
- [Key Vault in Azure Stack beheren met behulp van PowerShell](azure-stack-key-vault-manage-powershell.md)