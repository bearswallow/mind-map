在maven使用的settings.xml中需要进行以下配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <servers>
        <server>
            <id>nexus</id>
            <username>admin</username>
            <password>tmnj2018</password>
        </server>
    </servers>
    <mirrors>
        <mirror>
            <id>nexus</id>
            <name>nexus private repository</name>
            <mirrorOf>*</mirrorOf>
            <url>http://nexus.tianmiwl.com/repository/maven-public/</url>
        </mirror>
    </mirrors>
    <profiles>
        <profile>
            <id>nexus</id>
            <!--Enable snapshots for the built in central repo to direct -->
            <!--all requests to nexus via the mirror -->
            <repositories>
                <repository>
                    <id>central</id>
                    <url>http://central</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>central</id>
                    <url>http://central</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>
    <activeProfiles>
        <!--make the profile active all the time -->
        <activeProfile>nexus</activeProfile>
    </activeProfiles>
</settings>
```

在项目的pom文件中需要进行以下配置，最好把这个配置放在parent的pom文件中

```xml
	<distributionManagement>
        <repository>
            <id>nexus</id>
            <name>Releases</name>
            <url>http://nexus.tianmiwl.com/repository/maven-releases/</url>
        </repository>
        <snapshotRepository>
            <id>nexus</id>
            <name>Snapshot</name>
            <url>http://nexus.tianmiwl.com/repository/maven-snapshots/</url>
        </snapshotRepository>
    </distributionManagement>
```

这里的id=nexus需要与settings.xml里的server的id相对应，因为在上传文件到nexus时是需要进行身份验证的，而settings.xml的server里就是存储nexus的账号密码的。