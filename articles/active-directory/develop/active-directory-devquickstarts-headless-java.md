---
title: Azure AD-Java-opdrachtregel aan de slag | Microsoft Docs
description: Het bouwen van een Java-opdrachtregel-app die gebruikers zich aanmeldt voor toegang tot een API.
services: active-directory
documentationcenter: java
author: navyasric
manager: mtillman
editor: 
ms.assetid: 51e1a8f9-6ff0-4643-a350-0ba794e26fd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 01/23/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 2330ccf734944a8a563f9031a9d51902255c30d5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Gebruik van Java-opdrachtregel-App voor toegang tot een API met Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure AD kunt u eenvoudig en snel te uitbesteden identiteitsbeheer van uw web-app, bieden één aanmelden en afmelden met slechts een paar regels code.  In Java-web-apps, kunt u dit doen met behulp van Microsoft implementatie van de community aangestuurde ADAL4J.

  We gebruiken hier ADAL4J naar:

* Meld u aan de gebruiker in de app gebruikmaken van Azure AD als de id-provider.
* Sommige informatie over de gebruiker weergegeven.
* Meld u aan de gebruiker buiten de app.

Om dit te doen, moet u het:

1. Een toepassing registreren met Azure AD
2. Uw app instellen voor gebruik van de bibliotheek ADAL4J.
3. De bibliotheek ADAL4J gebruiken om aan- en afmeldingsaanvragen te verzenden naar Azure AD.
4. Gegevens over de gebruiker te drukken.

Aan de slag [de basis van de app downloaden](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) of [het voltooide voorbeeld downloaden](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).  U moet ook een Azure AD-tenant waarin u uw toepassing registreren.  Als u niet hebt, [Lees hoe u een](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1.  Een toepassing registreren met Azure AD
Om uw app om gebruikers te verifiëren, moet u eerst een nieuwe toepassing registreren in uw tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op de bovenste balk op je account en klikt u onder de **Directory** kiest u de Active Directory-tenant waar u wilt uw toepassing registreren.
3. Klik op **meer Services** in de nav linkerkant en kies **Azure Active Directory**.
4. Klik op **App registraties** en kies **toevoegen**.
5. Volg de aanwijzingen en maak een nieuwe **webtoepassing en/of WebAPI**.
  * De **naam** van de toepassing bevat een beschrijving van uw toepassing aan eindgebruikers
  * De **aanmeldings-URL** is de basis-URL van uw app.  Het basisproject standaardwaarde is `http://localhost:8080/adal4jsample/`.
6. Zodra u inschrijving hebt voltooid, toewijst AAD uw app een unieke id  U moet deze waarde in de volgende secties, dus kopiëren vanaf het toepassingstabblad.
7. Van de **instellingen** -> **eigenschappen** pagina voor uw toepassing, het bijwerken van de App ID URI. De **App ID URI** is de unieke id voor uw toepassing.  De overeenkomst is met `https://<tenant-domain>/<app-name>`, bijvoorbeeld `http://localhost:8080/adal4jsample/`.

Eenmaal in de portal voor uw app maakt een **sleutel** van de **instellingen** pagina voor uw toepassing en kopieer het naar beneden.  U hebt dit zo direct nodig.

## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. Uw app instellen voor gebruik van de bibliotheek ADAL4J en vereisten met behulp van Maven
Hier geconfigureerd ADAL4J voor het gebruik van het OpenID Connect-verificatieprotocol.  ADAL4J wordt gebruikt op aan- en afmeldingsaanvragen te verzenden en informatie ophalen over de gebruiker, onder andere de gebruikerssessie te beheren.

* In de hoofdmap van uw project openen/maken `pom.xml` en zoek de `// TODO: provide dependencies for Maven` en vervangen door het volgende:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. Het bestand Java PublicClient maken
Zoals hierboven vermeld we gebruiken de Graph API om gegevens over de aangemelde gebruiker te verkrijgen. Voor dit eenvoudig ons moet maken we beide een bestand vertegenwoordigt een **mapobject** en een afzonderlijk bestand vertegenwoordigt de **gebruiker** zodat het patroon OO van Java kan worden gebruikt.

* Maken van een bestand met de naam `DirectoryObject.java` die zullen worden gebruikt voor het opslaan van elementaire gegevens over een DirectoryObject (u kunt gerust dit later gebruiken voor andere grafiek query's kunt u doen). U kunt knippen en plakken dit uit het volgende:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


## <a name="compile-and-run-the-sample"></a>Compileren en uitvoeren van de steekproef
Wijzig weer uit in de hoofdmap en voer de volgende opdracht voor het bouwen van de steekproef u plaatsen samen met `maven`. Hiermee worden gebruikt. de `pom.xml` bestand dat u voor afhankelijkheden hebt geschreven.

`$ mvn package`

U hebt nu een `adal4jsample.war` bestand uw `/targets` directory. U kunt implementeren die in uw Tomcat-container en Ga naar de URL 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> Het is heel eenvoudig een WAR met de meest recente Tomcat-servers implementeren. Gewoon gaat u naar `http://localhost:8080/manager/` en volg de instructies over het uploaden van uw '' adal4jsample.war' bestand. Deze autodeploy wordt voor u met het juiste eindpunt.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Gefeliciteerd. U hebt nu een werkende Java-toepassing heeft de mogelijkheid om gebruikers te verifiëren, veilig aanroepen van Web-API's met behulp van OAuth 2.0 en algemene informatie over de gebruiker ophalen.  Als u nog niet gedaan hebt, is nu de tijd voor het vullen van uw tenant waarbij sommige gebruikers.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) [is opgegeven als een ZIP hier](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), of u kunt dit ook klonen vanuit GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

