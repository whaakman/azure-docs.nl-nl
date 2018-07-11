---
title: Hoe u PowerShell gebruiken voor het X.509-certificaten maken | Microsoft Docs
description: Hoe u PowerShell lokaal X.509-certificaten maken en inschakelen van de X.509 op basis van beveiliging in uw Azure-IoT-hub in een gesimuleerde omgeving.
author: dsk-2015
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: dkshir
ms.openlocfilehash: d0063ff79a0bda88fffb486f03286f6784ece7fa
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "34637596"
---
# <a name="powershell-scripts-to-manage-ca-signed-x509-certificates"></a>PowerShell-scripts voor het beheren van CA ondertekende X.509-certificaten

De x.509-certificaat op basis van beveiliging in de IoT-Hub, moet u beginnen met een [X.509-certificaatketen](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification), waaronder het basiscertificaat, evenals alle tussenliggende certificaten accountpagina het leaf-certificaat. Dit *hoe* handleiding vindt u voorbeelden van PowerShell-scripts die gebruikmaken van [OpenSSL](https://www.openssl.org/) te maken en te ondertekenen van X.509-certificaten. We raden u aan deze handleiding gebruiken om te experimenten, omdat veel van deze stappen gebeurt tijdens het fabriceren in de praktijk. U kunt deze certificaten gebruiken voor het simuleren van beveiliging in uw Azure IoT hub met behulp van de *X.509-certificaatverificatie*. De stappen in deze handleiding maken certificaten lokaal op uw Windows-computer. 

## <a name="prerequisites"></a>Vereisten
In deze zelfstudie wordt ervan uitgegaan dat u de binaire bestanden van OpenSSL hebt aangeschaft. Kunt u een
    - de OpenSSL-broncode downloaden en bouwen van de binaire bestanden op uw computer, of 
    - Download en installeer een [OpenSSL binaire bestanden van derden](https://wiki.openssl.org/index.php/Binaries), bijvoorbeeld van [dit project op SourceForge](https://sourceforge.net/projects/openssl/).

<a id="createcerts"></a>

## <a name="create-x509-certificates"></a>X.509-certificaten maken
De volgende stappen ziet u een voorbeeld van het maken van de X.509-basiscertificaten lokaal. 

1. Open een PowerShell-venster als een *beheerder*.  
   **Opmerking:** u moet openen in PowerShell zelf, niet PowerShell ISE, Visual Studio Code of andere hulpprogramma's die het verpakken van de onderliggende PowerShell-console.  Met behulp van een niet-console op basis van PowerShell, leidt dit `openssl` onderstaande verkeerd opdrachten.

2. Navigeer naar uw werkmap. Voer het volgende script om in te stellen van de globale variabelen. 
    ```PowerShell
    $openSSLBinSource = "<full_path_to_the_binaries>\OpenSSL\bin"
    $errorActionPreference    = "stop"

    # Note that these values are for test purpose only
    $_rootCertCommonName      = "Azure IoT Root CA"
    $_rootCertSubject         = "CN=$_rootCertCommonName"
    $_intermediateCertSubject = "Azure IoT Intermediate {0} CA"
    $_privateKeyPassword      = "123"

    $rootCACerFileName          = "./RootCA.cer"
    $rootCAPemFileName          = "./RootCA.pem"
    $intermediate1CAPemFileName = "./Intermediate1.pem"
    $intermediate2CAPemFileName = "./Intermediate2.pem"
    $intermediate3CAPemFileName = "./Intermediate3.pem"

    $openSSLBinDir              = Join-Path $ENV:TEMP "openssl-bin"

    # Whether to use ECC or RSA.
    $useEcc                     = $true
    ```
3. Voer het volgende script waarmee de OpenSSL-binaire bestanden worden gekopieerd naar uw werkmap en stelt de omgevingsvariabelen:

    ```PowerShell
    function Initialize-CAOpenSSL()
    {
        Write-Host ("Beginning copy of openssl binaries to {0} (and setting up env variables...)" -f $openSSLBinDir)
        if (-not (Test-Path $openSSLBinDir))
        {
            mkdir $openSSLBinDir | Out-Null
        }

        robocopy $openSSLBinSource $openSSLBinDir * /s 
        robocopy $openSSLBinSource . * /s 

        Write-Host "Setting up PATH and other environment variables."
        $ENV:PATH += "; $openSSLBinDir"
        $ENV:OPENSSL_CONF = Join-Path $openSSLBinDir "openssl.cnf"

        Write-Host "Success"
    }
    Initialize-CAOpenSSL
    ```
4. Voer vervolgens het volgende script die zoekt naar of een certificaat met de opgegeven *onderwerpnaam* al is geïnstalleerd, en of OpenSSL correct is geconfigureerd op uw computer:
    ```PowerShell
    function Get-CACertBySubjectName([string]$subjectName)
    {
        $certificates = gci -Recurse Cert:\LocalMachine\ |? { $_.gettype().name -eq "X509Certificate2" }
        $cert = $certificates |? { $_.subject -eq $subjectName -and $_.PSParentPath -eq "Microsoft.PowerShell.Security\Certificate::LocalMachine\My" }
        if ($NULL -eq $cert)
        {
            throw ("Unable to find certificate with subjectName {0}" -f $subjectName)
        }
    
        write $cert
    }
    function Test-CAPrerequisites()
    {
        $certInstalled = $null
        try
        {
            $certInstalled = Get-CACertBySubjectName $_rootCertSubject
        }
        catch {}

        if ($NULL -ne $certInstalled)
        {
            throw ("Certificate {0} already installed.  Cleanup CA certs 1st" -f $_rootCertSubject)
        }

        if ($NULL -eq $ENV:OPENSSL_CONF)
        {
            throw ("OpenSSL not configured on this system.  Run 'Initialize-CAOpenSSL' (even if you've already done so) to set everything up.")
        }
        Write-Host "Success"
    }
    Test-CAPrerequisites
    ```
    Als alles correct is geconfigureerd, ziet u "Geslaagd" weergegeven. 

<a id="createcertchain"></a>

## <a name="create-x509-certificate-chain"></a>X.509-certificaatketen maken
Maken van een certificaatketen met een basis-CA, bijvoorbeeld "CN = Azure IoT Root CA ' dat in dit voorbeeld wordt door het volgende PowerShell-script is uitgevoerd. Met dit script werkt ook in het certificaatarchief van uw Windows-besturingssysteem, evenals certificaatbestanden maakt in uw werkmap. 
    1. Het volgende script maakt een PowerShell-functie voor het maken van een zelfondertekend certificaat, voor een bepaalde *onderwerpnaam* en het ondertekenen van instantie. 
    ```PowerShell
    function New-CASelfsignedCertificate([string]$commonName, [object]$signingCert, [bool]$isASigner=$true)
    {
        # Build up argument list
        $selfSignedArgs =@{"-DnsName"=$commonName; 
                           "-CertStoreLocation"="cert:\LocalMachine\My";
                           "-NotAfter"=(get-date).AddDays(30); 
                          }

        if ($isASigner -eq $true)
        {
            $selfSignedArgs += @{"-KeyUsage"="CertSign"; }
            $selfSignedArgs += @{"-TextExtension"= @(("2.5.29.19={text}ca=TRUE&pathlength=12")); }
        }
        else
        {
            $selfSignedArgs += @{"-TextExtension"= @("2.5.29.37={text}1.3.6.1.5.5.7.3.2,1.3.6.1.5.5.7.3.1", "2.5.29.19={text}ca=FALSE&pathlength=0")  }
        }

        if ($signingCert -ne $null)
        {
            $selfSignedArgs += @{"-Signer"=$signingCert }
        }

        if ($useEcc -eq $true)
        {
            $selfSignedArgs += @{"-KeyAlgorithm"="ECDSA_nistP256";
                             "-CurveExport"="CurveName" }
        }

        # Now use splatting to process this
        Write-Host ("Generating certificate {0} which is for prototyping, NOT PRODUCTION.  It will expire in 30 days." -f $subjectName)
        write (New-SelfSignedCertificate @selfSignedArgs)
    }
    ``` 
    2. De volgende PowerShell-functie maakt tussenliggende X.509-certificaten met behulp van de vorige functie, evenals de OpenSSL-binaire bestanden. 
    ```PowerShell
    function New-CAIntermediateCert([string]$commonName, [Microsoft.CertificateServices.Commands.Certificate]$signingCert, [string]$pemFileName)
    {
        $certFileName = ($commonName + ".cer")
        $newCert = New-CASelfsignedCertificate $commonName $signingCert
        Export-Certificate -Cert $newCert -FilePath $certFileName -Type CERT | Out-Null
        Import-Certificate -CertStoreLocation "cert:\LocalMachine\CA" -FilePath $certFileName | Out-Null

        # Store public PEM for later chaining
        openssl x509 -inform deer -in $certFileName -out $pemFileName

        del $certFileName
   
        write $newCert
    }  
    ```
    3. De volgende PowerShell-functie maakt het X.509-certificaatketen. Lezen [ketens van het certificaat](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification) voor meer informatie.
    ```PowerShell
    function New-CACertChain()
    {
        Write-Host "Beginning to install certificate chain to your LocalMachine\My store"
        $rootCACert =  New-CASelfsignedCertificate $_rootCertSubject $null
    
        Export-Certificate -Cert $rootCACert -FilePath $rootCACerFileName  -Type CERT
        Import-Certificate -CertStoreLocation "cert:\LocalMachine\Root" -FilePath $rootCACerFileName

        openssl x509 -inform der -in $rootCACerFileName -out $rootCAPemFileName

        $intermediateCert1 = New-CAIntermediateCert ($_intermediateCertSubject -f "1") $rootCACert $intermediate1CAPemFileName
        $intermediateCert2 = New-CAIntermediateCert ($_intermediateCertSubject -f "2") $intermediateCert1 $intermediate2CAPemFileName
        $intermediateCert3 = New-CAIntermediateCert ($_intermediateCertSubject -f "3") $intermediateCert2 $intermediate3CAPemFileName
        Write-Host "Success"
    }    
    ```
    Dit script maakt u een bestand met de naam *RootCA.cer* in uw werkmap. 
    4. Ten slotte de bovenstaande PowerShell-functies gebruiken om te maken van de X.509-certificaatketen met de opdracht `New-CACertChain` in uw PowerShell-venster. 


<a id="signverificationcode"></a>

## <a name="proof-of-possession-of-your-x509-ca-certificate"></a>Bewijs van eigendom van uw X.509-CA-certificaat

Met dit script voert de *bewijs van bezit* flow voor uw X.509-certificaat. 

Voer de volgende code in het PowerShell-venster op uw bureaublad:
   
   ```PowerShell
   function New-CAVerificationCert([string]$requestedSubjectName)
   {
       $verifyRequestedFileName = ".\verifyCert4.cer"
       $rootCACert = Get-CACertBySubjectName $_rootCertSubject
       Write-Host "Using Signing Cert:::" 
       Write-Host $rootCACert
   
       $verifyCert = New-CASelfsignedCertificate $requestedSubjectName $rootCACert $false

       Export-Certificate -cert $verifyCert -filePath $verifyRequestedFileName -Type Cert
       if (-not (Test-Path $verifyRequestedFileName))
       {
           throw ("Error: CERT file {0} doesn't exist" -f $verifyRequestedFileName)
       }
   
       Write-Host ("Certificate with subject {0} has been output to {1}" -f $requestedSubjectName, (Join-Path (get-location).path $verifyRequestedFileName)) 
   }
   New-CAVerificationCert "<your verification code>"
   ```

Deze code maakt een certificaat met de opgegeven onderwerpnaam, ondertekend door de CA als een bestand met de naam *VerifyCert4.cer* in uw werkmap. Dit certificaatbestand kunt valideren met uw IoT-hub dat u gemachtigd bent de ondertekenen (dat wil zeggen, de persoonlijke sleutel) van deze CA.


<a id="createx509device"></a>

## <a name="create-leaf-x509-certificate-for-your-device"></a>Leaf X.509-certificaat voor uw apparaat maken

In deze sectie ziet dat u een PowerShell-script waarmee u een leaf-certificaat voor apparaten en de bijbehorende certificaatketen maakt kunt gebruiken. 

Voer het volgende script voor het maken van een CA ondertekende X.509-certificaat voor dit apparaat in de PowerShell-venster op uw lokale computer:

   ```PowerShell
   function New-CADevice([string]$deviceName, [string]$signingCertSubject=$_rootCertSubject)
   {
       $newDevicePfxFileName = ("./{0}.pfx" -f $deviceName)
       $newDevicePemAllFileName      = ("./{0}-all.pem" -f $deviceName)
       $newDevicePemPrivateFileName  = ("./{0}-private.pem" -f $deviceName)
       $newDevicePemPublicFileName   = ("./{0}-public.pem" -f $deviceName)
   
       $signingCert = Get-CACertBySubjectName $signingCertSubject ## "CN=Azure IoT CA Intermediate 1 CA"

       $newDeviceCertPfx = New-CASelfSignedCertificate $deviceName $signingCert $false
   
       $certSecureStringPwd = ConvertTo-SecureString -String $_privateKeyPassword -Force -AsPlainText

       # Export the PFX of the cert we've just created.  The PFX is a format that contains both public and private keys.
       Export-PFXCertificate -cert $newDeviceCertPfx -filePath $newDevicePfxFileName -password $certSecureStringPwd
       if (-not (Test-Path $newDevicePfxFileName))
       {
           throw ("Error: CERT file {0} doesn't exist" -f $newDevicePfxFileName)
       }

       # Begin the massaging.  First, turn the PFX into a PEM file which contains public key, private key, and other attributes.
       Write-Host ("When prompted for password by openssl, enter the password as {0}" -f $_privateKeyPassword)
       openssl pkcs12 -in $newDevicePfxFileName -out $newDevicePemAllFileName -nodes

       # Convert the PEM to get formats we can process
       if ($useEcc -eq $true)
       {
           openssl ec -in $newDevicePemAllFileName -out $newDevicePemPrivateFileName
       }
       else
       {
           openssl rsa -in $newDevicePemAllFileName -out $newDevicePemPrivateFileName
       }
       openssl x509 -in $newDevicePemAllFileName -out $newDevicePemPublicFileName
 
       Write-Host ("Certificate with subject {0} has been output to {1}" -f $cnNewDeviceSubjectName, (Join-Path (get-location).path $newDevicePemPublicFileName)) 
   }
   ```

Voer `New-CADevice "<yourTestDevice>"` in uw PowerShell-venster met behulp van de beschrijvende naam die u gebruikt voor het maken van uw apparaat. Wanneer u hierom wordt gevraagd om het wachtwoord voor persoonlijke sleutel van de CA, voer '123'. Hiermee maakt u een  _<yourTestDevice>pfx_ bestand in uw werkmap.

## <a name="clean-up-certificates"></a>Opschonen van certificaten

In de startbalk of **instellingen** app, zoek en selecteer **computercertificaten beheren**. Verwijder certificaten uitgegeven door ** Azure IoT CA TestOnly ***. Deze certificaten moeten aanwezig zijn in de volgende drie locaties: 

* Certificaten - lokale Computer > persoonlijke > certificaten
* Certificaten - lokale Computer > Trusted Root Certification Authorities > certificaten
* Certificaten - lokale Computer > tussenliggende certificeringsinstanties > certificaten

   ![Azure IoT CA TestOnly certificaten verwijderen](./media/iot-hub-security-x509-create-certificates/cleanup.png)
