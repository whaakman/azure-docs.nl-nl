---
title: Hoe u bewijs van bezit voor x.509-CA-certificaten met Azure IoT Hub apparaat inrichtingsservice doet | Microsoft Docs
description: Het x.509-CA-certificaten met uw service DP's controleren
services: iot-dps
keywords: ''
author: bryanla
ms.author: v-jamebr
ms.date: 02/26/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: eb37ce7e61796494be0a9282afdc620b0ca5886a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-do-proof-of-possession-for-x509-ca-certificates-with-your-device-provisioning-service"></a>Hoe u bewijs van bezit voor x.509-CA-certificaten met uw Service voor het inrichten van apparaat

Een geverifieerde x.509-certificaat (CA) is een CA-certificaat dat is geüpload en geregistreerd om in te richten uw certificaat service en bewijs van bezit met de service heeft doorlopen. 

Bewijs van bezit omvat de volgende stappen:
1. Een unieke verificatiecode gegenereerd door de inrichting service voor uw CA X.509-certificaat ophalen. U kunt dit doen via de Azure-portal.
2. Maken van een X.509-certificaat voor verificatie met de verificatiecode als het onderwerp en het certificaat met de persoonlijke sleutel die is gekoppeld aan de x.509-CA-certificaat ondertekenen.
3. Upload het ondertekende verificatiecertificaat naar de service. Valideert de service het verificatiecertificaat met behulp van het openbare deel van het CA-certificaat te worden geverifieerd, dus aan te tonen dat u zich in bezit is van de persoonlijke sleutel van het CA-certificaat.

