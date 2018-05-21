---
title: 'Implementeer de App-Services: Azure Stack | Microsoft Docs'
description: Gedetailleerde richtlijnen voor het implementeren van App Service in Azure-Stack
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: anwestg
ms.openlocfilehash: 9554309522e4a1e60fd3599b9a19bcf9cf4bbefb
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="add-an-app-service-resource-provider-to-azure-stack"></a>Een App Service-resourceprovider toevoegen aan Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

> [!IMPORTANT]
> De update 1804 toepassen op uw Azure-Stack geïntegreerd systeem of de nieuwste Azure-Stack development kit implementeren voordat u Azure App Service 1.2 implementeert.
>
>

Als een Azure-Stack-cloud-operator, kunt u uw gebruikers bieden de mogelijkheid voor het maken van websites en toepassingen van de API. Om dit te doen, moet u eerst toevoegen de [App Service-resourceprovider](azure-stack-app-service-overview.md) voor uw Azure-Stack-implementatie zoals beschreven in dit artikel. Nadat u de App Service resourceprovider hebt geïnstalleerd, kunt u deze opnemen in uw aanbiedingen en plannen. Gebruikers kunnen zich vervolgens aanmelden om de service en beginnen met het maken van toepassingen.

> [!IMPORTANT]
> Voordat u het installatieprogramma uitvoert, zorg ervoor dat u de instructies in hebt gevolgd [voordat u aan de slag](azure-stack-app-service-before-you-get-started.md).
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Voer het App Service resource provider-installatieprogramma

De App Service-resourceprovider te installeren in uw Azure-Stack-omgeving kan ten minste een uur duren afhankelijk van hoeveel rolinstanties die u hebt gekozen om te implementeren. Tijdens dit proces wordt het installatieprogramma:

* Maak een blob-container in het opgegeven Azure-Stack-opslagaccount.
* Maak een DNS-zone en vermeldingen voor App Service.
* Registreer de provider van de bron-App Service.
* Registreer de App Service-galerij-items.

Volg deze stappen voor het implementeren van App Service-resourceprovider:

1. Appservice.exe worden uitgevoerd als een beheerder van een pc die de Azure-Stack Azure Resource Management Beheereindpunt kan bereiken.

2. Klik op **of upgrade naar de nieuwste versie van de App Service implementeren**.

    ![App Service-installatie][1]

3. Controleer en accepteer de licentievoorwaarden voor Microsoft-Software en klik vervolgens op **volgende**.

4. Controleer en accepteer de licentievoorwaarden van derden en klik vervolgens op **volgende**.

5. Zorg ervoor dat de App Service-cloud-configuratie-informatie juist is. Als u de standaardinstellingen tijdens de implementatie van Azure Stack Development Kit gebruikt, kunt u hier de standaardwaarden accepteren. Als u de opties aangepast wanneer u Azure-Stack geïmplementeerd of op een geïntegreerd systeem implementeert, moet u de waarden in dit venster om weer te geven die bewerken. Als u het domein achtervoegsel mycloud.com gebruikt, kunt u voor uw Azure Stack Tenant Azure Resource Manager-eindpunt, moet wijzigen naar management. &lt;regio&gt;. mycloud.com. Nadat u uw gegevens bevestigen, klikt u op **volgende**.

    ![App Service-installatie][2]

6. Op de volgende pagina:
    1. Klik op de **Connect** naast de **Stack-Azure-abonnementen** vak.
        * Als u gebruikmaakt van Azure Active Directory (Azure AD), voert u de Azure AD-beheerdersaccount en het wachtwoord die u hebt opgegeven tijdens de implementatie van Azure-Stack. Klik op **aanmelden**.
        * Als u Active Directory Federation Services (AD FS), Geef uw beheerdersaccount. Bijvoorbeeld cloudadmin@azurestack.local. Voer uw wachtwoord in en klikt u op **aanmelden**.
    2. In de **Stack-Azure-abonnementen** de optie de **Provider-abonnement standaard**.
    3. In de **Azure stacklocaties** Selecteer de locatie die overeenkomt met de regio die u implementeert op. Selecteer bijvoorbeeld **lokale** als uw implementatie van de Azure-Stack Development Kit.

    ![App Service-installatie][3]

