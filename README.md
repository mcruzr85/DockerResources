Docker Engine (Docker Nativo) diretamente instalado no WSL2
O Docker Engine é o Docker nativo que roda no ambiente Linux e completamente suportado para WSL 2. Sua instalação é idêntica a descrita para as próprias distribuições Linux disponibilizadas no site do Docker.

Vantagens
Consume o mínimo de memória necessário para rodar o Docker Daemon (servidor do Docker).
É mais rápido ainda que com Docker Desktop, porque roda diretamente dentro da própria instância do WSL2 e não em uma instância separada de Linux.
Temos a melhor experiência de desenvolvimento, pois podemos usar o Docker diretamente dentro do WSL 2, sem precisar de uma instância separada do Docker Desktop.
Desvantagens
Necessário executar o comando sudo service docker start sempre que o WSL 2 foi reiniciado (Somente para usuários do Windows 10). Isto não é necessariamente uma desvantagem, mas é bom pontuar. Isto é um pequeno detalhe, mas no Windows 11 já é possível iniciar o servidor do Docker automaticamente pelo /etc/wsl.conf (Ver detalhes mais abaixo).
Se necessitar executar o Docker em outra instância do WSL 2, é necessário instalar novamente o Docker nesta instância ou configurar o acesso ao socket do Docker desejado para compartilhar o Docker entre as instâncias.
Não suporta containers no modo Windows.
Integrar Docker com WSL 2
No início deste tutorial vimos 4 modos de usar Docker no Windows, mas somente 2 que recomendamos:

Docker Engine (Docker Nativo) diretamente instalado no WSL2.
Docker Desktop com WSL2.
Recomendamos que escolha a 1ª opção pelos seus benefícios, já que a maioria das pessoas poderão usar o WSL 2 como ferramenta central para desenvolvimento, mas, neste tutorial vamos mostrar as duas formas de instalação.

1 - Instalar o Docker com Docker Engine (Docker Nativo)
A instalação do Docker no WSL 2 é idêntica a instalação do Docker em sua própria distribuição Linux, portanto se você tem o Ubuntu é igual ao Ubuntu, se é Fedora é igual ao Fedora. A documentação de instalação do Docker no Linux por distribuição está aqui, mas vamos ver como instalar no Ubuntu.
Instale os pré-requisitos:

sudo apt update && sudo apt upgrade
sudo apt remove docker docker-engine docker.io containerd runc
sudo apt-get install \
 apt-transport-https \
 ca-certificates \
 curl \
 gnupg \
 lsb-release

    Adicione o repositório do Docker na lista de sources do Ubuntu:

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
 "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
 $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

Instale o Docker Engine  
 sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

Dê permissão para rodar o Docker com seu usuário corrente:

sudo usermod -aG docker $USER

Inicie o serviço do Docker:

sudo service docker start

Este comando acima terá que ser executado toda vez que o Linux for reiniciado. Se caso o serviço do Docker não estiver executando, mostrará esta mensagem de erro ao rodar comando docker:

Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
O Docker Compose instalado agora estará na versão 2, para executa-lo em vez de docker-compose use docker compose.

Iniciar o Docker automaticamente no WSL (apenas para Windows 11)
No Windows 11 é possível especificar um comando padrão para ser executados sempre que o WSL for iniciado, isto permite que já coloquemos o serviço do docker para iniciar automaticamente. Edite o arquivo /etc/wsl.conf:

Rode o comando para editar:

sudo vim /etc/wsl.conf

Aperte a letra i (para entrar no modo de inserção de conteúdo) e cole o conteúdo:
[boot]
command="service docker start" Quando terminar a edição, pressione Esc, em seguida tecle : para entrar com o comando wq (salvar e sair) e pressione enter.

Pronto, basta reiniciar o WSL com o comando wsl --shutdown no DOS ou PowerShell para testar. Após abrir o WSL novamente, digite o comando docker ps para avaliar se o comando não retorna a mensagem acima: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
