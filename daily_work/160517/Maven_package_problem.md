# Maven 打包jar文件，jar包无法运行的问题
## 1. 打包配置pom.xml
	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>org.hainu.cs</groupId>
	<artifactId>circulation-checking</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>
	<name>circulationchecking</name>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-jar-plugin</artifactId>
				<version>2.2</version>
				<!-- nothing here -->
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-assembly-plugin</artifactId>
				<version>2.2-beta-4</version>
				<configuration>
					<descriptorRefs>
						<descriptorRef>jar-with-dependencies</descriptorRef>
					</descriptorRefs>
					<archive>
						<manifest>	<mainClass>org.hainu.cs.circulationchecking.frm.ExecutionSequenceDifferentFrm</mainClass>
						</manifest>
					</archive>
				</configuration>
				<executions>
					<execution>
						<phase>package</phase>
						<goals>
							<goal>single</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>
	<dependencies>
		<dependency>
			<groupId>org.eclipse</groupId>
			<artifactId>draw2d</artifactId>
			<version>3.2.100-v20070529</version>
		</dependency>
		<dependency>
			<groupId>org.eclipse.swt</groupId>
			<artifactId>org.eclipse.swt.win32.win32.x86_64</artifactId>
			<version>4.3</version>
		</dependency>
		<dependency>
			<groupId>org.apache.maven</groupId>
			<artifactId>maven-plugin-api</artifactId>
			<version>3.3.9</version>
		</dependency>
	</dependencies>
	</project>

该方法是采用 maven-assembly-plugin 打包，还能自动将依赖也打包jar-with-dependencies，mainClass指定了主函数的入口。

## 2. 出现的问题
生成了两个jar文件，一个是包含依赖的，一个是不包含依赖的。  
![](http://i.imgur.com/2a5vhOB.png)  
下面这个是点击包含依赖的 jar文件的提示  
![](http://i.imgur.com/i3tlLuC.png)

下面这个是点击不包含依赖的jar的提示    
![](http://i.imgur.com/Y6XxMts.png)

## 3. 网上给出的同样问题的链接

[http://stackoverflow.com/questions/1814526/problem-building-executable-jar-with-maven?answertab=oldest#tab-top](http://stackoverflow.com/questions/1814526/problem-building-executable-jar-with-maven?answertab=oldest#tab-top)

还是无法解决

## 4. 之前查到的Maven的3种打包方式
1. 使用maven-jar-plugin和maven-dependency-plugin插件打包，配置如下。
效果是在jar文件的同级的lib目路下，Maven自动将依赖的包拷贝进来了

	<build>
	<plugins>

		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-jar-plugin</artifactId>
			<version>2.6</version>
			<configuration>
				<archive>
					<manifest>
						<addClasspath>true</addClasspath>
						<classpathPrefix>lib/</classpathPrefix>
						<mainClass>com.xxg.Main</mainClass>
					</manifest>
				</archive>
			</configuration>
		</plugin>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-dependency-plugin</artifactId>
			<version>2.10</version>
			<executions>
				<execution>
					<id>copy-dependencies</id>
					<phase>package</phase>
					<goals>
						<goal>copy-dependencies</goal>
					</goals>
					<configuration>
						<outputDirectory>${project.build.directory}/lib</outputDirectory>
					</configuration>
				</execution>
			</executions>
		</plugin>

	</plugins>
	</build>

2. 就是上面我用到的方式，使用的是assembly插件。命令是package assembly:single 


3. 使用maven-shade-plugin插件打包  
同样使用mvn package命令

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
						<transformers>
							<transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
								<mainClass>com.xxg.Main</mainClass>
							</transformer>
						</transformers>
					</configuration>
				</execution>
			</executions>
			</plugin>
		</plugins>