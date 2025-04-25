
## Instalação

O processo de instalação da aplicação se inicia clonando o repositório para o seu computador local.

```bash
git clone https://github.com/gwrgwr/technova.git
```

Como a aplicação roda em um Cluster do Kubernetes, precisa-se instalar localmente o minikube (ou outro cluster de preferência) se já não instalado

### Instalação Minikube para Windows
#### Powershell (preferencialmente, rode como administrador)

```powershell
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing

```
O comando acima instala o minikube na sua máquina

```powershell
$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){
    [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine)
}
```

### Instalação em distros Ubuntu/Debian
#### Bash

```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```


#### Para rodar o cluster minikube, no seu terminal rode:

```bash
minikube start
```

Após algum tempo, o seu minikube estará configurado. Caso queira verificar se está tudo ok, no seu terminal, novamente, rode:

```bash
minikube status
```

O retorno deste comando deve ser algo como:

```
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

Se no seu terminal, retornou estes comandos, o seu minikube está configurado e pronto para ser usado.

## Incialização

#### Antes de começar a usar a aplicação

Um último comando é necessário para ser rodado. No repositório local (após ter dado `git clone https://github.com/gwrgwr/technova.git`), percebe-se que há uma pasta chamada "k8s". Esta pasta contém todos os scripts necessários para que você consiga iniciar a aplicação localmente. O comando utilizado para rodar esses arquivos é (no root da aplicação):

```bash
kubectl apply -f k8s/
```

Ele aplica toda configuração da nossa aplicação para o cluster (minikube) pegando todos arquivos contidos na pasta `k8s/`

#### Checar se estão rodando

Com isso, podemos verificar se os Pods do Kubernetes estão rodando normalmente com o comando:

```bash
kubectl get pods -n technova --watch
```

#### É necessário que apareça "Running" em todos os Pods

## Utilização

Para pode acessar os endpoints e o swagger, precisaremos pegar a porta do api-gateway (onde está contido todos os controllers) com o comando:

```bash
minikube ip
```

Vai retornar o ip à ser utilizado para o acesso dos endpoints. Com ele, acesse o seu navegador e na url insira:

```bash
http://<minikube-ip>:30080
```
