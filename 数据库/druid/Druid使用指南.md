# Druid使用指南

## 1. 密码加密

可以使用Druid对数据库的密码进行加密，这样就可以放心的将密码配置在 `application.properties` 或者 `application.yml` 中了。步骤如下

### 加密

```bash
java -cp ./druid-1.1.10.jar com.alibaba.druid.filter.config.ConfigTools 你的密码
```

会得到下面的内容

```bash
privateKey:MIIBVQIBADANBgkqhkiG9w0BAQEFAASCAT8wggE7AgEAAkEArlm+6+jQvL8T7uX4tiwhxeHD3gZ2qCC2vel0SAuXE8XmtFg5dPjrVAuhohjIl3jw5b96xs8LbroDplz4jWBKhQIDAQABAkABiPa+WvljgAcr5khvSiot9NPlo4bt6gPR3jlQ3RFCckrY0zkgY3zgW1f+O9adqtju6Nucyc8GUBcgu7X0Sc0BAiEA81HbPjoYL8pTuyJRnp2jnOeM7an9s3PjTVObpHZXrkUCIQC3b8RvM4GrMOLIErV71M2E56mq/uecQHHNePuWpDsPQQIhAMwRqhRdeu2R/nmjhdrHEXLGDL9DZAD+v/OZnJ7plg4VAiAYe49hNCOrYJP0FiMoyuc/RNgtXWY2QZeuz+XsXjEPwQIhAJvvhUBy0/jqm35Tql4lGueuNAxOxH74KEdey2eWj49J
publicKey:MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAK5Zvuvo0Ly/E+7l+LYsIcXhw94Gdqggtr3pdEgLlxPF5rRYOXT461QLoaIYyJd48OW/esbPC266A6Zc+I1gSoUCAwEAAQ==
password:MlmBeaecmhd3JWG+4tD+af+R40b0EpwnCDUuID0W195jgmGv26wyQYl83FjNyqWFzTistNFG+9UWJlJ0Rl3Ysw==
```

- password：加密后的密码
- privateKey：私钥，配置时不需要用到
- publicKey：公钥，配置时需要用到

### 解密

可以通过调用 `ConfigTools` 进行解密

```java
public static void main(String... args) {
    String password = "MlmBeaecmhd3JWG+4tD+af+R40b0EpwnCDUuID0W195jgmGv26wyQYl83FjNyqWFzTistNFG+9UWJlJ0Rl3Ysw==";
    String publicKey = "MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAK5Zvuvo0Ly/E+7l+LYsIcXhw94Gdqggtr3pdEgLlxPF5rRYOXT461QLoaIYyJd48OW/esbPC266A6Zc+I1gSoUCAwEAAQ==";
    try {
        String decryptword = ConfigTools.decrypt(publicKey, password);
        System.out.println(decryptword);
    }
    catch (Exception e) {
        e.printStackTrace();
    }
}
```

由上述代码可以看出，解密时只需要用到 `加密后的密码` 和 `公钥` ，所以配置的时候也只需要配置这两样即可。

### 配置

需要在 `application.yml` 中进行配置

```yml
spring:
  datasource:
    druid:
      url: jdbc:mysql://118.31.55.111:3306/tianmi_sms-dev?useUnicode=true&characterEncoding=utf8&useSSL=false
      username: root
      password: O6+mL05Yukn4RwvwvSv3PR5jXLxqQTfvV9YkyhUnmoihkTUwylKHr9W4sQBA9aPaHY3c5meXeXDoU8fLTSQgrw==
      connection-properties: druid.stat.slowSqlMillis=5000;config.decrypt=true;config.decrypt.key=MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBANEDQYcRJOfOwOQnztlcrl7e6MJqXZFwYDUCoPyjEcbnDCrA83aUTmE4vP6FA3ioW0oVLc+lacXJwDZ7fDMdMwECAwEAAQ==
```

