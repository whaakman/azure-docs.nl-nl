---
title: Beheren van een toepassing met Azure IoT Central | Microsoft Docs
description: Als een beheerder, het beheren van uw Azure IoT Central-toepassing
author: viv-liu
ms.author: viviali
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: ebca3060e902c0a45619c2bd64240537cd95308d
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55811848"
---
# <a name="administer-your-iot-central-application"></a>Uw IoT Central-toepassing beheren

Nadat u een IoT Central-toepassing hebt gemaakt, gaat u naar de **beheer** gedeelte:

- Toepassingsinstellingen beheren
- Gebruikers beheren
- Rollen beheren
- Uw factuur weergeven
- Uw evaluatieversie converteren naar betalen per gebruik
- Gegevens exporteren
- Apparaatverbinding beheren
- Toegangstokens gebruiken

Voor toegang tot en gebruik de **beheer** sectie die u moet zich in de **beheerder** rol voor een Azure IoT Central-toepassing. Als u een Azure IoT Central-toepassing maakt, wordt u automatisch toegewezen aan de **beheerder** rol voor de toepassing. De [gebruikers beheren](#manage-users) sectie in dit artikel wordt uitgelegd meer over het toewijzen de **beheerder** rol aan andere gebruikers.

## <a name="manage-application-settings"></a>Toepassingsinstellingen beheren

### <a name="change-application-name-and-url"></a>Naam van de toepassing wijzigen en de URL
In de **toepassingsinstellingen** pagina, kunt u de naam en de URL van uw toepassing wijzigen en vervolgens selecteert u **opslaan**.

![De instellingenpagina van toepassing](media/howto-administer-experimental/image0-a.png)

> [!Note]
> Als u de URL van uw wijzigt, kan de URL van uw oude kan worden uitgevoerd door een andere Azure IoT Central klant. Als dit gebeurt, is het niet meer beschikbaar voor gebruik. Wanneer u de URL van uw wijzigt, de oude URL is niet meer werkt en u moet op de hoogte stellen uw gebruikers over de nieuwe URL te gebruiken.

### <a name="prepare-and-upload-image"></a>Installatiekopie voorbereiden en uploaden
Als u wilt wijzigen van de installatiekopie van de toepassing, Zie [voorbereiden en uploaden van afbeeldingen aan uw Azure IoT Central toepassing](howto-prepare-images-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

### <a name="copy-an-application"></a>Kopiëren van een toepassing
U kunt een kopie van een toepassing, verminderd met elk apparaatexemplaren, de geschiedenis van apparaat en de gebruikersgegevens kunt maken. De kopie zich een betalen per gebruik-toepassing die u in rekening gebracht. U kunt een toepassing proefversie kan niet maken op deze manier.

Klik op de **kopie** knop. Geef de details voor de nieuwe betalen per gebruik-toepassing in het dialoogvenster. Klik vervolgens op de **kopie** knop om te bevestigen dat u wilt doorgaan. Meer informatie over de velden in dit formulier in [maken van een toepassing](quick-deploy-iot-central-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) Quick Start.

![De instellingenpagina van toepassing](media/howto-administer-experimental/appCopy2.png)

Nadat de app-kopieerbewerking is geslaagd, gaat u naar de nieuwe toepassing die is gemaakt door te kopiëren van uw toepassing met behulp van de koppeling die wordt weergegeven.

![De instellingenpagina van toepassing](media/howto-administer-experimental/appCopy3.png)

> [!Note]
> Kopiëren van een toepassing, kopieert ook de definitie van de regels en acties. Maar omdat gebruikers die toegang tot uw oorspronkelijke app hebben worden niet gekopieerd naar de gekopieerde app, hebt u handmatig gebruikers toevoegen aan acties zoals e-mailadres waarvoor gebruikers vereist zijn. In het algemeen is het een goed idee om te controleren of de regels en acties om ervoor te zorgen dat ze zijn bijgewerkt in de nieuwe app.

### <a name="delete-an-application"></a>Een toepassing verwijderen

> [!Note]
> Als u wilt verwijderen van een toepassing, moet u ook machtigingen om resources te verwijderen hebben in de Azure-abonnement u hebt gekozen tijdens het maken van de toepassing. Zie voor meer informatie, [op rollen gebaseerd toegangsbeheer gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

Gebruik de **verwijderen** knop om uw IoT Central-toepassing permanent te verwijderen. Dit doet, worden alle gegevens die is gekoppeld aan deze toepassing permanent verwijderd.

## <a name="manage-users"></a>Gebruikers beheren

### <a name="add-users"></a>Gebruikers toevoegen

Elke gebruiker moet een gebruikersaccount hebben voordat ze kunnen zich aanmelden en toegang een Azure IoT Central-toepassing tot. Microsoft-Accounts (MSA's) en Azure Active Directory (Azure AD)-accounts worden ondersteund in Azure IoT Central. Azure Active Directory-groepen worden momenteel niet ondersteund in Azure IoT Central.

Zie voor meer informatie, [help voor Microsoft-account](https://support.microsoft.com/products/microsoft-account?category=manage-account) en [Quick Start: Nieuwe gebruikers toevoegen aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory) als een testgebruiker zonder beheerdersbevoegdheden een wachtwoord heeft dat u kent, en u een gebruiker moet maken.

1. Als u wilt een gebruiker toevoegen aan een IoT Central-toepassing, gaat u naar de **gebruikers** pagina in de **beheer** sectie.

    ![Lijst met gebruikers](media/howto-administer-experimental/image1.png)

1. Toevoegen van een gebruiker op de **gebruikers** pagina, kies **+ gebruiker toevoegen**.

1. Kies een rol voor de gebruiker van de **rol** vervolgkeuzelijst. Meer informatie over functies in de [rollen beheren](#manage-roles) sectie van dit artikel.

    ![Een cloudrol kiezen](media/howto-administer-experimental/image3.png)

    > [!NOTE]
    >  Gebruikers bulksgewijs toevoegen, typt u de gebruikers-id van alle gebruikers die u wilt toevoegen van elkaar gescheiden door puntkomma's. Kies een rol van de **rol** vervolgkeuzelijst. Selecteer vervolgens **Opslaan**.

### <a name="edit-the-roles-that-are-assigned-to-users"></a>De functies die zijn toegewezen aan gebruikers bewerken

Rollen kunnen niet worden gewijzigd nadat ze zijn toegewezen. Als u wilt wijzigen van de rol die toegewezen aan een gebruiker, de gebruiker verwijderen en vervolgens de gebruiker opnieuw met een andere rol toevoegen.

### <a name="delete-users"></a>Gebruikers verwijderen

Als u wilt verwijderen van gebruikers, selecteert u een of meer selectievakjes in op de **gebruikers** pagina. Selecteer vervolgens **Verwijderen**.

## <a name="manage-roles"></a>Rollen beheren

Met functies kunt u om te bepalen wie binnen uw organisatie verschillende taken in IoT Central uitvoeren kunt. Er zijn drie rollen die u aan gebruikers van uw toepassing toewijzen kunt. 

### <a name="administrator"></a>Beheerder

Gebruikers in de **beheerder** rol hebben toegang tot alle functionaliteit in een toepassing.

De gebruiker die een toepassing maakt, wordt automatisch toegewezen aan de **beheerder** rol. Er moet altijd zijn ten minste één gebruiker in de **beheerder** rol.

### <a name="application-builder"></a>Application Builder

Gebruikers in de **toepassing Builder** rol kunt doen alles in een toepassing, behalve de toepassing beheren. Dit betekent dat builders kunnen maken, bewerken, en verwijderen van apparaatsjablonen en apparaten, Apparaatsets beheren en analyses en taken worden uitgevoerd. Builders geen toegang heeft tot de **beheer** sectie van de toepassing.

### <a name="application-operator"></a>Toepassingsoperator

Gebruikers in de **toepassing Operator** rol geen wijzigingen aanbrengen in apparaatsjablonen en de toepassing niet kan beheren. Dit betekent operators kunnen toevoegen en verwijderen van apparaten, Apparaatsets beheren en uitvoeren van analyses en taken. Operators geen toegang heeft tot de **toepassing Builder** en **beheer** pagina's.


## <a name="view-your-bill"></a>Uw factuur weergeven

Als u wilt uw factuur bekijken, gaat u naar de **facturering** pagina in de **beheer** sectie. De Azure-facturering pagina wordt geopend in een nieuw tabblad, waar u de factuur voor elk van uw Azure IoT Central toepassingen kunt zien.

### <a name="convert-your-trial-to-pay-as-you-go"></a>Uw evaluatieversie converteren naar betalen per gebruik

U kunt uw proefversie-toepassing naar een betalen per gebruik converteren. Dit zijn de verschillen tussen deze soorten toepassingen.

- **Proefversies** van toepassingen zijn zeven dagen gratis en verlopen daarna. Voordat ze verlopen, kunnen ze op elk gewenst moment worden geconverteerd naar betalen per gebruik.
- **Betalen per gebruik**-toepassingen worden per apparaat in rekening gebracht, waarbij de eerste vijf apparaten gratis zijn.

Lees meer over prijzen op de [prijzenpagina van IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).
    
Volg deze stappen voor het voltooien van deze Self-serviceproces:

1. Ga naar de **facturering** pagina in de **beheer** sectie. 

    ![De status van de proefversie](media/howto-administer-experimental/freetrialbilling.png)

1. Klik op **converteren naar betalen per gebruik**. 

    ![Evaluatieversie converteren](media/howto-administer-experimental/convert.png)

1. Selecteer de juiste Azure Active Directory, en vervolgens het Azure-abonnement moet worden gebruikt voor uw toepassing betalen per gebruik.

1. Nadat u op **converteren**, uw toepassing is nu een betalen per gebruik-toepassing en u wordt gefactureerd.

## <a name="export-data"></a>Gegevens exporteren

U kunt inschakelen **voortdurende gegevensexport** metingen, apparaten en apparaatgegevens sjablonen exporteren naar uw Azure Blob storage-account. Meer informatie over [hoe u uw gegevens te exporteren](#howto-export-data).

## <a name="manage-device-connection"></a>Apparaatverbinding beheren

Verbind apparaten op schaal in uw toepassing met behulp van de sleutels en certificaten hier. Meer informatie over [apparaten verbinden met](#concepts-connectivity).

## <a name="use-access-tokens"></a>Toegangstokens gebruiken

Toegangstokens voor het gebruik ervan in hulpprogramma's voor ontwikkelaars genereren. Er is momenteel een developer-programma beschikbaar waarmee de explorer IoT Central is voor het bewaken van apparaat-berichten en wijzigingen in propreties en instellingen. Meer informatie over de [IoT Central explorer](#howto-use-iotc-explorer). 

## <a name="use-the-azure-sdks-for-control-plane-operations"></a>Gebruik de Azure SDK's voor bewerkingen voor de controlelaag

IoT Central Azure Resource Manager SDK-pakketten zijn beschikbaar voor Node, Python, C#, Ruby, Java en Go. Deze bibliotheken ondersteuning voor bewerkingen voor de controlelaag voor IoT Central, waarmee u kunt maken, weergeven, bijwerken of verwijderen van IoT Central-toepassingen. Ze bieden ook hulpprogramma's voor het omgaan met verificatie en foutafhandeling die specifiek zijn voor elke taal is. 

Vindt u voorbeelden van hoe u met de Azure Resource Manager-SDK's op [ https://github.com/emgarten/iotcentral-arm-sdk-examples ](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Voor meer informatie, bekijk deze pakketten op GitHub.

| Taal | Opslagplaats | Pakket |
| ---------| ---------- | ------- |
| Knooppunt | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Aan de slag | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u uw Azure IoT Central toepassing beheert, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [De apparaat-sjabloon instellen](howto-set-up-template-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
