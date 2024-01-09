---
lab:
  title: 03 – Konfigurieren Ihres Mandanten für Microsoft Entra Verified ID
  module: Module 01 - Manage Identity and Access
---
# Lab 03: Konfigurieren Ihres Mandanten für Microsoft Entra Verified ID
# Lab-Handbuch für Kursteilnehmer

## Labszenario

# Konfigurieren Ihres Mandanten für Microsoft Entra Verified ID

>[!Note] 
> Microsoft Entra Verified ID, ein Feature der Microsoft Entra-Produktfamilie, ersetzt jetzt Azure Active Directory-Nachweise. Erfahren Sie mehr über die [Microsoft Entra-Familie](https://aka.ms/EntraAnnouncement) mit Lösungen für die Identitätsverwaltung, und beginnen Sie im [einheitlichen Microsoft Entra Admin Center](https://entra.microsoft.com).

Microsoft Entra Verified ID ist eine dezentrale Identitätslösung, mit der Sie Ihre Organisation schützen können. Mit dem Dienst können Sie Nachweise ausgeben und überprüfen. Aussteller können den Dienst Verified ID verwenden, um eigene benutzerdefinierte Nachweise auszugeben. Prüfer können die kostenlose REST-API des Diensts verwenden, um Nachweise in Apps und Diensten auf einfache Weise anzufordern und zu akzeptieren. In beiden Fällen muss Ihr Azure AD-Mandant so konfiguriert werden, dass entweder Ihre eigenen Nachweise ausgestellt werden oder die Präsentation der von einem Drittanbieter ausgestellten Nachweise eines Benutzers überprüft wird. Wenn Sie sowohl als Aussteller als auch als Prüfer fungieren, können Sie einen einzigen Azure AD-Mandanten verwenden, um Ihre eigenen Nachweise auszustellen und die Nachweise von Dritten zu überprüfen.

In diesem Tutorial erfahren Sie, wie Sie Ihren Azure AD-Mandanten so konfigurieren, dass er diesen Nachweisdienst verwenden kann.

Dabei wird insbesondere Folgendes vermittelt:

> - Erstellen einer Azure Key Vault-Instanz
> - Einrichten des Verified ID-Diensts
> - Registrieren einer Anwendung in Azure AD
> - Verifizieren des Domänenbesitzes für den dezentralen Bezeichner (DID)

Das folgende Diagramm veranschaulicht die Microsoft Entra Verified ID-Architektur und die von Ihnen konfigurierte Komponente.

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/8d4d01c2-3110-421a-91a8-7b052bc8d793)


## Voraussetzungen

- Vergewissern Sie sich, dass Sie über die Berechtigung Globaler Administrator oder Authentifizierungsrichtlinienadministrator für das zu konfigurierende Verzeichnis verfügen. Wenn Sie nicht der globale Administrator sind, benötigen Sie die Berechtigung Anwendungsadministrator, um die App-Registrierung einschließlich der Erteilung der Administratoreinwilligung abschließen zu können.
- Stellen Sie sicher, dass Sie für das Azure-Abonnement bzw. für die Ressourcengruppe, in dem/der Sie Azure Key Vault bereitstellen möchten, über die Rolle Mitwirkender verfügen.

## Erstellen eines Schlüsseltresors

Azure Key Vault ist ein Clouddienst, der das sichere Speichern von Geheimnissen und Schlüsseln und den sicheren Zugriff darauf ermöglicht. Der Nachweisdienst speichert öffentliche und private Schlüssel in Azure Key Vault. Diese Schlüssel werden zum Signieren und Überprüfen von Nachweisen verwendet.

### Verwenden Sie das Azure-Portal, um einen Azure Key Vault zu erstellen.

