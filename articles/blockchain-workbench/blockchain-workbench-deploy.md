---
title: Azure Blockchain Workbench implementeren
description: Azure Blockchain Workbench implementeren
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 7/13/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 57b610b40edff56207617e212d0eb6e591ad50d4
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224293"
---
# <a name="deploy-azure-blockchain-workbench"></a>Azure Blockchain Workbench implementeren

Azure Blockchain Workbench wordt geïmplementeerd met behulp van een oplossingssjabloon in de Azure Marketplace. De sjabloon vereenvoudigt de implementatie van onderdelen die nodig zijn om te maken van blockchain-toepassingen. Zodra geïmplementeerd, biedt Blockchain Workbench toegang tot de client-apps maken en beheren van gebruikers en blockchain-toepassingen.

Zie voor meer informatie over de onderdelen van Blockchain Workbench [Azure Blockchain Workbench architectuur](blockchain-workbench-architecture.md).

## <a name="prepare-for-deployment"></a>Implementatie voorbereiden

Blockchain Workbench kunt u een grootboek blockchain, samen met een set van relevante Azure-services meestal gebruikt voor het bouwen van een blockchain-gebaseerde toepassing implementeren. Blockchain Workbench implementeren resulteert in de volgende Azure-services binnen een resourcegroep in uw Azure-abonnement wordt ingericht.

* 1 event Grid-onderwerp
* 1-Service Bus Namespace
* 1 application Insights
* 1 SQL-Database (Standard S0)
* 2 app Services (standaard)
* 2 azure-Sleutelkluizen
* 2 azure Storage-accounts (Standard LRS)
* 2 virtuele-machineschaalsets (voor validatie en worker-knooppunten)
* 2 virtuele netwerken (met inbegrip van de load balancer, netwerkbeveiligingsgroep en openbaar IP-adres voor elk virtueel netwerk)
* Optioneel: Azure Monitor

Hieronder volgt een voorbeeld van een implementatie gemaakt in **myblockchain** resourcegroep.

![Voorbeeld van een implementatie](media/blockchain-workbench-deploy/example-deployment.png)

