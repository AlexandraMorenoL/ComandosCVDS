# ComandosCVDS
mvn -version

mvn archetype:generate -DgroupId=com.stockmonitor -DartifactId=stock-monitor -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
Ingresar al directorio del proyecto. cd stock-monitor

mvn clean install

En src/main/java/com/stockmonitor, crea los siguientes paquetes:

model: Para la clase Product.
service: Para la lógica de negocio (gestión de stock y notificaciones).
agent: Para los agentes LogAgent y WarningAgent.
controller: Para los endpoints de la API REST.
config: Para la configuración de Spring (inyección de dependencias).
public class Product {
    private String name;
    private double price;
    private int stock;
    private String category;

    // Constructores, getters y setters
}

git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <URL-DEL-REPOSITORIO>
git push -u origin main

<build>
    <plugins>
        <plugin>
            <groupId>org.jacoco</groupId>
            <artifactId>jacoco-maven-plugin</artifactId>
            <version>0.8.8</version>
            <executions>
                <execution>
                    <goals>
                        <goal>prepare-agent</goal>
                        <goal>report</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>

package com.stockmonitor.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.stockmonitor.model.Product;
import com.stockmonitor.agent.LogAgent;
import com.stockmonitor.agent.WarningAgent;

@Service
public class StockServiceImpl implements StockService {
    
    // Variable estática para la única instancia
    private static StockServiceImpl instance;

    @Autowired
    private LogAgent logAgent;
    @Autowired
    private WarningAgent warningAgent;

    // Constructor privado
    private StockServiceImpl() {}

    // Método estático para obtener la instancia única
    public static StockServiceImpl getInstance() {
        if (instance == null) {
            instance = new StockServiceImpl();
        }
        return instance;
    }

    @Override
    public void updateStock(Product product, int newStock) {
        product.setStock(newStock);
        logAgent.notify(product);
        warningAgent.notify(product);
    }
}
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.4.3</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>edu.eci</groupId>
	<artifactId>cvds</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>AppStock</name>
	<description>CVDS Parcial T1 AppStock</description>
	<url/>
	<licenses>
		<license/>
	</licenses>
	<developers>
		<developer/>
	</developers>
	<scm>
		<connection/>
		<developerConnection/>
		<tag/>
		<url/>
	</scm>
	<properties>
		<java.version>17</java.version>
	</properties>
	<dependencies>

		<dependency>
			<groupId>org.junit.jupiter</groupId>
			<artifactId>junit-jupiter-api</artifactId>
			<version>5.11.4</version>
			<scope>test</scope>
		</dependency>


		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
			<plugin>
				<groupId>org.jacoco</groupId>
				<artifactId>jacoco-maven-plugin</artifactId>
				<version>0.8.12</version>
				<executions>
					<execution>
						<goals>
							<goal>prepare-agent</goal>
						</goals>
					</execution>
					<execution>
						<id>report</id>
						<phase>test</phase>
						<goals>
							<goal>report</goal>
						</goals>
						<configuration>
							<excludes>
								<exclude>/configurators/</exclude>
							</excludes>
						</configuration>
					</execution>
					<execution>
						<id>jacoco-check</id>
						<goals>
							<goal>check</goal>
						</goals>
						<configuration>
							<rules>
								<rule>
									<element>PACKAGE</element>
									<limits>
										<limit>
											<counter>CLASS</counter>
											<value>COVEREDRATIO</value>
											<minimum>0.85</minimum><!--Porcentaje mínimo de cubrimiento para construir el proyecto-->
										</limit>
									</limits>
								</rule>
							</rules>
						</configuration>
					</execution>
				</executions>
			</plugin>


		</plugins>

	</build>




</project>
