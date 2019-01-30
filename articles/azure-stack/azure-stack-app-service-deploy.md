---
title: 'Implementeer App-Services: Azure Stack | Microsoft Docs'
description: Gedetailleerde richtlijnen voor het implementeren van App Service in Azure Stack
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
ms.date: 01/11/2019
ms.author: anwestg
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: c3d120b8d124ce8c5536365e25bf8ffa500d4cbb
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251161"
---
# <a name="add-an-app-service-resource-provider-to-azure-stack"></a>Een App Service-resourceprovider toevoegen aan Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Gebruik de richtlijnen in dit artikel om App Service in Azure Stack te implementeren.

> [!IMPORTANT]  
> De update 1809 toepassen op uw geïntegreerde Azure Stack-systeem of implementeren van de meest recente Azure Stack Development Kit (ASDK) voordat u Azure App Service 1.4 implementeert.

U kunt uw gebruikers de mogelijkheid om te maken van web- en API-toepassingen geven. Als u wilt dat gebruikers deze toepassingen maken, moet u naar:

 - Voeg de [App Service-resourceprovider](azure-stack-app-service-overview.md) voor uw Azure Stack-implementatie met behulp van de stappen in dit artikel beschreven.
 - Nadat u de App Service-resourceprovider hebt geïnstalleerd, kunt u deze kunt opnemen in uw aanbiedingen en plannen. Gebruikers kunnen zich vervolgens aanmelden om de service en -toepassingen maken.

> [!IMPORTANT]  
> Voordat u de resource provider-installatieprogramma uitvoert, zorgt u ervoor dat u de instructies in hebt gevolgd [voordat u aan de slag](azure-stack-app-service-before-you-get-started.md).

## <a name="run-the-app-service-resource-provider-installer"></a>Voer het installatieprogramma van App Service resource provider

Installeren van de App Service-resourceprovider is ten minste een uur. De lengte van de tijd die nodig is, is afhankelijk van op de rol van hoeveel u exemplaren implementeren. Het installatieprogramma uitgevoerd tijdens de implementatie van de volgende taken:

 - Maak een blobcontainer in het opgegeven Azure Stack-opslagaccount.
 - Maak een DNS-zone en vermeldingen voor App Service.
 - Registreer de resourceprovider App Service.
 - Registreer het galerie-items van het App Service.

 > [!IMPORTANT]
 > Controleer voordat u de resourceprovider implementeert, de opmerkingen bij de release voor meer informatie over nieuwe functionaliteit, correcties en bekende problemen die invloed kunnen zijn op uw implementatie.

Volg deze stappen voor het implementeren van App Service-resourceprovider:

1. Appservice.exe als beheerder uitvoeren vanaf een computer die toegang hebben tot de Azure Stack Admin Azure Resource Management-eindpunt.

2. Selecteer **of upgrade naar de nieuwste versie van de App Service implementeren**.

    ![App Service-installatieprogramma][1]

3. Controleer en accepteer de licentievoorwaarden voor Microsoft-Software en selecteer vervolgens **volgende**.

4. Controleer en accepteer de licentievoorwaarden van derden en selecteer vervolgens **volgende**.

5. Zorg ervoor dat de App Service-cloud-configuratie-informatie juist is. Als u de standaardinstellingen gebruikt tijdens de implementatie van Azure Stack Development Kit (ASDK), kunt u de standaardwaarden accepteren. Maar als u de opties aangepast wanneer u de ASDK geïmplementeerd, of op een geïntegreerde Azure Stack-systeem implementeert, moet u de waarden in dit venster in overeenstemming met de verschillen bewerken.

   Als u de domein-achtervoegsel mycloud.com gebruikt, kunt u voor uw Azure Stack Tenant Azure Resource Manager-eindpunt, moet wijzigen voor beheer. &lt;regio&gt;. mycloud.com. Controleer deze instellingen en selecteer vervolgens **volgende** de instellingen op te slaan.

   ![App Service-installatieprogramma][2]

