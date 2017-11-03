---
title: SSL configureren voor een cloudservice (klassiek) | Microsoft Docs
description: Informatie over het opgeven van een HTTPS-eindpunt voor een Webrol en het uploaden van een SSL-certificaat voor het beveiligen van uw toepassing.
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 4cbb7f38-7994-454d-b4f0-7259b558c766
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: 7df2371f64f0d8a9fabc0df37c17d4dfbc47cd7e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a>SSL configureren voor een toepassing in Azure
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-configure-ssl-certificate-portal.md)
> * [Klassieke Azure Portal](cloud-services-configure-ssl-certificate.md)
> 
> 

SSL-versleuteling (Secure Socket Layer) is de meest gebruikte methode voor het beveiligen van gegevens die via internet worden verzonden. In deze algemene taak wordt beschreven hoe u een HTTPS-eindpunt kunt opgeven voor een webrol en hoe u een SSL-certificaat kunt uploaden om uw toepassing te beveiligen.

> [!NOTE]
> De procedures in deze taak gelden voor Azure Cloud Services; Zie voor App-Services, [dit](../app-service/app-service-web-tutorial-custom-ssl.md) artikel.
> 
> 

Deze taak maakt gebruik van een productie-implementatie. Informatie over het gebruik van een gefaseerde installatie-implementatie is verstrekt aan het einde van dit onderwerp.

Lees [dit](cloud-services-how-to-create-deploy.md) eerst artikel als u nog geen een cloudservice hebt gemaakt.

## <a name="step-1-get-an-ssl-certificate"></a>Stap 1: Haal een SSL-certificaat
SSL configureren voor een toepassing, moet u eerst ophalen van een SSL-certificaat dat is ondertekend door een certificeringsinstantie (CA), een vertrouwde derde die certificaten voor dit doel verleent. Als u nog geen een, moet u aanvragen bij een bedrijf dat SSL-certificaten verkoopt.

Het certificaat moet voldoen aan de volgende vereisten voor SSL-certificaten in Azure:

* Het certificaat moet een persoonlijke sleutel bevatten.
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, geëxporteerd naar een bestand Personal Information Exchange (.pfx).
* De onderwerpnaam van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de cloudservice. U kunt een SSL-certificaat kan niet verkrijgen van een certificeringsinstantie (CA) voor het domein cloudapp.net. Moet u een aangepaste domeinnaam moeten worden gebruikt wanneer verkrijgen toegang tot uw service. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de naam van het aangepaste domein gebruikt voor toegang tot uw toepassing. Bijvoorbeeld, als uw aangepaste domeinnaam is **contoso.com** zou u een certificaat aanvragen van uw Certificeringsinstantie voor ***. contoso.com** of **www.contoso.com**.
* Het certificaat moet ten minste 2048-bits codering gebruiken.

Voor testdoeleinden kunt u [maken](cloud-services-certs-create.md) en een zelfondertekend certificaat gebruiken. Een zelfondertekend certificaat is niet geverifieerd via een CA en het domein cloudapp.net kunt gebruiken als de website-URL. De volgende taak gebruikt bijvoorbeeld een zelfondertekend certificaat waarin de algemene naam (CN) gebruikt in het certificaat is **sslexample.cloudapp.net**.

Vervolgens moet u informatie over het certificaat opnemen in uw servicedefinitie en configuratiebestanden van de service.

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Stap 2: De service-definitie en configuratie van bestanden wijzigen
Uw toepassing moet worden geconfigureerd voor het gebruik van het certificaat en een HTTPS-eindpunt moet worden toegevoegd. Als gevolg hiervan de servicedefinitie en configuratiebestanden van de service moeten worden bijgewerkt.

1. Open het servicedefinitiebestand (CSDEF) in uw ontwikkelomgeving, voegt een **certificaten** sectie binnen de **WebRole** sectie en bevatten de volgende informatie over het certificaat (en tussenliggende certificaten):
   
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
   
   De **certificaten** sectie definieert de naam van onze certificaat, de locatie en de naam van het archief waar deze zich bevinden.
   
   Machtigingen (`permisionLevel` kenmerk) kan worden ingesteld op een van de volgende waarden:
   
   | Waarde van machtiging | Beschrijving |
   | --- | --- |
   | limitedOrElevated |**(Standaard)**  Alle processen van de rol toegang heeft tot de persoonlijke sleutel. |
   | verhoogde |Alleen processen met verhoogde bevoegdheden hebben toegang tot de persoonlijke sleutel. |