7. U hebt nu de optie om te implementeren in een bestaand virtueel netwerk, zoals die zijn geconfigureerd door de stappen [hier](azure-stack-app-service-before-you-get-started.md#virtual-network), of het App Service-installatieprogramma voor het maken van een virtueel netwerk en gekoppelde subnetten toe.
    1. Selecteer **VNet maken met de standaardinstellingen**, accepteer de standaardinstellingen en klik op **volgende**, of;
    2. Selecteer **gebruiken van bestaande VNet en subnetten**.
        1. Selecteer de **resourcegroep** waarin het virtuele netwerk.
        2. Kies de juiste **virtueel netwerk** naam die u implementeren wilt in;
        3. Selecteer de juiste **Subnet** waarden voor elk van de vereiste functieservices subnetten;
        4. Klik op **Volgende**

    ![App Service-installatie][4]

8. Geef de gegevens voor de bestandsshare en klik vervolgens op **volgende**. Het adres van de bestandsshare moet gebruiken voor de FQDN-naam of IP-adres van de bestandsserver. Bijvoorbeeld: \\\appservicefileserver.local.cloudapp.azurestack.external\websites, of \\\10.0.0.1\websites.

   > [!NOTE]
   > Het installatieprogramma probeert te testen van verbinding met de bestandsshare voordat u doorgaat.  Als u hebt gekozen om te implementeren in een bestaand virtueel netwerk, maar het installatieprogramma mogelijk geen verbinding maken met de bestandsshare en een waarschuwing wordt weergegeven waarin wordt gevraagd of u wilt doorgaan.  Controleer de bestandsshare en doorgaan als ze juist zijn.
   >
   >

   ![App Service-installatie][7]

9. Op de volgende pagina:
    1. In de **identiteit toepassings-ID** Voer de GUID voor de toepassing die u voor de identiteit (van Azure AD gebruikt).
    2. In de **Application Identity-certificaatbestand** vak (Typ of blader naar) de locatie van het certificaatbestand.
    3. In de **identiteit toepassing certificaatwachtwoord** Voer het wachtwoord voor het certificaat. Dit wachtwoord is die u genoteerd hebt als u het script gebruikt voor het maken van de certificaten.
    4. In de **Azure Resource Manager basiscertificaatbestand** vak (Typ of blader naar) de locatie van het certificaatbestand.
    5. Klik op **Volgende**.

    ![App Service-installatie][9]

10. Elk van de drie vakken bestand van het certificaat, klikt u op **Bladeren** en navigeer naar het gewenste certificaatbestand. U moet het wachtwoord opgeven voor elk certificaat. Deze certificaten zijn degene die u hebt gemaakt in de [maken vereiste certificaten stap](azure-stack-app-service-before-you-get-started.md#get-certificates). Klik op **volgende** na het invoeren van alle informatie.

    | Box | Voorbeeld van de naam van een certificaat |
    | --- | --- |
    | **App Service standaard SSL-certificaat-bestand** | \_.appservice.local.AzureStack.external.pfx |
    | **API-App Service SSL-certificaatbestand** | api.appservice.local.AzureStack.external.pfx |
    | **App Service Publisher SSL-certificaat-bestand** | ftp.appservice.local.AzureStack.external.pfx |

    Als u een andere domeinachtervoegsel gebruikt bij het maken van de certificaten, de bestandsnamen van uw certificaat niet gebruikt *lokale. AzureStack.external*. In plaats daarvan gebruik de informatie van uw aangepaste domein.

    ![App Service-installatie][10]

11. Voer de SQL Server-gegevens voor het serverexemplaar waarmee de App Service resource provider-databases hosten en klik vervolgens op **volgende**. Het installatieprogramma valideert de eigenschappen van de SQL-verbinding.

    > [!NOTE]
    > Het installatieprogramma probeert te testen van verbinding met de SQl-Server voordat u doorgaat.  Als u hebt gekozen om te implementeren in een bestaand virtueel netwerk, maar het installatieprogramma mogelijk geen verbinding maken met de SQL-Server en een waarschuwing wordt weergegeven waarin wordt gevraagd of u wilt doorgaan.  Controleer of de SQL Server-gegevens en doorgaan als ze juist zijn.
    >
    >

    ![App Service-installatie][11]

12. Bekijk de rolinstantie en SKU-opties. De standaardinstellingen te vullen met het minimum aantal exemplaar en de minimale SKU voor elke rol in de implementatie van een ASDK. Een overzicht van de vereisten voor vCPU en het geheugen is opgegeven bij het plannen van uw implementatie. Nadat u uw selecties maken, klikt u op **volgende**.

    > [!NOTE]
    > Voor implementaties in de instructies in het volgende [planning van capaciteit voor Azure App Service-serverfuncties in Azure-Stack](azure-stack-app-service-capacity-planning.md).
    >
    >

    | Rol | Minimale exemplaren | Minimale SKU | Opmerkingen |
    | --- | --- | --- | --- |
    | Controller | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Beheert en onderhoudt de status van de cloud-App Service. |
    | Beheer | 1 | Standard_A2 - (2 vCPUs, 3584 MB) | Beheert de App Service Azure Resource Manager en API-eindpunten, portal-extensies (admin, tenant, Functions-portal) en de data-service. Ter ondersteuning van failover, vergroot u de aanbevolen exemplaren 2. |
    | Uitgever | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Inhoud via FTP en web deployment publiceert. |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Routes aanvragen voor App Service-toepassingen. |
    | Gedeelde Worker | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Hosts webtoepassingen of API en apps van Azure Functions. Mogelijk wilt meer exemplaren toe te voegen. Als operator kunt u definiëren van uw aanbod en eventuele SKU-categorie kiezen. De lagen moeten minimaal één vCPU hebben. |

    ![App Service-installatie][13]

    > [!NOTE]
    > **Windows Server 2016-Core, is geen ondersteund platforminstallatiekopie voor gebruik met Azure App Service op Azure-Stack.  Gebruik geen evaluatie installatiekopieën voor productie-implementaties.**

13. In de **Platforminstallatiekopie Selecteer** vak, kiest u de installatiekopie van uw implementatie van Windows Server 2016-virtuele machine van de afbeeldingen die beschikbaar zijn in de compute resourceprovider voor de cloud-App Service. Klik op **Volgende**.

14. Op de volgende pagina:
     1. Voer de gebruikersnaam voor Worker-rol virtuele machine-beheerder en het wachtwoord.
     2. Voer de gebruikersnaam van de beheerder voor andere rollen virtuele machine en het wachtwoord.
     3. Klik op **Volgende**.

    ![App Service-installatie][15]    

15. Op de pagina overzicht:
    1. Controleer of de selecties die u hebt aangebracht. Als u wilt wijzigen, gebruikt u de **vorige** knoppen naar vorige pagina's te gaan.
    2. Als de configuraties correct zijn, schakel het selectievakje in.
    3. Voor het starten van de implementatie, klikt u op **volgende**.

    ![App Service-installatie][16]

16. Op de volgende pagina:
    1. De installatievoortgang volgen. App-Service op Azure-Stack duurt ongeveer 60 minuten te implementeren op basis van de standaardselecties.
    2. Nadat het installatieprogramma met succes is voltooid, klikt u op **afsluiten**.

    ![App Service-installatie][17]

## <a name="validate-the-app-service-on-azure-stack-installation"></a>De App-Service op Azure-Stack installatie valideren

1. Ga in de Stack van Azure-beheerportal naar **Administration - App Service**.

2. Schakel in het overzicht onder status om te zien die de **Status** toont **alle rollen gereed zijn**.

    ![App Service-beheer](media/azure-stack-app-service-deploy/image12.png)
    
> [!NOTE]
> Als u wilt implementeren in een bestaand virtueel netwerk en een interne IP-adres conenct bij uw bestandsserver, moet u een uitgaande beveiligingsregel toevoegen voor het inschakelen van SMB-verkeer tussen het subnet van de werknemer en de bestandsserver aan te geven.  Hiervoor gaat u naar de WorkersNsg in het beheerportal en een uitgaande beveiligingsregel met de volgende eigenschappen toevoegen:
> * Bron:
> * Poortbereik van bron: *
> * Bestemming: IP-adressen
> * Doel-IP-adresbereik: bereik van IP-adressen voor uw bestandsserver
> * Poortbereik van doel: 445
> * Protocol: TCP
> * Actie: toestaan
> * Prioriteit: 700
> * Naam: Outbound_Allow_SMB445

## <a name="test-drive-app-service-on-azure-stack"></a>App Service-station op Azure-Stack testen

Nadat u de App Service-resourceprovider registreren en implementeert, testen om ervoor te zorgen dat gebruikers web- en API apps kunnen implementeren.

> [!NOTE]
> U moet een aanbieding met de naamruimte Microsoft.Web binnen het plan maken. Vervolgens moet u een tenantabonnement hebt met deze aanbieding is geabonneerd. Zie voor meer informatie [maken aanbieding](azure-stack-create-offer.md) en [plan maken](azure-stack-create-plan.md).
>
U *moet* beschikt over een tenantabonnement te maken van toepassingen die gebruikmaken van App Service op Azure-Stack. De enige mogelijkheden die een servicebeheerder kan worden voltooid binnen de beheerportal zijn gerelateerd aan het beheer van de resource provider van App Service. Deze mogelijkheden zijn capaciteit toe te voegen, implementatie-bronnen configureren en lagen van de werknemer en SKU's toe te voegen.
>
Als u wilt maken van web-API en Azure functies apps, moet u een tenantabonnement hebt en de tenantportal gebruiken.

1. Klik in de tenantportal Azure Stack op **nieuw** > **Web en mobiel** > **Web-App**.

2. Op de **Web-App** blade een naam in de **Web-app** vak.

3. Onder **resourcegroep**, klikt u op **nieuw**. Typ een naam in de **resourcegroep** vak.

4. Klik op **App Service-plan/-locatie** > **Nieuw**.

5. Op de **App Service-abonnement** blade een naam in de **App Service-abonnement** vak.

6. Klik op **prijscategorie** > **vrije gedeelde** of **gedeelde gedeelde** > **Selecteer**  >   **OK** > **maken**.

7. In minder dan een minuut een tegel voor de nieuwe web-app wordt weergegeven op het dashboard. Klik op de tegel.

8. Op de **Web-App** blade, klikt u op **Bladeren** om de standaardwebsite voor deze app weer te geven.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>Een WordPress, DNN of Django-website (optioneel) implementeren

1. Klik in de tenantportal Azure Stack op **+**, gaat u naar de Azure Marketplace een Django-website te implementeren en wacht met succes is voltooid. Een bestand gebaseerde database maakt gebruik van het Django webplatform. Eventuele extra resourceproviders, zoals SQL of MySQL nodig niet is.

2. Als u een MySQL-resourceprovider ook hebt geïmplementeerd, kunt u een WordPress-website van de Marketplace kunt implementeren. Wanneer u wordt gevraagd om de databaseparameters, voert u de naam van de gebruiker als *User1@Server1*, met de gebruikersnaam en de naam van uw keuze.

3. Als u een SQL Server-resourceprovider ook hebt geïmplementeerd, kunt u een website DNN vanuit de Marketplace kunt implementeren. Wanneer u wordt gevraagd om de databaseparameters, kiest u een database in de computer met SQL Server die verbonden met uw resourceprovider.

## <a name="next-steps"></a>Volgende stappen

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md).

- [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md)
- [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy/app-service-installer.png
[2]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-arm-endpoints.png
[3]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-subscription-information.png
[4]: ./media/azure-stack-app-service-deploy/app-service-default-VNET-config.png
[5]: ./media/azure-stack-app-service-deploy/app-service-custom-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy/app-service-custom-VNET-config-with-values.png
[7]: ./media/azure-stack-app-service-deploy/app-service-fileshare-configuration.png
[8]: ./media/azure-stack-app-service-deploy/app-service-fileshare-configuration-error.png
[9]: ./media/azure-stack-app-service-deploy/app-service-identity-app.png
[10]: ./media/azure-stack-app-service-deploy/app-service-certificates.png
[11]: ./media/azure-stack-app-service-deploy/app-service-sql-configuration.png
[12]: ./media/azure-stack-app-service-deploy/app-service-sql-configuration-error.png
[13]: ./media/azure-stack-app-service-deploy/app-service-cloud-quantities.png
[14]: ./media/azure-stack-app-service-deploy/app-service-windows-image-selection.png
[15]: ./media/azure-stack-app-service-deploy/app-service-role-credentials.png
[16]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-deployment-summary.png
[17]: ./media/azure-stack-app-service-deploy/app-service-deployment-progress.png
