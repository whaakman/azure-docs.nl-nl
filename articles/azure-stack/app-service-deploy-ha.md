---
title: Azure Stack App Service implementeren in een maximaal beschikbare configuratie | Microsoft Docs
description: Informatie over het implementeren van App Service in Azure Stack met behulp van een maximaal beschikbare configuratie.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: ''
ms.date: 03/23/2019
ms.author: jeffgilb
ms.reviewer: anwestg
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: 1c105548f19994c4ca0ce161eedcfe11736864c7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370020"
---
# <a name="deploy-app-service-in-a-highly-available-configuration"></a>App Service implementeren in een maximaal beschikbare configuratie

Dit artikel leest hoe u Azure Stack marketplace-items voor het App Service voor Azure Stack implementeren in een maximaal beschikbare configuratie. Naast het beschikbaar marketplace-items, gebruikt deze oplossing ook de [appservice-bestandsshare-SQL Server-ha](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) Azure Stack-Quickstart-sjabloon. Deze sjabloon automatiseert het maken van een maximaal beschikbare infrastructuur voor het hosten van de App Service-resourceprovider. App Service is geïnstalleerd op deze maximaal beschikbare VM-infrastructuur. 

## <a name="deploy-the-highly-available-app-service-infrastructure-vms"></a>De maximaal beschikbare virtuele machines van de App Service-infrastructuur implementeren
De [appservice-bestandsshare-SQL Server-ha](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) snelstartsjabloon van Azure Stack vereenvoudigt de implementatie van App Service in een maximaal beschikbare configuratie. Deze moet worden geïmplementeerd in het abonnement op standaard-Provider. 

Wanneer gebruikt voor het maken van een aangepaste resource in Azure Stack, maakt de sjabloon:
- Een virtueel netwerk en subnetten vereist.
- Netwerkbeveiligingsgroepen voor file server, SQL Server en Active Directory Domain Services (AD DS)-subnetten.
- Storage-accounts voor VM-schijven en cluster cloud-witness.
- Een interne load balancer voor SQL-VM's met particuliere IP-adres is gebonden aan de SQL AlwaysOn-listener.
- Drie beschikbaarheidssets voor de AD DS, bestandsservercluster en de SQL-cluster.
- Cluster met twee knooppunten SQL.
- Cluster met twee knooppunten bestandsserver.
- Twee domeincontrollers.

### <a name="required-azure-stack-marketplace-items"></a>Vereiste Azure Stack marketplace-items
Voordat u deze sjabloon gebruikt, zorg ervoor dat de volgende [items voor de Azure Stack marketplace](azure-stack-marketplace-azure-items.md) zijn beschikbaar in uw Azure Stack-exemplaar:

