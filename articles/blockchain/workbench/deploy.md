---
title: Azure Blockchain Workbench implementeren
description: Azure Blockchain Workbench implementeren
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 3ee169e4139f5fc9cd4208c53c4997d50521fed7
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242663"
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

Azure Blockchain Workbench heeft verschillende vereisten voor implementatie. Tot deze vereisten behoren ook Azure AD-configuratie en toepassing registraties.

### <a name="blockchain-workbench-api-app-registration"></a>Registratie van API-app voor Blockchain Workbench

Blockchain Workbench-implementatie vereist de registratie van een Azure AD-toepassing. U hebt een Azure Active Director-tenant  (Azure AD) nodig om de app te registreren. U kunt een bestaande tenant gebruiken of een nieuwe tenant maken. Als u van een bestaande Azure AD-tenant gebruikmaakt, moet u voldoende machtigingen hebben voor het registreren van toepassingen en het verlenen van Graph API machtigingen binnen deze Azure AD-tenant. Als u niet gemachtigd bent in een bestaande Azure AD-tenant maakt u een nieuwe tenant. 

> [!IMPORTANT]
> Workbench hoeft niet te worden geïmplementeerd in dezelfde tenant als het account dat u gebruikt voor het registreren van een Azure AD-toepassing. Workbench moet worden geïmplementeerd in een tenant wanneer u hebt onvoldoende machtigingen om resources te implementeren. Zie voor meer informatie over Azure AD-tenants, [over het verkrijgen van een Active Directory-tenant](../../active-directory/develop/quickstart-create-new-tenant.md) en [toepassingen integreren met Azure Active Directory](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md).

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

### <a name="modify-application-manifest"></a>Manifest van de toepassing wijzigen

Vervolgens moet u het toepassingsmanifest voor het gebruik van toepassingsrollen in Azure AD wijzigen om Blockchain Workbench beheerders op te geven.  Zie voor meer informatie over de toepassingsmanifesten [Azure Active Directory-toepassingsmanifest](../../active-directory/develop/reference-app-manifest.md).

1. Selecteer voor de toepassing die u hebt geregistreerd, **Manifest** in het detailvenster van de geregistreerde toepassing.
2. Genereer een GUID. U kunt een GUID genereren met behulp van de PowerShell-opdracht [guid] :: NewGuid () of de cmdlet New-GUID. Een andere optie is het gebruik van een GUID-generator-website.
3. U gaat het **appRoles** gedeelte van het manifest bijwerken. Selecteer in het manifest bewerkingsvenster **bewerken** en vervang `"appRoles": []` door de opgegeven JSON. Vervang de waarde voor de **id** veld door de GUID die u hebt gegenereerd. 

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

    ![Manifest bewerken](media/deploy/edit-manifest.png)

    > [!IMPORTANT]
    > De waarde **beheerder** is nodig voor het identificeren van Blockchain Workbench-beheerders.

4.  Klik op **opslaan** om de wijzigingen in het toepassingsmanifest op te slaan.

### <a name="add-graph-api-required-permissions"></a>Graph API vereist machtigingen toevoegen

De API-toepassing moet de gebruiker om toestemming voor toegang tot de map vragen. Stel de volgende vereiste machtiging in voor de API-App:

1. Selecteer in de Blockchain-API-app-registratie, **instellingen > vereiste machtigingen > Selecteer een API > Microsoft Graph**.

    ![Een API selecteren](media/deploy/client-app-select-api.png)

    Klik op **Selecteren**.

2. In **toegang inschakelen** onder **Toepassingsmachtigingen**, kiest u **het volledige profiel van alle gebruikers lezen**.

    ![Toegang inschakelen](media/deploy/client-app-read-perms.png)

    Klik op **Selecteer** klikt u vervolgens op **gedaan**.

3. In **vereiste machtigingen**, selecteer **machtigingen verlenen** Selecteer **Ja** verificatie wordt gevraagd.

   ![Machtigingen verlenen](media/deploy/client-app-grant-permissions.png)

   Door deze machtiging te verlenen krijgt Blockchain Workbench toegang tot gebruikers in de directory. De leesmachtiging is vereist voor het zoeken en toevoegen van leden in Blockchain Workbench.

