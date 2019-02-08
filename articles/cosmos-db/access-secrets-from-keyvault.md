---
title: Key Vault gebruiken voor het opslaan en openen van Azure Cosmos DB-sleutels
description: Azure Key Vault gebruiken voor het opslaan en toegang tot Azure Cosmos DB-verbindingsreeks, sleutels, -eindpunten.
author: rafats
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: rafats
ms.reviewer: sngun
ms.openlocfilehash: 770e1e543a16bb54acc216aa550c44be26ab7a1a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55858448"
---
# <a name="secure-azure-cosmos-keys-using-azure-key-vault"></a>Azure Cosmos-sleutels met behulp van Azure Key Vault beveiligen 

Wanneer u Azure Cosmos DB gebruikt voor uw toepassingen, kunt u de database, verzamelingen, documenten openen met behulp van het eindpunt en de sleutel in het configuratiebestand van de app.  Het is echter niet veilig om op te zetten van sleutels en de URL rechtstreeks in de code van de toepassing omdat ze beschikbaar in niet-versleutelde tekst opmaken voor alle gebruikers zijn. U wilt ervoor zorgen dat het eindpunt en de sleutels beschikbaar, maar via een beveiligde mechanisme zijn. Dit is waar Azure Key Vault kunt u veilig opslaan en beheren van toepassingsgeheimen.

De volgende stappen zijn vereist voor het opslaan en lezen van Azure Cosmos DB-toegangssleutels uit Key Vault:

* Een sleutelkluis maken  
* Toegangssleutels voor Azure Cosmos DB toevoegen aan de Key Vault  
* Een Azure-web-toepassing maken  
* Registreren van de toepassing en machtigingen verlenen voor het lezen van de Key Vault  


## <a name="create-a-key-vault"></a>Een sleutelkluis maken

1. Aanmelden bij [Azure-Portal](https://portal.azure.com/).  
2. Selecteer **een resource maken > Beveiliging > Key Vault**.  
3. Geef in de sectie **Sleutelkluis maken** de volgende gegevens op:  
   * **Naam:** Geef een unieke naam voor uw Key Vault.  
   * **Abonnement:** Kies het abonnement dat u wilt gebruiken.  
   * Kies **Nieuw** bij **Resourcegroep** en voer de naam van een resourcegroep in.  
   * Kies een locatie in de vervolgkeuzelijst locatie.  
   * Houd de overige opties de standaardwaarden.  
4. Selecteer na het opgeven van de bovenstaande gegevens **Maken**.  

## <a name="add-azure-cosmos-db-access-keys-to-the-key-vault"></a>Toegangssleutels voor Azure Cosmos DB toevoegen aan de Key Vault.
1. Navigeer naar de Sleutelkluis die u hebt gemaakt in de vorige stap, open de **geheimen** tabblad.  
2. Selecteer **+ genereren/importeren**, 

   * Selecteer **handmatig** voor **uploadopties**.
   * Geef een **naam** voor uw geheim
   * Geef de verbindingsreeks van uw Cosmos DB-account in de **waarde** veld. En selecteer vervolgens **maken**.

   ![Een geheim maken](./media/access-secrets-from-keyvault/create-a-secret.png)

4. Nadat de geheime sleutel is gemaakt, open het en kopieer de ** geheim-id die in de volgende indeling is. U gebruikt deze id in de volgende sectie. 

   `https://<Key_Vault_Name>.vault.azure.net/secrets/<Secret _Name>/<ID>`

## <a name="create-an-azure-web-application"></a>Een Azure-web-toepassing maken

1. Een Azure-web-toepassing maken of kunt u de app vanuit de [GitHub-opslagplaats](https://github.com/Azure/azure-cosmosdb-dotnet/tree/master/Demo/keyvaultdemo). Er is een eenvoudige MVC-toepassing.  

2. Pak het gedownloade toepassing uit en opent u de **HomeController.cs** bestand. Werk de geheime-ID in de volgende regel:

   `var secret = await keyVaultClient.GetSecretAsync("<Your Key Vault’s secret identifier>")`

3. **Sla** het bestand **bouwen** de oplossing.  
4. Vervolgens implementeert u de toepassing in Azure. Klik met de rechtermuisknop op het project en kies **publiceren**. Maak een nieuw app service-profiel (u kunt de app WebAppKeyVault1 naam) en selecteer **publiceren**.   

5. Wanneer de toepassing is geïmplementeerd. Vanuit de Azure-portal, gaat u naar de web-app die u hebt geïmplementeerd, en schakel de **beheerde service-identiteit** van deze toepassing.  

   ![Managed Service Identity](./media/access-secrets-from-keyvault/turn-on-managed-service-identity.png)

Als u de toepassing nu uitvoert, ziet u de volgende fout, zoals u machtiging voor deze toepassing niet in Key Vault gegeven hebt.

![-App geïmplementeerd zonder toegang tot](./media/access-secrets-from-keyvault/app-deployed-without-access.png)

## <a name="register-the-application--grant-permissions-to-read-the-key-vault"></a>Registreren van de toepassing en machtigingen verlenen voor het lezen van de Key Vault

In deze sectie maakt u de toepassing registreren bij Azure Active Directory en machtigingen voor de toepassing te lezen van de Key Vault. 

1. Navigeer naar de Azure-portal, open de **Key Vault** u in de vorige sectie hebt gemaakt.  

2. Open **toegangsbeleid**, selecteer **+ nieuwe toevoegen** vinden van de web-app die u hebt geïmplementeerd, machtigingen selecteren en selecteer **OK**.  

   ![Toegangsbeleid toevoegen](./media/access-secrets-from-keyvault/add-access-policy.png)

Als u de toepassing uitvoert, kunt u nu het geheim lezen uit Key Vault.

![-App geïmplementeerd met geheim](./media/access-secrets-from-keyvault/app-deployed-with-access.png)
 
Op deze manier kunt u een gebruiker voor toegang tot de key Vault toevoegen. U moet zelf toevoegen aan de Key Vault hiervoor **toegangsbeleid** en hen vervolgens machtigen alle machtigingen die u nodig hebt voor het uitvoeren van de toepassing vanuit Visual studio. Als deze toepassing wordt uitgevoerd vanaf het bureaublad, gaat uw identiteit.

## <a name="next-steps"></a>Volgende stappen

* Een firewall configureren voor Azure Cosmos DB Zie [firewall-ondersteuning](firewall-support.md) artikel.
* Zie configureren van service-eindpunt voor virtueel netwerk [toegang beveiligen met behulp van VNet-service-eindpunt](vnet-service-endpoint.md) artikel.
