---
title: Azure Service Fabric service model XML-schema-kenmerkgroepen | Microsoft Docs
description: Beschrijft de kenmerkgroepen in het XML-schema van de Service Fabric ServiceModel.
services: service-fabric
documentationcenter: na
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/10/2018
ms.author: atsenthi
ms.openlocfilehash: 4235f6c1c299c2267d8ebe78ae907c4f4bbc33b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60837620"
---
<!-- This article was generated by the Python script found in the service-fabric-service-model-schema.md file -->

# <a name="service-model-xml-schema-attribute-groups"></a>Service model XML-schema-kenmerkgroepen

## <a name="accountcredentialsgroup-attributegroup"></a>AccountCredentialsGroup attributeGroup

|Kenmerk|Value|
|---|---|
|content|2-kenmerken|
|name|AccountCredentialsGroup|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="AccountCredentialsGroup">
        <xs:attribute name="AccountName" type="xs:string" use="optional">
          <xs:annotation>
            <xs:documentation>User name or Service Account Name (for example, MyMachine\JohnDoe or John.Doe@department.contoso.com).</xs:documentation>
          </xs:annotation>
        </xs:attribute>
        <xs:attribute name="Password" type="xs:string" use="optional">
            <xs:annotation>
                <xs:documentation>Password for the user account.</xs:documentation>
            </xs:annotation>
        </xs:attribute>
    </xs:attributeGroup>
    
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="accountname"></a>AccountName
Gebruikersnaam of de naam van het serviceaccount (bijvoorbeeld MyMachine\JohnDoe of John.Doe@department.contoso.com).

|Kenmerk|Value|
|---|---|
|name|AccountName|
|type|xs:String|
|Gebruik|Optioneel|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="AccountName" type="xs:string" use="optional">
          <xs:annotation>
            <xs:documentation>User name or Service Account Name (for example, MyMachine\JohnDoe or John.Doe@department.contoso.com).</xs:documentation>
          </xs:annotation>
        </xs:attribute>
        
```

#### <a name="password"></a>Wachtwoord
Wachtwoord voor het gebruikersaccount.

|Kenmerk|Value|
|---|---|
|name|Wachtwoord|
|type|xs:String|
|Gebruik|Optioneel|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Password" type="xs:string" use="optional">
            <xs:annotation>
                <xs:documentation>Password for the user account.</xs:documentation>
            </xs:annotation>
        </xs:attribute>
    
```

## <a name="applicationinstanceattrgroup-attributegroup"></a>ApplicationInstanceAttrGroup attributeGroup
Kenmerkgroep voor het exemplaar van toepassing.

|Kenmerk|Value|
|---|---|
|content|2-kenmerken|
|name|ApplicationInstanceAttrGroup|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ApplicationInstanceAttrGroup">
    <xs:annotation>
      <xs:documentation>Attribute group for application instance.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="NameUri" type="FabricUri" use="required">
      <xs:annotation>
        <xs:documentation>Fully qualified name of the application.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
    <xs:attribute name="ApplicationId" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>Id of this application.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="nameuri"></a>NameUri
Volledig gekwalificeerde naam van de toepassing.

|Kenmerk|Value|
|---|---|
|name|NameUri|
|type|FabricUri|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="NameUri" type="FabricUri" use="required">
      <xs:annotation>
        <xs:documentation>Fully qualified name of the application.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
    
```

#### <a name="applicationid"></a>ApplicationId
Id van deze toepassing.

|Kenmerk|Value|
|---|---|
|name|ApplicationId|
|type|xs:String|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ApplicationId" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>Id of this application.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="applicationmanifestattrgroup-attributegroup"></a>ApplicationManifestAttrGroup attributeGroup
Kenmerkgroep voor manifest van de toepassing.

|Kenmerk|Value|
|---|---|
|content|3 of meer kenmerken|
|name|ApplicationManifestAttrGroup|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ApplicationManifestAttrGroup">
    <xs:annotation>
      <xs:documentation>Attribute group for application manifest.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="ApplicationTypeName" use="required">
      <xs:annotation>
        <xs:documentation>The type identifier for this application.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="ApplicationTypeVersion" use="required">
      <xs:annotation>
        <xs:documentation>The version of this application type, an unstructured string.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="ManifestId" use="optional" default="" type="xs:string">
      <xs:annotation>
        <xs:documentation>The identifier of this application manifest, an unstructured string.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
    <xs:anyAttribute processContents="skip"/> <!-- Allow unknown attributes to be used. -->
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="applicationtypename"></a>ApplicationTypeName
De type-id voor deze toepassing.

|Kenmerk|Value|
|---|---|
|name|ApplicationTypeName|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ApplicationTypeName" use="required">
      <xs:annotation>
        <xs:documentation>The type identifier for this application.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    
```

