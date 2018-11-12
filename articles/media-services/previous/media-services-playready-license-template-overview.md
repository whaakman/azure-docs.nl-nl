---
title: Overzicht van Media Services PlayReady-licentiesjablonen
description: In dit onderwerp biedt een overzicht van een PlayReady-licentiesjabloon die wordt gebruikt voor het configureren van PlayReady-licenties.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: fddce5d0-1278-478f-ae05-9b985c748731
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2018
ms.author: juliako
ms.openlocfilehash: 780fa37351ec590beff6fd2d9e80ec8f22afa84d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228501"
---
# <a name="media-services-playready-license-template-overview"></a>Overzicht van Media Services PlayReady-licentiesjablonen
Azure Media Services biedt nu een service voor het leveren van PlayReady-licenties. Wanneer de speler (bijvoorbeeld Silverlight) probeert uw PlayReady beveiligde inhoud af te spelen, wordt een aanvraag verzonden naar de service voor het leveren van licenties om een licentie te verkrijgen. Als de licentieservice de aanvraag goedkeurt, geeft deze de licentie die is verzonden naar de client en wordt gebruikt om te ontsleutelen en de opgegeven inhoud af te spelen.

Media Services biedt ook API's die u gebruiken kunt om uw PlayReady-licenties te configureren. Licenties bevatten de rechten en beperkingen die u wilt dat de runtime PlayReady digital rights management (DRM) om af te dwingen wanneer een gebruiker probeert om af te spelen beveiligde inhoud.
Hier volgen enkele voorbeelden van de beperkingen van de PlayReady-licentie die u kunt opgeven:

