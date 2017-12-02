---
title: 'Implementeer de App-Services: Azure Stack | Microsoft Docs'
description: Gedetailleerde richtlijnen voor het implementeren van App Service in Azure-Stack
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: anwestg
ms.openlocfilehash: 522e5a334b5165344b66524d03f0d85468b81332
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="add-an-app-service-resource-provider-to-azure-stack"></a>Een App Service-resourceprovider toevoegen aan Azure-Stack

Als een Azure-Stack-cloud-operator, kunt u uw gebruikers bieden de mogelijkheid voor het maken van websites en toepassingen van de API. Om dit te doen, moet u eerst toevoegen de [App Service-resourceprovider](azure-stack-app-service-overview.md) voor uw Azure-Stack-implementatie zoals beschreven in dit artikel. Nadat u de App Service resourceprovider hebt geïnstalleerd, kunt u deze opnemen in uw aanbiedingen en plannen. Gebruikers kunnen zich vervolgens aanmelden om de service en beginnen met het maken van toepassingen.

> [!IMPORTANT]
> Voordat u het installatieprogramma uitvoert, zorg ervoor dat u de instructies in hebt gevolgd [voordat u aan de slag](azure-stack-app-service-before-you-get-started.md).
> 
>



## <a name="run-the-app-service-resource-provider-installer"></a>Voer het App Service resource provider-installatieprogramma

De App Service-resourceprovider te installeren in uw Azure-Stack-omgeving kan een uur duren. Tijdens dit proces wordt het installatieprogramma:

* Maak een blob-container in het opgegeven Azure-Stack-opslagaccount.
* Maak een DNS-zone en vermeldingen voor App Service.
* Registreer de provider van de bron-App Service.
* Registreer de App Service-galerij-items.

Volg deze stappen voor het implementeren van App Service-resourceprovider:

1. Appservice.exe worden uitgevoerd als een beheerder (azurestack\CloudAdmin).

2. Klik op **App Service implementeren in uw Azure-Stack cloud**.

    ![App Service-installatie](media/azure-stack-app-service-deploy/image01.png)

3. Controleer en accepteer de licentievoorwaarden voor Microsoft-Software en klik vervolgens op **volgende**.

4. Controleer en accepteer de licentievoorwaarden van derden en klik vervolgens op **volgende**.

5. Zorg ervoor dat de App Service-cloud-configuratie-informatie juist is. Als u de standaardinstellingen tijdens de implementatie van Azure Stack Development Kit gebruikt, kunt u hier de standaardwaarden accepteren. Als u de opties aangepast wanneer u Azure-Stack geïmplementeerd, moet u de waarden in dit venster om weer te geven die bewerken. Als u het domein achtervoegsel mycloud.com gebruikt, kunt u voor uw eindpunt, moet wijzigen naar management.mycloud.com. Nadat u uw gegevens bevestigen, klikt u op **volgende**.

    ![App Service-installatie](media/azure-stack-app-service-deploy/image02.png)

6. Op de volgende pagina:
    1. Klik op de **Connect** naast de **Stack-Azure-abonnementen** vak.
        - Als u gebruikmaakt van Azure Active Directory (Azure AD), voert u de Azure AD-beheerdersaccount en het wachtwoord die u hebt opgegeven tijdens de implementatie van Azure-Stack. Klik op **aanmelden**.
        - Als u Active Directory Federation Services (AD FS), Geef uw beheerdersaccount. Bijvoorbeeld cloudadmin@azurestack.local. Voer uw wachtwoord in en klikt u op **aanmelden**.
    2. In de **Stack-Azure-abonnementen** Selecteer uw abonnement.
    3. In de **Azure stacklocaties** Selecteer de locatie die overeenkomt met de regio die u implementeert op. Selecteer bijvoorbeeld **lokale** als uw implementatie van de Azure-Stack Development Kit.
    4. Voer een **Resourcegroepnaam** voor uw App Service-implementatie. Standaard ingesteld op **APPSERVICE\<regio\>**.
    5. Voer de **Opslagaccountnaam** dat u wilt maken als onderdeel van de installatie-App Service. Standaard ingesteld op **appsvclocalstor**.
    6. Klik op **Volgende**.

    ![App Service-installatie](media/azure-stack-app-service-deploy/image03.png)

7. Geef de gegevens voor de bestandsshare en klik vervolgens op **volgende**. Het adres van de bestandsshare moet gebruiken de volledig gekwalificeerde domeinnaam van uw bestandsserver, bijvoorbeeld \\\appservicefileserver.local.cloudapp.azurestack.external\websites of het IP-adres, bijvoorbeeld \\\10.0.0.1\websites.

    ![App Service-installatie](media/azure-stack-app-service-deploy/image04.png)

8. Op de volgende pagina:
    1. In de **identiteit toepassings-ID** Voer de GUID voor de toepassing die u voor de identiteit (van Azure AD gebruikt).
    2. In de **Application Identity-certificaatbestand** vak (Typ of blader naar) de locatie van het certificaatbestand.
    3. In de **identiteit toepassing certificaatwachtwoord** Voer het wachtwoord voor het certificaat. Dit wachtwoord is die u genoteerd hebt als u het script gebruikt voor het maken van de certificaten.
    4. In de **Azure Resource Manager basiscertificaatbestand** vak (Typ of blader naar) de locatie van het certificaatbestand.
    5. Klik op **Volgende**.

    ![App Service-installatie](media/azure-stack-app-service-deploy/image05.png)