#### <a name="applicationtypeversion"></a>ApplicationTypeVersion
De versie van dit toepassingstype, een niet-gestructureerde tekenreeks.

|Kenmerk|Value|
|---|---|
|name|ApplicationTypeVersion|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ApplicationTypeVersion" use="required">
      <xs:annotation>
        <xs:documentation>The version of this application type, an unstructured string.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    
```

#### <a name="manifestid"></a>ManifestId
De id van het manifest van deze toepassing, een niet-gestructureerde tekenreeks.

|Kenmerk|Value|
|---|---|
|name|ManifestId|
|Gebruik|Optioneel|
|standaardinstelling||
|type|xs:String|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ManifestId" use="optional" default="" type="xs:string">
      <xs:annotation>
        <xs:documentation>The identifier of this application manifest, an unstructured string.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
    
```

## <a name="configoverridesidentifier-attributegroup"></a>ConfigOverridesIdentifier attributeGroup
Hiermee geeft u onderdrukkingen van de configuratie voor een servicepakket.

|Kenmerk|Value|
|---|---|
|content|2-kenmerken|
|name|ConfigOverridesIdentifier|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ConfigOverridesIdentifier">
    <xs:annotation>
      <xs:documentation>Identifies configuration overrides for a service package.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="ServicePackageName" type="xs:string" use="required"/>
    <xs:attribute name="RolloutVersion" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>ID of the rollout in which changes were made to the overrides element.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="servicepackagename"></a>ServicePackageName

|Kenmerk|Value|
|---|---|
|name|ServicePackageName|
|type|xs:String|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ServicePackageName" type="xs:string" use="required"/>
    
```

#### <a name="rolloutversion"></a>RolloutVersion
ID van de implementatie waarin wijzigingen zijn aangebracht in het element onderdrukkingen.

|Kenmerk|Value|
|---|---|
|name|RolloutVersion|
|type|xs:String|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="RolloutVersion" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>ID of the rollout in which changes were made to the overrides element.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="connectionstring-attributegroup"></a>ConnectionString attributeGroup

|Kenmerk|Value|
|---|---|
|content|1 of meer kenmerken|
|name|connectionString|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ConnectionString">
                <xs:attribute name="ConnectionString" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Connection string to the Azure storage account. Format: DefaultEndpointsProtocol=https;AccountName=[];AccountKey=[]</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="connectionstring"></a>connectionString
Verbindingsreeks voor de Azure storage-account. Indeling: DefaultEndpointsProtocol=https;AccountName=[];AccountKey=[]

|Kenmerk|Value|
|---|---|
|name|connectionString|
|type|xs:String|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ConnectionString" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Connection string to the Azure storage account. Format: DefaultEndpointsProtocol=https;AccountName=[];AccountKey=[]</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="containername-attributegroup"></a>ContainerName attributeGroup

|Kenmerk|Value|
|---|---|
|content|1 of meer kenmerken|
|name|ContainerName|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ContainerName">
    <xs:attribute name="ContainerName" type="xs:string">
      <xs:annotation>
        <xs:documentation>The name of the container in Azure blob storage where data is uploaded.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="containername"></a>ContainerName
De naam van de container in Azure blob-opslag waar gegevens worden geüpload.

|Kenmerk|Value|
|---|---|
|name|ContainerName|
|type|xs:String|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ContainerName" type="xs:string">
      <xs:annotation>
        <xs:documentation>The name of the container in Azure blob storage where data is uploaded.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="datadeletionageindays-attributegroup"></a>DataDeletionAgeInDays attributeGroup

|Kenmerk|Value|
|---|---|
|content|1 of meer kenmerken|
|name|DataDeletionAgeInDays|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="DataDeletionAgeInDays">
    <xs:attribute name="DataDeletionAgeInDays" type="xs:string">
      <xs:annotation>
        <xs:documentation>Number of days after which old data is deleted from this location.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="datadeletionageindays"></a>DataDeletionAgeInDays
Aantal dagen waarna oude gegevens vanaf deze locatie wordt verwijderd.

|Kenmerk|Value|
|---|---|
|name|DataDeletionAgeInDays|
|type|xs:String|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="DataDeletionAgeInDays" type="xs:string">
      <xs:annotation>
        <xs:documentation>Number of days after which old data is deleted from this location.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="isenabled-attributegroup"></a>IsEnabled attributeGroup

|Kenmerk|Value|
|---|---|
|content|1 of meer kenmerken|
|name|isEnabled|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="IsEnabled">
                <xs:attribute name="IsEnabled" type="xs:string">
                        <xs:annotation>
                                <xs:documentation>Whether or not data transfer to this destination is enabled. By default, it is not enabled.</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        </xs:attributeGroup>
        
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="isenabled"></a>isEnabled
Overdracht van gegevens naar deze bestemming is wel of niet ingeschakeld. Standaard is deze niet ingeschakeld.

|Kenmerk|Value|
|---|---|
|name|isEnabled|
|type|xs:String|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="IsEnabled" type="xs:string">
                        <xs:annotation>
                                <xs:documentation>Whether or not data transfer to this destination is enabled. By default, it is not enabled.</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        
```

