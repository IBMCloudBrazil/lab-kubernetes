1. Abra um terminal e confirme que você tem o docker instalado através do comando:
```
docker -v
```

2. Como em todo exemplo, vamos começar com um hello world.
```
docker run hello-world
```
Note que a primeira linha depois da execução do comando é "Unable to find image 'hello-world:latest' locally"

3. Execute novamente o mesmo comando
```
docker run -l lab hello-world
```
Na segunda execução o comando rodou um pouco mais rápido pois a imagem necessária para a execução já existia localmente.
Porém esse é um container atípico pois ele é executado uma única vez. O mais comum são containers que continuam sendo executados após a criação.

4. Crie um container que executa um banco de dados com o comando:
```
docker run -l lab -d couchdb
```
Nesse comando utilizamos um label "lab" para ajudar a identificar o que criamos.

5. Execute o comando abaixo para listar todos os containers em execução:
```
docker ps
```

6. Se houverem muitos containers em execução e você quiser ver apenas o que criamos nesse lab, execute o comando com um filtro:
```
docker ps --filter "label=lab"
```

7. Execute novamente o comando que executa um container com um banco de dados:
```
docker run -l lab -d couchdb
```

8. Observe que agora o comando **docker ps** irá apresentar dois containers diferentes em execução, baseados na mesma imagem.
```
docker ps --filter "label=lab"
```

9. Agora vamos interromper a execução do último container criado. Copie o **CONTAINER ID** apresentado no comando anterior para o último container, e substitua ele no comando abaixo:
```
docker stop <CONTAINER ID>
```
Exemplo:

>docker stop **b0512af341c7**

10. Execute novamento o comando **docker ps** e note que apenas 1 container será exibido.
```
docker ps --filter "label=lab"
```

11. Interrompa a execução do container restante, copiando **CONTAINER ID** apresentado no comando anterior e executando o comando **stop**
```
docker stop <CONTAINER ID>
```
12. Verifique os containers criados nesse lab já não estão mais em execução.
```
docker ps --filter "label=lab"
```

13. Agora liste todas as imagens disponveis localmente através do comando **images** e encontre uma imagem do **couchdb**
```
docker images
```

14. Tente remover a imagem do **couchdb** e observe que não é possível pois ainda há referências à ela.
```
docker rmi couchdb
```
