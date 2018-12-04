---
title: Zelfstudie - de gebruikersinterface van uw toepassingen in Azure Active Directory B2C aanpassen | Microsoft Docs
description: Informatie over het aanpassen van de gebruikersinterface van uw toepassingen in Azure Active Directory B2C met behulp van de Azure portal.
services: B2C
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f8ebb282d3f6abbc37739891c0f7228bef110d82
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842676"
---
# <a name="tutorial-customize-the-user-interface-of-your-applications-in-azure-active-directory-b2c"></a>Zelfstudie: De gebruikersinterface van uw toepassingen in Azure Active Directory B2C aanpassen

Voor meer algemene gebruikerservaringen, zoals registratie, aanmelding en profielbewerking, kunt u [gebruikersstromen](active-directory-b2c-reference-policies.md) in Azure Active Directory (Azure AD) B2C. De informatie in deze zelfstudie helpt u om te leren hoe u [aanpassen van de gebruikersinterface (UI)](customize-ui-overview.md) van deze ervaringen met behulp van uw eigen HTML en CSS-bestanden.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * UI-aanpassing-bestanden maken
> * Een zich kunnen registreren en aanmelden gebruikersstroom die gebruikmaakt van de bestanden maken
> * De aangepaste gebruikersinterface testen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit nog niet hebt gemaakt uw eigen [Azure AD B2C-Tenant](tutorial-create-tenant.md), Maak nu een. U kunt een bestaande tenant gebruiken als u een in een vorige zelfstudie hebt gemaakt.

## <a name="create-customization-files"></a>Van aanpassingsbestanden maken

U maakt een Azure storage-account en een container en plaatst vervolgens eenvoudige HTML en CSS-bestanden in de container.

### <a name="create-a-storage-account"></a>Create a storage account

Hoewel u kunt uw bestanden opslaan op veel manieren voor deze zelfstudie, u ze in opslaat [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md).

1. Zorg ervoor dat u de map met uw Azure-abonnement. Selecteer de **map- en abonnementsfilter** in het bovenste menu en kiest u de map waarin u uw abonnement. Deze map is anders dan de database met uw Azure B2C-tenant.

    ![Schakel over naar de abonnementsmap](./media/tutorial-customize-ui/switch-directories.png)

2. Kies alle services in de linkerbovenhoek van Azure portal, zoek en selecteer **opslagaccounts**. 
3. Selecteer **Toevoegen**.
4. Onder **resourcegroep**, selecteer **nieuw**, voer een naam voor de nieuwe resourcegroep en klik vervolgens op **OK**.
5. Voer een naam in voor het opslagaccount. De naam die u kiest, moet uniek zijn in Azure, moet tussen de 3 en 24 tekens lang zijn en mag alleen cijfers en kleine letters bevatten.
6. Selecteer de locatie van het storage-account of accepteer de standaardlocatie. 
7. Accepteer alle overige standaardwaarden, selecteert u **revisie + maken**, en klik vervolgens op **maken**.
8. Nadat het opslagaccount is gemaakt, selecteert u **naar de resource gaan**.

### <a name="create-a-container"></a>Een container maken

1. Selecteer op de overzichtspagina van het opslagaccount dat **Blobs**.
2. Selecteer **Container**, voer een naam voor de container, kies **Blob (anonieme leestoegang voor alleen blobs)**, en klik vervolgens op **OK**.

### <a name="enable-cors"></a>CORS inschakelen

 Azure AD B2C-code in een browser gebruikmaakt van een moderne en standard-benadering aangepaste inhoud laden vanuit een URL die u in de gebruikersstroom van een opgeeft. Cross-origin resource sharing (CORS) kunt beperkte resources op een webpagina moet worden gevraagd uit andere domeinen.

1. Selecteer in het menu **CORS**.
2. Voor **oorsprongen toegestaan**, voer `https://your-tenant-name.b2clogin.com`. Vervang `your-tenant-name` met de naam van uw Azure AD B2C-tenant. Bijvoorbeeld `https://fabrikam.b2clogin.com`. U moet alle kleine letters gebruiken bij het invoeren van de tenantnaam van uw.
3. Voor **toegestaan methoden**, selecteert u beide `GET` en `OPTIONS`.
4. Voor **toegestaan Headers**, geeft u een sterretje (*).
5. Voor **blootgesteld Headers**, geeft u een sterretje (*).
6. Voor **maximumleeftijd**, Voer 200.

    ![CORS inschakelen](./media/tutorial-customize-ui/enable-cors.png)

5. Klik op **Opslaan**.

### <a name="create-the-customization-files"></a>De bestanden voor aanpassingen maken

Voor het aanpassen van de gebruikersinterface van de registratie-ervaring, begint u met het maken van een eenvoudige HTML en CSS-bestand. U kunt uw HTML-code zoals u dat wilt, maar deze moet een **div** element met een id van `api`. Bijvoorbeeld `<div id="api"></div>`. Azure AD B2C injects elementen toe aan de `api` container wanneer de pagina wordt weergegeven.

