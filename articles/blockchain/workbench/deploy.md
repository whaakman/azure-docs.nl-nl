---
title: Azure Blockchain Workbench implementeren
description: Azure Blockchain Workbench implementeren
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 1/8/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: 2c5e3664a9ef26319eb8a51d03ddef6a5392ffa9
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54102989"
---
# <a name="deploy-azure-blockchain-workbench"></a>Azure Blockchain Workbench implementeren

Azure Blockchain Workbench wordt geïmplementeerd met behulp van een oplossingssjabloon in de Azure Marketplace. De sjabloon vereenvoudigt de implementatie van onderdelen die nodig zijn voor het maken van blockchain-toepassingen. Eenmaal geïmplementeerd, biedt Blockchain Workbench toegang tot de client-apps om gebruikers en blockchain-toepassingen te maken en te beheren.

Zie voor meer informatie over de onderdelen van Blockchain Workbench [Azure Blockchain Workbench architectuur](architecture.md).

## <a name="prepare-for-deployment"></a>Implementatie voorbereiden

Met Blockchain Workbench kunt u een blockchain grootboek, samen met een set van relevante Azure-services voor het bouwen van een blockchain-gebaseerde toepassing, implementeren. De implementatie van Blockchain Workbench resulteert in de volgende Azure-services die binnen een resourcegroep in uw Azure-abonnement worden ingericht.

* 1 Event Grid-Topic
* 1-Service Bus Namespace
* 1 Application Insights
* 1 SQL-Database (Standard S0)
* 2 App Services (standaard)
* 2 Azure Key Vaults
* 2 Azure Storage-accounts (Standard LRS)
* 2 virtuele-machine schaalsets (voor validatie en worker-knooppunten)
* 2 virtuele netwerken (met inbegrip van de load balancer, netwerkbeveiligingsgroep en openbaar IP-adres voor elk virtueel netwerk)
* Optioneel: Azure Monitor

Hieronder volgt een voorbeeld van een implementatie gemaakt in **myblockchain** resourcegroep.

![Voorbeeld van een implementatie](media/deploy/example-deployment.png)