1. Starten Sie eine Browsersitzung, und melden Sie sich beim [Azure-Portal-Menü](https://portal.azure.com/) an.
   
2. Geben Sie im Suchfeld im Azure-Portal den Wert **Key Vault** ein.

3. Wählen Sie in der Liste „Ergebnisse“ die Option **Key Vault** aus.

4. Klicken Sie im Abschnitt „Schlüsseltresore“ auf **Erstellen**.

5. Geben Sie auf der Registerkarte **Grundlagen** unter **Schlüsseltresor erstellen** diese Informationen ein, oder wählen Sie diese aus:
   
   |Einstellung|Wert|
   |---|---|
   |**Projektdetails**|
   |Subscription|Wählen Sie Ihr Abonnement aus.|
   |Resource group|Geben Sie **azure-rg-1** ein. Klicken Sie auf **OK**.|
   |**Instanzendetails**|
   |Name des Schlüsseltresors|Geben Sie **Contoso-vault2** ein.|
   |Region|Wählen Sie **USA, Osten** aus.|
   |Tarif|Systemstandard **Standard**|
   |Aufbewahrungsdauer für gelöschte Tresore in Tagen|Systemstandard **90**|

6. Wählen Sie die Registerkarte **Überprüfen + erstellen** aus, oder wählen Sie unten auf der Seite die blaue Schaltfläche „Überprüfen + Erstellen“ aus.
  
7. Wählen Sie **Erstellen** aus.

Beachten Sie diese beiden Eigenschaften:

* **Tresorname**: In diesem Beispiel wird **Contoso-Vault2** verwendet. Dieser Name wird noch für andere Schritte benötigt.
* **Tresor-URI**: Im Beispiel lautet der Tresor-URI `https://contoso-vault2.vault.azure.net/`. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

>[!NOTE]
>Standardmäßig ist das Konto, mit dem ein Tresor erstellt wird, das einzige mit Zugriff. Der Dienst für überprüfbare Anmeldeinformationen benötigt Zugriff auf den Schlüsseltresor. Sie müssen den Schlüsseltresor mit einer Zugriffsrichtlinie konfigurieren, die es dem während der Konfiguration verwendeten Konto ermöglicht, Schlüssel zu erstellen und zu löschen. Das während der Konfiguration verwendete Konto erfordert außerdem die Berechtigung zum Signieren, um die Domänenbindung für Verified ID zu erstellen. Wenn Sie beim Testen dasselbe Konto verwenden, ändern Sie die Standardrichtlinie, um dem Konto die Berechtigung „Signieren“ zusätzlich zu den Standardberechtigungen zu erteilen, die Tresorerstellern erteilt wurden.

### Festlegen von Zugriffsrichtlinien für den Schlüsseltresor

Ein Key Vault definiert, ob ein angegebener Sicherheitsprinzipal Vorgänge für Key Vault-Geheimnisse und -Schlüssel ausführen kann. Legen Sie Zugriffsrichtlinien in Ihrem Schlüsseltresor sowohl für das Administratorkonto von Microsoft Entra Verified ID als auch für den von Ihnen erstellten Prinzipal der Anforderungsdienst-API fest.
Nach der Erstellung Ihres Schlüsseltresors werden mit Nachweisen Schlüssel generiert, die die Nachrichtensicherheit gewährleisten. Diese Schlüssel werden in Key Vault gespeichert. Sie verwenden einen Schlüsselsatz zum Signieren, Aktualisieren und Wiederherstellen von Nachweisen.

### Festlegen von Zugriffsrichtlinien für Administratorbenutzer von Verified ID

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu dem für dieses Tutorial verwendeten Schlüsseltresor.

3. Wählen Sie unter **Einstellungen** die Option **Zugriffsrichtlinien** aus.

4. Wählen Sie unter **Zugriffsrichtlinie hinzufügen** unter **BENUTZER** das Konto aus, das Sie für dieses Tutorial verwenden möchten.

5. Überprüfen Sie unter **Schlüsselberechtigungen**, ob die folgenden Berechtigungen ausgewählt wurden: **Get**, **Erstellen**, **Löschen** und **Signieren**. Standardmäßig sind **Erstellen** und **Löschen** bereits aktiviert. **Signieren** sollte die einzige Schlüsselberechtigung sein, die Sie aktualisieren müssen.

      ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/7c8a92ea-24f1-41e6-9656-869e8486af72)


6. Wählen Sie zum Speichern der Änderungen **Speichern** aus.

## Verifizierte ID einrichten.

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/b4b857a2-24b8-4c3f-9f5c-43d23b58427f)


Führen Sie die folgenden Schritte aus, um Verified ID einzurichten.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach *Überprüfte ID*. Wählen Sie dann **Verified ID** aus.

3. Klicken Sie im linken Menü auf **Einrichten**.

4. Wählen Sie im mittleren Menü **Organisationseinstellungen definieren** aus.