1. Maak in een lokale map het volgende bestand en zorg ervoor dat u wijzigen `your-storage-account` op de naam van het opslagaccount en `your-container` op de naam van de container die u hebt gemaakt. Bijvoorbeeld `https://store1.blob.core.windows.net/b2c/style.css`.

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>My B2C Application</title>
        <link rel="stylesheet" href="https://your-storage-account.blob.core.windows.net/your-container/style.css">
      </head>
      <body>  
        <h1>My B2C Application</h1>
        <div id="api"></div>
      </body>
    </html>
    ```

    De pagina een manier die u wilt, kan worden ontworpen, maar de **api** div-element is vereist voor een HTML-bestand voor aanpassing die u maakt. 

3. Sla het bestand op als *aangepaste ui.html*.
4. Maak de volgende eenvoudige CSS die alle elementen op de pagina met het registreren of aanmelden met inbegrip van de elementen die Azure AD B2C injects datacenters.

    ```css
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center; 
    }
    .entry {
      width: 300px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center; 
    }
    .create {
      width: 300px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

5. Sla het bestand op als *style.css*.

### <a name="upload-the-customization-files"></a>De aanpassingsbestanden uploaden

In deze zelfstudie slaat u de bestanden die u in de storage-account hebt gemaakt, zodat ze toegankelijk in de Azure AD B2C.

1. Kies **alle services** Zoek in de linkerbovenhoek van Azure portal en selecteer **opslagaccounts**.
2. Selecteer het opslagaccount dat u hebt gemaakt, selecteert u **Blobs**, en selecteer vervolgens de container die u hebt gemaakt.
3. Selecteer **uploaden**, navigeer naar en selecteer de *aangepaste ui.html* bestand en klik vervolgens op **uploaden**.

    ![Uploaden van bestanden voor aanpassing](./media/tutorial-customize-ui/upload-blob.png)

4. Kopieer de URL voor het bestand dat u hebt geüpload voor het gebruik van later in de zelfstudie.
5. Herhaal stap 3 en 4 voor de *style.css* bestand.

## <a name="create-a-sign-up-and-sign-in-user-flow"></a>Een zich kunnen registreren en aanmelden gebruikersstroom maken

Als u wilt de stappen in deze zelfstudie hebt voltooid, moet u een stroom van de toepassing en meld u aan of aanmelden gebruiker test maken in Azure AD B2C. U kunt de beginselen beschreven in deze zelfstudie aan de andere gebruikerservaringen, zoals het bewerken van profielen toepassen.

### <a name="create-an-azure-ad-b2c-application"></a>Een Azure AD B2C-toepassing maken

Communicatie met Azure AD B2C vindt plaats via een toepassing die u in uw tenant maakt. De volgende stappen maakt u een toepassing waarmee het verificatietoken dat wordt geretourneerd naar [ https://jwt.ms ](https://jwt.ms).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
4. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
5. Voer een naam voor de toepassing, bijvoorbeeld *testapp1*.
6. Voor **Web-App / Web-API**, selecteer `Yes`, en voer vervolgens `https://jwt.ms` voor de **antwoord-URL**.
7. Klik op **Create**.

### <a name="create-the-user-flow"></a>De gebruikersstroom maken

Als u wilt testen van uw aanpassingsbestanden, maakt u een ingebouwde registreren of aanmelden gebruiker-stroom die gebruikmaakt van de toepassing die u eerder hebt gemaakt.

1. Selecteer in uw Azure AD B2C-tenant, **gebruikersstromen**, en klik vervolgens op **nieuwe gebruikersstroom**.
2. Op de **aanbevolen** tabblad **aanmelden en aanmelden**.
3. Voer een naam voor de gebruikersstroom. Bijvoorbeeld, *signup_signin*. Het voorvoegsel *B2C_1* wordt automatisch toegevoegd aan de naam van de wanneer de gebruikersstroom wordt gemaakt.
4. Onder **id-providers**, selecteer **registratie via E-mail**.
5. Onder **gebruikerskenmerken en claims**, klikt u op **meer weergeven**.
6. In de **verzamelen kenmerk** kolom, kiest u de kenmerken die u wenst te verzamelen van de klant tijdens de registratie. Bijvoorbeeld, stel **land/regio**, **weergavenaam**, en **postcode**.
7. In de **geretourneerde claim** kolom, kies de claims die u laten retourneren in de autorisatietokens die is verzonden naar de toepassing na een geslaagde registratie- of aanmelden wilt. Selecteer bijvoorbeeld **Weergavenaam**, **Id-provider**, **Postcode**, **Gebruiker is nieuw** en **Object-id van gebruiker**.
8. Klik op **OK**.
9. Klik op **Create**.
10. Onder **aanpassen**, selecteer **pagina-indelingen**. Selecteer **Unified registreren of aanmelden pagina**, en klik op **Ja** voor **aangepaste pagina-inhoud gebruiken**.
11. In **aangepaste pagina URI**, voert u de URL voor de *aangepaste ui.html* -bestand dat u eerder hebt genoteerd.
12. Aan de bovenkant van de pagina, klikt u op **opslaan**.

## <a name="test-the-user-flow"></a>De gebruikersstroom testen

1. Selecteer in uw Azure AD B2C-tenant, **gebruikersstromen** en selecteert u de gebruikersstroom die u hebt gemaakt. Bijvoorbeeld, *B2C_1_signup_signin*.
2. Aan de bovenkant van de pagina, klikt u op **gebruikersstroom uitvoeren**.
3. Klik op de **gebruikersstroom uitvoeren** knop.

    ![De gebruikersstroom registreren of aanmelden uitvoeren](./media/tutorial-customize-ui/run-user-flow.png)

    U ziet een pagina zoals in het volgende voorbeeld met de elementen die zijn gericht op basis van het CSS-bestand dat u hebt gemaakt:

    ![Gebruiker stroom resultaten](./media/tutorial-customize-ui/run-now.png) 

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u:

> [!div class="checklist"]
> * UI-aanpassing-bestanden maken
> * Een zich kunnen registreren en aanmelden gebruikersstroom die gebruikmaakt van de bestanden maken
> * De aangepaste gebruikersinterface testen

> [!div class="nextstepaction"]
> [Aanpassing van taal in Azure Active Directory B2C](active-directory-b2c-reference-language-customization.md)