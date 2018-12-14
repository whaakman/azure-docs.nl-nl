---
title: SSL configureren voor een cloudservice | Microsoft Docs
description: Leer hoe u een HTTPS-eindpunt voor een Webrol en over het uploaden van een SSL-certificaat om uw toepassing te beveiligen. Deze voorbeelden gebruiken de Azure-portal.
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 371ba204-48b6-41af-ab9f-ed1d64efe704
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeconnoc
ms.openlocfilehash: cf2fe10d6a0ab81ff71c948ee2defe6bc7edfd70
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300186"
---
# <a name="configuring-ssl-for-an-application-in-azure"></a>SSL configureren voor een toepassing in Azure

SSL-versleuteling (Secure Socket Layer) is de meest gebruikte methode voor het beveiligen van gegevens die via internet worden verzonden. In deze algemene taak wordt beschreven hoe u een HTTPS-eindpunt kunt opgeven voor een webrol en hoe u een SSL-certificaat kunt uploaden om uw toepassing te beveiligen.

> [!NOTE]
> De procedures in deze taak gelden voor Azure Cloud Services; Zie voor App-Services, [dit](../app-service/app-service-web-tutorial-custom-ssl.md).
>

Deze taak maakt gebruik van een productie-implementatie. Informatie over het gebruik van een staging-implementatie wordt verstrekt aan het einde van dit onderwerp.

Lezen [dit](cloud-services-how-to-create-deploy-portal.md) eerst als u een service in de cloud nog niet hebt gemaakt.

## <a name="step-1-get-an-ssl-certificate"></a>Stap 1: Een SSL-certificaat ophalen
SSL configureren voor een toepassing, moet u eerst om op te halen van een SSL-certificaat dat is ondertekend door een certificeringsinstantie (CA), een vertrouwde derde die certificaten voor dit doel uitgeeft. Als u nog geen een, moet u aanvragen bij een bedrijf dat SSL-certificaten wordt verkocht.

Het certificaat moet voldoen aan de volgende vereisten voor SSL-certificaten in Azure:

* Het certificaat moet een persoonlijke sleutel bevatten.
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, exporteerbaar naar een Personal Information Exchange (PFX)-bestand.
* Naam van het onderwerp van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de service in de cloud. U kunt een SSL-certificaat kan niet verkrijgen van een certificeringsinstantie (CA) voor het domein cloudapp.net. Moet u een aangepaste domeinnaam moeten worden gebruikt wanneer verkrijgen toegang tot uw service. Wanneer u een certificaat van een CA aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam gebruikt voor toegang tot uw toepassing. Bijvoorbeeld, als uw aangepaste domeinnaam is **contoso.com** zou u een certificaat aanvragen bij uw CA voor ***. contoso.com** of ** **www.contoso.com**.
* Het certificaat moet minimaal 2048-bits versleuteling gebruiken.

Voor test-doeleinden, kunt u [maken](cloud-services-certs-create.md) een zelf-ondertekend certificaat gebruiken. Een zelfondertekend certificaat is niet geverifieerd via een CA en het domein cloudapp.net kunt gebruiken als de website-URL. De volgende taak gebruikt bijvoorbeeld een zelfondertekend certificaat waarin de algemene naam (CN) die wordt gebruikt in het certificaat is **sslexample.cloudapp.net**.

Vervolgens moet u informatie over het certificaat opnemen in uw servicedefinitie en service-configuratiebestanden.

<a name="modify"> </a>

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Stap 2: De definitie en configuratiebestanden van de service wijzigen
Uw toepassing moet worden geconfigureerd voor het gebruik van het certificaat en een HTTPS-eindpunt moet worden toegevoegd. Als gevolg hiervan moeten de servicedefinitie en configuratiebestanden van de service worden bijgewerkt.

1. Open het servicedefinitiebestand (CSDEF) in uw ontwikkelomgeving, Voeg een **certificaten** sectie binnen de **WebRole** uit en voeg de volgende informatie over het certificaat (en tussenliggende certificaten):

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by the CA root, you
            must include all the intermediate certificates
            here. You must list them here, even if they are
            not bound to any endpoints. Failing to list any of
            the intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```

   De **certificaten** gedeelte definieert de naam van onze certificaat, de locatie en de naam van de winkel waar deze zich bevinden.

   Machtigingen (`permissionLevel` kenmerk) kan worden ingesteld op een van de volgende waarden:

   | Waarde van machtiging | Beschrijving |
   | --- | --- |
   | limitedOrElevated |**(Standaard)**  Alle processen van de rol toegang heeft tot de persoonlijke sleutel. |
   | met verhoogde bevoegdheid |Alleen processen met verhoogde bevoegdheden hebben toegang tot de persoonlijke sleutel. |

2. In het servicedefinitiebestand toevoegen een **Invoereindpunt** element in de **eindpunten** sectie HTTPS inschakelen:

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Endpoints>
            <InputEndpoint name="HttpsIn" protocol="https" port="443"
                certificate="SampleCertificate" />
        </Endpoints>
    ...
    </WebRole>
    ```

