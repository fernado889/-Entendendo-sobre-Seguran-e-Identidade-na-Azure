# -Entendendo-sobre-Seguran-e-Identidade-na-Azure
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>microsoft-authentication-library</artifactId>
    <version>1.11.0</version>
</dependency>
import com.microsoft.aad.msal4j.*;

import java.net.MalformedURLException;
import java.util.Collections;
import java.util.concurrent.CompletableFuture;

public class AzureAuthExample {
    private static final String CLIENT_ID = "YOUR_CLIENT_ID"; // ID do seu aplicativo
    private static final String CLIENT_SECRET = "YOUR_CLIENT_SECRET"; // Segredo do seu aplicativo
    private static final String TENANT_ID = "YOUR_TENANT_ID"; // ID do seu locatário
    private static final String AUTHORITY = "https://login.microsoftonline.com/" + TENANT_ID;
    private static final String SCOPE = "https://graph.microsoft.com/.default"; // Escopo de acesso

    public static void main(String[] args) {
        try {
            String token = getAccessToken();
            System.out.println("Token de acesso: " + token);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static String getAccessToken() throws MalformedURLException {
        ConfidentialClientApplication app = ConfidentialClientApplication.builder(CLIENT_ID, ClientCredentialFactory.createFromSecret(CLIENT_SECRET))
                .authority(AUTHORITY)
                .build();

        ClientCredentialParameters parameters = ClientCredentialParameters.builder(
                Collections.singleton(SCOPE))
                .build();

        CompletableFuture<IAuthenticationResult> future = app.acquireToken(parameters);
        IAuthenticationResult result = future.join();

        return result.accessToken();
    }
}