* De datum en tijd van waaruit de licentie geldig is.
* De datum/tijd-waarde wanneer de licentie is verlopen. 
* Voor de licentie om te worden opgeslagen in een permanente opslag op de client. Permanente licenties worden meestal gebruikt om toe te staan offline afspelen van de inhoud.
* Het minimale beveiligingsniveau dat een speler hebben moet tot uw inhoud afspelen. 
* Het beveiligingsniveau van de uitvoer van de uitvoer-besturingselementen voor audio\video inhoud. 
* Zie voor meer informatie de sectie 'Uitvoer besturingselementen' (3.5) in de [PlayReady Compliantieregels](https://www.microsoft.com/playready/licensing/compliance/) document.

> [!NOTE]
> U kunt op dit moment alleen de PlayRight van de PlayReady-licentie configureren. Dit recht is vereist. De PlayRight geeft de client de mogelijkheid voor het afspelen van de inhoud. U kunt ook de PlayRight beperkingen die specifiek zijn voor afspelen configureren gebruiken. Zie voor meer informatie, [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).
> 
> 

Als u wilt configureren PlayReady-licenties met behulp van Media Services, moet u de Media Services PlayReady-licentiesjabloon configureren. De sjabloon is gedefinieerd in XML.

Het volgende voorbeeld ziet de eenvoudigste (en meest voorkomende)-sjabloon waarmee een basic streaming-licentie worden geconfigureerd. Met deze licentie, kunnen uw clients afspelen uw PlayReady beveiligde inhoud.

    <?xml version="1.0" encoding="utf-8"?>
    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" 
                                      xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <PlayRight />
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

Het XML-bestand voldoet aan de PlayReady-licentie sjabloon XML-schema in de sectie 'PlayReady-licentie sjabloon XML-schema' gedefinieerd.

Media Services kunt u ook een set van .NET-klassen die kunnen worden gebruikt voor het serialiseren en deserialiseren van en naar het XML-bestand gedefinieerd. Zie voor een beschrijving van de belangrijkste klassen, de [Media Services .NET-klassen](media-services-playready-license-template-overview.md#classes) die worden gebruikt voor het configureren van de licentie-sjablonen.

Zie voor een voorbeeld van end-to-end die gebruikmaakt van .NET-klassen voor het configureren van de PlayReady-licentiesjabloon [dynamische versleuteling met PlayReady en de service voor het leveren van licenties](media-services-protect-with-playready-widevine.md).

## <a id="classes"></a>Media Services .NET-klassen die worden gebruikt voor het configureren van de licentie-sjablonen
De volgende klassen zijn de belangrijkste .NET-klassen die worden gebruikt voor het configureren van sjablonen voor Media Services PlayReady-licentie. Deze klassen worden toegewezen aan de typen die zijn gedefinieerd [PlayReady-licentie sjabloon XML-schema](media-services-playready-license-template-overview.md#schema).

De [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) klasse wordt gebruikt voor het serialiseren en deserialiseren van en naar de Media Services-licentiesjabloon XML.

### <a name="playreadylicenseresponsetemplate"></a>PlayReadyLicenseResponseTemplate
[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx): deze klasse vertegenwoordigt de sjabloon voor het antwoord terug naar de gebruiker verzonden. Het bevat een veld voor een tekenreeks met aangepaste gegevens tussen de licentieserver en de toepassing (dit kan handig zijn voor aangepaste app-logica). Het bevat ook een lijst met een of meer licentie-sjablonen.

De antwoord-sjabloon bevat als de klasse 'op het hoogste niveau' in de Sjabloonhiërarchie, een lijst met sjablonen van de licentie. De licentie-sjablonen bevatten (direct of indirect) alle andere klassen die gezamenlijk de sjabloongegevens worden geserialiseerd.

### <a name="playreadylicensetemplate"></a>PlayReadyLicenseTemplate
[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx): deze klasse vertegenwoordigt een licentiesjabloon die wordt gebruikt voor het maken van PlayReady-licenties aan gebruikers worden geretourneerd. Het bevat de gegevens op de inhoudssleutel in de licentie. Dit omvat ook alle rechten en beperkingen die de PlayReady DRM-runtime afdwingen moet wanneer de inhoudssleutel wordt gebruikt.

### <a id="PlayReadyPlayRight"></a>PlayReadyPlayRight
[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx): deze klasse vertegenwoordigt de PlayRight van een PlayReady-licentie. Deze geeft de gebruiker de mogelijkheid voor het afspelen van de inhoud onderworpen aan de beperkingen die zijn geconfigureerd in de licentie en op de PlayRight zelf (voor het afspelen van beleid). Veel van de beleidsregels op een PlayRight betreft beperkingen voor uitvoer die welke typen uitvoer die voor de inhoud kan worden afgespeeld bepalen via. Dit omvat ook de beperkingen die moeten worden geplaatst in plaats wanneer een bepaalde uitvoer wordt gebruikt. Bijvoorbeeld, als DigitalVideoOnlyContentRestriction is ingeschakeld, kunt de DRM-runtime alleen de video moet worden weergegeven via digitale uitvoer. (Analoog video-uitgangen zijn niet toegestaan om door te geven van de inhoud.)

> [!IMPORTANT]
> Dit soort beperkingen kunnen krachtige, maar ze ook kunnen invloed hebben op de ervaring van consumenten. Als de uitvoer goed te beperken, is het mogelijk dat de inhoud afgespeeld op sommige clients. Zie voor meer informatie de [PlayReady Compliantieregels](https://www.microsoft.com/playready/licensing/compliance/).
> 
> 

Zie voor een voorbeeld van de beveiliging niveaus die Silverlight ondersteunt, [Silverlight-ondersteuning voor uitvoer beveiligingen](https://go.microsoft.com/fwlink/?LinkId=617318).

## <a id="schema"></a>PlayReady-licentie sjabloon XML-schema
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />
      <xs:complexType name="AgcAndColorStripeRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction" />
      <xs:simpleType name="ContentType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Unspecified" />
          <xs:enumeration value="UltravioletDownload" />
          <xs:enumeration value="UltravioletStreaming" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="ContentType" nillable="true" type="tns:ContentType" />
      <xs:complexType name="ExplicitAnalogTelevisionRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="BestEffort" type="xs:boolean" />
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ExplicitAnalogTelevisionRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction" />
      <xs:complexType name="PlayReadyContentKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="PlayReadyContentKey" nillable="true" type="tns:PlayReadyContentKey" />
      <xs:complexType name="ContentEncryptionKeyFromHeader">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence />
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromHeader" nillable="true" type="tns:ContentEncryptionKeyFromHeader" />
      <xs:complexType name="ContentEncryptionKeyFromKeyIdentifier">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence>
              <xs:element minOccurs="0" name="KeyIdentifier" type="ser:guid" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromKeyIdentifier" nillable="true" type="tns:ContentEncryptionKeyFromKeyIdentifier" />
      <xs:complexType name="PlayReadyLicenseResponseTemplate">
        <xs:sequence>
          <xs:element name="LicenseTemplates" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
          <xs:element minOccurs="0" name="ResponseCustomData" nillable="true" type="xs:string">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseResponseTemplate" nillable="true" type="tns:PlayReadyLicenseResponseTemplate" />
      <xs:complexType name="ArrayOfPlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfPlayReadyLicenseTemplate" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
      <xs:complexType name="PlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AllowTestDevices" type="xs:boolean" />
          <xs:element minOccurs="0" name="BeginDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element name="ContentKey" nillable="true" type="tns:PlayReadyContentKey" />
          <xs:element minOccurs="0" name="ContentType" type="tns:ContentType">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExpirationDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="GracePeriod" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="LicenseType" type="tns:PlayReadyLicenseType" />
          <xs:element minOccurs="0" name="PlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
      <xs:simpleType name="PlayReadyLicenseType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Nonpersistent" />
          <xs:enumeration value="Persistent" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="PlayReadyLicenseType" nillable="true" type="tns:PlayReadyLicenseType" />
      <xs:complexType name="PlayReadyPlayRight">
        <xs:sequence>
          <xs:element minOccurs="0" name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AllowPassingVideoContentToUnknownOutput" type="tns:UnknownOutputPassingOption">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AnalogVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="DigitalVideoOnlyContentRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExplicitAnalogTelevisionOutputRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="FirstPlayExpiration" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComponentVideoRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComputerMonitorRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyPlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
      <xs:simpleType name="UnknownOutputPassingOption">
        <xs:restriction base="xs:string">
          <xs:enumeration value="NotAllowed" />
          <xs:enumeration value="Allowed" />
          <xs:enumeration value="AllowedWithVideoConstriction" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="UnknownOutputPassingOption" nillable="true" type="tns:UnknownOutputPassingOption" />
      <xs:complexType name="ScmsRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction" />
    </xs:schema>



## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

