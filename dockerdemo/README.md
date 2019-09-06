使用 docker-maven-plugin 编辑 推送 docker 镜像
```xml
<plugin>
    <groupId>com.spotify</groupId>
    <artifactId>docker-maven-plugin</artifactId>
    <version>1.2.0</version>

    <!--
        命令节省:
        执行 mvn package 后会自动执行 docker:build docker:push
    -->
    <executions>
        <execution>
            <id>build-image</id>
            <phase>package</phase>
            <goals>
                <goal>build</goal>
                <goal>push</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
        <imageTags>${project.version}</imageTags>
        <baseImage>java:8</baseImage>
        <exposes>
            <expose>8080</expose>
        </exposes>
        <entryPoint>["java", "-jar", "/${project.build.finalName}.jar"]</entryPoint>

        <!--
            buildArgs 这个可以配置 Dockerfile 文件中需要的变量 ${JAR-NAME}
        -->
        <!--
        <dockerDirectory>Dockerfile Directory</dockerDirectory>
        <buildArgs>
            <JAR-NAME>${project.build.finalName}.jar</JAR-NAME>
        </buildArgs>
        -->
        <resources>
            <resource>
                <targetPath>/</targetPath>
                <directory>${project.build.directory}</directory>
                <include>${project.build.finalName}.jar</include>
            </resource>
        </resources>
        <!--
            镜像库的路径/用户/密码设置
            这里的 serverId 需要去 maven 的setting.xml文件设置 server:
            如:
            <servers>
                <server>
                  <id>docker-hub</id>
                  <username>liangyongtong</username>
                  <password>********</password>
                </server>
            <servers>
        -->
        <serverId>docker-hub</serverId>
        <registryUrl>https://cloud.docker.com/repository/docker/liangyongtong</registryUrl>
    </configuration>
</plugin>
```