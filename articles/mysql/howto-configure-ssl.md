---
title: SSL-verbindingen veilig verbinding kunnen maken met Azure-Database voor MySQL configureren
description: Instructies voor het correcte configuratie van Azure-Database voor MySQL en de bijbehorende toepassingen juist gebruik van SSL-verbindingen
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: ea4fd90670c2137fbe6127ba5c0b7641d846f407
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265424"
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>SSL-verbindingen in uw toepassing veilig verbinding kunnen maken met Azure-Database voor MySQL configureren
Azure MySQL-Database ondersteunt verbindingen van uw Azure-Database voor de MySQL-server met clienttoepassingen met Secure Sockets Layer (SSL). Het afdwingen van SSL-verbindingen tussen uw databaseserver en clienttoepassingen zorgt dat u bent beschermt tegen 'man in the middle'-aanvallen omdat de gegevensstroom tussen de server en uw toepassing wordt versleuteld.

## <a name="step-1-obtain-ssl-certificate"></a>Stap 1: SSL-certificaat verkrijgen
Download het certificaat nodig om te communiceren via SSL met uw Azure-Database voor de server uit een MySQL [ https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem ](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) en sla het certificaatbestand op de lokale schijf (in deze zelfstudie gebruikt c:\ssl bijvoorbeeld).
**Voor Microsoft Internet Explorer en Microsoft Edge:** nadat het downloaden is voltooid, wijzig de naam van het certificaat naar BaltimoreCyberTrustRoot.crt.pem.

## <a name="step-2-bind-ssl"></a>Stap 2: SSL is gekoppeld
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>Verbinding maken met server met behulp van de MySQL-Workbench via SSL
Configureer de MySQL Workbench veilig verbinding te maken via SSL. Navigeer via de dialoog nieuwe Setup-verbinding naar de **SSL** tabblad. In de **SSL CA bestand:** en voer de locatie van het bestand van de **BaltimoreCyberTrustRoot.crt.pem**. 
![Sla aangepaste tegel](./media/howto-configure-ssl/mysql-workbench-ssl.png) voor bestaande verbindingen kunt u SSL binden door met de rechtermuisknop op het verbindingspictogram en kiest u bewerken. Navigeer naar de **SSL** tabblad en verbindt u het certificaatbestand.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>Verbinding maken met server met behulp van de CLI MySQL via SSL
De SSL-certificaat binden op een andere manier is met de opdrachtregelinterface MySQL door het uitvoeren van de volgende opdracht:
```dos
mysql.exe -h mydemoserver.mysql.database.azure.com -u Username@mydemoserver -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>Stap 3: Afdwingen van SSL-verbindingen in Azure 
### <a name="using-the-azure-portal"></a>Azure Portal gebruiken
Met de Azure portal, gaat u naar uw Azure-Database voor de MySQL-server en klik vervolgens op **verbindingsbeveiliging**. Gebruik de wisselknop inschakelen of uitschakelen de **afdwingen SSL-verbinding** instellen en klik vervolgens op **opslaan**. Microsoft raadt aan om in te schakelen altijd de **afdwingen SSL-verbinding** instellen voor een betere beveiliging.
![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Azure CLI gebruiken
U kunt inschakelen of uitschakelen de **ssl-afdwinging** parameter waarden ingeschakeld of uitgeschakeld door respectievelijk te gebruiken in Azure CLI.
```azurecli-interactive
az mysql server update --resource-group myresource --name mydemoserver --ssl-enforcement Enabled
```

## <a name="step-4-verify-the-ssl-connection"></a>Stap 4: De SSL-verbinding controleren
Uitvoeren van de mysql **status** om te controleren of u verbinding hebt gemaakt met uw MySQL-server via SSL:
```dos
mysql> status
```
Controleer de verbinding is versleuteld aan de hand van de uitvoer, die moet worden weergegeven: **SSL: Cipher in gebruik is AES256 SHA** 

## <a name="sample-code"></a>Voorbeeldcode
Voor het opzetten van een beveiligde verbinding met Azure-Database voor MySQL via SSL van uw toepassing, raadpleegt u de volgende codevoorbeelden:

### <a name="php"></a>PHP
```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'mydemoserver.mysql.database.azure.com', 'myadmin@mydemoserver', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python-mysqlconnector-python"></a>Python (MySQLConnector Python)
```python
try:
    conn=mysql.connector.connect(user='myadmin@mydemoserver', 
        password='yourpassword', 
        database='quickstartdb', 
        host='mydemoserver.mysql.database.azure.com', 
        ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
    print(err)
```
### <a name="python-pymysql"></a>Python (PyMySQL)
```python
conn = pymysql.connect(user = 'myadmin@mydemoserver', 
        password = 'yourpassword', 
        database = 'quickstartdb', 
        host = 'mydemoserver.mysql.database.azure.com', 
        ssl = {'ssl': {'ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}})
```
### <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(
        :host     => 'mydemoserver.mysql.database.azure.com', 
        :username => 'myadmin@mydemoserver',      
        :password => 'yourpassword',    
        :database => 'quickstartdb',
        :ssl_ca => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
    )
```
### <a name="golang"></a>Golang
```go
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("/var/www/html/BaltimoreCyberTrustRoot.crt.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool, InsecureSkipVerify: true})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin@mydemoserver' , 'yourpassword', 'mydemoserver.mysql.database.azure.com', 'quickstartdb')   
db, _ := sql.Open("mysql", connectionString)
```
### <a name="javajdbc"></a>JAVA(JDBC)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```
### <a name="javamariadb"></a>JAVA(MariaDB)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

## <a name="next-steps"></a>Volgende stappen
Bekijk verschillende toepassing connectiviteitsopties na [verbindingsbibliotheken voor Azure-Database voor MySQL](concepts-connection-libraries.md)