3. In het servicedefinitiebestand toevoegen een **Binding** element in de **Sites** sectie. Dit element voegt een HTTPS-binding het eindpunt toewijzen aan uw site:

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="HttpsIn" endpointName="HttpsIn" />
                </Bindings>
            </Site>
        </Sites>
    ...
    </WebRole>
    ```

   Alle vereiste wijzigingen aan het servicedefinitiebestand zijn voltooid; maar u moet nog steeds de certificaatgegevens toevoegen aan het configuratiebestand van de service.
4. Voeg in uw serviceconfiguratiebestand (CSCFG), ServiceConfiguration.Cloud.cscfg, een **certificaten** waarde met die van uw certificaat. Het volgende codevoorbeeld vindt u informatie over de **certificaten** sectie, met uitzondering van de vingerafdrukwaarde.

   ```xml
    <Role name="Deployment">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff"
                thumbprintAlgorithm="sha1" />
            <Certificate name="CAForSampleCertificate"
                thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc"
                thumbprintAlgorithm="sha1" />
        </Certificates>
    ...
    </Role>
    ```

(In dit voorbeeld wordt **sha1** voor de algoritme van de vingerafdruk. Geef de juiste waarde voor de algoritme van de vingerafdruk van het certificaat.)

Nu dat de servicedefinitie en service-configuratiebestanden zijn bijgewerkt, Verpak uw implementatie voor het uploaden naar Azure. Als u **cspack**, gebruik niet de **/generateConfigurationFile** markeren, zoals die de gegevens van het certificaat dat u zojuist hebt ingevoegd wordt overschreven.

## <a name="step-3-upload-a-certificate"></a>Stap 3: Een certificaat uploaden
Verbinding maken met de Azure-portal en...

1. In de **alle resources** sectie van de Portal, selecteert u uw cloudservice.

    ![Uw cloudservice publiceren](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. Klik op **certificaten**.

    ![Klik op het pictogram certificaten](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. Klik op **uploaden** aan de bovenkant van het gebied van certificaten.

    ![Klik op het menu-item uploaden](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. Geef de **bestand**, **wachtwoord**, klikt u vervolgens op **uploaden** onder aan het gegevensgebied vermelding.

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Stap 4: Verbinding maken met de rolinstantie met behulp van HTTPS
Nu dat uw implementatie actief in Azure is, kunt u verbinding met het gebruik van HTTPS.

1. Klik op de **Site-URL** om de webbrowser te openen.

   ![Klik op de Site-URL](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. Wijzig de koppeling om te gebruiken in uw webbrowser **https** in plaats van **http**, en ga vervolgens naar de pagina.

   > [!NOTE]
   > Als u een zelfondertekend certificaat gebruikt wanneer u bladert naar een HTTPS-eindpunt dat is gekoppeld aan het zelfondertekende certificaat ziet u mogelijk een certificaatfout in de browser. Met behulp van een certificaat dat is ondertekend door een vertrouwde certificeringsinstantie wordt voorkomen dat dit probleem. in de tussentijd kunt u de fout negeren. (Er is een andere optie is het zelfondertekende certificaat toevoegen aan certificaatarchief Vertrouwde certificaat van de gebruiker.)
   >
   >

   ![Site-preview](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > Als u wilt het gebruik van SSL voor een staging-implementatie in plaats van een productie-implementatie, moet u eerst om te bepalen van de URL die wordt gebruikt voor de staging-implementatie. Zodra uw cloudservice is geïmplementeerd, de URL van de testomgeving wordt bepaald door de **implementatie-ID** GUID in deze indeling: `https://deployment-id.cloudapp.net/`  
   >
   > Maak een certificaat met de algemene naam (CN) gelijk zijn aan de URL op basis van een GUID (bijvoorbeeld **328187776e774ceda8fc57609d404462.cloudapp.net**). Gebruikt de portal voor het certificaat toevoegen aan uw gefaseerde cloudservice. Informatie over het certificaat vervolgens toevoegen aan uw CSDEF- en CSCFG-bestanden, uw toepassing opnieuw verpakken en bijwerken van de gefaseerde implementatie voor het gebruik van het nieuwe pakket.
   >

## <a name="next-steps"></a>Volgende stappen
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Meer informatie over het [implementeren van een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md).
* [Uw cloudservice beheren](cloud-services-how-to-manage-portal.md).
