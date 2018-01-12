---
title: Konfigurace ochrany dat v ASP.NET Core
author: rick-anderson
description: Informace o konfiguraci ochrany dat v ASP.NET Core.
keywords: ASP.NET Core, ochrany dat, konfigurace
ms.author: riande
manager: wpickett
ms.date: 07/17/2017
ms.topic: article
ms.assetid: 0e4881a3-a94d-4e35-9c1c-f025d65dcff0
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 20e3d974e7790cd01f78f8db09225b5887f1772a
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="configuring-data-protection-in-aspnet-core"></a>Konfigurace ochrany dat v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Při inicializaci systému ochrany dat, se vztahuje [výchozí nastavení](xref:security/data-protection/configuration/default-settings) na základě provozní prostředí. Tato nastavení jsou obecně vhodné pro aplikace běžící na jednom počítači. Existují případy, kdy vývojář chtít možná změnit výchozí nastavení, protože jejich aplikace je rozdělena mezi několika počítači, nebo kvůli dodržování předpisů. Pro tyto scénáře ochrany dat systému nabízí bohaté konfigurační rozhraní API.

Metody rozšíření je [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) , který vrací [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder`zpřístupní metody rozšíření, můžete společně zřetězit ke konfiguraci ochrany dat možnosti.

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

Chcete ukládat klíče na sdílenou jednotku UNC místo na *LOCALAPPDATA %* výchozí umístění, nakonfigurujte systém s [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> Pokud změníte umístění klíče trvalost, systém šifruje nebude automaticky klíče v klidu, vzhledem k tomu, že není známo, zda je rozhraní DPAPI mechanismus příslušným šifrovacím.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

Můžete nakonfigurovat systému k ochraně klíče v klidovém stavu pomocí některé z volání [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) rozhraní API pro konfiguraci. Vezměte v úvahu následující příklad, který ukládá klíče na sdílenou jednotku UNC a šifruje tyto klíče v klidovém stavu s konkrétní certifikát X.509:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

V tématu [klíč šifrování na Rest](xref:security/data-protection/implementation/key-encryption-at-rest) Další příklady a zabývat mechanismy integrovaný šifrovací klíče.

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Chcete-li konfigurovat systém pro použití klíče životnost 14 dnů místo výchozí 90 dnů, použijte [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

Ve výchozím nastavení ochrany dat systému izoluje aplikace od sebe navzájem i v případě, že se jejich sdílení stejného fyzického úložiště klíčů. To brání pochopení vzájemně chráněných datových částí aplikace. Chcete-li sdílet chráněné datové části mezi dvěma aplikacemi, použijte [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) se stejnou hodnotou pro každou aplikaci:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

Můžete mít scénář, kde nechcete, aby aplikace automaticky vrátit klíče (vytvořit nové klíče) jako přiblížení se vypršení platnosti. Příkladem toho je možné nastavit ve vztahu primární a sekundární, kde je zodpovědná za riziko z hlediska správy klíčů pouze aplikace primární a sekundární aplikací mít jednoduše zobrazení jen pro čtení prstence klíč aplikace. Sekundární aplikace lze nakonfigurovat, aby považovat za prstenec klíč jen pro čtení tak, že nakonfigurujete systém s [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Izolace za aplikací

Je-li ASP.NET Core hostitelů v systému ochrany dat, je aplikace od sebe navzájem automaticky izoluje, i když tyto aplikace běží pod stejným účtem pracovní proces a používají stejné materiál hlavního klíče. Toto je trochu podobné modifikátor IsolateApps z prostředí System.Web na  **\<machineKey >** element.

Tento mechanismus izolace funguje tak, že s každou aplikaci v místním počítači jako jedinečný klienta, proto [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) root pro aplikace automaticky zahrnují ID aplikace jako diskriminátoru. Jedinečné ID aplikace pochází z jednoho ze dvou míst:

1. Pokud je aplikace hostované ve službě IIS, je jedinečný identifikátor cesta konfigurace aplikace. Pokud je aplikace nasazená v prostředí webové farmy, tato hodnota by měla být stabilní, za předpokladu, že prostředí služby IIS jsou nakonfigurované podobně mezi všechny počítače ve webové farmě.

2. Pokud aplikace není hostovaný ve službě IIS, je jedinečný identifikátor fyzickou cestu aplikace.

Jedinečný identifikátor slouží k zůstanou platné i po resetování &mdash; jednotlivé aplikace a počítače sám sebe.

Tento mechanismus izolace předpokládá, že aplikací nejsou škodlivý. Škodlivý aplikace může ovlivnit vždy žádné jiné aplikace spuštěna pod stejným účtem pracovní proces. Ve sdíleném hostování prostředí kde aplikace jsou vzájemně nedůvěryhodných by měl poskytovatel hostingu provést kroky k zajištění OS úroveň izolace mezi aplikacemi, včetně oddělení aplikací základní klíče úložiště.

Pokud není zadaný systému ochrany dat ASP.NET Core hostitelů v (například pokud instanci můžete vytvořit pomocí `DataProtectionProvider` konkrétní typ) izolace aplikace ve výchozím nastavení vypnutá. Při izolaci aplikace je zakázaná, můžete všechny aplikace založenou na stejný klíčový materiál tak dlouho, dokud poskytují odpovídající sdílet datových částí [účely](xref:security/data-protection/consumer-apis/purpose-strings). K zajištění izolace aplikace v tomto prostředí, volání [SetApplicationName](#setapplicationname) metoda na konfiguraci objektu a zadejte jedinečný název pro každou aplikaci.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Změna algoritmy s UseCryptographicAlgorithms

Ochrana dat zásobníku umožňuje změnit výchozí algoritmus používaný nově vygenerované klíče. Nejjednodušší způsob, jak to udělat je volat [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) ze zpětného volání konfigurace:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

Výchozí hodnota EncryptionAlgorithm je AES-256-CBC a ValidationAlgorithm výchozí hodnota je HMACSHA256. Výchozí zásady můžete nastavit, že správce systému prostřednictvím [celého zásad](xref:security/data-protection/configuration/machine-wide-policy), ale explicitní volání `UseCryptographicAlgorithms` přepíše výchozí zásadu.

Volání metody `UseCryptographicAlgorithms` vám umožní určit požadovanou algoritmus z předdefinovaného seznamu předdefinované. Nemusíte si dělat starosti o implementaci algoritmu. Ve scénáři výše ochrany dat systému pokusí použít CNG implementace AES, pokud v systému Windows. Jinak se vrátí k spravovaný [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) třídy.

Můžete ručně zadat implementace prostřednictvím volání [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Změna algoritmy neovlivní existující klíče v řetězci klíč. Ovlivňuje jenom nově vygenerované klíče.

### <a name="specifying-custom-managed-algorithms"></a>Určení vlastních spravované algoritmy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Pokud chcete zadat vlastní spravované algoritmů, vytvářet [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance, která odkazuje na typy implementace:

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Pokud chcete zadat vlastní spravované algoritmů, vytvářet [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance, která odkazuje na typy implementace:

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

Obecně \*typ vlastnosti musí odkazovat na konkrétní, instantiable (prostřednictvím veřejného konstruktoru bez parametrů) implementace [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) a [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), ačkoli systém některé hodnoty jako na případy zvláštní `typeof(Aes)` ke zvýšení pohodlí.

> [!NOTE]
> SymmetricAlgorithm musí mít délku klíče ≥ 128 bitů a velikost bloku ≥ 64bitová verze a musí podporovat režimu CBC šifrování s odsazení PKCS #7. KeyedHashAlgorithm musí mít velikost digest > = 128 bitů, a musí podporovat klíče délce délku digest algoritmus hash. KeyedHashAlgorithm není nezbytně nutné být HMAC s klíčem.

### <a name="specifying-custom-windows-cng-algorithms"></a>Určení vlastních algoritmy Windows CNG

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Chcete-li zadat vlastní Windows CNG algoritmus HMAC s klíčem ověření pomocí režimu CBC šifrování, vytvořte [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance, který obsahuje algoritmické informace:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Chcete-li zadat vlastní Windows CNG algoritmus HMAC s klíčem ověření pomocí režimu CBC šifrování, vytvořte [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance, který obsahuje algoritmické informace:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> Šifrovací algoritmus symetrického bloku musí mít délku klíče > = 128 bitů, velikost bloku > = 64 bitů, a musí podporovat režimu CBC šifrování s odsazení PKCS #7. Algoritmus hash musí mít velikost digest > = 128 bitů a musí podporovat otevíráte s BCRYPT\_ALG\_zpracování\_HMAC\_příznak příznak. \*Lze nastavit vlastnosti zprostředkovatele hodnoty null lze použít výchozího poskytovatele pro zadaný algoritmus. Najdete v článku [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Další informace naleznete v dokumentaci.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Chcete-li zadat vlastní Windows CNG algoritmus šifrování Galois/čítač režimu pomocí ověření, vytvořte [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance, který obsahuje algoritmické informace:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Chcete-li zadat vlastní Windows CNG algoritmus šifrování Galois/čítač režimu pomocí ověření, vytvořte [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance, který obsahuje algoritmické informace:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> Šifrovací algoritmus symetrického bloku musí mít délku klíče > = 128 bitů, velikost bloku přesně 128 bitů, a musí podporovat GCM šifrování. Můžete nastavit [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) vlastnost na hodnotu null. Chcete-li použít výchozího poskytovatele pro zadaný algoritmus. Najdete v článku [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Další informace naleznete v dokumentaci.

### <a name="specifying-other-custom-algorithms"></a>Určení dalších vlastních algoritmů

I když nejsou viditelné jako první třídy rozhraní API, ochrany dat systému je dostatečně extensible umožňující zadání téměř k libovolnému druhu algoritmu. Například je možné, aby všechny klíče, které jsou obsažené v modulu hardwarového zabezpečení (HSM) a k zajištění vlastní implementace základních šifrování a dešifrování rutiny. V tématu [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) v [základní kryptografie rozšiřitelnost](xref:security/data-protection/extensibility/core-crypto) Další informace.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Zachování klíče při hostování v kontejner Docker

Při hostování v [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kontejneru klíčů by se měl zachovat buď:

* Složka, která je Docker svazek, který potrvají nad rámec životnost kontejneru, například sdíleného svazku nebo hostitele připojeného svazku.
* Externího poskytovatele, jako například [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) nebo [Redis](https://redis.io/).

## <a name="see-also"></a>Viz také

* [Scénáře využívající bez DI](xref:security/data-protection/configuration/non-di-scenarios)
* [Široké zásady počítače](xref:security/data-protection/configuration/machine-wide-policy)