5. Richten Sie Ihre Organisation ein, indem Sie die folgenden Informationen bereitstellen:

    1. **Organisationsname**: Geben Sie einen Namen ein, um in Verified ID auf Ihr Unternehmen zu verweisen. Dieser Name wird Kunden nicht angezeigt.

    1. **Vertrauenswürdige Domäne**: Geben Sie eine Domäne ein, die einem Dienstendpunkt in Ihrem DID-Dokument (Decentralized Identity, dezentralisierte Identität) hinzugefügt wird. Die Domäne bindet Ihre DID an etwas Konkretes, das der Benutzer möglicherweise über Ihr Unternehmen kennt. Microsoft Authenticator und andere digitale Geldbörsen überprüfen anhand dieser Informationen, ob Ihre DID mit Ihrer Domäne verknüpft ist. Wenn das Wallet die DID überprüfen kann, wird ein verifiziertes Symbol angezeigt. Wenn das Wallet die DID nicht überprüfen kann, wird der Benutzer darüber informiert, dass die Anmeldeinformationen von einer Organisation ausgestellt wurden, die nicht überprüfen werden konnte.

        >[!IMPORTANT]
        > Die Domäne kann keine Umleitung sein. Andernfalls können DID und Domäne nicht verknüpft werden. Achten Sie darauf, HTTPS für die Domäne zu verwenden. Beispiel: `https://did.woodgrove.com`

    1. **Schlüsseltresor:** Wählen Sie den Schlüsseltresor aus, den Sie zuvor erstellt haben.

    1. Unter **Erweitert** können Sie das **Vertrauenssystem** auswählen, das Sie für Ihren Mandanten verwenden möchten. Sie können **Web** oder **ION** auswählen. „Web“ bedeutet, dass Ihr Mandant [did:web](https://w3c-ccg.github.io/did-method-web/) als DID-Methode verwendet, und „ION“ bedeutet, dass er [did:ion](https://identity.foundation/ion/) verwendet.

        >[!IMPORTANT]
        > Die einzige Möglichkeit zum Ändern des Vertrauenssystems besteht darin, die Verwendung des Verified ID-Diensts zu beenden und das Onboarding zu wiederholen.

1. Wählen Sie **Speichern** aus.  

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/b191676e-03e3-423f-aa28-1e3d2887ea07)


### Festlegen von Zugriffsrichtlinien für Dienstprinzipale von Verified ID

Wenn Sie im vorherigen Schritt Verified ID einrichten, werden die Zugriffsrichtlinien in Azure Key Vault automatisch aktualisiert, um den Dienstprinzipalen für Verified ID die erforderlichen Berechtigungen zu erteilen.  
Wenn Sie die Berechtigungen einmal manuell zurücksetzen müssen, sollte die Zugriffsrichtlinie wie unten aussehen.

| Dienstprinzipal | AppId | Schlüsselberechtigungen |
| -------- | -------- | -------- |
| Nachweisdienst | bb2a64ee-5d29-4b07-a491-25806dc854d3 | Abrufen, Signieren |
| Nachweisdienstanforderung | 3db474b9-6a0c-4840-96ac-1fceb342124f | Signieren |



![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/896b8ea0-b88b-43b8-a93b-4771defedfde)


## Registrieren einer Anwendung in Azure AD

Ihre Anwendung muss Zugriffstoken abrufen, wenn sie Microsoft Entra Verified ID aufrufen möchte, um Anmeldeinformationen ausstellen oder überprüfen zu können. Zum Abrufen von Zugriffstoken müssen Sie eine Anwendung registrieren und dem Verified ID-Anforderungsdienst eine API-Berechtigung gewähren. Befolgen Sie beispielsweise für eine Webanwendung die folgenden Schritte:

1. Melden Sie sich mit Ihrem Administratorkonto beim [Azure-Portal](https://portal.azure.com) an.

1. Wenn Sie Zugriff auf mehrere Mandanten haben, wählen Sie **Verzeichnis und Abonnement** aus. Suchen Sie dann nach Ihrer Instanz von **Azure Active Directory**, und wählen Sie sie aus.

1. Wählen Sie unter **Verwalten** Folgendes aus: **App-Registrierungen** > **Neue Registrierung**.  

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/03ca3d13-5564-4ce2-b42c-f8333bc97fb5)