### <a name="add-graph-api-key-to-application"></a>Graph API-sleutel toevoegen aan toepassing

Blockchain Workbench maakt gebruik van Azure AD als het belangrijkste identiteitsbeheersysteem voor gebruikers die werken met blockchain-toepassingen. Opdat Blockchain Workbench toegang krijgt tot Azure AD voor het ophalen van gebruikersgegevens, zoals namen en e-mailberichten, moet u een toegangssleutel toevoegen. Blockchain Workbench maakt gebruik van de sleutel voor verificatie met Azure AD.

1. Selecteer voor de toepassing die u hebt geregistreerd, **instellingen** in het detailvenster van de geregistreerde toepassing.
2. Selecteer **Sleutels**.
3. Voeg een nieuwe sleutel toe door een sleutel **beschrijving** en een waarde voor **verloopt** op te geven. 

    ![Sleutel maken](media/deploy/app-key-create.png)

    |Instelling  | Waarde  |
    |---------|---------|
    | Beschrijving | `Service` |
    | Verloopt op | Kies een vervaltijd |

4. Selecteer **Opslaan**. 
5. Kopieer de waarde van de sleutel en bewaar deze voor later. U hebt deze nodig voor de implementatie.

    > [!IMPORTANT]
    >  Als u de sleutel voor de implementatie niet opslaat, moet u een nieuwe sleutel te genereren. U kunt waarde van de sleutel niet later ophalen vanuit de portal.

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

## <a name="deploy-blockchain-workbench"></a>Blockchain Workbench implementeren

Als de vereiste stappen zijn voltooid, bent u klaar om de Blockchain Workbench te implementeren. De volgende secties beschrijven hoe het framework moet worden geïmplementeerd.

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Selecteer uw account in de rechterbovenhoek hoek en schakel over naar de gewenste Azure AD-tenant waar u Azure Blockchain Workbench wilt implementeren.
3.  Selecteer in het linkerdeelvenster **een resource maken**. Zoek naar `Azure Blockchain Workbench` in de **Marketplace doorzoeken** zoekbalk. 

    ![Zoekbalk Marketplace](media/deploy/marketplace-search-bar.png)

4.  Selecteer **Azure Blockchain Workbench**.

    ![De resultaten voor zoeken op Marketplace](media/deploy/marketplace-search-results.png)

4.  Selecteer **Maken**.
5.  Vul de algemene instellingen in.

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

6.  Selecteer **OK** om de configuratiesectie met basisinstellingen te voltooien.

