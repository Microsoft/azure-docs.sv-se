---
ms.openlocfilehash: f2e4bf603fa4cfb93c7ca51f64029ccaedcff727
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103021612"
---
## <a name="add-managed-identity-to-your-communication-services-solution-java"></a>Lägg till hanterad identitet i kommunikations tjänst lösningen (Java)

### <a name="install-the-client-library-packages"></a>Installera klient biblioteks paketen
I pom.xml-filen lägger du till följande beroende element i gruppen med beroenden.

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-identity</artifactId>
    <version>1.0.0-beta.6</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-sms</artifactId>
    <version>1.0.0-beta.4</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-identity</artifactId>
    <version>1.2.3</version>
</dependency>
```

### <a name="use-the-client-library-packages"></a>Använda klient biblioteks paketen

Lägg till följande `import` direktiv i koden för att använda klient biblioteken Azure-identitet och Azure-kommunikation.

```java
import com.azure.communication.common.*;
import com.azure.communication.identity.*;
import com.azure.communication.identity.models.*;
import com.azure.communication.sms.*;
import com.azure.core.credential.*;
import com.azure.core.http.*;
import com.azure.core.http.netty.*;
import com.azure.identity.*;

import java.io.IOException;
import java.util.*;
```

Exemplen nedan använder [DefaultAzureCredential](/java/api/azure.identity.defaultazurecredential). Den här autentiseringsuppgiften är lämplig för produktions-och utvecklings miljöer.

`AZURE_CLIENT_SECRET``AZURE_CLIENT_ID`och `AZURE_TENANT_ID` miljövariabler krävs för att skapa ett `DefaultAzureCredential` objekt. Om du vill skapa ett registrerat program i utvecklings miljön och konfigurera miljövariabler, se [bevilja åtkomst med hanterad identitet](../managed-identity-from-cli.md).

### <a name="create-an-identity-and-issue-a-token-with-managed-identity"></a>Skapa en identitet och utfärda en token med hanterad identitet

I följande kod exempel visas hur du skapar ett tjänst klient objekt med hanterad identitet.
Använd sedan klienten för att utfärda en token för en ny användare:

```java
     public AccessToken createIdentityAndGetTokenAsync() {
          // You can find your endpoint and access key from your resource in the Azure portal
          String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();
          TokenCredential credential = new DefaultAzureCredentialBuilder().build();

          CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
               .endpoint(endpoint)
               .credential(credential)
               .httpClient(httpClient)
               .buildClient();

          CommunicationUserIdentifier user = communicationIdentityClient.createUser();
          AccessToken userToken = communicationIdentityClient.getToken(user, new ArrayList<>(Arrays.asList(CommunicationTokenScope.CHAT)));
          return userToken;
     }
```

### <a name="send-an-sms-with-managed-identity"></a>Skicka ett SMS med hanterad identitet

Följande kod exempel visar hur du skapar ett tjänst klient objekt med hanterad identitet och sedan använder klienten för att skicka ett SMS-meddelande:

```java
     public SendSmsResponse sendSms() {
          // You can find your endpoint and access key from your resource in the Azure portal
          String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

          HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();
          TokenCredential credential = new DefaultAzureCredentialBuilder().build();

          SmsClient smsClient = new SmsClientBuilder()
               .endpoint(endpoint)
               .credential(credential)
               .httpClient(httpClient)
               .buildClient();

          // Send the message and check the response for a message id
          SmsSendResult response = smsClient.send(
               "<from-phone-number>",
               "<to-phone-number>",
               "your message"
          );

          return response;
    }
```