9. Elk van de drie vakken bestand van het certificaat, klikt u op **Bladeren** en navigeer naar het gewenste certificaatbestand. U moet het wachtwoord opgeven voor elk certificaat. Deze certificaten zijn degene die u hebt gemaakt in de [maken vereiste certificaten stap](azure-stack-app-service-deploy.md#create-the-required-certificates). Klik op **volgende** na het invoeren van alle informatie.

    | Box | Voorbeeld van de naam van een certificaat |
    | --- | --- |
    | **App Service standaard SSL-certificaat-bestand** | \_. appservice.local.AzureStack.external.pfx |
    | **API-App Service SSL-certificaatbestand** | api.appservice.local.AzureStack.external.pfx |
    | **App Service Publisher SSL-certificaat-bestand** | ftp.appservice.local.AzureStack.external.pfx |

    Als u een andere domeinachtervoegsel gebruikt bij het maken van de certificaten, de bestandsnamen van uw certificaat niet gebruikt *lokale. AzureStack.external*. In plaats daarvan gebruik de informatie van uw aangepaste domein.

    ![App Service-installatie](media/azure-stack-app-service-deploy/image06.png)    

10. Voer de SQL Server-gegevens voor het serverexemplaar waarmee de App Service resource provider-databases hosten en klik vervolgens op **volgende**. Het installatieprogramma valideert de eigenschappen van de SQL-verbinding.

    ![App Service-installatie](media/azure-stack-app-service-deploy/image07.png)    

11. Bekijk de rolinstantie en SKU-opties. De standaardwaarden worden ingevuld met het minimum aantal exemplaar en de minimale SKU voor elke rol in de implementatie van een ASDK. Een overzicht van de vereisten voor vCPU en het geheugen is opgegeven bij het plannen van uw implementatie. Nadat u uw selecties maken, klikt u op **volgende**.

    > [!NOTE]
    > Voor implementaties in de instructies in het volgende [planning van capaciteit voor Azure App Service-serverfuncties in Azure-Stack](azure-stack-app-service-capacity-planning.md).
    > 
    >

    | Rol | Minimale exemplaren | Minimale SKU | Opmerkingen |
    | --- | --- | --- | --- |
    | Controller | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Beheert en onderhoudt de status van de cloud-App Service. |
    | Beheer | 1 | Standard_A2 - (2 Vcpu, 3584 MB) | Beheert de App Service Azure Resource Manager en API-eindpunten, portal-extensies (admin, tenant, Functions-portal) en de data-service. Ter ondersteuning van failover, vergroot u de aanbevolen exemplaren 2. |
    | Uitgever | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Inhoud via FTP en web deployment publiceert. |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Routes aanvragen voor App Service-toepassingen. |
    | Gedeelde Worker | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Hosts webtoepassingen of API en apps van Azure Functions. Mogelijk wilt meer exemplaren toe te voegen. Als operator kunt u definiëren van uw aanbod en eventuele SKU-categorie kiezen. De lagen moeten minimaal één vCPU hebben. |

    ![App Service-installatie](media/azure-stack-app-service-deploy/image08.png)    

    > [!NOTE]
    > **Windows Server 2016-Core, is geen ondersteund platforminstallatiekopie voor gebruik met Azure App Service op Azure-Stack**.

12. In de **Platforminstallatiekopie Selecteer** vak, kiest u de installatiekopie van uw implementatie van Windows Server 2016-virtuele machine die beschikbaar zijn in de compute resourceprovider voor de cloud-App Service. Klik op **Volgende**.

13. Op de volgende pagina:
     1. Voer de gebruikersnaam voor Worker-rol virtuele machine-beheerder en het wachtwoord.
     2. Voer de gebruikersnaam van de beheerder voor andere rollen virtuele machine en het wachtwoord.
     3. Klik op **Volgende**.

    ![App Service-installatie](media/azure-stack-app-service-deploy/image09.png)    

14. Op de pagina overzicht:
    1. Controleer of de selecties die u hebt aangebracht. Als u wilt wijzigen, gebruikt u de **vorige** knoppen naar vorige pagina's te gaan.
    2. Als de configuraties correct zijn, schakel het selectievakje in.
    3. Voor het starten van de implementatie, klikt u op **volgende**.

    ![App Service-installatie](media/azure-stack-app-service-deploy/image10.png)    

15. Op de volgende pagina:
    1. De installatievoortgang volgen. App-Service op Azure-Stack duurt ongeveer 60 minuten te implementeren op basis van de standaardselecties.
    2. Nadat het installatieprogramma met succes is voltooid, klikt u op **afsluiten**.

    ![App Service-installatie](media/azure-stack-app-service-deploy/image11.png)    


## <a name="validate-the-app-service-on-azure-stack-installation"></a>De App-Service op Azure-Stack installatie valideren

1. Ga in de Stack van Azure-beheerportal naar **Administration - App Service**.

2. Schakel in het overzicht onder status om te zien die de **Status** toont **alle rollen gereed zijn**.

    ![App Service-beheer](media/azure-stack-app-service-deploy/image12.png)    

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

1. Klik in de tenantportal Azure Stack op  **+** , gaat u naar de Azure Marketplace een Django-website te implementeren en wacht met succes is voltooid. Een bestand gebaseerde database maakt gebruik van het Django webplatform. Eventuele extra resourceproviders, zoals SQL of MySQL nodig niet is.

2. Als u een MySQL-resourceprovider ook hebt geïmplementeerd, kunt u een WordPress-website van de Marketplace kunt implementeren. Wanneer u wordt gevraagd om de databaseparameters, voert u de naam van de gebruiker als  *User1@Server1* , met de gebruikersnaam en de naam van uw keuze.

3. Als u een SQL Server-resourceprovider ook hebt geïmplementeerd, kunt u een website DNN vanuit de Marketplace kunt implementeren. Wanneer u wordt gevraagd om de databaseparameters, kiest u een database in de computer met SQL Server die verbonden met uw resourceprovider.

## <a name="next-steps"></a>Volgende stappen

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md).

- [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md)
- [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