Geverifieerde certificaten speelt een belangrijke rol bij gebruik van groepen van de inschrijving. Verifiëren van certificaat eigenaars biedt een extra beveiligingslaag door ervoor te zorgen dat de uploader van het certificaat in het bezit zijn van de persoonlijke sleutel van het certificaat is. Verificatie wordt voorkomen dat een kwaadwillende actor uw verkeer van een tussenliggende certificaat ophalen en dat certificaat gebruiken voor het maken van een inschrijvingsgroep voor in hun eigen provisioning-service, het bewaken van netwerken effectief kaapt uw apparaten. Door het eigendom van de hoofdmap of een tussenliggende certificaat in een certificaatketen bewijzen, bent u aan te tonen dat u gemachtigd bent voor het genereren van leaf-certificaten voor de apparaten die zullen worden geregistreerd als onderdeel van die inschrijvingsgroep. Voor deze reden, de basis- of tussenliggende certificaat dat is geconfigureerd in een inschrijvingsgroep een geverifieerde certificaat moet of moet worden getotaliseerd naar een geverifieerde certificaat in de certificaatketen die een apparaat geeft tijdens de verificatie met de service. Zie voor meer informatie over inschrijving groepen, [X.509-certificaten](concepts-security.md#x509-certificates) en [Apparaattoegang tot de inrichting service met X.509-certificaten beheren](concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates).

## <a name="register-the-public-part-of-an-x509-certificate-and-get-a-verification-code"></a>Registreren van het openbare deel van een X.509-certificaat en een verificatiecode ophalen

Volg deze stappen om een CA-certificaat registreren bij uw provisioning-service en krijgt u een verificatiecode waarmee u tijdens bewijs van bezit kunt. 

1. In de Azure-portal, gaat u naar uw provisioning-service en open **certificaten** uit in het menu links. 
2. Klik op **toevoegen** toevoegen van een nieuw certificaat.
3. Voer een beschrijvende weergavenaam voor het certificaat. Blader naar het cer- of .pem-bestand dat staat voor het openbare deel van het X.509-certificaat. Klik op **Uploaden**.
4. Als u een melding krijgt dat het certificaat is geüpload, klikt u op **opslaan**.

    ![Certificaat uploaden](./media/how-to-verify-certificates/add-new-cert.png)  

   Uw certificaat wordt weergegeven in de **certificaat Explorer** lijst. Houd er rekening mee dat de **STATUS** van dit certificaat is *Unverified*.

5. Klik op het certificaat dat u in de vorige stap hebt toegevoegd.

6. In **certificaatdetails**, klikt u op **verificatiecode genereren**.

7. De inrichting service maakt een **verificatiecode** waarmee u kunt de eigendom van het certificaat te valideren. De code naar het Klembord kopiëren. 

   ![Controleer of certificaat](./media/how-to-verify-certificates/verify-cert.png)  

## <a name="digitally-sign-the-verification-code-to-create-a-verification-certificate"></a>De verificatiecode voor het maken van een verificatiecertificaat digitaal te ondertekenen

Nu moet u voor het ondertekenen van de *verificatiecode* met de persoonlijke sleutel die is gekoppeld aan de x.509-CA-certificaat, die een handtekening genereert. Dit staat bekend als [bewijs van eigendom](https://tools.ietf.org/html/rfc5280#section-3.1) en resulteert in een ondertekende verificatiecertificaat.

Microsoft biedt hulpprogramma's en voorbeelden kunt u een ondertekende verificatiecertificaat maken: 

- De **C-SDK van Azure IoT Hub** biedt PowerShell (Windows) en (Linux) Bash-scripts voor het maken van de CA en leaf certificaten voor ontwikkeling en voor het uitvoeren van bewijs van bezit met een verificatiecode. U kunt downloaden via de [bestanden](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) relevant zijn voor uw systeem naar een werkmap en volg de instructies in de [beheren CA certificaten Leesmij](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) bewijs van bezit uitvoeren op een CA-certificaat. 
- De **Azure IoT Hub C# SDK** bevat de [groep certificaat verificatie voorbeeld](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples/GroupCertificateVerificationSample), waarin u kunt doen bewijs van bezit.
- U kunt de stappen in de [PowerShell-scripts voor het beheren van CA ondertekend X.509-certificaten](https://docs.microsoft.com/azure/iot-hub/iot-hub-security-x509-create-certificates) artikel in de documentatie van de IoT Hub, specifiek het script vermeld in het gedeelte [bewijs van eigendom van uw X.509-CA-certificaat](https://docs.microsoft.com/azure/iot-hub/iot-hub-security-x509-create-certificates#signverificationcode).
 
> [!IMPORTANT]
> Naast bewijs van bezit uitvoert, kunnen de eerder genoemde ook PowerShell en Bash-scripts u basiscertificaten, tussenliggende certificaten en leaf-certificaten die kunnen worden gebruikt om te verifiëren en apparaten inrichten maken. Deze certificaten moeten worden gebruikt voor alleen-ontwikkeling. Ze moeten nooit worden gebruikt in een productieomgeving. 

Afhankelijk van de PowerShell en Bash-scripts die zijn opgegeven in de documentatie en SDK's [OpenSSL](https://www.openssl.org/). U kunt ook OpenSSL of andere hulpprogramma's van derden om te doen bewijs van bezit. Zie voor meer informatie over tooling opgegeven met de SDK's, [het gebruik van hulpprogramma's in de SDK's](how-to-use-sdk-tools.md). 


## <a name="upload-the-signed-verification-certificate"></a>Upload het ondertekende verificatiecertificaat

1. De resulterende handtekening als een verificatiecertificaat naar uw provisioning-service in de portal uploaden. In **certificaatdetails** op de Azure-portal, gebruikt u de _Verkenner_ pictogram naast de **verificatiecertificaat .pem of .cer-bestand** veld voor het uploaden van de ondertekende verificatiecertificaat van uw systeem.

2. Zodra het certificaat is geüpload, klikt u op **controleren**. De **STATUS** van uw certificaat wijzigingen aan **_gecontroleerd_** in de **certificaat Explorer** lijst. Klik op **vernieuwen** als deze wordt niet automatisch bijgewerkt.

   ![Certificaatverificatie uploaden](./media/how-to-verify-certificates/upload-cert-verification.png)  

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het gebruik van de portal voor het maken van een inschrijvingsgroep voor, [apparaatinschrijvingen met Azure portal beheren](how-to-manage-enrollments.md).
- Zie voor meer informatie over het gebruik van de service-SDK's voor het maken van een inschrijvingsgroep voor, [apparaatinschrijvingen met service-SDK's beheren](how-to-manage-enrollments-sdks.md).