De kosten van Blockchain Workbench is een statistische functie van de kosten van de onderliggende Azure-services. Informatie over de prijzen voor Azure-services kunnen worden berekend met behulp van de [prijscalculator](https://azure.microsoft.com/pricing/calculator/).

Azure Blockchain Workbench vereist verschillende vereisten voordat u de implementatie. De vereisten behoren ook Azure AD-configuratie en toepassing registraties.

### <a name="blockchain-workbench-api-app-registration"></a>Blockchain Workbench API app-registratie

Blockchain Workbench-implementatie vereist de registratie van een Azure AD-toepassing. U moet een tenant Azure Active Directory (Azure AD) om de app te registreren. U kunt een bestaande tenant gebruiken of een nieuwe tenant maken. Als u van een bestaande Azure AD-tenant gebruikmaakt, moet u voldoende machtigingen voor het registreren van toepassingen en Graph API machtigingen verlenen binnen een Azure AD-tenant. Als u bent niet gemachtigd in een bestaande Azure AD-tenant maakt u een nieuwe tenant. 

> [!IMPORTANT]
> Workbench hoeft niet te worden geïmplementeerd in dezelfde tenant als het account dat u gebruikt voor het registreren van een Azure AD-toepassing. Workbench moet worden geïmplementeerd in een tenant wanneer u hebt onvoldoende machtigingen om resources te implementeren. Zie voor meer informatie over Azure AD-tenants, [over het verkrijgen van een Active Directory-tenant](../active-directory/develop/active-directory-howto-tenant.md) en [toepassingen integreren met Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer uw account in de rechterbovenhoek hoek en schakel over naar de gewenste Azure AD-tenant. De tenant moet de tenant van de abonnementsbeheerder van het abonnement waar Workbench is geïmplementeerd en u hebt onvoldoende machtigingen om toepassingen te registreren.
3. Selecteer in het navigatiedeelvenster links het **Azure Active Directory** service. Selecteer **App-registraties** > **nieuwe toepassing registreren**.

    ![App-registratie](media/blockchain-workbench-deploy/app-registration.png)

4. Geef een **naam** en **aanmeldings-URL** voor de toepassing. U kunt de tijdelijke aanduiding voor waarden gebruiken omdat de waarden worden gewijzigd tijdens de implementatie. 

    ![Maken van app-registratie](media/blockchain-workbench-deploy/app-registration-create.png)

    |Instelling  | Waarde  |
    |---------|---------|
    |Naam | `Blockchain API` |
    |Toepassingstype |Web-app/-API|
    |Aanmeldings-URL | `https://blockchainapi` |

5. Selecteer **maken** om de Azure AD-toepassing te registreren.

### <a name="modify-application-manifest"></a>Manifest van de toepassing wijzigen

Vervolgens moet u het toepassingsmanifest voor het gebruik van toepassingsrollen in Azure AD om op te geven van Blockchain Workbench beheerders wijzigen.  Zie voor meer informatie over de Toepassingsmanifesten [Azure Active Directory-toepassingsmanifest](../active-directory/develop/active-directory-application-manifest.md).

1. Selecteer voor de toepassing die u hebt geregistreerd, **Manifest** in het detailvenster van de geregistreerde toepassing.
2. Een GUID niet genereren. U kunt een GUID met behulp van de PowerShell-opdracht [guid] genereren:: NewGuid () of de cmdlet New-GUID. Een andere optie is het gebruik van een GUID-generator-website.
3. U gaat bijwerken de **appRoles** gedeelte van het manifest. Selecteer in het manifest bewerkingsvenster **bewerken** en vervang `"appRoles": []` met de opgegeven JSON. Vervang de waarde voor de **id** veld met de GUID die u hebt gegenereerd. 

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
    > De waarde **beheerder** nodig is voor het identificeren van Blockchain Workbench-beheerders.

4.  Klik op **opslaan** het manifest wijzigingen van toepassing op te slaan.

### <a name="add-graph-api-required-permissions"></a>Graph API vereist machtigingen toevoegen

De API-toepassing moet om toestemming te vragen van de gebruiker toegang tot de map. Stel de volgende vereiste machtiging voor de API-App:

1. Selecteer in de Blockchain-API-app-registratie, **instellingen > vereiste machtigingen > Selecteer een API > Microsoft Graph**.

    ![Een API selecteren](media/blockchain-workbench-deploy/client-app-select-api.png)

    Klik op **Selecteren**.

2. In **toegang inschakelen** onder **Toepassingsmachtigingen**, kiest u **het volledige profiel van alle gebruikers lezen**.

    ![Toegang inschakelen](media/blockchain-workbench-deploy/client-app-read-perms.png)

    Klik op **Selecteer** klikt u vervolgens op **gedaan**.

3. In **vereiste machtigingen**, selecteer **machtigingen verlenen** Selecteer **Ja** verificatie wordt gevraagd.

   ![Machtigingen verlenen](media/blockchain-workbench-deploy/client-app-grant-permissions.png)

   Machtiging verlenen, kunt Blockchain Workbench voor toegang tot gebruikers in de directory. De leesmachtiging is vereist om te zoeken en leden toevoegen aan Blockchain Workbench.

### <a name="add-graph-api-key-to-application"></a>Graph API-sleutel toevoegen aan toepassing

Blockchain Workbench maakt gebruik van Azure AD als de belangrijkste identiteitsbeheersysteem voor gebruikers die interactie met blockchain-toepassingen. Opdat Blockchain Workbench voor toegang tot Azure AD en ophalen van gebruikersgegevens, zoals namen en e-mailberichten, moet u een toegangssleutel toevoegen. Blockchain Workbench maakt gebruik van de sleutel voor verificatie met Azure AD.

1. Selecteer voor de toepassing die u hebt geregistreerd, **instellingen** in het detailvenster van de geregistreerde toepassing.
2. Selecteer **Sleutels**.
3. Voeg een nieuwe sleutel toe door op te geven van een sleutel **beschrijving** en het kiezen van **verloopt** duurwaarde. 

    ![Sleutel maken](media/blockchain-workbench-deploy/app-key-create.png)

    |Instelling  | Waarde  |
    |---------|---------|
    | Beschrijving | `Service` |
    | Verloopt op | Kies een vervaltijd |

4. Selecteer **Opslaan**. 
5. Kopieer de waarde van de sleutel en bewaar deze voor later. U hebt deze nodig voor de implementatie.

    > [!IMPORTANT]
    >  Als u de sleutel voor de implementatie niet opslaat, moet u om een nieuwe sleutel te genereren. U kunt waarde van de sleutel niet later ophalen vanuit de portal.

### <a name="get-application-id"></a>Toepassings-ID ophalen

De toepassing-ID en tenant-gegevens zijn vereist voor de implementatie. Verzamelen en opslaan van de gegevens voor gebruik tijdens de implementatie.

1. Selecteer voor de toepassing die u hebt geregistreerd, **instellingen** > **eigenschappen**.
2.  In de **eigenschappen** deelvenster, kopiëren en de volgende voor later gebruik tijdens de implementatie waarden-archief.

    ![Eigenschappen van de API-app](media/blockchain-workbench-deploy/app-properties.png)

    | Instelling voor het opslaan  | Gebruik in de implementatie |
    |------------------|-------------------|
    | Toepassings-id | Installatie van de Azure Active Directory > toepassings-ID |

### <a name="get-tenant-domain-name"></a>Domeinnaam van tenant ophalen

Verzamelen en opslaan van de domeinnaam van de Active Directory-tenant waar de toepassingen die zijn geregistreerd. 

Selecteer in het navigatiedeelvenster links het **Azure Active Directory** service. Selecteer **Namen van aangepaste domeinen**. Kopiëren en opslaan van de domeinnaam.

![Domeinnaam](media/blockchain-workbench-deploy/domain-name.png)

## <a name="deploy-blockchain-workbench"></a>Blockchain Workbench implementeren

Als de vereiste stappen zijn voltooid, bent u klaar om te implementeren, de Blockchain Workbench. De volgende secties overzicht over het implementeren van het framework.

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Selecteer uw account in de rechterbovenhoek hoek en schakel over naar de gewenste Azure AD-tenant waar u wilt implementeren, Azure Blockchain Workbench.
3.  Selecteer in het linkerdeelvenster **een resource maken**. Zoeken naar `Azure Blockchain Workbench` in de **Marketplace doorzoeken** zoekbalk. 

    ![Zoekbalk Marketplace](media/blockchain-workbench-deploy/marketplace-search-bar.png)

4.  Selecteer **Azure Blockchain Workbench**.

    ![De resultaten voor zoeken op Marketplace](media/blockchain-workbench-deploy/marketplace-search-results.png)

4.  Selecteer **Maken**.
5.  De algemene instellingen hebt voltooid.

    ![Azure Blockchain Workbench maken](media/blockchain-workbench-deploy/blockchain-workbench-settings-basic.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Voorvoegsel van de resource | Korte unieke id voor uw implementatie. Deze waarde wordt gebruikt als basis voor de naamgeving van resources. |
    | VM-gebruikersnaam | Naam van de gebruiker wordt gebruikt als beheerder voor alle virtuele machines (VM). |
    | Verificatietype | Selecteer als u wilt gebruiken van een wachtwoord of de sleutel voor de verbinding met virtuele machines. |
    | Wachtwoord | Het wachtwoord wordt gebruikt om verbinding te maken met VM's. |
    | SSH | Een openbare RSA-sleutel gebruiken in de indeling met één regel die begint met **ssh-rsa** of gebruik de PEM-indeling van meerdere regels. U kunt met behulp van SSH-sleutels genereren `ssh-keygen` op Linux en OS X of PuTTYGen met op Windows. Zie voor meer informatie over SSH-sleutels, [over het gebruik van SSH-sleutels met Windows op Azure](../virtual-machines/linux/ssh-from-windows.md). |
    | Wachtwoord van de database / wachtwoord bevestigen | Geef het wachtwoord moet worden gebruikt voor toegang tot de database gemaakt als onderdeel van de implementatie. |
    | Implementatieregio | Geef op waar om Blockchain Workbench-resources te implementeren. Voor beschikbaarheid van aanbevolen, dit moet overeenkomen met de **locatie** instelling. |
    | Abonnement | Geef het Azure-abonnement dat u wilt gebruiken voor uw implementatie. |
    | Resourcegroepen | Een nieuwe resourcegroep maken door te selecteren **nieuw** en geeft u de naam van een unieke resource-groep. |
    | Locatie | Geef de regio die u wilt implementeren, het framework. |

6.  Selecteer **OK** voltooien van de instelling basis configuratiesectie.

7.  Voltooi de **Azure Active Directory-installatie**.

    ![Installatie van de Azure AD](media/blockchain-workbench-deploy/blockchain-workbench-settings-aad.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Domeinnaam | Gebruik Azure AD-tenant die worden verzameld de [Get tenant domeinnaam](#get-tenant-domain-name) sectie vereisten. |
    | Toepassings-id | Gebruik de toepassings-ID van de app-registratie van de Blockchain-client die worden verzameld de [toepassings-ID ophalen](#get-application-id) sectie vereisten. |
    | Toepassingssleutel | Gebruik de sleutel van de toepassing van de Blockchain-app clientregistratie verzameld de [toevoegen Graph API-sleutel aan toepassing](#add-graph-api-key-to-application) sectie vereisten. |


8.  Klik op **OK** voltooien van de sectie van de configuratie van Azure AD-Parameters.

9.  In **netwerkinstellingen en de prestaties**, kiezen als u wilt een nieuw blockchain-netwerk maken of een bestaand proof-of-authority blockchain-netwerk gebruiken.

    Voor **nieuw**:

    De *Maak een nieuwe* optie maakt u een set knooppunten binnen één enkel lid abonnement Ethereum Proof-of-Authority (PoA). 

    ![Netwerkinstellingen en -prestaties](media/blockchain-workbench-deploy/blockchain-workbench-settings-network-new.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Aantal blockchain-knooppunten | Kies het aantal Ethereum PoA validator knooppunten die worden geïmplementeerd in uw netwerk. |
    | Opslagprestaties | Kies de gewenste VM-opslagprestaties van uw blockchain-netwerk. |
    | Grootte virtuele machine | Kies de gewenste VM-grootte voor uw blockchain-netwerk. |

    Voor **gebruik bestaande**:

    De *gebruik bestaande* optie kunt u een netwerk van de blockchain Ethereum Proof-of-Authority (PoA) opgeven. Eindpunten hebben de volgende vereisten.

    * Het eindpunt moet een netwerk van de blockchain Ethereum Proof-of-Authority (PoA).
    * Het eindpunt moet openbaar toegankelijk is via het netwerk.
    * Het PoA blockchain-netwerk moet worden geconfigureerd met gas prijs is ingesteld op nul (Opmerking: Blockchain Workbench-accounts worden niet gefinancierd. "Als fondsen vereist zijn, wordt de transacties mislukken).

    ![Netwerkinstellingen en -prestaties](media/blockchain-workbench-deploy/blockchain-workbench-settings-network-existing.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Ethereum RPC-eindpunt | Geef het RPC-eindpunt van een bestaand PoA blockchain-netwerk. Het eindpunt begint met http:// en eindigt met een poortnummer op. Bijvoorbeeld: `http://contoso-chain.onmicrosoft.com:8545` |
    | Opslagprestaties | Kies de gewenste VM-opslagprestaties van uw blockchain-netwerk. |
    | Grootte virtuele machine | Kies de gewenste VM-grootte voor uw blockchain-netwerk. |

10. Selecteer **OK** om netwerkinstellingen en prestaties te voltooien.

11. Voltooi de **Azure Monitor** instellingen.

    ![Azure Monitor](media/blockchain-workbench-deploy/blockchain-workbench-settings-oms.png)

    | Instelling | Beschrijving  |
    |---------|--------------|
    | Bewaking | Kies of u om in te schakelen van Azure Monitor om uw blockchain-netwerk te controleren |
    | Maak verbinding met bestaande Log Analytics-exemplaar | Kies of u wilt gebruiken met een bestaande Log Analytics-exemplaar of een nieuwe maken. Als u een bestaand exemplaar gebruikt, voert u uw werkruimte-ID en primaire sleutel. |

12. Klik op **OK** voltooien van de sectie Azure Monitor.

13. Bekijk de samenvatting om te controleren of dat uw parameters correct zijn.

    ![Samenvatting](media/blockchain-workbench-deploy/blockchain-workbench-summary.png)

14. Selecteer **maken** Ga akkoord met de voorwaarden en implementeren van uw Azure Blockchain Workbench.

De implementatie kan tot 90 minuten duren. U kunt de Azure-portal om de voortgang te gebruiken. Selecteer in de zojuist gemaakte resourcegroep **implementaties > overzicht** om de status van de geïmplementeerde artefacten te zien.

## <a name="blockchain-workbench-web-url"></a>Blockchain Workbench Web-URL

Als de implementatie van de Blockchain Workbench is voltooid, bevat een nieuwe resourcegroep waarin uw Blockchain Workbench-resources. Blockchain Workbench services toegankelijk zijn via een web-URL. De volgende stappen laten zien hoe u de web-URL van de geïmplementeerde framework niet ophalen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het navigatiedeelvenster links **resourcegroepen**
3. Kies de Resourcegroepnaam die u hebt opgegeven bij het implementeren van Blockchain Workbench.
4. Klik op de **TYPE** kolomkop om de lijst alfabetisch gesorteerd op type te sorteren.
5. Er zijn twee resources met type **App Service**. Selecteer de resource van het type **App Service** *zonder* de '-api ' achtervoegsel.

    ![Lijst met App service](media/blockchain-workbench-deploy/resource-group-list.png)

6.  In de App Service **Essentials** sectie, Kopieer de **URL** -waarde voor de web-URL naar uw geïmplementeerde Blockchain Workbench.

    ![App service essentials](media/blockchain-workbench-deploy/app-service.png)

Als u wilt koppelen een aangepaste domeinnaam met Blockchain Workbench, Zie [configureren van een aangepaste domeinnaam voor een web-app in Azure App Service met Traffic Manager](../app-service/web-sites-traffic-manager-custom-domain-name.md).

## <a name="configuring-the-reply-url"></a>De antwoord-URL configureren

Als de Azure Blockchain Workbench eenmaal is geïmplementeerd, wordt de volgende stap is om ervoor te zorgen dat de clienttoepassing voor Azure Active Directory (Azure AD) is geregistreerd bij de juiste **antwoord-URL** web-URL van de geïmplementeerde Blockchain Workbench.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Controleer of dat u bent van de tenant waar u de Azure AD-clienttoepassing geregistreerd.
3. Selecteer in het navigatiedeelvenster links het **Azure Active Directory** service. Selecteer **App-registraties**.
4. Selecteer de Azure AD-clienttoepassing die u in de sectie vereisten hebt geregistreerd.
5. Selecteer **instellingen > antwoord-URL's**.
6. Geef de belangrijkste web-URL van de Azure Blockchain Workbench-implementatie die u hebt opgehaald in de **ophalen van de Web-URL van Azure Blockchain Workbench** sectie. De antwoord-URL wordt voorafgegaan door `https://`. Bijvoorbeeld: `https://myblockchain2-7v75.azurewebsites.net`

    ![Antwoord-URL's](media/blockchain-workbench-deploy/configure-reply-url.png)

7. Selecteer **opslaan** om bij te werken van de clientregistratie van de.

## <a name="remove-a-deployment"></a>Een implementatie verwijderen

Wanneer u een implementatie is niet meer nodig hebt, kunt u een implementatie verwijderen door de Blockchain Workbench-resourcegroep te verwijderen.

1. In de Azure-portal, gaat u naar **resourcegroep** in het navigatiedeelvenster links in en selecteer de resourcegroep die u wilt verwijderen. 
2. Selecteer **Resourcegroep verwijderen**. Verwijdering controleren door in te voeren van de naam van de resourcegroep en selecteer **verwijderen**.

    ![Resourcegroep verwijderen](media/blockchain-workbench-deploy/delete-resource-group.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel met instructies, moet u Azure Blockchain Workbench geïmplementeerd. Doorgaan naar de volgende procedures voor artikel voor meer informatie over het maken van een toepassing blockchain.

> [!div class="nextstepaction"]
> [Een blockchain-toepassing maken in Azure Blockchain Workbench](blockchain-workbench-create-app.md)
