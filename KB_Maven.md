### LifeCycle

La construction d'un projet Maven suit un cycle de vie bien sépécifique, il y a trois cycle maven:

	* default: the main life cycle as it's responsible for project deployment
	* clean  : to clean the project and remove all files generated by the previous build
	* site   : to create the project's site documentation
  
```
	LifeCycle (Chaque cycle possede une sequence de phases)
	   |
	   |____ phases (c'est une etape dans le lifecycle)
				|
				|___goals ( chaque phase est une sequence de goals, chaque goal a une tache spécifique)

				----plugin ( c'est un ensemble de goal, mais pas necesserement lié a la meme phase )
```				

### Phase:
exp le lifecycle Default possede 23 phases puisq'il est considèré comme le principal lifecycle de contruction, 
le clean lifeCycle possede 3 phase, et le site lifeCycle possede 4 phases.

Les phases sont executés dans un ordre bien déterminé, exemple de phases de default lifeCycle.

		validate: check if all information necessary for the build is available
		compile: compile the source code
		test-compile: compile the test source code
		test: run unit tests
		package: package compiled source code into the distributable format (jar, war, …)
		integration-test: process and deploy the package if needed to run integration tests
		install: install the package to a local repository
		deploy: copy the package to the remote repository
		
link : https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#lifecycle-reference

### Goal:
Voici un exemple de phases ainsi leurs respectives goals 
plugin:goal

```
compiler:compile – the compile goal from the compiler plugin is bound to the compile phase
compiler:testCompile is bound to the test-compile phase
surefire:test is bound to test phase
install:install is bound to install phase
jar:jar and war:war is bound to package phase
We can list all goals bound to a specific phase and their plugins using the command:
```

mvn help:describe -Dcmd=PHASENAME
For example, to list all goals bound to the compile phase, we can run:

```mvn help:describe -Dcmd=compile```
And get the sample output:
```
compile' is a phase corresponding to this plugin:
org.apache.maven.plugins:maven-compiler-plugin:3.1:compile
```
link https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#built-in-lifecycle-bindings


### Plugin:

----plugin ( c'est un ensemble de goal, mais pas necesserement lié a la meme phase )

A Maven plugin is a group of goals. However, these goals aren't necessarily all bound to the same phase.

For example, here's a simple configuration of the Maven Failsafe plugin which is responsible for running integration tests:

```
<build>
    <plugins>
        <plugin>
            <artifactId>maven-failsafe-plugin</artifactId>
            <version>${maven.failsafe.version}</version>
            <executions>
                <execution>
                    <goals>
                        <goal>integration-test</goal>
                        <goal>verify</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```
As we can see, the Failsafe plugin has two main goals configured here:

* integration-test: run integration tests
* verify: verify all integration tests passed

We can use the following command to list all goals in a specific plugin:

mvn <PLUGIN>:help
For example, to list all goals in the Failsafe plugin:
```mvn failsafe:help```

To run a specific goal, without executing its entire phase (and the preceding phases) we can use the command:

mvn <PLUGIN>:<GOAL>
For example, to run integration-test goal from Failsafe plugin, we need to run:
```mvn failsafe:integration-test```

link: https://www.baeldung.com/maven-goals-phases