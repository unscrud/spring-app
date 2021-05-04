Criar um projeto spring boot com as depências descritas no pom.xml
Criar um dockerfile como descrito na aplicação

Executar o comando 'docker build -t spring-app .'
    sendo o -t para definir o nome (spring-app) da imagem e o ponto (pasta atual) o local do dockerfile 

Executar o comando 'docker images' para ver a imagem criada

Executar o comando 'docker run -p 8080:8080 spring-app'
    esse 8080:8080 serve para que ao fazer uma requisição local para a porta 8080,
    então ela será redirecionada para a porta 8080 do tomcat rodando  dentro do container

Executar o comando 'curl http://localhost:8080/ping' 
    para chamar o controller criado na aplicação e ver o resultado no terminal

Devemos também realizar uma configuração para usar o devtools de forma que a aplicação seja atualizada ao salvar algum arquivo
    
    no pom.xml devemos adicionar a configuration:
    <build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<excludeDevtools>false</excludeDevtools>
				</configuration>
			</plugin>
		</plugins>
	</build> 

    em application.properties devemos usar uma chave que o devtools usará para saber qual aplicação deve atualizar:
        spring.devtools.remote.secret=<<chave desejada>>

    Após devemos recompilar o projeto com o maven (O Spring Initializr já provê um maven junto com a aplicação)
        './mvnw clean package -DskipTests' e logo em seguida realizar a build da imagem docker e rodar a imagem

    por fim faremos um link com a aplicação remota.
        no vscode...
            clicaremos no menu Run > add configuration e adicionaremos o bloco abaixo no launch.json
                {
                    "type": "java",
                    "name": "Launch Remote SpringAppApplication",
                    "request": "launch",
                    "mainClass": "org.springframework.boot.devtools.RemoteSpringApplication",
                    "args": "http://localhost:8080",
                    "projectName": "spring-app"
                }

Agora nossa aplicação estará funcionando em um container usufruindo dos benefícios do devtools
    