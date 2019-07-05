---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/27/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: cae108a1d4226e8c0fe39f9cd1cedc1e6a024ffc
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465455"
---
## <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Meld u aan bij Azure Portal op https://portal.azure.com.

> [!NOTE]
> Als u geregistreerd voor het gebruik van gedeelde Afbeeldingsgalerieën tijdens de Preview-versie, moet u mogelijk opnieuw registreren de `Microsoft.Compute` provider. Open [Cloud Shell](https://shell.azure.com/bash) en typ: `az provider register -n Microsoft.Compute`

## <a name="create-an-image-gallery"></a>Een galerie met installatiekopieën maken

Een galerie met installatiekopieën is de primaire bron die wordt gebruikt voor het inschakelen van de installatiekopie van het delen. Toegestane tekens voor de naam van de galerie worden hoofdletters of kleine letters, cijfers, punten en perioden. Naam van de galerie mag geen streepjes bevatten.  Galerie-namen moeten uniek zijn binnen uw abonnement. 

Het volgende voorbeeld wordt een galerie met de naam *myGallery* in de *myGalleryRG* resourcegroep.

1. Selecteer in de linkerbovenhoek van Azure Portal **Een resource maken**.
1. Gebruik van het type **gedeelde afbeeldingengalerie** in het zoekvak en selecteer **gedeelde afbeeldingengalerie** in de resultaten.
1. In de **gedeelde afbeeldingengalerie** pagina, klikt u op **maken**.
1. Selecteer het juiste abonnement.
1. In **resourcegroep**, selecteer **nieuw** en het type *myGalleryRG* voor de naam.
1. In **naam**, type *myGallery* voor de naam van de galerie.
1. Laat de standaardwaarde voor **regio**.
1. U kunt een korte beschrijving van de galerie, zoals typen *mijn galerie met installatiekopieën voor het testen.* en klik vervolgens op **revisie + maken**.
1. Nadat de validatie is geslaagd, schakelt u **maken**.
1. Wanneer de implementatie is voltooid, selecteert u **naar de resource gaan**.
   
## <a name="create-an-image-definition"></a>De definitie van een installatiekopie maken 

Definities van de installatiekopie van maken een logische groepering van installatiekopieën. Ze worden gebruikt voor het beheren van informatie over de versies van een installatiekopie die in deze worden gemaakt. Namen van de definitie van afbeeldingen kunnen bestaan uit hoofdletters of kleine letters, cijfers, punten, streepjes en punten. Zie voor meer informatie over de waarden die u voor de definitie van een installatiekopie opgeven kunt [Image definities](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

De definitie van de galerie installatiekopie binnen uw galerie maken. In dit voorbeeld wordt de afbeelding met de naam *myImageDefinition*.

1. Selecteer op de pagina voor uw nieuwe galerie met installatiekopieën **toevoegen van de definitie van een nieuwe installatiekopie** vanaf de bovenkant van de pagina. 
1. Voor **installatiekopie Definitienaam**, type *myImageDefinition*.
1. Voor **besturingssysteem**, selecteert u de juiste optie op basis van de broninstallatiekopie.
1. Voor **Publisher**, type *myPublisher*. 
1. Voor **bieden**, type *myOffer*.
1. Voor **SKU**, type *mySKU*.
1. Wanneer u klaar bent, selecteert u **revisie + maken**.
1. Nadat de definitie van de installatiekopie is gevalideerd, schakelt u **maken**.
1. Wanneer de implementatie is voltooid, selecteert u **naar de resource gaan**.


## <a name="create-an-image-version"></a>De versie van een installatiekopie maken

De versie van een installatiekopie van een beheerde installatiekopie maken. In dit voorbeeld de versie van de installatiekopie is *1.0.0* en deze worden gerepliceerd naar beide *West-Centraal VS* en *Zuid-centraal VS* datacenters. Bij het kiezen van doelregio's voor replicatie, houd er rekening mee dat u ook hebt om op te nemen de *bron* regio bevinden als een doel voor replicatie.

Toegestane tekens in voor de versie van installatiekopie zijn cijfers en punten. Cijfers moet binnen het bereik van een 32-bits geheel getal zijn. Indeling: *MajorVersion*.*MinorVersion*.*Patch*.

1. Selecteer op de pagina voor de definitie van uw installatiekopie **toevoegen versie** vanaf de bovenkant van de pagina.
1. In **regio**, selecteer de regio waarin uw beheerde installatiekopie wordt opgeslagen. Versies van een installatiekopie moeten worden gemaakt in dezelfde regio als de installatiekopie van het beheerde die ze zijn gemaakt op basis van.
1. Voor **naam**, type *1.0.0*. De naam van de installatiekopie-versie moet volgen *belangrijke*. *secundaire*. *patch* opmaken met gehele getallen. 
1. In **broninstallatiekopie**, selecteert u uw beheerde broninstallatiekopie in de vervolgkeuzelijst.
1. In **uitsluiten van de meest recente**, laat de standaardwaarde van *Nee*.
1. Voor **leven datum**, selecteer een datum in de kalender die nog een paar maanden in de toekomst.
1. In **replicatie**, laat de **aantal replica's standaard** als 1. U wilt repliceren naar de regio van de gegevensbron, dus laat de eerste replica als de standaardwaarde en kies vervolgens een tweede regio replica moet *VS-Oost*.
1. Wanneer u klaar bent, selecteert u **revisie + maken**. Azure valideert de configuratie.
1. Wanneer de versie van installatiekopie de validatietests doorstaat, selecteert u **maken**.
1. Wanneer de implementatie is voltooid, selecteert u **naar de resource gaan**.

Duurt het even voor het repliceren van de installatiekopie naar alle van de doelregio's.

## <a name="share-the-gallery"></a>Delen van de galerie

Het is raadzaam dat u toegang tot op het niveau van de galerie installatiekopie delen. De volgende begeleidt u bij het delen van de galerie die u zojuist hebt gemaakt.

1. Open de [Azure Portal](https://portal.azure.com).
1. Selecteer in het menu aan de linkerkant, **resourcegroepen**. 
1. Selecteer in de lijst met resourcegroepen, **myGalleryRG**. De blade voor de resourcegroep wordt geopend.
1. In het menu aan de linkerkant van de **myGalleryRG** weergeeft, schakelt **toegangsbeheer (IAM)** . 
1. Onder **een roltoewijzing toevoegen**, selecteer **toevoegen**. De **een roltoewijzing toevoegen** deelvenster wordt geopend. 
1. Onder **rol**, selecteer **lezer**.
1. Onder **toegang toewijzen aan**, laat de standaardwaarde **Azure AD-gebruiker, groep of service-principal**.
1. Onder **Selecteer**, typt u het e-mailadres van de persoon die u wilt uitnodigen.
1. Als de gebruiker zich buiten uw organisatie, ziet u het bericht **deze gebruiker ontvangt een e-mailbericht kan samenwerken met Microsoft.** Selecteer de gebruiker met het e-mailadres en klik vervolgens op **opslaan**.

Als de gebruiker zich buiten uw organisatie, krijgen ze een e-mailbericht naar het lid van de organisatie. De gebruiker moet de uitnodiging te accepteren en vervolgens deze is mogelijk om te zien van de galerie en alle van de installatiekopie van definities en versies in de lijst met resources.