6. Voer de volgende stappen uit op de volgende pagina van de App Service-installatieprogramma:

    a. Selecteer **Connect** naast de **Azure Stack-abonnementen**.

     - Als u Azure Active Directory (Azure AD), voert u de Azure AD-beheerdersaccount en het wachtwoord die u hebt opgegeven tijdens de implementatie van Azure Stack. Selecteer **aanmelden**.
     - Als u Active Directory Federation Services (AD FS), Geef uw beheerdersaccount. Bijvoorbeeld cloudadmin@azurestack.local. Voer uw wachtwoord in en selecteer vervolgens **aanmelden**.

   b. In **Azure Stack-abonnementen**, selecteer de **Providerabonnement standaard**.

     > [!IMPORTANT]  
     > App Service **moet** worden geïmplementeerd op de **Providerabonnement standaard**.

   c. In de **Azure Stack-locaties**, selecteert u de locatie die overeenkomt met de regio die u naar implementeert. Selecteer bijvoorbeeld **lokale** als uw implementatie van de Azure Stack Development Kit.

    ![App Service-installatieprogramma][3]

7. Nu kunt u implementeren in een bestaand virtueel netwerk die u hebt geconfigureerd [met de volgende stappen](azure-stack-app-service-before-you-get-started.md#virtual-network), of laat het App Service-installatieprogramma maakt een nieuw virtueel netwerk en subnetten. Volg deze stappen voor het maken van een VNet:

   a. Selecteer **VNet maken met standaardinstellingen**, accepteer de standaardwaarden en selecteer vervolgens **volgende**.

   b. U kunt ook selecteren **bestaande VNet en subnetten gebruiken**. De volgende acties uitvoeren:

     - Selecteer de **resourcegroep** die uw Virtueelnetwerk bevat.
     - Kies de **Virtueelnetwerk** naam die u implementeren wilt op.
     - Selecteer de juiste **Subnet** waarden voor elk van de subnetten van de vereiste rol.
     - Selecteer **Volgende**.

   ![App Service-installatieprogramma][4]

8. Voer de gegevens voor de bestandsshare en selecteer vervolgens **volgende**. Het adres van de bestandsshare moet gebruiken voor het volledig FULLY Qualified Domain Name () of het IP-adres van de bestandsserver. Bijvoorbeeld, \\\appservicefileserver.local.cloudapp.azurestack.external\websites, of \\\10.0.0.1\websites.

   >[!NOTE]
   >Het installatieprogramma wil test de verbinding met de bestandsshare voordat u doorgaat. Maar als u naar een bestaand virtueel netwerk implementeert, deze Connectiviteitstest mislukken. Krijgt u een waarschuwing en een prompt om door te gaan. Als de gegevens van bestandsshare juist is, blijven de implementatie.

   ![App Service-installatieprogramma][7]

9. Voer de volgende stappen uit op de volgende pagina van de App Service-installatieprogramma:

   a. In de **Identiteitstoepassings-ID** voert u de GUID voor de toepassing die u voor de identiteit (van Azure AD).

   b. In de **Identiteitstoepassing certificaatbestand** vak, typ (of blader naar) de locatie van het certificaatbestand.

   c. In de **Identiteitstoepassing certificaatwachtwoord** vak, voer het wachtwoord voor het certificaat. Dit wachtwoord is degene die u hebt genoteerd als u het script gebruikt voor het maken van de certificaten.

   d. In de **basiscertificaatbestand voor Azure Resource Manager** vak, typ (of blader naar) de locatie van het certificaatbestand.

   e. Selecteer **Volgende**.

   ![App Service-installatieprogramma][9]

10. Selecteer voor elk van de drie vakken **Bladeren** en Ga naar het juiste certificaatbestand. U moet het wachtwoord opgeven voor elk certificaat. Deze certificaten zijn degene die u hebt gemaakt in de [maken vereiste certificaten stap](azure-stack-app-service-before-you-get-started.md#get-certificates). Selecteer **volgende** na het invoeren van alle informatie.

    | Box | Voorbeeld van de naam van certificaat |
    | --- | --- |
    | **Standaard SSL-certificaatbestand van App Service** | \_.appservice.local.AzureStack.external.pfx |
    | **SSL-certificaatbestand voor App Service API** | api.appservice.local.AzureStack.external.pfx |
    | **App Service Publisher SSL-certificaatbestand** | ftp.appservice.local.AzureStack.external.pfx |

    Als u een andere domeinachtervoegsel gebruikt tijdens het maken van de certificaten, de bestandsnamen van uw certificaat niet gebruikt *lokale. AzureStack.external*. In plaats daarvan gebruik de informatie van uw aangepaste domein.

    ![App Service-installatieprogramma][10]

11. Geef de SQL Server-details voor de server-exemplaar dat is gebruikt voor het hosten van de App Service resource provider-databases en selecteer vervolgens **volgende**. Het installatieprogramma evalueert de eigenschappen van de SQL-verbinding.

    > [!NOTE]
    > Het installatieprogramma wil test de verbinding met de SQL-Server voordat u doorgaat. Maar als u naar een bestaand virtueel netwerk implementeert, deze Connectiviteitstest mislukken. Krijgt u een waarschuwing en een prompt om door te gaan. Als de informatie over SQL Server juist is, blijven de implementatie.
    >
    > Het installatieprogramma wordt gecontroleerd dat de SQL-Server ingeschakeld op het niveau van de SQL Server database-containment is van Azure App Service op Azure Stack 1.3 en hoger.  Als dat niet het geval is, wordt u gevraagd met de volgende uitzondering:
    > ```sql
    >    Enable contained database authentication for SQL server by running below command on SQL server (Ctrl+C to copy)
    >    ***********************************************************
    >    sp_configure 'contained database authentication', 1;  
    >    GO  
    >    RECONFIGURE;  
    >    GO
    >    ***********************************************************
    > ```
    > Raadpleeg de [opmerkingen bij de release voor Azure App Service op Azure Stack 1.3](azure-stack-app-service-release-notes-update-three.md) voor meer informatie.

    ![App Service-installatieprogramma][11]

12. Bekijk de rolinstantie en SKU-opties. De standaardinstellingen te vullen met het minimum aantal exemplaren en de minimale SKU voor elke rol in een ASDK-implementatie. Een overzicht van de vereisten voor vCPU en het geheugen wordt geboden om u te helpen bij uw implementatie plannen. Nadat u uw selecties hebt uitgevoerd, selecteert u **volgende**.

    >[!NOTE]
    >Voor productie-implementaties, de instructies in het volgende [planning van capaciteit voor Azure App Service-server-rollen in Azure Stack](azure-stack-app-service-capacity-planning.md).

    | Rol | Minimale exemplaren | Minimale SKU | Opmerkingen |
    | --- | --- | --- | --- |
    | Controller | 1 | Standard_A2 - (2 vCPU, 3584 MB) | Beheert en onderhoudt de status van de App Service-cloud. |
    | Beheer | 1 | Standard_A2 - (2 vCPUs, 3584 MB) | De App Service Azure Resource Manager en API-eindpunten, portal-extensies (admin, tenant, Functions-portal) en de gegevensservice beheert. Ter ondersteuning van failover, vergroot u de aanbevolen 2-exemplaren. |
    | Uitgever | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Inhoud via FTP en web deployment publiceert. |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Stuurt aanvragen door naar App Service-toepassingen. |
    | Gedeelde Werkrol | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Hosts web of API-Apps en Azure Functions-apps. Mogelijk wilt u meer instanties toevoegen. Als operator kunt u definiëren van uw aanbieding en het kiezen van een SKU-laag. De lagen moeten minimaal één vCPU hebben. |

    ![App Service-installatieprogramma][13]

    >[!NOTE]
    >**Windows Server 2016 Core is niet een ondersteund platform-installatiekopie voor gebruik met Azure App Service in Azure Stack.  Gebruik geen evaluatie installatiekopieën voor productie-implementaties.**

13. In de **Platforminstallatiekopie selecteren** vak, kiest u de installatiekopie van uw implementatie van Windows Server 2016 virtuele machine via de installatiekopieën die beschikbaar zijn in de resourceprovider voor compute voor de App Service-cloud. Selecteer **Volgende**.

14. Voer de volgende stappen uit op de volgende pagina van de App Service-installatieprogramma:

     a. Voer de gebruikersnaam voor Worker-rol virtuele machine-beheerder en het wachtwoord.

     b. Voer de gebruikersnaam van beheerder van overige rollen virtuele machine en het wachtwoord.

     c. Selecteer **Volgende**.

    ![App Service-installatieprogramma][15]

15. Voer de volgende stappen uit op de overzichtspagina van App Service-installatieprogramma:

    a. Controleer of de selecties die u hebt gemaakt. Als u wilt wijzigen, gebruikt u de **vorige** knoppen om naar de vorige pagina's te gaan.

    b. Als de configuratie juist zijn, selecteert u het selectievakje in.

    c. Selecteer eerst de implementatie **volgende**.

    ![App Service-installatieprogramma][16]

16. Voer de volgende stappen uit op de volgende pagina van de App Service-installatieprogramma:

    a. De van de installatievoortgang bijhouden. App Service in Azure Stack duurt ongeveer 60 minuten om te implementeren op basis van de standaardselecties.

    b. Nadat het installatieprogramma met succes is voltooid, selecteert u **afsluiten**.

    ![App Service-installatieprogramma][17]

## <a name="validate-the-app-service-on-azure-stack-installation"></a>Valideren van de App-Service op Azure Stack-installatie

1. In de Azure Stack-beheerportal, gaat u naar **beheer - App Service**.

2. Schakel in het overzicht, onder status om te zien die de **Status** weergegeven **alle rollen zijn gereed**.

    ![App Service-beheer](media/azure-stack-app-service-deploy/image12.png)

    Als u naar een bestaand virtueel netwerk implementeert en met behulp van een interne IP-adres verbinding maken met de bestandsserver, moet u een uitgaande beveiligingsregel toevoegen. Deze regel kunnen SMB-verkeer tussen het worker-subnet en de bestandsserver.  Om dit te doen, gaat u naar de WorkersNsg in de beheerportal en voeg een uitgaande beveiligingsregel met de volgende eigenschappen toe:

    - Bron: Alle
    - Poortbereik van bron: *
    - Bestemming: IP-adressen
    - Doel-IP-adresbereik: Bereik van IP-adressen voor uw bestandsserver
    - Poortbereik van doel: 445
    - Protocol: TCP
    - Actie: Toestaan
    - Prioriteit: 700
    - Naam: Outbound_Allow_SMB445

## <a name="test-drive-app-service-on-azure-stack"></a>Probeer de App Service in Azure Stack

Nadat u implementeert en registreer de resourceprovider App Service, het testen om ervoor te zorgen dat gebruikers web-apps en API apps kunnen implementeren.

>[!NOTE]
>U moet maken van een aanbieding met de naamruimte Microsoft.Web in het abonnement. U moet ook een tenantabonnement dat u zich op de aanbieding abonneert. Zie voor meer informatie, [maken aanbieding](azure-stack-create-offer.md) en [plan maken](azure-stack-create-plan.md).
>
>U *moet* een tenantabonnement te maken van toepassingen die gebruikmaken van App Service in Azure Stack hebt. Alleen de volgende taken die een servicebeheerder in de beheerportal uitvoeren kunt zijn met betrekking tot het beheer van de resource provider van App Service. Dit omvat het toevoegen van capaciteit, implementatiebronnen configureren en werkrolniveaus en SKU's toe te voegen.
>
>Voor het maken van web-API en Azure Functions apps, moet u de tenantportal te gebruiken en een tenantabonnement hebt.
>

Volg deze stappen voor het maken van een web-app testen:

1. Selecteer in de gebruikersportal van Azure Stack **+ een resource maken** > **Web en mobiel** > **Web-App**.

2. Onder **Web-App**, voer een naam in **Web-app**.

3. Onder **resourcegroep**, selecteer **nieuw**. Voer een naam voor de **resourcegroep**.

4. Selecteer **App Service-plan /-locatie** > **nieuw**.

5. Onder **App Service-plan**, voer een naam voor de **App Service-plan**.

6. Selecteer **prijscategorie** > **gratis gedeelde** of **Shared-gedeelde** > **selecteren**  >  **OK** > **maken**.

7. Een tegel voor de nieuwe web-app wordt weergegeven op het dashboard. Selecteer de tegel.

8. Op **Web-App**, selecteer **Bladeren** om de standaardwebsite voor deze app weer te geven.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>Een WordPress, DNN of Django-website (optioneel) implementeren

1. Selecteer in de portal voor Azure Stack-tenant **+**, gaat u naar de Azure Marketplace, een Django-website kunt implementeren en wacht totdat de implementatie is voltooid. Een bestand system gebaseerde database maakt gebruik van het Django webplatform. Er is aanvullende resourceproviders, zoals SQL- of MySQL geen vereist.

2. Als u ook een MySQL-resourceprovider hebt geïmplementeerd, kunt u een WordPress-website vanuit de Marketplace kunt implementeren. Wanneer u wordt gevraagd voor databaseparameters, voert u de naam van de gebruiker als *User1@Server1*, met de gebruikersnaam en de naam van de server van uw keuze.

3. Als u ook een SQL Server-resourceprovider hebt geïmplementeerd, kunt u een website met DNN vanuit de Marketplace kunt implementeren. Wanneer u wordt gevraagd voor databaseparameters, kiest u een database in de computer met SQL Server die verbonden met de resourceprovider.

## <a name="next-steps"></a>Volgende stappen

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md).

 - [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md)
 - [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: https://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: https://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: https://go.microsoft.com/fwlink/?LinkId=733525

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