## <a name="levelfilter-attributegroup"></a>LevelFilter attributeGroup

|Kenmerk|Value|
|---|---|
|content|1 of meer kenmerken|
|name|LevelFilter|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="LevelFilter">
    <xs:attribute name="LevelFilter" type="xs:string">
      <xs:annotation>
        <xs:documentation>Level at which ETW events should be filtered. All events at the same or lower level than the specified level are included.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="levelfilter"></a>LevelFilter
Op welke ETW-gebeurtenissen moeten worden gefilterd op niveau. Alle gebeurtenissen op hetzelfde of een lager niveau dan het opgegeven niveau zijn opgenomen.

|Kenmerk|Value|
|---|---|
|name|LevelFilter|
|type|xs:String|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="LevelFilter" type="xs:string">
      <xs:annotation>
        <xs:documentation>Level at which ETW events should be filtered. All events at the same or lower level than the specified level are included.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="namevaluepair-attributegroup"></a>NameValuePair attributeGroup
De naam en waarde die is gedefinieerd als een kenmerk.

|Kenmerk|Value|
|---|---|
|content|2-kenmerken|
|name|NameValuePair|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="NameValuePair">
    <xs:annotation>
      <xs:documentation>Name and Value defined as an attribute.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="Name" use="required">
      <xs:annotation>
        <xs:documentation>The name of the setting to override.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="Value" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>The new value of the setting.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="name"></a>Name
De naam van de instelling voor de onderdrukking.

|Kenmerk|Value|
|---|---|
|name|Name|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Name" use="required">
      <xs:annotation>
        <xs:documentation>The name of the setting to override.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    
```

#### <a name="value"></a>Value
De nieuwe waarde van de instelling.

|Kenmerk|Value|
|---|---|
|name|Value|
|type|xs:String|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Value" type="xs:string" use="required">
      <xs:annotation>
        <xs:documentation>The new value of the setting.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="path-attributegroup"></a>Pad attributeGroup

|Kenmerk|Value|
|---|---|
|content|1 of meer kenmerken|
|name|Pad|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Path">
                <xs:attribute name="Path" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Path to the file share. Format: file:[]</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        </xs:attributeGroup>
        
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="path"></a>Pad
Pad naar de bestandsshare. Indeling: bestand:]

|Kenmerk|Value|
|---|---|
|name|Pad|
|type|xs:String|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Path" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Path to the file share. Format: file:[]</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        
```

## <a name="relativefolderpath-attributegroup"></a>RelativeFolderPath attributeGroup

|Kenmerk|Value|
|---|---|
|content|1 of meer kenmerken|
|name|RelativeFolderPath|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="RelativeFolderPath">
                <xs:attribute name="RelativeFolderPath" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Path to the folder, relative to the application log directory.</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        </xs:attributeGroup>
        
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="relativefolderpath"></a>RelativeFolderPath
Pad naar de map, ten opzichte van de logboekmap voor de toepassing.

|Kenmerk|Value|
|---|---|
|name|RelativeFolderPath|
|type|xs:String|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="RelativeFolderPath" type="xs:string" use="required">
                        <xs:annotation>
                                <xs:documentation>Path to the folder, relative to the application log directory.</xs:documentation>
                        </xs:annotation>
                </xs:attribute>
        
