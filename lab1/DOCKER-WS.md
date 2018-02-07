# Laboratório 1.2 Docker Websphere Liberty

1. Baixe do docker hub a imagem do Websphere Liberty através do comando:
```
docker pull websphere-liberty
```

2. Verifique que a imagem está disponível localmente agora.
```
docker images
```

3. Rode o comando a seguir para verificar quanto 
```
uptime
```

4. Com esse comando 
```
for i in 80 81 82 83 84; do docker run -d -p $i:9080  websphere-liberty:webProfile7 ; done
```

5. Rode o comando a seguir para verificar quanto 
```
uptime
```
