
## maven打包生成jar
在maven的pom.xml配置中添加如下节点，再执行mvn  package就能实现将方法打包成jar。会在target下生成两个jar文件，其中非original开头的jar文件包含项目中的全部依赖。

相关解释

* 其中excludes节点中包含的信息是为了不将一些签证之类的文件打进jar包，如果打入在执行的时候可能会报错sign失败。。
```xml
<build>
	<plugins>

		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-shade-plugin</artifactId>
			<version>2.4.1</version>
			<executions>
				<execution>
					<phase>package</phase>
					<goals>
						<goal>shade</goal>
					</goals>
					<configuration>

						<filters>
							<filter>
								<artifact>*:*</artifact>
								<excludes>
									<exclude>META-INF/*.SF</exclude>
									<exclude>META-INF/*.DSA</exclude>
									<exclude>META-INF/*.RSA</exclude>
								</excludes>
							</filter>
						</filters>

						<transformers>
							<transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
								<mainClass>SignTool</mainClass>
							</transformer>
						</transformers>
					</configuration>
				</execution>
			</executions>
		</plugin>

	</plugins>
</build>

```