```

## <a name="servicemanifestidentifier-attributegroup"></a>ServiceManifestIdentifier attributeGroup
Hiermee geeft u een servicemanifest.

|Kenmerk|Value|
|---|---|
|content|2-kenmerken|
|name|ServiceManifestIdentifier|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ServiceManifestIdentifier">
    <xs:annotation>
      <xs:documentation>Identifies a service manifest.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="ServiceManifestName" use="required">
      <xs:annotation>
        <xs:documentation>The name of the service manifest this is referenced. The name must match the Name declared in the ServiceManifest element of the service manifest.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="ServiceManifestVersion" use="required">
      <xs:annotation>
        <xs:documentation>The version of the service manifest that is referenced. The version must match the version declared in the service manifest.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="servicemanifestname"></a>ServiceManifestName
De naam van het servicemanifest dat dit waarnaar wordt verwezen. De naam moet overeenkomen met de naam is gedeclareerd in het ServiceManifest-element van het servicemanifest.

|Kenmerk|Value|
|---|---|
|name|ServiceManifestName|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ServiceManifestName" use="required">
      <xs:annotation>
        <xs:documentation>The name of the service manifest this is referenced. The name must match the Name declared in the ServiceManifest element of the service manifest.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    
```

#### <a name="servicemanifestversion"></a>ServiceManifestVersion
De versie van het servicemanifest waarnaar wordt verwezen. De versie moet overeenkomen met de versie die is gedeclareerd in het servicemanifest.

|Kenmerk|Value|
|---|---|
|name|ServiceManifestVersion|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ServiceManifestVersion" use="required">
      <xs:annotation>
        <xs:documentation>The version of the service manifest that is referenced. The version must match the version declared in the service manifest.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
  
```

## <a name="uploadintervalinminutes-attributegroup"></a>UploadIntervalInMinutes attributeGroup

|Kenmerk|Value|
|---|---|
|content|1 of meer kenmerken|
|name|UploadIntervalInMinutes|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="UploadIntervalInMinutes">
    <xs:attribute name="UploadIntervalInMinutes" type="xs:string">
      <xs:annotation>
        <xs:documentation>Interval in minutes at which data is uploaded to this destination.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="uploadintervalinminutes"></a>UploadIntervalInMinutes
Interval in minuten, waarmee gegevens worden geüpload naar deze bestemming.

|Kenmerk|Value|
|---|---|
|name|UploadIntervalInMinutes|
|type|xs:String|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="UploadIntervalInMinutes" type="xs:string">
      <xs:annotation>
        <xs:documentation>Interval in minutes at which data is uploaded to this destination.</xs:documentation>
      </xs:annotation>
    </xs:attribute>
  
```

## <a name="versioneditemattrgroup-attributegroup"></a>VersionedItemAttrGroup attributeGroup
Kenmerkgroep voor versiebeheer secties in ApplicationInstance en ServicePackage documenten.

|Kenmerk|Value|
|---|---|
|content|1 of meer kenmerken|
|name|VersionedItemAttrGroup|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="VersionedItemAttrGroup">
    <xs:annotation>
      <xs:documentation>Attribute group for versioning sections in ApplicationInstance and ServicePackage documents.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="RolloutVersion" type="xs:string" use="required"/>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="rolloutversion"></a>RolloutVersion

|Kenmerk|Value|
|---|---|
|name|RolloutVersion|
|type|xs:String|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="RolloutVersion" type="xs:string" use="required"/>
  
```

## <a name="versionedname-attributegroup"></a>VersionedName attributeGroup
Groep met een naam en een versie van het kenmerk.

|Kenmerk|Value|
|---|---|
|content|2-kenmerken|
|name|VersionedName|

### <a name="xml-source"></a>XML-bron
```xml
<xs:attributeGroup xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="VersionedName">
    <xs:annotation>
      <xs:documentation>Attribute group that includes a Name and a Version.</xs:documentation>
    </xs:annotation>
    <xs:attribute name="Name" use="required">
      <xs:annotation>
        <xs:documentation>Name of the versioned item.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="Version" use="required">
      <xs:annotation>
        <xs:documentation>Version of the versioned item, an unstructured string.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
  </xs:attributeGroup>
  
```
### <a name="attribute-details"></a>Details van kenmerk

#### <a name="name"></a>Name
De naam van het item is samengesteld.

|Kenmerk|Value|
|---|---|
|name|Name|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Name" use="required">
      <xs:annotation>
        <xs:documentation>Name of the versioned item.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    
```

#### <a name="version"></a>Version
De versie van het versienummer item, een niet-gestructureerde tekenreeks.

|Kenmerk|Value|
|---|---|
|name|Version|
|Gebruik|Vereist|

##### <a name="xml-source"></a>XML-bron
```xml
<xs:attribute xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="Version" use="required">
      <xs:annotation>
        <xs:documentation>Version of the versioned item, an unstructured string.</xs:documentation>
      </xs:annotation>
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:minLength value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
  
```

