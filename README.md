# Instalação do WikiJS, em um cluster Kubernetes, via manifestos YAML:

## Antes de tudo:

- Configurar o acesso ao cluster em que será instalado o WikiJS:

```bash
export KUBECONFIG=/path/to/kubeconfig
```


- Verificar a conexão com o cluster:

```bash
kubectl get nodes
```

Você deve ser capaz de ver todos os nodes do seu cluster Kubernetes.


- Faça o clone do repositório:

```bash
git clone https://git.jac.bsb.br/JAC/infra/wikijs.git

cd wikijs
```

## Algumas alterações necessárias:

Com o clone do repostiório já feito, será preciso alterar algumas variáveis nos Secrets e no ```storageClassName```:

### MariaDB

- Alterar o valor da variável ```mysql-root-passwd``` no Secret do MariaDB

```
vim WikiJS-manifestos/mariadb-wikijs/mariadb-secret.yml
```

**NOTA:** Lembre-se que o valor passado no Secret precisa estar em Base64.

- Alterar o campo ```storageClassName```, no arquivo que cria o PVC para o MariaDB, para o nome do seu storage:

```
vim WikiJS-manifestos/mariadb-wikijs/mariadb-pvc.yml
```

### App WikiJS

- Alterar o valor da variável ```db-pass``` no Secret do WikiJS

```
vim WikiJS-manifestos/app-wikijs/wikijs-secret.yml
```

**NOTA:** Lembre-se que o valor passado no Secret precisa estar em Base64 e, no caso do Secret do WikiJS, a senha passada precisa ser a mesma senha do MariaDB.


## Instalando o WikiJS no cluster:

Com os valores das variáveis apontadas acima já definidos, podemos partir para a instalação do WikiJS.

- Fazer o Deploy do MariaDB:

```bash
kubectl apply -f WikiJS-manifestos/mariadb-wikijs/
```

Esperamos o MariaDB concluir a instalação e em seguida instalamos a aplicação do WikiJS:

```bash
kubectl apply -f WikiJS-manifestos/app-wikijs/
```
