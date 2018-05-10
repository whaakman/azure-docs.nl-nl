---
title: Azure Blockchain Workbench implementeren
description: Azure Blockchain Workbench implementeren
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/22/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 01f396b4a2b8851ce1433a297981d30328c113b8
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="deploy-azure-blockchain-workbench"></a>Azure Blockchain Workbench implementeren

Azure Blockchain Workbench wordt geïmplementeerd met behulp van een oplossingssjabloon in Azure Marketplace. De sjabloon vereenvoudigt de implementatie van onderdelen die nodig zijn om blockchain toepassingen te maken. Zodra geïmplementeerd, biedt Blockchain Workbench toegang tot de client-apps maken en beheren van gebruikers en blockchain toepassingen.

Zie voor meer informatie over de onderdelen van Blockchain Workbench [Azure Blockchain Workbench architectuur](blockchain-workbench-architecture.md).

## <a name="prepare-for-deployment"></a>Implementatie voorbereiden

Azure Blockchain Workbench vereist verschillende vereisten voor de installatie. De vereisten zijn de configuratie en toepassing registraties Azure AD.

### <a name="blockchain-workbench-api-app-registration"></a>Blockchain Workbench API-app-registratie

Blockchain Workbench implementeren, moet de registratie van een Azure AD-toepassing. U moet een tenant van Azure Active Directory (Azure AD) om de app te registreren. U kunt een bestaande tenant gebruiken of maak een nieuwe tenant. Als u van een bestaande Azure AD-tenant gebruikmaakt, moet u voldoende machtigingen om de toepassingen binnen een Azure AD-tenant te registreren. De toepassing registraties moeten in de tenant van de abonnementsbeheerder van het abonnement waarop Workbench wordt geïmplementeerd. Zie voor meer informatie over Azure AD-tenants [een Active Directory-tenant verkrijgen](../active-directory/develop/active-directory-howto-tenant.md) en [toepassingen integreren met Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer uw account in de rechterbovenhoek hoek en schakel over naar de gewenste Azure AD-tenant. De tenant moet de abonnementsbeheerder tenant van het abonnement waarbij Workbench is geïmplementeerd en u voldoende rechten hebt om toepassingen te registreren.
3. Selecteer in het navigatiedeelvenster links de **Azure Active Directory** service. Selecteer **App registraties** > **registratie van de nieuwe toepassing**.

    ![App-registratie](media/blockchain-workbench-deploy/app-registration.png)

4. Geef een **naam** en **aanmeldings-URL** voor de toepassing. U kunt de tijdelijke aanduiding voor waarden gebruiken omdat de waarden worden gewijzigd tijdens de implementatie. 

    ![Registratie van de app maken](media/blockchain-workbench-deploy/app-registration-create.png)

    |Instelling  | Waarde  |
    |---------|---------|
    |Naam | `Blockchain API` |
    |Toepassingstype |Web-app/-API|
    |Aanmeldings-URL | `https://blockchainapi` |

5. Selecteer **maken** registreren van de Azure AD-toepassing.

### <a name="modify-application-manifest"></a>Het toepassingsmanifest wijzigen

Vervolgens moet u het toepassingsmanifest voor het gebruik van toepassingsrollen binnen Azure AD kunt u opgeven Blockchain Workbench beheerders wijzigen.  Zie voor meer informatie over Toepassingsmanifesten [Azure Active Directory-toepassingsmanifest](../active-directory/develop/active-directory-application-manifest.md).

1. Selecteer voor de toepassing die u hebt geregistreerd, **Manifest** in het detailvenster van de geregistreerde toepassing.
2. Genereer een GUID. U kunt de PowerShell-opdracht `[guid]::NewGuid()` of online-hulpprogramma's voor het genereren van een GUID. 
3. U gaat bijwerken de **appRoles** sectie van het manifest. Selecteer in het manifest deelvenster bewerken **bewerken** en vervang `"appRoles": []` met de opgegeven JSON. Zorg ervoor dat u de waarde voor de **id** veld met de GUID die u hebt gegenereerd. 

    ``` json
    "appRoles": [
         {
           "allowedMemberTypes": [
             "User",
             "Application"
           ],
           "displayName": "Administrator",
           "id": "<A unique GUID>",
           "isEnabled": true,
           "description": "Blockchain Workbench administrator role allows creation of applications, user to role assignments, etc.",
           "value": "Administrator"
         }
       ],
    ```

    ![Manifest bewerken](media/blockchain-workbench-deploy/edit-manifest.png)

    > [!IMPORTANT]
    > De waarde **beheerder** nodig is voor het identificeren van Blockchain Workbench beheerders.

4.  Klik op **opslaan** de application manifest wijzigingen wilt opslaan.

### <a name="add-graph-api-key-to-application"></a>Graph API-sleutel toevoegen aan de toepassing

Blockchain Workbench maakt gebruik van Azure AD als het beheersysteem hoofdidentiteit voor gebruikers die interactie met blockchain toepassingen. Opdat de Blockchain Workbench voor toegang tot Azure AD en ophalen van gebruikersgegevens, zoals namen en e-mailberichten, moet u een toegangssleutel toevoegen. De sleutel Blockchain Workbench gebruikt voor verificatie met Azure AD.

1. Selecteer voor de toepassing die u hebt geregistreerd, **instellingen** in het detailvenster van de geregistreerde toepassing.
2. Selecteer **Sleutels**.
3. Voeg een nieuwe sleutel toe door op te geven van een sleutel **beschrijving** en het kiezen van **verloopt** duurwaarde. 

    ![Sleutel maken](media/blockchain-workbench-deploy/app-key-create.png)

    |Instelling  | Waarde  |
    |---------|---------|
    | Beschrijving | `Service` |
    | Verloopt op | Kies een duur voor het verlopen |

4. Selecteer **Opslaan**. 
5. Kopieer de waarde van de sleutel en opgeslagen voor later. U moet voor de implementatie.

    > [!IMPORTANT]
    >  Als u de sleutel voor de implementatie niet opslaat, moet u een nieuwe sleutel te genereren. U kunt de sleutelwaarde niet later ophalen vanuit de portal.

### <a name="add-graph-api-required-permissions"></a>Graph API vereist machtigingen toevoegen

De API-App moet om toestemming te vragen van de gebruiker toegang tot de map. De volgende vereiste machtiging voor de API-toepassing instellen:

1. Selecteer in de registratie van de app Blockchain API **instellingen > machtigingen vereist > Selecteer een API > Microsoft Graph**.

    ![Een API selecteren](media/blockchain-workbench-deploy/client-app-select-api.png)

    Klik op **Selecteren**.

2. In **toegang inschakelen** onder **Toepassingsmachtigingen**, kies **volledige profielen voor alle gebruikers lezen**.

    ![Toegang inschakelen](media/blockchain-workbench-deploy/client-app-read-perms.png)

    Klik op **Selecteer** klikt u vervolgens op **gedaan**.

3. In **vereist machtigingen**, selecteer **machtiging verlenen** Selecteer **Ja** verificatie wordt gevraagd.

   ![Machtigingen verlenen](media/blockchain-workbench-deploy/client-app-grant-permissions.png)

   Machtiging verlenen kunt Blockchain Workbench voor toegang tot gebruikers in de directory. De leesmachtiging is vereist om te zoeken en leden toevoegen aan Blockchain Workbench.

### <a name="get-application-id"></a>Toepassings-ID ophalen

ID- en tenantverkeer informatie over de toepassing zijn vereist voor de implementatie. Verzamelen en opslaan van de gegevens voor gebruik tijdens de implementatie.

1. Selecteer voor de toepassing die u hebt geregistreerd, **instellingen** > **eigenschappen**.
2.  In de **eigenschappen** deelvenster, kopiëren en de volgende voor later gebruik tijdens de implementatie waarden-archief.

    ![API-app-eigenschappen](media/blockchain-workbench-deploy/app-properties.png)

    | Instelling voor het opslaan van  | Gebruik in de implementatie |
    |------------------|-------------------|
    | Toepassings-id | Installatie van de Azure Active Directory > toepassings-ID |

### <a name="get-tenant-domain-name"></a>Tenant domeinnaam ophalen

Verzamelen en opslaan van de domeinnaam van de Active Directory-tenant waar de toepassingen die zijn geregistreerd. 

Selecteer in het navigatiedeelvenster links de **Azure Active Directory** service. Selecteer **aangepaste domeinnamen**. Kopiëren en opslaan van de domeinnaam.

![Domeinnaam](media/blockchain-workbench-deploy/domain-name.png)

## <a name="deploy-blockchain-workbench"></a>Blockchain Workbench implementeren

Nadat de vereiste stappen zijn voltooid, bent u klaar voor het implementeren van de Blockchain Workbench. De volgende secties worden het implementeren van het framework.

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Selecteer uw account in de rechterbovenhoek hoek en overschakelt naar de gewenste Azure AD-tenant waarvoor Azure Blockchain Workbench implementeren.
3.  Selecteer in het linkerdeelvenster **maken van een resource**. Zoeken naar `Azure Blockchain Workbench` in de **zoeken van de Marketplace** zoekbalk. 

    ![De zoekbalk Marketplace](media/blockchain-workbench-deploy/marketplace-search-bar.png)

4.  Selecteer **Azure Blockchain Workbench**.

    ![Zoekresultaten Marketplace](media/blockchain-workbench-deploy/marketplace-search-results.png)

4.  Selecteer **Maken**.
5.  Voltooi de basisinstellingen.

    ![Azure Blockchain Workbench maken](media/blockchain-workbench-deploy/blockchain-workbench-settings-basic.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Resource-voorvoegsel | Korte unieke id voor uw implementatie. Deze waarde wordt gebruikt als basis voor de naamgeving van resources. |
    | VM-gebruikersnaam | Naam van de gebruiker wordt gebruikt als beheerder voor alle virtuele machines (VM). |
    | Verificatietype | Selecteer als u wilt gebruiken van een wachtwoord of sleutel voor de verbinding met virtuele machines. |
    | Wachtwoord | Het wachtwoord wordt gebruikt voor de verbinding met virtuele machines. |
    | SSH | Gebruik van een openbare RSA-sleutel in de indeling van één regel die begint met **ssh-rsa** of gebruik de PEM-indeling van meerdere regels. U kunt met behulp van SSH-sleutels genereren `ssh-keygen` op Linux en OS X of PuTTYGen in Windows met. Zie voor meer informatie over het SSH-sleutels [het gebruik van SSH-sleutels met Windows in Azure](../virtual-machines/linux/ssh-from-windows.md). |
    | Databasewachtwoord / databasewachtwoord bevestigen | Geef het wachtwoord moet worden gebruikt voor toegang tot de database gemaakt als onderdeel van de implementatie. |
    | Abonnement | Geef het Azure-abonnement dat u wilt gebruiken voor uw implementatie. |
    | Resourcegroepen | Een nieuwe resourcegroep maken door te selecteren **nieuw** en geef de naam van een unieke resource-groep. |
    | Locatie | Geef de regio die u wilt implementeren van het framework. |

6.  Selecteer **OK** voor het voltooien van de instelling basis-configuratiesectie.

7.  Voltooi de **Azure Active Directory-installatie**.

    ![Installatie van de Azure AD](media/blockchain-workbench-deploy/blockchain-workbench-settings-aad.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Domeinnaam | Gebruik de Azure AD-tenant verzameld in de [Get tenant domeinnaam](#get-tenant-domain-name) sectie vereisten. |
    | Toepassings-id | Gebruik de toepassings-ID van de Blockchain app clientregistratie verzameld in de [toepassings-ID ophalen](#get-application-id) sectie vereisten. |
    | Toepassingssleutel | Gebruik de sleutel van de toepassing van de Blockchain client app-registratie verzameld in de [toevoegen Graph API-sleutel voor toepassing](#add-graph-api-key-to-application) sectie vereisten. |


8.  Klik op **OK** voor het voltooien van de configuratiesectie van Azure AD-Parameters.

9.  Voltooi de **grootte en de prestaties** instellingen.

    ![Instellingen voor netwerk- en prestaties](media/blockchain-workbench-deploy/blockchain-workbench-settings-network.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Aantal blockchain knooppunten | Het aantal Ethereum PoA validator knooppunten te worden geïmplementeerd in uw netwerk kiezen. |
    | Prestaties van de opslag | Kies de gewenste VM-opslagprestaties voor uw netwerk blockchain. |
    | Grootte virtuele machine | Kies de gewenste VM-grootte voor het netwerk blockchain. |

10. Klik op **OK** naar de sectie network grootte en de prestaties te voltooien.

11. Voltooi de **Azure Monitor** instellingen.

    ![Azure Monitor](media/blockchain-workbench-deploy/blockchain-workbench-settings-oms.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Bewaking | Kies of u wilt dat Azure Monitor moet worden gebruikt voor het bewaken van uw netwerk blockchain |
    | Verbinding maken met bestaande OMS-exemplaar | Kies of u voor het gebruik van een bestaande Operations Management Suite-exemplaar of een nieuwe maken |
    | Locatie van OMS-werkruimte | Kies een regio voor de OMS-werkruimte. Dit moet overeenkomen met de regio voor de locatie Blockchain Workbench |

12. Klik op **OK** voor het voltooien van de Monitor van de Azure-sectie.

13. Bekijk het overzicht om te controleren of dat de parameters juist zijn.

    ![Samenvatting](media/blockchain-workbench-deploy/blockchain-workbench-summary.png)

14. Selecteer **maken** akkoord met de voorwaarden en implementeren van uw Azure Blockchain Workbench.

De implementatie kan tot 90 minuten duren. U kunt de Azure-portal om de voortgang te gebruiken. Selecteer in de zojuist gemaakte resourcegroep **implementaties > overzicht** voor de status van de geïmplementeerde artefacten.

## <a name="blockchain-workbench-web-url"></a>Blockchain Workbench Web-URL

Zodra de implementatie van de Blockchain Workbench is voltooid, bevat een nieuwe resourcegroep uw resources Blockchain Workbench. Blockchain Workbench services toegankelijk zijn via een URL. De volgende stappen laten zien hoe de URL van de geïmplementeerde framework ophalen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het navigatiedeelvenster links **resourcegroepen**
3. U hebt opgegeven bij het implementeren van Blockchain Workbench naam van de resourcegroep kiezen.
4. Klik op de **TYPE** kolomkop om de lijst alfabetisch op type te sorteren.
5. Er zijn twee resources met type **App Service**. Selecteer de bron van het type **App Service** *zonder* de '-api ' achtervoegsel.

    ![Lijst van App service](media/blockchain-workbench-deploy/resource-group-list.png)

6.  In de App Service **Essentials** sectie, Kopieer de **URL** -waarde voor de web-URL voor uw geïmplementeerde Blockchain Workbench.

    ![App service essentials](media/blockchain-workbench-deploy/app-service.png)

## <a name="configuring-the-reply-url"></a>Configuratie van de antwoord-URL

Zodra de Blockchain Azure Workbench is geïmplementeerd, wordt de volgende stap is om ervoor te zorgen dat de clienttoepassing Azure Active Directory (Azure AD) is geregistreerd op de juiste **antwoord-URL** web-URL van de geïmplementeerde Blockchain Workbench.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Controleer of dat u zich in de tenant waar u de Azure AD-clienttoepassing geregistreerd.
3. Selecteer in het navigatiedeelvenster links de **Azure Active Directory** service. Selecteer **App-registraties**.
4. Selecteer de Azure AD-clienttoepassing die u hebt geregistreerd in de sectie vereisten.
5. Selecteer **instellingen > URL's beantwoorden**.
6. Geef de belangrijkste web-URL van de Azure Blockchain Workbench-implementatie die u hebt opgehaald in de **ophalen van de Web-URL van Azure Blockchain Workbench** sectie. De antwoord-URL wordt voorafgegaan door `https://`.  Bijvoorbeeld: `https://myblockchain2-7v75.azurewebsites.net`

    ![Antwoord-URL's](media/blockchain-workbench-deploy/configure-reply-url.png)

7. Selecteer **opslaan** bijwerken van de clientregistratie van de.

## <a name="next-steps"></a>Volgende stappen

Nu dat u Azure Blockchain Workbench is geïmplementeerd hebt, wordt de volgende stap is het [beheren van gebruikers in Azure Blockchain Workbench](blockchain-workbench-manage-users.md).