1. Geben Sie einen Anzeigenamen für Ihre Anwendung ein. Beispiel: *verifiable-credentials-app*

1. Wählen Sie unter **Unterstützte Kontotypen** die Option **Nur Konten in diesem Organisationsverzeichnis (nur Standardverzeichnis – einzelner Mandant)** aus.

1. Wählen Sie **Registrieren** aus, um die Anwendung zu erstellen.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/d0a15737-c8a9-4522-990d-1cf6bc12cc1e)


### Erteilen von Berechtigungen zum Abrufen von Zugriffstoken

In diesem Schritt erteilen Sie dem Prinzipal des **Nachweisanforderungsdiensts** Berechtigungen.

Führen Sie die folgenden Schritte aus, um die erforderlichen Berechtigungen hinzuzufügen:

1. Bleiben Sie auf der Seite mit den Details für die Anwendung **verifiable-credentials-app**. Klicken Sie auf **API-Berechtigungen** > **Berechtigung hinzufügen**.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/adf97022-2081-4273-8d61-f2b53628e989)

1. Wählen Sie **Von meiner Organisation verwendete APIs** aus.

1. Suchen Sie nach dem Dienstprinzipal **Nachweisdienstanforderung**, und wählen Sie ihn aus.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/01691eb2-ab7f-4778-9911-342eb9350f99)

1. Wählen Sie **Anwendungsberechtigung** aus, und erweitern Sie **VerifiableCredential.Create.All**.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/3c5e008e-2ba5-417a-90ff-22e8f622ad34)


1. Wählen Sie **Berechtigungen hinzufügen** aus.

1. Wählen Sie **Administratoreinwilligung erteilen für \<your tenant name\>** aus.

Sie können die Ausstellungs- und Präsentationsberechtigungen separat erteilen, wenn Sie die Bereiche für unterschiedliche Anwendungen trennen möchten.

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/640def45-e666-423e-bf69-598e8980b125)

## Eine dezentrale ID registrieren und den Domänenbesitz überprüfen

Nachdem Azure Key Vault eingerichtet wurde und der Dienst über einen Signaturschlüssel verfügt, müssen Sie die Schritte 2 und 3 im Setup ausführen.

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/40e10177-b025-4d10-b16a-d66c06762c3f)

1. Navigieren Sie im Azure-Portal zum Verified ID-Dienst.  
1. Klicken Sie im linken Menü auf **Einrichten**.
1. Wählen Sie im mittleren Menü **Domänenbesitz überprüfen** aus.

# Verifizieren des Domänenbesitzes für den dezentralen Bezeichner (DID)

## Verifizieren des Domänenbesitzes und Verteilen der Datei „did-configuration.json“

Die Domäne muss eine Domäne sein, die von Ihnen kontrolliert wird, und sie sollte das Format `https://www.example.com/` haben. 

1. Navigieren Sie vom Azure-Portal zur Seite „VerifiedID“.

1. Wählen Sie **Setup**, dann **Domänenbesitz überprüfen** aus, und wählen Sie **Überprüfen** für die Domäne aus.

1. Kopieren Sie die Datei `did-configuration.json`, oder laden Sie sie herunter.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/342fa12f-2d0d-40cf-a2f9-8796a86f0824)

1. Hosten Sie die `did-configuration.json`-Datei am angegebenen Speicherort. Beispiel: Wenn Sie die Domäne `https://www.example.com` angegeben haben, muss die Datei unter dieser URL gehostet werden: `https://www.example.com/.well-known/did-configuration.json`.
   Außer dem Namen `.well-known path` darf die URL keinen zusätzlichen Pfad enthalten.

1. Wenn die `did-configuration.json` unter der URL „.well-known/did-configuration.json“ öffentlich verfügbar ist, überprüfen Sie die Konfiguration, indem Sie auf die Schaltfläche **Überprüfungsstatus aktualisieren** drücken.

   ![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/49a06251-af56-49b2-9059-0bd4ca678da6)

> Ergebnis: Sie haben erfolgreich eine Azure Key Vault-Instanz erstellt, den Dienst „Überprüfte ID“ eingerichtet, eine Anwendung in Azure AD registriert und den Domänenbesitz für Ihren dezentralen Bezeichner (Decentralized Identifier, DID) überprüft.

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen. 