7.  Voltooi de **Azure Active Directory-installatie**.

    ![Installatie van de Azure AD](media/deploy/blockchain-workbench-settings-aad.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Domeinnaam | Gebruik de Azure AD-tenant die is verzameld in de [Get tenant domeinnaam](#get-tenant-domain-name) sectie vereisten. |
    | Toepassings-id | Gebruik de toepassings-ID van de app-registratie van de Blockchain-client die is verzameld in de [toepassings-ID ophalen](#get-application-id) sectie vereisten. |
    | Toepassingssleutel | Gebruik de sleutel van de toepassing van de Blockchain-app clientregistratie die is verzameld in de [toevoegen Graph API-sleutel aan toepassing](#add-graph-api-key-to-application) sectie vereisten. |


8.  Klik op **OK** om de sectie van de configuratie van Azure AD-Parameters te voltooien.

9.  In **netwerkinstellingen en de prestaties** kiest u om een nieuw blockchain-netwerk maken of een bestaand proof-of-authority blockchain-netwerk gebruiken.

    Voor **nieuw**:

    Met de *Maak een nieuwe* optie maakt u een set Ethereum Proof-of-Authority (PoA) knooppunten binnen één enkel abonnement. 

    ![Netwerkinstellingen en -prestaties](media/deploy/blockchain-workbench-settings-network-new.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Aantal blockchain-knooppunten | Kies het aantal Ethereum PoA validator knooppunten die worden geïmplementeerd in uw netwerk. |
    | Opslagprestaties | Kies de gewenste VM-opslagprestaties van uw blockchain-netwerk. |
    | Grootte virtuele machine | Kies de gewenste VM-grootte voor uw blockchain-netwerk. |

    Voor **gebruik bestaande**:

    Met de *gebruik bestaande* optie kunt u een netwerk van de blockchain Ethereum Proof-of-Authority (PoA) opgeven. Eindpunten hebben de volgende vereisten.

    * Het eindpunt moet een Ethereum Proof-of-Authority (PoA) blockchain-netwerk zijn.
    * Het eindpunt moet openbaar toegankelijk zijn via het netwerk.
    * Het PoA blockchain-netwerk moet worden geconfigureerd met de gasprijs ingesteld op nul (Opmerking: Blockchain Workbench-accounts worden niet gefinancierd. "Als fondsen vereist zijn, mislukken de transacties).

    ![Netwerkinstellingen en -prestaties](media/deploy/blockchain-workbench-settings-network-existing.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Ethereum RPC-eindpunt | Geef het RPC-eindpunt van een bestaand PoA blockchain-netwerk. Het eindpunt begint met http:// en eindigt met een poortnummer. Bijvoorbeeld: `http://contoso-chain.onmicrosoft.com:8545` |
    | Opslagprestaties | Kies de gewenste VM-opslagprestaties van uw blockchain-netwerk. |
    | Grootte virtuele machine | Kies de gewenste VM-grootte voor uw blockchain-netwerk. |

10. Selecteer **OK** om netwerkinstellingen en prestaties te voltooien.

11. Voltooi de **Azure Monitor** instellingen.

    ![Azure Monitor](media/deploy/blockchain-workbench-settings-oms.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Bewaking | Kies of u Azure Monitor wilt inschakelen om uw blockchain-netwerk te controleren |
    | Maak verbinding met bestaande Log Analytics-exemplaar | Kies of u een bestaande Log Analytics-exemplaar wilt gebruiken of een nieuwe maken. Als u een bestaand exemplaar gebruikt, voert u uw werkruimte-ID en primaire sleutel in. |

12. Klik op **OK** om de Azure Monitor-sectie te voltooien.

13. Bekijk de samenvatting om te controleren of uw parameters correct zijn.

    ![Samenvatting](media/deploy/blockchain-workbench-summary.png)

14. Selecteer **maken** om akkoord te gaan met de voorwaarden en uw Azure Blockchain Workbench te implementeren.

De implementatie kan tot 90 minuten duren. U kunt de Azure-portal gebruiken om de voortgang te bekijken. Selecteer in de zojuist gemaakte resourcegroep **implementaties > overzicht** om de status van de geïmplementeerde artefacten te zien.

## <a name="blockchain-workbench-web-url"></a>Blockchain Workbench Web-URL

Als de implementatie van de Blockchain Workbench is voltooid, bevat een nieuwe resourcegroep uw Blockchain Workbench-resources. Blockchain Workbench services zijn toegankelijk via een web-URL. De volgende stappen laten zien hoe u de web-URL van het geïmplementeerde framework kunt ophalen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het navigatiedeelvenster links **resourcegroepen**
3. Kies de Resourcegroepnaam die u hebt opgegeven bij het implementeren van Blockchain Workbench.
4. Klik op de **TYPE** kolomkop om de lijst alfabetisch gesorteerd op type te sorteren.
5. Er zijn twee resources met type **App Service**. Selecteer de resource van het type **App Service** *zonder* de '-api ' achtervoegsel.

    ![Lijst met App service](media/deploy/resource-group-list.png)

6.  In de App Service **Essentials** sectie, kopieer de **URL** -waarde voor de web-URL naar uw geïmplementeerde Blockchain Workbench.

    ![App service essentials](media/deploy/app-service.png)

Als u een aangepaste domeinnaam met Blockchain Workbench wilt koppelen, zie [configureren van een aangepaste domeinnaam voor een web-app in Azure App Service met Traffic Manager](../../app-service/web-sites-traffic-manager-custom-domain-name.md).

## <a name="configuring-the-reply-url"></a>De antwoord-URL configureren

Als de Azure Blockchain Workbench eenmaal is geïmplementeerd, zorgt de volgende stap ervoor dat de clienttoepassing voor Azure Active Directory (Azure AD) is geregistreerd bij de juiste **antwoord-URL** web-URL van de geïmplementeerde Blockchain Workbench.

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
