1. Abra um terminal e confirme que você tem o docker instalado através do comando:
```
docker -v
```

2. Como em todo exemplo, vamos começar com um hello world.
```
docker run -l lab hello-world
```
Nesse comando utilizamos um label "lab" para ajudar a identificar o que criamos. Note também que a primeira linha depois da execução do comando é "Unable to find image 'hello-world:latest' locally"

3. Execute novamente o mesmo comando
```
docker run -l lab hello-world
```
