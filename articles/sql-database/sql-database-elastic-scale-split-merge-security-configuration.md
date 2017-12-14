---
title: Gesplitste samenvoegen Beveiligingsconfiguratie | Microsoft Docs
description: Instellen van x409 certificaten voor codering met de service gesplitste/merge voor elastisch uitschalen.
metakeywords: Elastic Database certificates security
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: f9e89c57-61a0-484f-b787-82dae2349cb6
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 98f046a943c1fe33b421b3ab6830d78e6c4304a0
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="split-merge-security-configuration"></a>Gesplitste samenvoegen Beveiligingsconfiguratie
Als u wilt de splitsing/Merge-service gebruikt, moet u correct beveiliging configureren. De service maakt deel uit van de functie van de elastische Schaalfunctionaliteit van Microsoft Azure SQL Database. Zie voor meer informatie [elastische Scale splitsen en Merge-zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Configureren van certificaten
Certificaten worden op twee manieren geconfigureerd. 

1. [Het SSL-certificaat configureren](#to-configure-the-ssl-certificate)
2. [Voor het configureren van clientcertificaten](#to-configure-client-certificates) 

## <a name="to-obtain-certificates"></a>Om certificaten te verkrijgen
Certificaten kunnen worden opgehaald van openbare certificeringsinstanties (CA's) of van de [Windows certificaatservice](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Dit zijn de aanbevolen methoden om certificaten te verkrijgen.

Als u deze opties niet beschikbaar zijn, kunt u genereren **zelfondertekende certificaten**.

## <a name="tools-to-generate-certificates"></a>Hulpprogramma's voor het genereren van certificaten
* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>De hulpprogramma's uitvoeren
* Zie van ontwikkelaars vanaf de opdrachtprompt voor Visual Studios [Visual Studio-opdrachtprompt](http://msdn.microsoft.com/library/ms229859.aspx) 
  
    Als u hebt geïnstalleerd, gaat u naar:
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* Ophalen van de WDK van [Windows 8.1: Download kits en hulpprogramma's](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>Het SSL-certificaat configureren
Een SSL-certificaat is vereist voor het versleutelen van de communicatie en de server te verifiëren. Kies het meest van toepassing van de onderstaande drie scenario's en alle bijbehorende stappen uitvoeren:

### <a name="create-a-new-self-signed-certificate"></a>Een nieuw zelfondertekend certificaat maken
1. [Een zelfondertekend certificaat maken](#create-a-self-signed-certificate)
2. [PFX-bestand voor zelfondertekende SSL-certificaat maken](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Cloud Service SSL-certificaat uploaden](#upload-ssl-certificate-to-cloud-service)
4. [SSL-certificaat in het configuratiebestand van de Service bijwerken](#update-ssl-certificate-in-service-configuration-file)
5. [SSL-certificeringsinstantie importeren](#import-ssl-certification-authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Een bestaand certificaat uit het certificaatarchief gebruiken
1. [SSL-certificaat exporteren uit het certificaatarchief](#export-ssl-certificate-from-certificate-store)
2. [Cloud Service SSL-certificaat uploaden](#upload-ssl-certificate-to-cloud-service)
3. [SSL-certificaat in het configuratiebestand van de Service bijwerken](#update-ssl-certificate-in-service-configuration-file)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Een bestaand certificaat gebruiken in een PFX-bestand
1. [Cloud Service SSL-certificaat uploaden](#upload-ssl-certificate-to-cloud-service)
2. [SSL-certificaat in het configuratiebestand van de Service bijwerken](#update-ssl-certificate-in-service-configuration-file)

## <a name="to-configure-client-certificates"></a>Voor het configureren van clientcertificaten
Clientcertificaten zijn vereist voor het verifiëren van aanvragen voor de service. Kies het meest van toepassing van de onderstaande drie scenario's en alle bijbehorende stappen uitvoeren:

### <a name="turn-off-client-certificates"></a>Clientcertificaten uitschakelen
1. [Op basis van certificaten clientverificatie uitschakelen](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Nieuwe zelfondertekende certificaten uitgeven
1. [Een zelfondertekend certificeringsinstantie maken](#create-a-self-signed-certification-authority)
2. [Uploaden van de CA-certificaat voor de Cloudservice](#upload-ca-certificate-to-cloud-service)
3. [CA-certificaat in het configuratiebestand van de Service bijwerken](#update-ca-certificate-in-service-configuration-file)
4. [Clientcertificaten uitgeven](#issue-client-certificates)
5. [PFX-bestanden maken voor clientcertificaten](#create-pfx-files-for-client-certificates)
6. [Client-certificaat importeren](#Import-Client-Certificate)
7. [Certificaatvingerafdrukken Client kopiëren](#copy-client-certificate-thumbprints)
8. [Toegestane Clients in het configuratiebestand van de Service configureren](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a>Bestaande clientcertificaten gebruiken
1. [Vinden van openbare sleutel van CA](#find-ca-public-key)
2. [Uploaden van de CA-certificaat voor de Cloudservice](#Upload-CA-certificate-to-cloud-service)
3. [CA-certificaat in het configuratiebestand van de Service bijwerken](#Update-CA-Certificate-in-Service-Configuration-File)
4. [Certificaatvingerafdrukken Client kopiëren](#Copy-Client-Certificate-Thumbprints)
5. [Toegestane Clients in het configuratiebestand van de Service configureren](#configure-allowed-clients-in-the-service-configuration-file)
6. [Controle van certificaatintrekking Client configureren](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Toegestane IP-adressen
Toegang tot de service-eindpunten kan worden beperkt tot specifieke bereiken van IP-adressen.

## <a name="to-configure-encryption-for-the-store"></a>Versleuteling voor de store te configureren
Een certificaat is vereist voor het versleutelen van de referenties die zijn opgeslagen in het metagegevensarchief met. Kies het meest van toepassing van de onderstaande drie scenario's en alle bijbehorende stappen uitvoeren:

### <a name="use-a-new-self-signed-certificate"></a>Een nieuw zelfondertekend certificaat gebruiken
1. [Een zelfondertekend certificaat maken](#create-a-self-signed-certificate)
2. [PFX-bestand voor het versleutelingscertificaat zelfondertekend maken](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Versleutelingscertificaat cloud Service uploaden](#upload-encryption-certificate-to-cloud-service)
4. [Versleutelingscertificaat in het configuratiebestand van de Service bijwerken](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Een bestaand certificaat uit het certificaatarchief gebruiken
1. [Versleutelingscertificaat exporteren uit het certificaatarchief](#export-encryption-certificate-from-certificate-store)
2. [Versleutelingscertificaat cloud Service uploaden](#upload-encryption-certificate-to-cloud-service)
3. [Versleutelingscertificaat in het configuratiebestand van de Service bijwerken](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Een bestaand certificaat gebruiken in een PFX-bestand
1. [Versleutelingscertificaat cloud Service uploaden](#upload-encryption-certificate-to-cloud-service)
2. [Versleutelingscertificaat in het configuratiebestand van de Service bijwerken](#update-encryption-certificate-in-service-configuration-file)

## <a name="the-default-configuration"></a>De standaardconfiguratie
De standaardconfiguratie weigert alle toegang tot het HTTP-eindpunt. Dit is de aanbevolen instelling, aangezien de aanvragen voor deze eindpunten gevoelige informatie, zoals databasereferenties mag uitvoeren.
De standaardconfiguratie kunt alle toegang tot het HTTPS-eindpunt. Deze instelling kan verder worden beperkt.

### <a name="changing-the-configuration"></a>Wijzigen van de configuratie
De groep toegangscontroleregels die gelden voor- en eindpunt zijn geconfigureerd in de  **<EndpointAcls>**  sectie het **serviceconfiguratiebestand**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

De regels in een besturingselement toegangsgroep zijn geconfigureerd in een <AccessControl name=""> gedeelte van het configuratiebestand van de service. 

De indeling wordt uitgelegd in de documentatie voor Network Access Control Lists.
Bijvoorbeeld, zodat alleen IP-adressen in het bereik 100.100.0.0-100.100.255.255 voor toegang tot het HTTPS-eindpunt eruit de regels als volgt:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Denial of service te voorkomen
Er zijn twee verschillende methoden die worden ondersteund om te detecteren en Denial of Service-aanvallen te voorkomen:

* Beperk het aantal gelijktijdige aanvragen per externe host (standaard uitgeschakeld)
* Snelheid van toegang per externe host beperken (op standaard)

Deze zijn gebaseerd op de functies die verder beschreven in de dynamische IP-beveiliging in IIS. Wanneer deze configuratie wijzigen pas op de volgende factoren:

* Het gedrag van proxy's en apparaten via de externe hostinformatie Network Address Translation
* Elke aanvraag naar een resource in de Webrol wordt beschouwd (bijvoorbeeld bij het laden van scripts, afbeeldingen, enzovoort)

## <a name="restricting-number-of-concurrent-accesses"></a>Het aantal gelijktijdige toegang beperken
De instellingen die voor het configureren van dit probleem zijn:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Wijzig DynamicIpRestrictionDenyByConcurrentRequests in waar deze beveiliging in te schakelen.

## <a name="restricting-rate-of-access"></a>Snelheid van de toegang beperken
De instellingen die voor het configureren van dit probleem zijn:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Het antwoord op een geweigerde aanvraag configureren
De volgende instelling configureert u het antwoord op een aanvraag afgewezen:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Raadpleeg de documentatie voor dynamische IP-beveiliging in IIS voor andere ondersteunde waarden.

## <a name="operations-for-configuring-service-certificates"></a>Bewerkingen voor het configureren van servicecertificaten
Dit onderwerp is alleen ter informatie. Volg de configuratiestappen die worden beschreven in:

* De SSL-certificaat configureren
* Clientcertificaten configureren

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken
Uitvoeren:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Om aan te passen:

* -n met de service-URL. Jokertekens ("CN = * .cloudapp .net ') en alternatieve namen (CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net') worden ondersteund.
* e - met de vervaldatum van het certificaat een sterk wachtwoord maken en geef hem wanneer u wordt gevraagd.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>PFX-bestand voor zelf-ondertekend SSL-certificaat maken
Uitvoeren:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Wachtwoord invoeren en exporteer het certificaat met deze opties:

* Ja, de persoonlijke sleutel exporteren
* Alle uitgebreide eigenschappen exporteren

## <a name="export-ssl-certificate-from-certificate-store"></a>SSL-certificaat exporteren uit het certificaatarchief
* Certificaat zoeken
* Klik op Acties -> alle taken -> exporteren...
* Exporteer het certificaat in een. PFX-bestand met deze opties:
  * Ja, de persoonlijke sleutel exporteren
  * Indien mogelijk exporteren met alle certificaten in het certificeringspad * alle uitgebreide eigenschappen exporteren

## <a name="upload-ssl-certificate-to-cloud-service"></a>SSL-certificaat uploaden naar cloudservice
Uploaden van het certificaat met de bestaande of gegenereerd. PFX-bestand met de SSL-sleutelpaar:

* Voer het wachtwoord voor het beveiligen van gegevens van de persoonlijke sleutel

## <a name="update-ssl-certificate-in-service-configuration-file"></a>SSL-certificaat in het configuratiebestand van de service bijwerken
Werk de vingerafdrukwaarde van de volgende instelling in het configuratiebestand van de service met de vingerafdruk van het certificaat dat is geüpload naar de cloudservice:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>SSL-certificeringsinstantie importeren
Volg deze stappen in alle account/machine die communiceert met de service:

* Dubbelklik op de. CER-bestand in Windows Verkenner
* Klik in het dialoogvenster certificaat certificaat installeren...
* Certificaat importeren in het archief Vertrouwde basiscertificeringsinstanties

## <a name="turn-off-client-certificate-based-authentication"></a>Op basis van certificaten clientverificatie uitschakelen
Alleen op basis van certificaten clientverificatie wordt ondersteund en u dit uitschakelt wilt toestaan voor openbare toegang tot de service-eindpunten, tenzij andere beveiligingsmechanismen geïmplementeerd (zoals Microsoft Azure Virtual Network) zijn.

Deze instellingen wijzigen in false in het serviceconfiguratiebestand de functie om uit te schakelen:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Kopieer de vingerafdruk van het hetzelfde als het SSL-certificaat in de instelling voor CA-certificaat:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Een zelfondertekend certificeringsinstantie maken
De volgende stappen voor het maken van een zelfondertekend certificaat om te fungeren als een certificeringsinstantie worden uitgevoerd:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Aanpassen

* e - met de vervaldatum van de certificeringsinstantie

## <a name="find-ca-public-key"></a>Vinden van openbare sleutel van CA
Alle clientcertificaten zijn uitgegeven door een certificeringsinstantie wordt vertrouwd door de service. De openbare sleutel aan de certificeringsinstantie die certificaten de client die u gebruikt voor verificatie om te uploaden naar de cloudservice vinden.

Als het bestand met de openbare sleutel niet beschikbaar is, uit het certificaatarchief exporteren:

* Certificaat zoeken
  * Zoeken naar een clientcertificaat dat is uitgegeven door de dezelfde certificeringsinstantie
* Dubbelklik op het certificaat.
* Selecteer het tabblad certificeringspad in het dialoogvenster certificaat.
* Dubbelklik op de CA-vermelding in het pad.
* Notities van de eigenschappen voor certificaat.
* Sluit de **certificaat** dialoogvenster.
* Certificaat zoeken
  * Zoeken naar de CA die hierboven vermeld.
* Klik op Acties -> alle taken -> exporteren...
* Exporteer het certificaat in een. CER met deze opties:
  * **Nee, de persoonlijke sleutel niet exporteren**
  * Indien mogelijk exporteren met alle certificaten in het certificeringspad.
  * Alle uitgebreide eigenschappen exporteren.

## <a name="upload-ca-certificate-to-cloud-service"></a>CA-certificaat uploaden naar cloudservice
Uploaden van het certificaat met de bestaande of gegenereerd. CER-bestand met de openbare sleutel van de CA.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Update-CA-certificaat in het configuratiebestand van de service
Werk de vingerafdrukwaarde van de volgende instelling in het configuratiebestand van de service met de vingerafdruk van het certificaat dat is geüpload naar de cloudservice:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Werk de waarde van de volgende instelling met dezelfde vingerafdruk:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Clientcertificaten uitgeven
Elke gebruiker die toegang hebben tot de service moet een certificaat uitgegeven voor his/hers exclusieve gebruik hebben en his/hers eigenaar sterk wachtwoord op ter beveiliging van de persoonlijke sleutel moet kiezen. 

De volgende stappen moeten worden uitgevoerd in dezelfde machine waar het zelfondertekende certificaat van de CA is gegenereerd en worden opgeslagen:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Aanpassen:

* -n met een ID voor het naar de client die met dit certificaat wordt geverifieerd
* e - met de vervaldatum van certificaat
* MyID.pvk en MyID.cer met unieke bestandsnamen voor dit clientcertificaat

Met deze opdracht wordt gevraagd om een wachtwoord worden gemaakt en vervolgens één keer worden gebruikt. Gebruik een sterk wachtwoord.

## <a name="create-pfx-files-for-client-certificates"></a>PFX-bestanden voor client-certificaten maken
Voor elk certificaat gegenereerde client uitvoeren:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Aanpassen:

    MyID.pvk and MyID.cer with the filename for the client certificate

Wachtwoord invoeren en exporteer het certificaat met deze opties:

* Ja, de persoonlijke sleutel exporteren
* Alle uitgebreide eigenschappen exporteren
* De persoon aan wie dit certificaat wordt uitgegeven moet de export-wachtwoord kiezen

## <a name="import-client-certificate"></a>Client-certificaat importeren
Elke gebruiker voor wie een clientcertificaat is uitgegeven, moet het sleutelpaar in de machines die hij wordt gebruikt voor communicatie met de service importeren:

* Dubbelklik op de. PFX-bestand in Windows Verkenner
* Importeren in het persoonlijk certificaatarchief met ten minste deze optie:
  * Alle uitgebreide eigenschappen ingeschakeld

## <a name="copy-client-certificate-thumbprints"></a>Certificaatvingerafdrukken client kopiëren
Elke gebruiker voor wie een clientcertificaat is uitgegeven, moet deze stappen volgen om te kunnen verkrijgen van de vingerafdruk van his/hers certificaat dat wordt toegevoegd aan het configuratiebestand van de service:

* Certmgr.exe uitvoeren
* Selecteer het tabblad Persoonlijk
* Dubbelklik op het certificaat moet worden gebruikt voor verificatie
* Selecteer het tabblad met Details in het dialoogvenster certificaat dat wordt geopend.
* Controleer of dat weergeven alle wordt weergegeven
* Selecteer het veld met de vingerafdruk van de naam in de lijst
* Kopieer de waarde van de vingerafdruk van het ** verwijderen van niet-zichtbare Unicode-tekens voor het eerste cijfer ** verwijdert alle spaties

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Toegestane clients in het configuratiebestand van de service configureren
Werk de waarde van de volgende instelling in het configuratiebestand van de service met een door komma's gescheiden lijst met de vingerafdrukken van de clientcertificaten die toegang hebben tot de service:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Controle van certificaatintrekking client configureren
De standaardinstelling wordt niet gecontroleerd met de certificeringsinstantie voor de clientstatus certificaat intrekken. U schakelt de controles als de certificeringsinstantie die certificaten hebben verleend de client deze controles ondersteunt door de volgende instelling te wijzigen met een van de waarden die zijn gedefinieerd in de opsomming X509RevocationMode:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>PFX-bestand voor zelfondertekende versleutelingscertificaten maken
Voor een versleutelingscertificaat uitvoeren:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Aanpassen:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Wachtwoord invoeren en exporteer het certificaat met deze opties:

* Ja, de persoonlijke sleutel exporteren
* Alle uitgebreide eigenschappen exporteren
* U moet het wachtwoord tijdens het uploaden van het certificaat naar de cloudservice.

## <a name="export-encryption-certificate-from-certificate-store"></a>Versleutelingscertificaat exporteren uit het certificaatarchief
* Certificaat zoeken
* Klik op Acties -> alle taken -> exporteren...
* Exporteer het certificaat in een. PFX-bestand met deze opties: 
  * Ja, de persoonlijke sleutel exporteren
  * Indien mogelijk exporteren met alle certificaten in het certificeringspad 
* Alle uitgebreide eigenschappen exporteren

## <a name="upload-encryption-certificate-to-cloud-service"></a>Versleutelingscertificaat uploaden naar cloudservice
Uploaden van het certificaat met de bestaande of gegenereerd. PFX-bestand met het sleutelpaar van versleuteling:

* Voer het wachtwoord voor het beveiligen van gegevens van de persoonlijke sleutel

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Versleutelingscertificaat in het configuratiebestand van de service bijwerken
Werk de vingerafdrukwaarde van de volgende instellingen in het configuratiebestand van de service met de vingerafdruk van het certificaat dat is geüpload naar de cloudservice:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Algemene bewerkingen van het certificaat
* De SSL-certificaat configureren
* Clientcertificaten configureren

## <a name="find-certificate"></a>Certificaat zoeken
Volg deze stappen:

1. Mmc.exe worden uitgevoerd.
2. File -> module toevoegen/verwijderen...
3. Selecteer **certificaten**.
4. Klik op **Add**.
5. Kies de locatie van certificaat store.
6. Klik op **Voltooien**.
7. Klik op **OK**.
8. Vouw **certificaten**.
9. Vouw het knooppunt van de store certificaat.
10. Vouw het knooppunt van de onderliggende certificaat.
11. Selecteer een certificaat in de lijst.

## <a name="export-certificate"></a>Certificaat exporteren
In de **Export Wizard certificaat**:

1. Klik op **Volgende**.
2. Selecteer **Ja**, klikt u vervolgens **de persoonlijke sleutel exporteren**.
3. Klik op **Volgende**.
4. Selecteer de gewenste uitvoer-bestandsindeling.
5. Controleer de gewenste opties.
6. Controleer **wachtwoord**.
7. Voer een sterk wachtwoord in en Bevestig het.
8. Klik op **Volgende**.
9. Typ of blader een bestandsnaam waar het certificaat wordt opgeslagen (gebruik een. PFX-extensie).
10. Klik op **Volgende**.
11. Klik op **Voltooien**.
12. Klik op **OK**.

## <a name="import-certificate"></a>Certificaat importeren
In de Wizard Certificaat importeren:

1. Selecteer de locatie van de store.
   
   * Selecteer **huidige gebruiker** als alleen de processen die worden uitgevoerd onder de huidige gebruiker heeft toegang tot de service
   * Selecteer **lokale Machine** als andere processen in deze computer toegang de service tot heeft
2. Klik op **Volgende**.
3. Als uit een bestand importeren, controleert u het bestandspad.
4. Als u deze importeert een. PFX-bestand:
   1. Voer het wachtwoord voor het beveiligen van de persoonlijke sleutel
   2. Selecteer opties voor importeren
5. Selecteer "Locatie" certificaten in het onderstaande archief opslaan
6. Klik op **Bladeren**.
7. Selecteer de gewenste winkel.
8. Klik op **Voltooien**.
   
   * Als het archief Vertrouwde basiscertificeringsinstanties hebt gekozen, klikt u op **Ja**.
9. Klik op **OK** op alle vensters van het dialoogvenster.

## <a name="upload-certificate"></a>Certificaat uploaden
In de [Azure-portal](https://portal.azure.com/)

1. Selecteer **Cloudservices**.
2. Selecteer de cloudservice.
3. Klik op het bovenste menu **certificaten**.
4. Klik op de balk onderaan **uploaden**.
5. Selecteer het certificaatbestand.
6. Als het is een. PFX-bestand, typ het wachtwoord voor de persoonlijke sleutel.
7. Als voltooid, kopieert u de vingerafdruk van het certificaat van de nieuwe vermelding in de lijst.

## <a name="other-security-considerations"></a>Andere beveiligingsoverwegingen
De SSL-instellingen in dit document beschreven versleutelen van communicatie tussen de service en de clients wanneer het HTTPS-eindpunt wordt gebruikt. Dit is belangrijk omdat de referenties voor toegang tot de database en mogelijk andere vertrouwelijke informatie zijn opgenomen in de communicatie. Let echter op dat de service zich blijft interne status voordoen, inclusief referenties, in de interne tabellen in de Microsoft Azure SQL-database die u hebt opgegeven voor de metagegevensopslag van in uw Microsoft Azure-abonnement. Deze database is gedefinieerd als onderdeel van de volgende instelling in uw serviceconfiguratiebestand (. CSCFG-bestand): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

In deze database opgeslagen referenties zijn versleuteld. Echter, als een best practice, zorg ervoor dat web- en werkrollen rollen van uw service-implementaties actueel worden gehouden en veilig zijn als ze beide toegang tot de database met metagegevens en het certificaat dat wordt gebruikt voor versleuteling en ontsleuteling van opgeslagen referenties hebben. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