- Windows Server 2016 Datacenter Core-installatiekopie (voor AD DS- en -server VM's)
- SQL Server 2016 SP2 op WindowsServer 2016 (Enterprise)
- Nieuwste SQL IaaS-extensie 
- Nieuwste versie van PowerShell Desired State Configuration-extensie 

> [!TIP]
> Beoordeling [het Leesmij-bestand van het sjabloon](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) op GitHub voor meer informatie over de sjabloonvereisten en standaardwaarden. 

### <a name="deploy-the-app-service-infrastructure"></a>De App Service-infrastructuur implementeren
Gebruik de stappen in deze sectie voor het maken van een aangepaste implementatie met de **appservice-bestandsshare-SQL Server-ha** Azure Stack-Quickstart-sjabloon.

1. [!INCLUDE [azs-admin-portal](../../includes/azs-admin-portal.md)]

2. Selecteer **\+** **een resource maken** > **aangepaste**, en vervolgens **sjabloonimplementatie**.

   ![Implementatie van de aangepaste sjabloon](media/app-service-deploy-ha/1.png)


3. Op de **aangepaste implementatie** Selecteer **template bewerken** > **Quickstart-sjabloon** en vervolgens de vervolgkeuzelijst met beschikbare aangepaste sjablonen te gebruiken Selecteer de **appservice-bestandsshare-SQL Server-ha** sjabloon, klikt u op **OK**, en vervolgens **opslaan**.

   ![Selecteer de appservice-bestandsshare-SQL Server-ha QuickStart-sjabloon](media/app-service-deploy-ha/2.png)

4. Op de **aangepaste implementatie** Selecteer **parameters bewerken** en blader omlaag naar de standaardwaarden van de sjabloon bekijken. Wijzig deze waarden die nodig is voor de vereiste parameterinformatie te verstrekken en klik vervolgens op **OK**.<br><br> Geef ten minste complexe wachtwoorden voor de parameters ADMINPASSWORD, FILESHAREOWNERPASSWORD, FILESHAREUSERPASSWORD, SQLSERVERSERVICEACCOUNTPASSWORD en SQLLOGINPASSWORD.
    
   ![Aangepaste implementatieparameters bewerken](media/app-service-deploy-ha/3.png)

5. Op de **aangepaste implementatie** blade, zorg ervoor dat **Providerabonnement standaard** is geselecteerd als het abonnement te gebruiken en maak een nieuwe resourcegroep of Selecteer een bestaande resourcegroep voor de aangepaste de implementatie.<br><br> Selecteer vervolgens de locatie van de resourcegroep (**lokale** voor installaties ASDK) en klik vervolgens op **maken**. De aangepaste implementatie-instellingen worden gevalideerd voordat de sjabloonimplementatie van de wordt gestart.

    ![Aangepaste implementatie maken](media/app-service-deploy-ha/4.png)

6. Selecteer in de beheerportal **resourcegroepen** en vervolgens de naam van de resourcegroep waarin u hebt gemaakt voor de aangepaste implementatie (**app-service-ha** in dit voorbeeld). Bekijk de status van de implementatie om te controleren of dat alle implementaties zijn voltooid.

   > [!NOTE]
   > De sjabloonimplementatie zullen ongeveer een uur duren.

   [![](media/app-service-deploy-ha/5-sm.png "Controleer de implementatiestatus van de sjabloon")](media/app-service-deploy-ha/5-lg.png#lightbox)


### <a name="record-template-outputs"></a>Recordsjabloon weergeeft
Nadat de is implementatie voltooid, en record die de sjabloonimplementatie levert. U moet deze informatie opgeven bij het uitvoeren van het App Service-installatieprogramma. 

Zorg ervoor dat u elk van deze uitvoerwaarden vastleggen:
- FileSharePath
- FileShareOwner
- FileShareUser
- SQLserver
- SQLuser

Volg deze stappen voor het detecteren van de uitvoerwaarden voor de sjabloon:

1. [!INCLUDE [azs-admin-portal](../../includes/azs-admin-portal.md)]

2. Selecteer in de beheerportal **resourcegroepen** en vervolgens de naam van de resourcegroep waarin u hebt gemaakt voor de aangepaste implementatie (**app-service-ha** in dit voorbeeld). 

3. Klik op **implementaties** en selecteer **Microsoft.Template**.

    ![Microsoft.Template implementatie](media/app-service-deploy-ha/6.png)

4. Na het selecteren van de **Microsoft.Template** implementatie, selecteer **uitvoer** en de uitvoer van de sjabloon-parameter opnemen. Deze informatie is vereist bij het implementeren van App Service.

    ![Parameter-uitvoer](media/app-service-deploy-ha/7.png)


## <a name="deploy-app-service-in-a-highly-available-configuration"></a>App Service implementeren in een maximaal beschikbare configuratie
Volg de stappen in deze sectie om de App Service voor Azure Stack implementeren in een maximaal beschikbare configuratie op basis van de [appservice-bestandsshare-SQL Server-ha](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) Azure Stack-Quickstart-sjabloon. 

Nadat u de App Service-resourceprovider hebt geïnstalleerd, kunt u deze kunt opnemen in uw aanbiedingen en plannen. Gebruikers kunnen zich vervolgens aanmelden om de service en -toepassingen maken.

> [!IMPORTANT]
> Voordat u de resource provider-installatieprogramma uitvoert, controleert u dat u de releaseopmerkingen die gepaard gaan met elke versie van de App Service, voor meer informatie over nieuwe functionaliteit, correcties en bekende problemen die invloed kunnen zijn op uw implementatie hebt gelezen.

### <a name="prerequisites"></a>Vereisten
Voordat u het installatieprogramma van de App Service uitvoeren kunt, verschillende stappen zijn vereist zoals beschreven in de [voordat u aan de slag met App Service op Azure Stack-artikel](azure-stack-app-service-before-you-get-started.md):

> [!TIP]
> Niet alle stappen die worden beschreven in de voordat u aan de slag artikel zijn vereist, omdat de sjabloon deploymnet configureert de infrastructuur-VM's voor u. 

- [De App Service-installatieprogramma en helper scripts downloaden](azure-stack-app-service-before-you-get-started.md#download-the-installer-and-helper-scripts).
- [Download de nieuwste extensie voor aangepaste scripts naar de Azure Stack marketplace](azure-stack-app-service-before-you-get-started.md#syndicate-the-custom-script-extension-from-the-marketplace).
- [Vereist certificaten genereren](azure-stack-app-service-before-you-get-started.md#get-certificates).
- Maak de ID-toepassing op basis van de identiteit-provider die u hebt gekozen voor Azure Stack. Een toepassing-ID kunnen worden gemaakt voor een [Azure AD](azure-stack-app-service-before-you-get-started.md#create-an-azure-active-directory-application) of [Active Directory Federation Services](azure-stack-app-service-before-you-get-started.md#create-an-active-directory-federation-services-application) en noteer de toepassings-ID.
- Zorg ervoor dat u de installatiekopie van het Windows Server 2016 Datacenter hebt toegevoegd aan de Azure Stack marketplace. Dit is vereist voor de installatie van App Service.

### <a name="deploy-app-service-in-highly-available-configuration"></a>App Service implementeren in maximaal beschikbare configuratie
Installeren van de App Service-resourceprovider is ten minste een uur. De lengte van de tijd die nodig is, is afhankelijk van op de rol van hoeveel u exemplaren implementeren. Het installatieprogramma uitgevoerd tijdens de implementatie van de volgende taken:

- Maak een blobcontainer in het opgegeven Azure Stack-opslagaccount.
- Maak een DNS-zone en vermeldingen voor App Service.
- Registreer de resourceprovider App Service.
- Registreer het galerie-items van het App Service.

Volg deze stappen voor het implementeren van App Service-resourceprovider:

1. Voer het eerder gedownloade App Service-installatieprogramma (**appservice.exe**) als beheerder vanaf een computer die toegang hebben tot de Azure Stack Admin Azure Resource Management-eindpunt.

2. Selecteer **of upgrade naar de nieuwste versie van de App Service implementeren**.

    ![Implementeren of upgraden](media/app-service-deploy-ha/01.png)

3. Microsoft-licentievoorwaarden accepteren en klik op **volgende**.

    ![Microsoft-licentievoorwaarden](media/app-service-deploy-ha/02.png)

4. Niet-Microsoft-licentievoorwaarden accepteren en klik op **volgende**.

    ![Niet-Microsoft-licentievoorwaarden](media/app-service-deploy-ha/03.png)

5. Geef de App Service cloudconfiguratie-eindpunt voor uw Azure Stack-omgeving.

    ![App Service-eindpunt cloudconfiguratie](media/app-service-deploy-ha/04.png)

6. **Verbinding maken met** aan het Azure Stack-abonnement om te worden gebruikt voor de installatie en kies de locatie. 

    ![Verbinding maken met de Azure Stack-abonnement](media/app-service-deploy-ha/05.png)

7. Selecteer **bestaande VNet en subnetten gebruiken** en de **groepsnaam voor Accountresources** voor de resourcegroep die wordt gebruikt om de maximaal beschikbare sjabloon te implementeren.<br><br>Vervolgens selecteert u het virtuele netwerk gemaakt als onderdeel van de sjabloonimplementatie en selecteer vervolgens de juiste rol subnetten in de vervolgkeuzelijst met opties. 

    ![Vnet-selectie](media/app-service-deploy-ha/06.png)

8. Geef dat de sjabloon die eerder geregistreerde voert informatie voor het pad naar de bestandsshare en file share de eigenaar van parameters. Wanneer u klaar bent, klikt u op **volgende**.

    ![Gegevens van bestandsshare uitvoer](media/app-service-deploy-ha/07.png)

9. Omdat de machine die wordt gebruikt voor het installeren van App Service niet in hetzelfde VNet als de bestandsserver wordt gebruikt bevindt zich voor het hosten van de App Service-bestandsshare, kunt u zich niet kunt oplossen door de naam. **Dit is normaal**.<br><br>Controleer of de informatie hebt ingevoerd voor de bestandsshare UNC-pad en de accounts informatie juist is en druk op **Ja** op de waarschuwing dialoogvenster om door te gaan met App Service-installatie.

    ![Verwachte foutberichtvenster](media/app-service-deploy-ha/08.png)

    Als u wilt implementeren in een bestaand virtueel netwerk en een interne IP-adres verbinding maken met de bestandsserver, moet u een uitgaande beveiligingsregel toevoegen voor het inschakelen van SMB-verkeer tussen de worker-subnet en de bestandsserver aan te geven. Ga naar de WorkersNsg in de beheerportal en voeg een uitgaande beveiligingsregel met de volgende eigenschappen:
    - Bron: Alle
    - Poortbereik van bron: *
    - Bestemming: IP-adressen
    - Doel-IP-adresbereik: Bereik van IP-adressen voor uw bestandsserver
    - Poortbereik van doel: 445
    - Protocol: TCP
    - Actie: Toestaan
    - Prioriteit: 700
    - Naam: Outbound_Allow_SMB445

10. Geef de Identiteitstoepassings-ID en het pad en de wachtwoorden voor de identiteit van certificaten en klik op **volgende**:
    - Certificaat voor identiteit-toepassing (in de indeling van **sso.appservice.local.azurestack.external.pfx**)
    - Azure Resource Manager-basiscertificaat (**AzureStackCertificationAuthority.cer**)

    ![Het toepassingscertificaat ID en -basiscertificaat](media/app-service-deploy-ha/008.png)

10. Vervolgens geeft u de resterende benodigde gegevens voor de volgende certificaten en op **volgende**:
    - Standaard-Azure Stack-SSL-certificaat (in de indeling van **_.appservice.local.azurestack.external.pfx**)
    - API-SSL-certificaat (in de indeling van **api.appservice.local.azurestack.external.pfx**)
    - Uitgeverscertificaat (in de vorm van **ftp.appservice.local.azurestack.external.pfx**) 

    ![Aanvullende configuratie-certificaten](media/app-service-deploy-ha/09.png)

11. De SQL Server-verbindingsgegevens met behulp van de SQL Server-verbindingsgegevens van de implementatie van hoge beschikbaarheid sjabloonuitvoerwaarden opgeven:

    ![SQL Server-verbindingsgegevens](media/app-service-deploy-ha/10.png)

12. Omdat de machine die wordt gebruikt voor het installeren van App Service niet in hetzelfde VNet als de SQL-server wordt gebruikt bevindt zich voor het hosten van de App Service-databases, kunt u zich niet kunt oplossen door de naam.  **Dit is normaal**.<br><br>Controleer of de informatie hebt ingevoerd voor de SQL Server-naam en de accounts informatie juist is en druk op **Ja** om door te gaan van de installatie van App Service. Klik op **volgende**.

    ![SQL Server-verbindingsgegevens](media/app-service-deploy-ha/11.png)

13. Accepteer de standaardwaarden voor de configuratie van rol of te wijzigen in de aanbevolen waarden en klik op **volgende**.<br><br>We raden aan dat de standaardwaarden voor de rolinstanties voor App Service-infrastructuur als volgt worden gewijzigd voor maximaal beschikbare configuraties:

    |Rol|Standaard|Maximaal beschikbare aanbeveling|
    |-----|-----|-----|
    |De functie van domeincontroller|2|2|
    |Beheerrol|1|3|
    |Uitgeverrol|1|3|
    |Front-endrol|1|3|
    |Gedeelde Werkrol|1|10|
    |     |     |     |

    ![Infrastructuur rol exemplaarwaarden](media/app-service-deploy-ha/12.png)

    > [!NOTE]
    > Het wijzigen van de standaardwaarden die aanbevolen in dit tutoral verhoogt de hardwarevereisten voor het installeren van App Service. Totaal 26 kernen en 46,592 MB aan RAM-geheugen is vereist voor ondersteuning van de aanbevolen 21 VM's in plaats van de standaard 18 kernen en 32,256 MB aan RAM-geheugen voor 15 virtuele machines.

14. Selecteer het platforminstallatiekopie moet worden gebruikt voor het installeren van de App Service-infrastructuur-VM's en klik op **volgende**:

    ![Platform-installatiekopie selecteren](media/app-service-deploy-ha/13.png)

15. App Service bieden infrastructuur rol referentie-informatie om te worden gebruikt en op **volgende**:

    ![Referenties voor infrastructuur-rol](media/app-service-deploy-ha/14.png)

16. Lees de informatie om te worden gebruikt voor het App Service implementeren en klik op **volgende** om te beginnen met de implementatie. 

    ![Samenvatting van de installatie controleren](media/app-service-deploy-ha/15.png)

17. Controleer de voortgang van de App Service-implementatie. Dit kan duren ruim een uur, afhankelijk van uw specifieke implementatie-configuratie en hardware. Nadat het installatieprogramma met succes is voltooid, selecteert u **afsluiten**.

    ![Setup is voltooid](media/app-service-deploy-ha/16.png)


## <a name="next-steps"></a>Volgende stappen

[Scale-out App Service](azure-stack-app-service-add-worker-roles.md). Mogelijk moet u extra werkrollen van App Service-infrastructuur rol om te voldoen aan de vraag van de verwachte toepassing in uw omgeving toevoegen. App Service in Azure Stack biedt standaard ondersteuning voor gratis en gedeelde werkrolniveaus. Als u wilt andere werkrolniveaus toevoegen, moet u meer werkrollen toevoegen.

[Implementatiebronnen configureren](azure-stack-app-service-configure-deployment-sources.md). Er is aanvullende configuratie vereist voor de ondersteuning van de implementatie op aanvraag vanuit meerdere cloudproviders voor broncodebeheer zoals GitHub, BitBucket, OneDrive en DropBox.

[Maak een back-up van App Service](app-service-back-up.md). Na een succesvolle implementatie en het configureren van App Service, moet u ervoor zorgen dat alle onderdelen die nodig zijn voor herstel na noodgevallen back-ups om te voorkomen dat gegevens verloren gaan en onnodige service uitvaltijd te voorkomen tijdens herstelbewerkingen.