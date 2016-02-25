title: Maven-聚合和继承
date: 2015-12-10 01:23:02
categories: Maven
tags: Maven
---
## 聚合
* 如果一个项目依赖数十个模块，那么要一个一个编译起来是非常浪费时间的，所以就需要把所需模块聚合在一起，一起编译。
* 比如当前有User有两个模块：1.user-dao, 2.user-service，在eclipse中创建一个`create a simple project(skip archetype selection)`,
* 填入：Group Id, Artifact Id(这里为user-parent), Version, Packaging(这里选择pom),然后创建。
* 在`user-parent/pom.xml`中添加如下代码：

		<modules>
		  <module>../user-dao</module>
		  <module>../user-service</module>
		</modules>
* 接下来就可以直接在`user-parent/pom.xml`编译了。
## 继承
* 将`pom.xml`中相同的配置也放入`user-parent/pom.xml`中
* 依赖包管理，将所需的依赖包放入`user-parent/pom.xml`中进行统一管理
* `user-parent/pom.xml`的整体代码如下：

		<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
			<modelVersion>4.0.0</modelVersion>
			<groupId>com.m2.user</groupId>
			<artifactId>user-parent</artifactId>
			<version>0.0.1-SNAPSHOT</version>
			<packaging>pom</packaging>

			<modules>
				<module>../user-dao</module>
				<module>../user-service</module>
			</modules>

			<url>http://maven.apache.org</url>

			<properties>
				<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
			</properties>

			<dependencyManagement>
				<dependencies>
					<dependency>
						<groupId>junit</groupId>
						<artifactId>junit</artifactId>
						<version>4.10</version>
						<scope>test</scope>
					</dependency>
					<dependency>
						<groupId>log4j</groupId>
						<artifactId>log4j</artifactId>
						<version>1.2.16</version>
					</dependency>

				</dependencies>
			</dependencyManagement>
		</project>
* `user-dao/pom.xml`如下：

		<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
			<modelVersion>4.0.0</modelVersion>
			<parent>
				<groupId>com.m2.user</groupId>
				<artifactId>user-parent</artifactId>
				<version>0.0.1-SNAPSHOT</version>
				<relativePath>../user-parent/pom.xml</relativePath>
			</parent>
			<artifactId>user-dao</artifactId>
			<packaging>jar</packaging>

			<name>user-dao</name>

			<dependencies>
				<dependency>
					<groupId>junit</groupId>
					<artifactId>junit</artifactId>
				</dependency>
				<dependency>
					<groupId>log4j</groupId>
					<artifactId>log4j</artifactId>
				</dependency>
			</dependencies>
		</project>
* `user-service/pom.xml`如下:

		<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
			<modelVersion>4.0.0</modelVersion>
			<parent>
				<groupId>com.m2.user</groupId>
				<artifactId>user-parent</artifactId>
				<version>0.0.1-SNAPSHOT</version>
				<relativePath>../user-parent/pom.xml</relativePath>
			</parent>
			<artifactId>user-service</artifactId>
			<packaging>jar</packaging>

			<name>user-service</name>

			<dependencies>
				<dependency>
					<groupId>junit</groupId>
					<artifactId>junit</artifactId>
				</dependency>
				<dependency>
					<groupId>log4j</groupId>
					<artifactId>log4j</artifactId>
				</dependency>
			</dependencies>
		</project>
* 这样就可以对包进行统一管理、代码重复的问题。


