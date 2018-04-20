---
title: App Service implementeren in een offline-omgeving in Azure-Stack | Microsoft Docs
description: Gedetailleerde richtlijnen voor het App Service implementeren in een omgeving zonder verbinding Azure Stack beveiligd met AD FS.
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
ms.date: 03/09/2018
ms.author: anwestg
ms.openlocfilehash: 7907056635049ce90a2653b0d58ef6299b77c71e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
# <a name="add-an-app-service-resource-provider-to-a-disconnected-azure-stack-environment-secured-by-ad-fs"></a>Een App Service-resourceprovider toevoegen aan een Azure-Stack omgeving zonder verbinding beveiligd met AD FS

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

> [!IMPORTANT]
> De update 1802 toepassen op uw Azure-Stack geïntegreerd systeem of de nieuwste Azure-Stack development kit implementeren voordat u Azure App Service implementeert.
>
>

Volg de instructies in dit artikel, kunt u de [App Service-resourceprovider](azure-stack-app-service-overview.md) naar een Azure-Stack-omgeving is:

- niet verbonden met Internet
- beveiligd door Active Directory Federation Services (AD FS).

Als de App Service-resourceprovider toevoegen aan uw offline Azure Stack-implementatie, moet u deze op het hoogste niveau taken uitvoeren:

1. Voltooi de [vereiste stappen](azure-stack-app-service-before-you-get-started.md) (zoals de aanschaf van certificaten die kan duren ontvangt een paar dagen).
2. [Downloaden en uitpakken van de installatie en Help-bestanden](azure-stack-app-service-before-you-get-started.md) op een computer die is verbonden met Internet.
3. Maak een pakket offline-installatie.
4. Voer het installatiebestand appservice.exe.

## <a name="create-an-offline-installation-package"></a>Maken van een offline-installatie-pakket

Voor het App Service implementeren in een omgeving zonder verbinding, moet u eerst een offline-installatie-pakket op een computer die verbonden met Internet maken.

1. Het installatieprogramma AppService.exe uitvoeren op een computer die verbonden met Internet.

2. Klik op **Geavanceerd** > **maken offline installatiepakket**.

    ![App Service-installatie][1]

3. Het App Service-installatieprogramma maakt een offline-installatiepakket en het pad naar het wordt weergegeven. U kunt klikken op **map openen** de map in de Verkenner openen.

    ![App Service-installatie](media/azure-stack-app-service-deploy-offline/image02.png)

4. Kopieer het installatieprogramma (AppService.exe) en het pakket offline-installatie naar uw Azure-Stack-hostcomputer.

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>De offline-installatie van App-Service op Azure-Stack voltooid

1. Appservice.exe als administrator uitvoeren vanaf een computer die de Azure-Stack Admin Azure Resource Management-eindpunt kan bereiken.

2. Klik op **Geavanceerd** > **offline-installatie voltooien**.

    ![App Service-installatie][2]

3. Blader naar de locatie van het offline installatiepakket die u eerder hebt gemaakt en klik vervolgens op **volgende**.

    ![App Service-installatie](media/azure-stack-app-service-deploy-offline/image04.png)

4. Controleer en accepteer de licentievoorwaarden voor Microsoft-Software en klik vervolgens op **volgende**.

5. Controleer en accepteer de licentievoorwaarden van derden en klik vervolgens op **volgende**.

6. Zorg ervoor dat de App Service-cloud-configuratie-informatie juist is. Als u de standaardinstellingen tijdens de implementatie van Azure Stack Development Kit gebruikt, kunt u hier de standaardwaarden accepteren. Als u de opties aangepast wanneer u Azure-Stack geïmplementeerd of op een geïntegreerd systeem implementeert, moet u de waarden in dit venster om weer te geven die bewerken. Als u het domein achtervoegsel mycloud.com gebruikt, kunt u voor uw Azure Stack Tenant Azure Resource Manager-eindpunt, moet wijzigen naar management. <region>. mycloud.com. Nadat u uw gegevens bevestigen, klikt u op **volgende**.

    ![App Service-installatie][3]

7. Op de volgende pagina:
    1. Klik op de **Connect** naast de **Stack-Azure-abonnementen** vak.
        - Geef uw beheerdersaccount. Bijvoorbeeld cloudadmin@azurestack.local. Voer uw wachtwoord in en klikt u op **aanmelden**.
    2. In de **Stack-Azure-abonnementen** Selecteer uw abonnement.
    3. In de **Azure stacklocaties** Selecteer de locatie die overeenkomt met de regio die u implementeert op. Selecteer bijvoorbeeld **lokale** als uw implementatie van de Azure-Stack Development Kit.
    4. Klik op **Volgende**.

    ![App Service-installatie][4]