2. Voeg in het servicedefinitiebestand een **Invoereindpunt** element in de **eindpunten** sectie HTTPS inschakelen:
   
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

3. Voeg in het servicedefinitiebestand een **Binding** element in de **Sites** sectie. Deze sectie wordt een HTTPS-binding het eindpunt toewijzen aan uw site toegevoegd:
   
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
   
   Alle vereiste wijzigingen in het servicedefinitiebestand zijn voltooid, maar u toch wilt gegevens van het certificaat toevoegen aan het configuratiebestand van de service.
4. Voeg in uw serviceconfiguratiebestand (CSCFG) ServiceConfiguration.Cloud.cscfg, een **certificaten** sectie binnen de **rol** sectie, vervangen de vingerafdrukwaarde van voorbeeld hieronder wordt weergegeven met die van uw certificaat:
   
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

(Maakt gebruik van het vorige voorbeeld **sha1** voor de vingerafdruk van het algoritme. Geef op de juiste waarde voor de algoritme van de vingerafdruk van het certificaat.)

Nu dat de servicedefinitie en de configuratiebestanden van de service is bijgewerkt, het pakket uw implementatie voor het uploaden naar Azure. Als u **cspack**, gebruik niet de **/generateConfigurationFile** markeren, zoals die de gegevens van het certificaat dat u ingevoegd worden overschreven.

## <a name="step-3-upload-a-certificate"></a>Stap 3: Een certificaat uploaden
Uw implementatiepakket is bijgewerkt om het certificaat te gebruiken en een HTTPS-eindpunt is toegevoegd. U kunt nu het pakket en een certificaat uploaden naar Azure met de klassieke Azure portal.

1. Meld u aan bij de [klassieke Azure-portal][Azure classic portal]. 
2. Klik op **Cloudservices** in het navigatievenster aan de linkerkant.
3. Klik op de gewenste cloudservice.
4. Klik op de **certificaten** tabblad.
   
    ![Klik op het tabblad certificaten](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Klik op de knop **Uploaden**.
   
    ![Uploaden](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Geef de **bestand**, **wachtwoord**, klikt u vervolgens op **Complete** (het vinkje).

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Stap 4: Verbinding maken met het rolexemplaar met behulp van HTTPS
Nu dat uw implementatie actief in Azure is, kunt u verbinding met het gebruik van HTTPS.

1. In de klassieke Azure portal, selecteert u uw implementatie en klik op de koppeling onder **Site-URL**.
   
   ![Site-URL bepalen][2]
2. Wijzig de koppeling om te gebruiken in uw webbrowser **https** in plaats van **http**, en ga vervolgens naar de pagina.
   
   > [!NOTE]
   > Als u een zelfondertekend certificaat gebruikt wanneer u naar een HTTPS-eindpunt dat is gekoppeld aan het zelfondertekende certificaat bladert, ziet u mogelijk een certificaatfout in de browser. Met behulp van een certificaat dat is ondertekend door een vertrouwde certificeringsinstantie wordt voorkomen dat u dit probleem; in de tussentijd kunt u de fout negeren. (Er is een andere optie is het zelfondertekende certificaat toevoegen aan certificaatarchief Vertrouwde certificaat van de gebruiker.)
   > 
   > 
   
   ![Website voor SSL-voorbeeld][3]

Als u SSL gebruiken voor een gefaseerde installatie-implementatie in plaats van een productie-implementatie wilt, moet u eerst de URL die wordt gebruikt voor de implementatie van fasering bepalen. Uw cloudservice implementeren in de testomgeving zonder een certificaat of de gegevens van een certificaat. Zodra geïmplementeerd, kunt u de URL op basis van GUID, die wordt vermeld in de klassieke Azure portal van bepalen **Site-URL** veld. Een certificaat maken met de algemene naam (CN) gelijk zijn aan de URL op basis van GUID (bijvoorbeeld **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). Gebruik de klassieke Azure portal naar het certificaat toevoegen aan uw voorbereide cloudservice. Gegevens van het certificaat vervolgens toevoegen aan uw CSDEF- en CSCFG-bestanden, opnieuw inpakken van uw toepassing en uw gefaseerde implementatie voor het gebruik van het nieuwe pakket bijwerken.

## <a name="next-steps"></a>Volgende stappen
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure.md).
* Meer informatie over hoe [implementeren van een cloudservice](cloud-services-how-to-create-deploy.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name.md).
* [Beheren van uw cloudservice](cloud-services-how-to-manage.md).

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
