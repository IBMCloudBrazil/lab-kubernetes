# Laboratório 1.2 Docker Websphere Liberty

1. Baixe do docker hub a imagem do Websphere Liberty através do comando:
```
docker pull websphere-liberty
```

2. Verifique que a imagem está disponível localmente agora.
```
docker images
```

3. Rode o comando a seguir para verificar quanto sua máquina está consumindo de CPU.
```
uptime
```

4. Com esse comando você irá criar 5 instâncias do Websphere Liberty
```
for i in 9980 9981 9982 9983 9984; do docker run -l lab -d -p $i:9080  websphere-liberty:webProfile7 ; done
```

5. Cada linha de resultado do comando anterior apresenta o ID de um dos containers criados. Para ver quanto de recurso esse container está consumindo rode o comando abaixo, substituindo o **<CONTAINER_ID>** por um dos IDs do comando anterior.
```
docker stats <CONTAINER_ID>
```
Exemplo
> docker stats **da0891348decb2b3deaff486f9a200a2bc43be842a6e962d7e14c3a3cbb71a93** 

6. Para interromper o comando anterior pressione **Ctrl+C**. Rode novamente o comando para verificar o consumo de CPU e observe que a carga foi pequena.
```
uptime
```
7. Nesse passo você ira navegar para uma página que está rodando no servidor Websphere Liberty que você acabou de criar. Abra uma nova aba no seu navegador e abra o endereço http://localhost:9980

8. Note que como subimos 5 instâncias do Websphere Liberty, você pode navegar também para os endereços com as portas 9981, 9982, 9983 e 9984.

9. Agora vamos listar todos os containers criados nesse laboratório.
```
docker ps --filter "label=lab"
```

10. Antes de prosseguir, vamos parar os container através do comando:
```
docker stop <CONTAINER_ID>
```

11. Nessa parte vamos fazer o deploy de uma aplicação de exemplo no Liberty. Comece criando um novo diretório e um arquivo vazio chamado Dockerfile
```
mkdir JavaSample
cd JavaSample
touch Dockerfile
```

12. Abra o arquivo **Dockerfile** com o seu editor de texto preferido.
```
gedit Dockerfile
```

13. Copie o conteúdo abaixo para o arquivo e salve ele.
```
FROM websphere-liberty:webProfile7
ADD JavaHelloWorldApp.war
```

14. Execute o comando abaixo para fazer o download de uma aplicação de exemplo.
```
curl https://raw.githubusercontent.com/IBM-Cloud/java-helloworld/master/target/JavaHelloWorldApp.war >> JavaHelloWorldApp.war
```

15. Com esse comando você irá construir uma nova imagem Docker baseada no arquivo Dockerfile.
```
docker build -t app .
```

16. O comando abaixo executa um container baseado na imagem criada.
```
docker run -d -p 9985:9080 app
```

17. Abra um navegador e digite a URL http://localhost:9985/JavaHelloWorldApp/

Pronto! Você concluiu com sucesso o laboratório 1.
Voltar ao início [Laboratórios](../README.md)