8. U hebt nu de optie om te implementeren in een bestaand virtueel netwerk, zoals die zijn geconfigureerd door de stappen [hier](azure-stack-app-service-before-you-get-started.md#virtual-network), of het App Service-installatieprogramma voor het maken van een virtueel netwerk en gekoppelde subnetten toe.
    1. Selecteer **VNet maken met de standaardinstellingen**, accepteer de standaardinstellingen en klik vervolgens op **volgende**, of;
    2. Selecteer **gebruiken van bestaande VNet en subnetten**.
        1. Selecteer de **resourcegroep** waarin het virtuele netwerk.
        2. Kies de juiste **virtueel netwerk** naam die u implementeren wilt in;
        3. Selecteer de juiste **Subnet** waarden voor elk van de vereiste functieservices subnetten;
        4. Klik op **Volgende**

    ![App Service-installatie][5]

9. Geef de gegevens voor de bestandsshare en klik vervolgens op **volgende**. Het adres van de bestandsshare moet gebruiken voor de FQDN-naam of IP-adres van de bestandsserver. Bijvoorbeeld: \\\appservicefileserver.local.cloudapp.azurestack.external\websites, of \\\10.0.0.1\websites.

> [!NOTE]
> Het installatieprogramma probeert te testen van verbinding met de bestandsshare voordat u doorgaat.  Echter, als u wilt implementeren in een bestaand virtueel netwerk, het installatieprogramma mogelijk geen verbinding maken met de bestandsshare en een waarschuwing weergegeven waarin wordt gevraagd of u wilt doorgaan.  Controleer de bestandsshare en doorgaan als ze juist zijn.
>
>

   ![App Service-installatie][8]

10. Op de volgende pagina:
    1. In de **identiteit toepassings-ID** Voer de GUID voor de toepassing die u voor de identiteit (van Azure AD gebruikt).
    2. In de **Application Identity-certificaatbestand** vak (Typ of blader naar) de locatie van het certificaatbestand.
    3. In de **identiteit toepassing certificaatwachtwoord** Voer het wachtwoord voor het certificaat. Dit wachtwoord is die u genoteerd hebt als u het script gebruikt voor het maken van de certificaten.
    4. In de **Azure Resource Manager basiscertificaatbestand** vak (Typ of blader naar) de locatie van het certificaatbestand.
    5. Klik op **Volgende**.

    ![App Service-installatie][10]

11. Elk van de drie vakken bestand van het certificaat, klikt u op **Bladeren** en navigeer vervolgens naar het gewenste certificaatbestand. U moet het wachtwoord opgeven voor elk certificaat. Deze certificaten zijn degene die u hebt gemaakt in de [maken vereiste certificaten stap](azure-stack-app-service-before-you-get-started.md#get-certificates). Klik op **volgende** na het invoeren van alle informatie.

    | Box | Voorbeeld van de naam van een certificaat |
    | --- | --- |
    | **App Service standaard SSL-certificaat-bestand** | \_.appservice.local.AzureStack.external.pfx |
    | **API-App Service SSL-certificaatbestand** | api.appservice.local.AzureStack.external.pfx |
    | **App Service Publisher SSL-certificaat-bestand** | ftp.appservice.local.AzureStack.external.pfx |

    Als u een andere domeinachtervoegsel gebruikt bij het maken van de certificaten, de bestandsnamen van uw certificaat niet gebruikt *lokale. AzureStack.external*. In plaats daarvan gebruik de informatie van uw aangepaste domein.

    ![App Service-installatie][11]

12. Voer de SQL Server-gegevens voor het serverexemplaar waarmee de App Service resource provider-databases hosten en klik vervolgens op **volgende**. Het installatieprogramma valideert de eigenschappen van de SQL-verbinding.

> [!NOTE]
> Het installatieprogramma probeert te testen van verbinding met de SQl-Server voordat u doorgaat.  Echter, als u wilt implementeren in een bestaand virtueel netwerk, het installatieprogramma mogelijk geen verbinding maken met de SQL-Server en toont een waarschuwing waarin wordt gevraagd of u wilt doorgaan.  Controleer of de SQL Server-gegevens en doorgaan als ze juist zijn.
>
>
   
   ![App Service-installatie][12]

13. Bekijk de rolinstantie en SKU-opties. De standaardwaarden worden ingevuld met het minimum aantal exemplaar en de minimale SKU voor elke rol in de implementatie van een ASDK. Een overzicht van de vereisten voor vCPU en het geheugen is opgegeven bij het plannen van uw implementatie. Nadat u uw selecties maken, klikt u op **volgende**.

     > [!NOTE]
     > Volg de instructies in voor de productie-implementaties [planning van capaciteit voor Azure App Service-serverfuncties in Azure-Stack](azure-stack-app-service-capacity-planning.md).
     >
     >

    | Rol | Minimale exemplaren | Minimale SKU | Opmerkingen |
    | --- | --- | --- | --- |
    | Controller | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Beheert en onderhoudt de status van de cloud-App Service. |
    | Beheer | 1 | Standard_A2 - (2 vCPUs, 3584 MB) | Beheert de App Service Azure Resource Manager en API-eindpunten, portal-extensies (admin, tenant, Functions-portal) en de data-service. Ter ondersteuning van failover, vergroot u de aanbevolen exemplaren 2. |
    | Uitgever | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Inhoud via FTP en web deployment publiceert. |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Routes aanvragen voor App Service-toepassingen. |
    | Gedeelde Worker | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Hosts webtoepassingen of API en apps van Azure Functions. Mogelijk wilt meer exemplaren toe te voegen. Als operator kunt u definiëren van uw aanbod en eventuele SKU-categorie kiezen. De lagen moeten minimaal één vCPU hebben. |

    ![App Service-installatie][14]

    > [!NOTE]
    > **Windows Server 2016-Core, is geen ondersteund platforminstallatiekopie voor gebruik met Azure App Service op Azure-Stack.  Gebruik geen evaluatie installatiekopieën voor productie-implementaties.**

14. In de **Platforminstallatiekopie Selecteer** vak, kiest u de installatiekopie van uw implementatie van Windows Server 2016-virtuele machine die beschikbaar zijn in de compute resourceprovider voor de cloud-App Service. Klik op **Volgende**.

15. Op de volgende pagina:
     1. Voer de gebruikersnaam voor Worker-rol virtuele machine-beheerder en het wachtwoord.
     2. Voer de gebruikersnaam van de beheerder voor andere rollen virtuele machine en het wachtwoord.
     3. Klik op **Volgende**.

    ![App Service-installatie][16]

16. Op de pagina overzicht:
    1. Controleer of de selecties die u hebt aangebracht. Als u wilt wijzigen, gebruikt u de **vorige** knoppen naar vorige pagina's te gaan.
    2. Als de configuraties correct zijn, schakel het selectievakje in.
    3. Voor het starten van de implementatie, klikt u op **volgende**.

    ![App Service-installatie][17]

17. Op de volgende pagina:
    1. De installatievoortgang volgen. App-Service op Azure-Stack duurt ongeveer 60 minuten te implementeren op basis van de standaardselecties.
    2. Nadat het installatieprogramma met succes is voltooid, klikt u op **afsluiten**.

    ![App Service-installatie][18]

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
>

## <a name="test-drive-app-service-on-azure-stack"></a>App Service-station op Azure-Stack testen

Nadat u de App Service-resourceprovider registreren en implementeert, testen om ervoor te zorgen dat gebruikers web- en API apps kunnen implementeren.

> [!NOTE]
> U moet een aanbieding met de naamruimte Microsoft.Web binnen het plan maken. Vervolgens moet u een tenantabonnement hebt met deze aanbieding is geabonneerd. Zie voor meer informatie [maken aanbieding](azure-stack-create-offer.md) en [plan maken](azure-stack-create-plan.md).
>
U *moet* beschikt over een tenantabonnement te maken van toepassingen die gebruikmaken van App Service op Azure-Stack. De enige mogelijkheden die een servicebeheerder kan worden voltooid binnen de beheerportal zijn gerelateerd aan het beheer van de resource provider van App Service. Deze mogelijkheden zijn capaciteit toe te voegen, implementatie-bronnen configureren en lagen van de werknemer en SKU's toe te voegen.
>
Vanaf de derde technical preview functies voor het maken van web-API en Azure apps, moet u een tenantabonnement hebt en de tenantportal gebruiken.

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
[1]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-create-package.png
[2]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-complete-offline.png
[3]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-arm-endpoints.png
[4]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-subscription-information.png
[5]: ./media/azure-stack-app-service-deploy-offline/app-service-default-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config.png
[7]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config-with-values.png
[8]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration.png
[9]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration-error.png
[10]: ./media/azure-stack-app-service-deploy-offline/app-service-identity-app.png
[11]: ./media/azure-stack-app-service-deploy-offline/app-service-certificates.png
[12]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration.png
[13]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration-error.png
[14]: ./media/azure-stack-app-service-deploy-offline/app-service-cloud-quantities.png
[15]: ./media/azure-stack-app-service-deploy-offline/app-service-windows-image-selection.png
[16]: ./media/azure-stack-app-service-deploy-offline/app-service-role-credentials.png
[17]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-deployment-summary.png
[18]: ./media/azure-stack-app-service-deploy-offline/app-service-deployment-progress.png