De kosten van Blockchain Workbench is een optelling van de kosten van de onderliggende Azure-services. Informatie over de prijzen voor Azure-services kunnen worden berekend met behulp van de [prijscalculator](https://azure.microsoft.com/pricing/calculator/).

> [!IMPORTANT]
> Als u van een abonnement met lage Servicelimieten, zoals een gratis laag van Azure-abonnement gebruikmaakt, wordt de implementatie mislukken vanwege onvoldoende quotum van VM-kernen. Voorafgaand aan de implementatie, Controleer uw quotum aanvragen met behulp van de richtlijnen van de [virtuele machine vCPU-quota](../../virtual-machines/windows/quotas.md) artikel. De standaardselectie voor de virtuele machine moet 6 VM-kernen. Wijzigen in een kleinere virtuele machine zoals *Standard DS1 versie 2* vermindert het aantal kernen tot en met 4.

## <a name="prerequisites"></a>Vereisten

Azure Blockchain Workbench vereist Azure AD-configuratie en toepassing registraties. U kunt de Azure AD [configuraties handmatig](#azure-ad-configuration) vóór implementatie of een script uitvoeren na de implementatie. Als u opnieuw wilt Blockchain Workbench implementeren, Zie [Azure AD-configuratie](#azure-ad-configuration) om te controleren of de configuratie van uw Azure AD.

> [!IMPORTANT]
> Workbench hoeft niet te worden geïmplementeerd in dezelfde tenant als het account dat u gebruikt voor het registreren van een Azure AD-toepassing. Workbench moet worden geïmplementeerd in een tenant wanneer u hebt onvoldoende machtigingen om resources te implementeren. Zie voor meer informatie over Azure AD-tenants, [over het verkrijgen van een Active Directory-tenant](../../active-directory/develop/quickstart-create-new-tenant.md) en [toepassingen integreren met Azure Active Directory](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md).



## <a name="deploy-blockchain-workbench"></a>Blockchain Workbench implementeren

Als de vereiste stappen zijn voltooid, bent u klaar om de Blockchain Workbench te implementeren. De volgende secties beschrijven hoe het framework moet worden geïmplementeerd.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer uw account in de rechterbovenhoek en schakel over naar de gewenste Azure AD-tenant waar u wilt implementeren, Azure Blockchain Workbench.
3. Selecteer **Een resource maken** in het linkerdeelvenster. Zoek naar `Azure Blockchain Workbench` in de **Marketplace doorzoeken** zoekbalk. 

    ![Zoekbalk Marketplace](media/deploy/marketplace-search-bar.png)

4. Selecteer **Azure Blockchain Workbench**.

    ![De resultaten voor zoeken op Marketplace](media/deploy/marketplace-search-results.png)

5. Selecteer **Maken**.
6. Vul de algemene instellingen in.

    ![Azure Blockchain Workbench maken](media/deploy/blockchain-workbench-settings-basic.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Voorvoegsel van de resource | Korte unieke id voor uw implementatie. Deze waarde wordt gebruikt als basis voor de naamgeving van resources. |
    | VM-gebruikersnaam | Naam van de gebruiker wordt gebruikt als beheerder voor alle virtuele machines (VM). |
    | Verificatietype | Selecteer of u een wachtwoord wilt gebruiken of een sleutel voor de verbinding met virtuele machines. |
    | Wachtwoord | Het wachtwoord wordt gebruikt om verbinding te maken met VM's. |
    | SSH | Gebruik een openbare RSA-sleutel in de indeling met één regel die begint met **ssh-rsa** of gebruik de PEM-indeling van meerdere regels. U kunt SSH-sleutels genereren met behulp van `ssh-keygen` op Linux en OS X of met PuTTYGen op Windows. Zie voor meer informatie over SSH-sleutels, [over het gebruik van SSH-sleutels met Windows op Azure](../../virtual-machines/linux/ssh-from-windows.md). |
    | Wachtwoord van de database / wachtwoord bevestigen | Geef het wachtwoord voor toegang tot de database die wordt gemaakt als onderdeel van de implementatie. |
    | Implementatieregio | Geef op waar de Blockchain Workbench-resources worden geïmplementeerd. Voor beschikbaarheid wordt  aanbevolen om dit overeen te laten komen met de **locatie** instelling. |
    | Abonnement | Geef het Azure-abonnement dat u wilt gebruiken voor uw implementatie. |
    | Resourcegroepen | Maak een nieuwe resourcegroep door **nieuw** te selecteren en geef de naam op van een unieke resource-groep. |
    | Locatie | Geef de regio op waaar u het framework wilt implementeren. |

7. Selecteer **OK** om de configuratiesectie met basisinstellingen te voltooien.

8. In **geavanceerde instellingen**, kiezen als u wilt een nieuw blockchain-netwerk maken of een bestaand proof-of-authority blockchain-netwerk gebruiken.

    Voor **nieuw**:

    De *Maak een nieuwe* optie maakt u een set knooppunten binnen één enkel lid abonnement Ethereum Proof-of-Authority (PoA). 

    ![Geavanceerde instellingen voor nieuwe blockchain-netwerk](media/deploy/advanced-blockchain-settings-new.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Bewaking | Kies of u Azure Monitor wilt inschakelen om uw blockchain-netwerk te controleren |
    | Azure Active Directory-instellingen | Kies **Later toevoegen**.</br>Opmerking: Als u ervoor kiest [vooraf configureren van Azure AD](#azure-ad-configuration) of opnieuw wilt implementeren, wilt *nu toevoegen*. |
    | Selectie van de virtuele machine | Kies de gewenste VM-grootte voor uw blockchain-netwerk. Kies een kleinere virtuele machine zoals *Standard DS1 versie 2* als u van een abonnement met lage Servicelimieten, zoals de gratis laag van Azure gebruikmaakt. |

    Voor **gebruik bestaande**:

    Met de *gebruik bestaande* optie kunt u een netwerk van de blockchain Ethereum Proof-of-Authority (PoA) opgeven. Eindpunten hebben de volgende vereisten.

    * Het eindpunt moet een Ethereum Proof-of-Authority (PoA) blockchain-netwerk zijn.
    * Het eindpunt moet openbaar toegankelijk zijn via het netwerk.
    * Het PoA blockchain-netwerk moet worden geconfigureerd met gas prijs is ingesteld op nul.

    > [!NOTE]
    > Blockchain Workbench-accounts worden niet gefinancierd. Als er fondsen zijn vereist, mislukt de transacties.

    ![Geavanceerde instellingen voor bestaande blockchain-netwerk](media/deploy/advanced-blockchain-settings-existing.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Ethereum RPC-eindpunt | Geef het RPC-eindpunt van een bestaand PoA blockchain-netwerk. Het eindpunt begint met https:// of http:// en eindigt met een poortnummer op. Bijvoorbeeld: `http<s>://<network-url>:<port>` |
    | Azure Active Directory-instellingen | Kies **Later toevoegen**.</br>Opmerking: Als u ervoor kiest [vooraf configureren van Azure AD](#azure-ad-configuration) of opnieuw wilt implementeren, wilt *nu toevoegen*. |
    | Selectie van de virtuele machine | Kies de gewenste VM-grootte voor uw blockchain-netwerk. |

9. Selecteer **OK** geavanceerde instellingen te voltooien.

10. Bekijk de samenvatting om te controleren of uw parameters correct zijn.

    ![Samenvatting](media/deploy/blockchain-workbench-summary.png)

11. Selecteer **maken** om akkoord te gaan met de voorwaarden en uw Azure Blockchain Workbench te implementeren.

De implementatie kan tot 90 minuten duren. U kunt de Azure-portal gebruiken om de voortgang te bekijken. Selecteer in de zojuist gemaakte resourcegroep **implementaties > overzicht** om de status van de geïmplementeerde artefacten te zien.

> [!IMPORTANT]
> Post-implementatie, moet u Active Directory-instellingen hebt voltooid. Als u ervoor hebt gekozen **Later toevoegen**, moet u uitvoeren de [Azure AD-configuratiescript](#azure-ad-configuration-script).  Als u ervoor hebt gekozen **nu toevoegen**, moet u [configureren van de antwoord-URL](#configuring-the-reply-url).

## <a name="blockchain-workbench-web-url"></a>Blockchain Workbench Web-URL

Als de implementatie van de Blockchain Workbench is voltooid, bevat een nieuwe resourcegroep uw Blockchain Workbench-resources. Blockchain Workbench services zijn toegankelijk via een web-URL. De volgende stappen laten zien hoe u de web-URL van het geïmplementeerde framework kunt ophalen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het navigatiedeelvenster links **resourcegroepen**
3. Kies de Resourcegroepnaam die u hebt opgegeven bij het implementeren van Blockchain Workbench.
4. Selecteer de **TYPE** kolomkop om de lijst alfabetisch gesorteerd op type te sorteren.
5. Er zijn twee resources met type **App Service**. Selecteer de resource van het type **App Service** *zonder* de '-api ' achtervoegsel.

    ![Lijst met App service](media/deploy/resource-group-list.png)

6. In de App Service **Essentials** sectie, kopieer de **URL** -waarde voor de web-URL naar uw geïmplementeerde Blockchain Workbench.

    ![App service essentials](media/deploy/app-service.png)

Als u een aangepaste domeinnaam met Blockchain Workbench wilt koppelen, zie [configureren van een aangepaste domeinnaam voor een web-app in Azure App Service met Traffic Manager](../../app-service/web-sites-traffic-manager-custom-domain-name.md).

## <a name="azure-ad-configuration-script"></a>Azure AD-configuratiescript

Azure AD moet worden geconfigureerd om uw Blockchain Workbench-implementatie te voltooien. Een PowerShell-script gebruikt u de configuratie.

1. Navigeer in een browser naar de [Blockchain Workbench Web-URL](#blockchain-workbench-web-url).
2. Hier ziet u instructies voor het instellen van Azure AD met behulp van Cloud Shell. Kopieer de opdracht en open Cloud Shell.

    ![AAD-script starten](media/deploy/launch-aad-script.png)

3. Kies de Azure AD-tenant waar u Blockchain Workbench hebt geïmplementeerd.
4. Plak in Cloud Shell en voer de opdracht uit.
5. Wanneer u hierom wordt gevraagd, voert u de Azure AD-tenant die u wilt gebruiken voor Blockchain Workbench. Dit is de tenant met de gebruikers voor Blockchain Workbench.

    > [!IMPORTANT]
    > De geverifieerde gebruiker vereist machtigingen voor Azure AD-toepassingsregistraties maken, en verleen gedelegeerde machtigingen in de tenant. Mogelijk moet u de vraag een beheerder van de tenant de Azure AD-configuratiescript uitvoeren of een nieuwe tenant maken.

    ![Azure AD-tenant invoeren](media/deploy/choose-tenant.png)

6. U wordt gevraagd om te verifiëren bij Azure AD-tenant met behulp van een browser. De web-URL in een browser geopend, voer de code en verifiëren.

    ![Verifiëren met code](media/deploy/authenticate.png)

7. Het script voert de verschillende statusberichten. U krijgt een **SUCCES** statusbericht als de tenant is ingericht.
8. Navigeer naar de URL Blockchain Workbench. U wordt gevraagd om u te toestemming geven voor het verlenen van machtigingen voor lezen naar de map. Hierdoor kunnen de Blockchain Workbench web-apptoegang tot de gebruikers in de tenant. Als u de tenantbeheerder bent, kunt u toestemming geven voor de hele organisatie. Deze optie accepteert toestemming voor alle gebruikers in de tenant. Anders wordt wordt elke gebruiker gevraagd om toestemming bij het eerste gebruik van de webtoepassing Blockchain Workbench.
9. Selecteer **accepteren** om in te stemmen.

     ![Toestemming voor het profiel van gebruikers lezen](media/deploy/graph-permission-consent.png)

10. Na de toestemming te geven, kan de Blockchain Workbench web-app worden gebruikt.

## <a name="azure-ad-configuration"></a>Azure AD-configuratie

Als u ervoor kiest handmatig configureren of Azure AD-instellingen voorafgaand aan de implementatie controleren, moet u alle stappen in deze sectie uitvoeren. Als u liever de Azure AD-instellingen automatisch configureren, gebruikt u [Azure AD-configuratiescript](#azure-ad-configuration-script) nadat u Blockchain Workbench hebt geïmplementeerd.

### <a name="blockchain-workbench-api-app-registration"></a>Registratie van API-app voor Blockchain Workbench

Blockchain Workbench-implementatie vereist de registratie van een Azure AD-toepassing. U hebt een Azure Active Director-tenant  (Azure AD) nodig om de app te registreren. U kunt een bestaande tenant gebruiken of een nieuwe tenant maken. Als u van een bestaande Azure AD-tenant gebruikmaakt, moet u voldoende machtigingen voor het registreren van toepassingen, Graph API-machtigingen verlenen en toestaan van toegang voor gasten binnen een Azure AD-tenant. Als u niet gemachtigd bent in een bestaande Azure AD-tenant maakt u een nieuwe tenant.


1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer uw account in de rechterbovenhoek en schakel over naar de gewenste Azure AD-tenant. De tenant moet de tenant zijn van de abonnementsbeheerder van het abonnement waar Workbench is geïmplementeerd en waar u voldoende machtigingen hebt om toepassingen te registreren.
3. Selecteer in het navigatiedeelvenster aan de linkerkant de **Azure Active Directory**-service. Selecteer **App-registraties** > **nieuwe toepassing registreren**.

    ![App-registratie](media/deploy/app-registration.png)

4. Geef een **naam** en **aanmeldings-URL** voor de toepassing. U kunt de tijdelijke aanduiding voor waarden gebruiken omdat de waarden worden gewijzigd tijdens de implementatie. 

    ![Maken van app-registratie](media/deploy/app-registration-create.png)

    |Instelling  | Waarde  |
    |---------|---------|
    |Naam | `Blockchain API` |
    |Toepassingstype |Web-app/-API|
    |Aanmeldings-URL | `https://blockchainapi` |

5. Selecteer **maken** om de Azure AD-toepassing te registreren.

### <a name="modify-manifest"></a>Wijzigen van het manifest

Vervolgens moet u het manifest voor het gebruik van toepassingsrollen in Azure AD om op te geven van Blockchain Workbench beheerders wijzigen.  Zie voor meer informatie over de toepassingsmanifesten [Azure Active Directory-toepassingsmanifest](../../active-directory/develop/reference-app-manifest.md).

1. Selecteer voor de toepassing die u hebt geregistreerd, **Manifest** in het detailvenster van de geregistreerde toepassing.
2. Genereer een GUID. U kunt een GUID met behulp van de PowerShell-opdracht [guid] genereren: NewGuid () of de cmdlet New-GUID. Een andere optie is het gebruik van een GUID-generator-website.
3. U gaat het **appRoles** gedeelte van het manifest bijwerken. Selecteer in het manifest bewerkingsvenster **bewerken** en vervang `"appRoles": []` door de opgegeven JSON. Vervang de waarde voor de **id** veld door de GUID die u hebt gegenereerd. 

    ![Manifest bewerken](media/deploy/edit-manifest.png)

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

    > [!IMPORTANT]
    > De waarde **beheerder** is nodig voor het identificeren van Blockchain Workbench-beheerders.

4. In het manifest, wijzigt u ook de **Oauth2AllowImplictFlow** waarde die moet worden **waar**.

    ``` json
    "oauth2AllowImplicitFlow": true,
    ```

5. Selecteer **opslaan** het manifest wijzigingen op te slaan.

### <a name="add-graph-api-required-permissions"></a>Graph API vereist machtigingen toevoegen

De API-toepassing moet de gebruiker om toestemming voor toegang tot de map vragen. Stel de volgende vereiste machtiging in voor de API-App:

1. Selecteer in de Blockchain-API-app-registratie, **instellingen > vereiste machtigingen > Selecteer een API > Microsoft Graph**.

    ![Een API selecteren](media/deploy/client-app-select-api.png)

    Klik op **Selecteren**.

2. In **toegang inschakelen** onder **overgedragen machtigingen**, kiest u **basisprofielen van alle gebruikers lezen**.

    ![Toegang inschakelen](media/deploy/client-app-read-perms.png)

    Selecteer **opslaan** Selecteer **gedaan**.

3. In **vereiste machtigingen**, selecteer **machtigingen verlenen** Selecteer **Ja** verificatie wordt gevraagd.

   ![Machtigingen verlenen](media/deploy/client-app-grant-permissions.png)

   Door deze machtiging te verlenen krijgt Blockchain Workbench toegang tot gebruikers in de directory. De leesmachtiging is vereist voor het zoeken en toevoegen van leden in Blockchain Workbench.

### <a name="get-application-id"></a>Toepassings-ID ophalen

De toepassing-ID en tenant-gegevens zijn vereist voor de implementatie. Verzamel en bewaar de gegevens voor gebruik tijdens de implementatie.

1. Selecteer **instellingen** > **eigenschappen** voor de toepassing die u hebt geregistreerd.
2. In het **eigenschappen** deelvenster, kopieer en bewaar de volgende waarden voor later gebruik tijdens de implementatie.

    ![Eigenschappen van de API-app](media/deploy/app-properties.png)

    | Instelling voor het opslaan  | Gebruik in de implementatie |
    |------------------|-------------------|
    | Toepassings-id | Installatie van de Azure Active Directory > toepassings-ID |

### <a name="get-tenant-domain-name"></a>Domeinnaam van tenant ophalen

Verzamel en bewaar de domeinnaam van de Active Directory-tenant waar de toepassingen zijn geregistreerd. 

Selecteer in het navigatiedeelvenster aan de linkerkant de **Azure Active Directory**-service. Selecteer **Namen van aangepaste domeinen**. Kopieer en bewaar de domeinnaam.

![Domeinnaam](media/deploy/domain-name.png)

### <a name="guest-user-settings"></a>Gast-gebruikersinstellingen

Als u gastgebruikers ook kunnen in uw Azure AD-tenant hebt, volgt u de extra stappen om ervoor te zorgen Blockchain Workbench Gebruikerstoewijzing en beheer goed werkt.

1. Schakelt u over uw Azure AD-tenant en selecteer **Azure Active Directory > Instellingen > instellingen voor externe samenwerking beheren**.
2. Stel **gebruikersmachtigingen Gast zijn beperkt** te **Nee**.
    ![Instellingen voor externe samenwerking](media/deploy/user-collaboration-settings.png)

## <a name="configuring-the-reply-url"></a>De antwoord-URL configureren

Nadat de Azure Blockchain Workbench is geïmplementeerd, hebt u de Azure Active Directory (Azure AD)-clienttoepassing configureren **antwoord-URL** web-URL van de geïmplementeerde Blockchain Workbench.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Controleer of u in de tenant bent waar u de Azure AD-clienttoepassing hebt geregistreerd.
3. Selecteer in het navigatiedeelvenster aan de linkerkant de **Azure Active Directory**-service. Selecteer **App-registraties**.
4. Selecteer de Azure AD-clienttoepassing die u in de sectie vereisten hebt geregistreerd.
5. Selecteer **instellingen > antwoord-URL's**.
6. Geef de belangrijkste web-URL van de Azure Blockchain Workbench-implementatie die u hebt opgehaald in de **ophalen van de Web-URL van Azure Blockchain Workbench** sectie. De antwoord-URL wordt voorafgegaan door `https://`. Bijvoorbeeld: `https://myblockchain2-7v75.azurewebsites.net`

    ![Antwoord-URL's](media/deploy/configure-reply-url.png)

7. Selecteer **opslaan** om de clientregistratie bij te werken.

## <a name="remove-a-deployment"></a>Een implementatie verwijderen

Wanneer u een implementatie niet meer nodig hebt, kunt u een implementatie verwijderen door de Blockchain Workbench-resourcegroep te verwijderen.

1. In de Azure-portal, gaat u naar **resourcegroep** in het navigatiedeelvenster links en selecteert u de resourcegroep die u wilt verwijderen. 
2. Selecteer **Resourcegroep verwijderen**. Bevestig de verwijdering door de naam van de resourcegroep in te voeren en selecteer **verwijderen**.

    ![Resourcegroep verwijderen](media/deploy/delete-resource-group.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel met instructies hebt u Azure Blockchain Workbench geïmplementeerd. Ga door naar het volgende artikel voor meer informatie over het maken van een blockchain-toepassing.

> [!div class="nextstepaction"]
> [Een blockchain-toepassing maken in Azure Blockchain Workbench](create-app.md)
