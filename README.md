# build-tools
Library to manage static code analysis configuration files. 
This will help manage organization wide static code analysis rules in a single place and allows
developers to run code analysis during development rather than waiting to be analyzed during build-analyze phase during continuous integration.

Usage:
Use in maven project along with a maven profile to run the code analysis.
1. Setup a maven profile and add the library as a dependency to static code analysis plugins.
Its best to setup the profiles at a parent project pom.xml so that all the modules and sub-modules will be analyzed.

<profiles>
		<profile>
			<id>analyze</id>
			<build>
				<plugins>
					<!-- sca: checkstyle plugin -->
					<plugin>
						<groupId>org.apache.maven.plugins</groupId>
						<artifactId>maven-checkstyle-plugin</artifactId>
						<version>${plugin.checkstyle.version}</version>
						<dependencies>
							<dependency>
								<groupId>com.risenture.rise.tools</groupId>
								<artifactId>build-tools</artifactId>
								<version>${rise.buildtools.version}</version>
							</dependency>
						</dependencies>
						<executions>
							<execution>
								<phase>validate</phase>
								<configuration>
									<configLocation>chekstyle/chekstyle.xml</configLocation>
									<encoding>UTF-8</encoding>
									<consoleOutput>true</consoleOutput>
									<failOnViolation>true</failOnViolation>
									<failsOnError>true</failsOnError>
								</configuration>
								<goals>
									<goal>check</goal>
								</goals>
							</execution>
						</executions>
					</plugin>
					<!-- sca: findbugs plugin -->
					<plugin>
						<groupId>org.codehaus.mojo</groupId>
						<artifactId>findbugs-maven-plugin</artifactId>
						<version>${plugin.findbugs.version}</version>
						<configuration>
							<xmlOutput>true</xmlOutput>
							<effort>MAX</effort>
							<threshold>LOW</threshold>
							<excludeFilterFile>findbugs/findbugs-exclude.xml</excludeFilterFile>
							<failOnError>true</failOnError>
						</configuration>
						<dependencies>
							<dependency>
								<groupId>com.risenture.rise.tools</groupId>
								<artifactId>build-tools</artifactId>
								<version>${rise.buildtools.version}</version>
							</dependency>
						</dependencies>
						<executions>
							<execution>
								<phase>validate</phase>
								<goals>
									<goal>check</goal>
								</goals>
							</execution>
						</executions>
					</plugin>
					<!-- sca: pmd plugin -->
					<plugin>
						<groupId>org.apache.maven.plugins</groupId>
						<artifactId>maven-pmd-plugin</artifactId>
						<version>${plugin.pmd.version}</version>
						<configuration>
							<linkXRef>true</linkXRef>
							<sourceEncoding>UTF-8</sourceEncoding>
							<minimumTokens>100</minimumTokens>
							<skipEmptyReport>false</skipEmptyReport>
							<targetJdk>${java.version}</targetJdk>
							<rulesets>
								<ruleset>pmd/pmd-rules.xml</ruleset>
							</rulesets>
						</configuration>
						<executions>
							<execution>
								<phase>validate</phase>
								<goals>
									<goal>check</goal>
									<goal>cpd-check</goal>
								</goals>
							</execution>
						</executions>
					</plugin>
					<!-- Javadocs -->
					<plugin>
						<groupId>org.apache.maven.plugins</groupId>
						<artifactId>maven-javadoc-plugin</artifactId>
						<version>${plugin.javadoc.version}</version>
						<configuration>
							<additionalparam>-Xdoclint:none</additionalparam>
							<failOnError>true</failOnError>
						</configuration>
						<executions>
							<execution>
								<id>attach-javadocs</id>
								<goals>
									<goal>jar</goal>
								</goals>
							</execution>
						</executions>
					</plugin>
				</plugins>
			</build>
		</profile>
	</profiles>
	
	2. Once the profile is run and activated, individual reports get generated in the target folder
