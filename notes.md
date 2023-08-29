#### 28/08/2023

Curso de Jenkins e Docker: Pipeline de entrega continua

```
Utils Command:

sudo lsof -i:8080
kill -9

brew services start jenkins-lts
brew services stop jenkins-lts
brew services restart jenkins-lts

php -S localhost:8000

nginx -s reload
nginx -t

php -S localhost:8000

brew install nginx
brew install php
```


https://stackoverflow.com/questions/70541720/jenkins-has-no-installation-candidate-error-while-trying-to-install-jenkins-on
https://stackoverflow.com/questions/15174194/jenkins-host-key-verification-failed
https://github.com/ome/devspace/issues/38

1  sudo su
2  ssh-keygen
3  cat /var/lib/jenkins/.ssh/id_rsa.pub
4  exit
5  vim ~/.ssh/id_rsa
6  ssh -oStrictHostKeyChecking=no host
7  git ls-remote -h -- git@github.com:souzaleandror/jenkins-todo-list.git HEAD
8  git ls-remote -h -- git@github.com:souzaleandror/jenkins-todo-list.git HEAD
9  history

1  cd /vagrant/scripts
2  sudo ./jenkins.sh
3  cp jenkins.sh jenkins.sh.ckp
4  mv jenkins.sh.ckp jenkins.sh.bkp
5  ls
6  sudo apt update
7  sudo apt install openjdk-17-jre
8  curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
9  echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
10  vi jenkins.sh
11  sudo ./jenkins.sh
12  ip addr
13  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
14  vi jenkins.sh
15  ls
16  cd /
17  cdls
18  ls
19  cd vagrant/
20  ls
21  cd jenkins-todo-list/
22  cat ~/.ssh/id_rsa
23  git ls-remote -h -- git@github.com:souzaleandror/jenkins-todo-list.git HEAD
24  cat ~/.ssh/id_rsa.pub
25  cat ~/.ssh/id_rsa
26  git ls-remote -h -- git@github.com:souzaleandror/jenkins-todo-list.git HEAD
27  cat ~/.ssh/id_rsa
28  ssh -T git@github.com
29  git ls-remote -h -- git@github.com:souzaleandror/jenkins-todo-list.git HEAD
30  cat ~/.ssh/known_hosts
31  cat ~/.ssh/authorized_keys
32  ssh-keyscan github.com >> ~/.ssh/known_hosts
33  sudo su -s /bin/bash jenkins
34  vi .ssh/authorized_keys
35  sudo vim .ssh/authorized_keys
36  vim ~/.ssh/
37  git ls-remote -h -- git@github.com:souzaleandror/jenkins-todo-list.git HEAD
38  sudo vim .ssh/authorized_keys
39  sudo vim ~/.ssh/authorized_keys
40  ls
41  ssh -oStrictHostKeyChecking=no host
42  history

@01-O primeiro job com Jenkins

@@01
Introdução

[00:00] Olá pessoal, tudo bem? Bem-vindos ao curso de Jenkins e Pipeline.
[0:05] Meu nome é Rafael Zago, eu trabalho como SysAdmin há mais de 10 anos, trabalho há muitos anos com DevOps Integração Contínua e eu tô aqui pra apresentar pra vocês esse conteúdo que é muito legal, muito interessante, e, com certeza, vai ajudar demais o dia a dia de vocês, seja desenvolvedor, seja o pessoal de teste ou pessoal de produção e operações.

[0:29] O que a gente vai aprender nesse curso? A gente vai aprender a criar um Pipeline utilizando Jenkins que é a maior ferramenta de Integração Contínua do mundo. A gente vai aprender a usar o Docker junto com Jenkins, integração de Docker e Jenkins pra colocar a nossa aplicação em container.

[0:46] Nós vamos registrar essas imagens criadas numa nuvem dentro do Docker Hub. A gente vai conseguir também integrar os testes de uma maneira muito fácil e contínua, a integrar com Slack pra notificar o time se funciona ou não funciona o nosso Deploy, se deve continuar ou não deve continuar.

[1:05] Nós vamos trabalhar também com SonarQube pra fazer cobertura de código, pra fazer análise de qualidade de código, pra fazer uma melhora continua... Que, na verdade, quando a gente fala em Integração Contínua com Jenkins a gente tá falando em melhoria de ambiente de desenvolvimento e produção, melhoria de entrega.

[1:21] O famoso Time to Market que a gente tem que ter, quanto mais integrado, mais ágil, mais rápido a gente entrega o produto pro nosso cliente.

[1:30] Eu espero vocês. Aproveitem durante esse curso a gente vai montar todo esse Pipeline que tá aqui atrás de mim e vamos para a primeira aula.

@@02
O que é integração contínua?

[00:00] Olá aluno, bem vindo. Antes da gente começar o nosso curso de Jenkins, eu queria bater um papo com vocês sobre onde ele se insere e qual é a importância dessas metodologias novas que a gente tá abordando.
[00:12] O Jenkins se insere dentro de um ambiente de Integração Contínua, e eu queria falar com você sobre Integração Contínua agora.

[00:20] A Integração Contínua existe pra, basicamente, solucionar alguns problemas antigos pra quem trabalha com desenvolvimento de software, uma metodologia Waterfall, por exemplo.

[00:33] O primeiro deles é que os times trabalham em silos, então pelo fato deles trabalharem em silos a comunicação entre eles é muito lenta. O Dev vai lá e monta o pacotezinho dele e joga pelo muro pro pessoal de produção colocar e aplicar isso pro cliente, ou às vezes, ele pega o código dele e entrega pro pessoal de teste e fala "Olha tá aqui pronto o código, pode trabalhar".

[00:59] E essa falta de comunicação entre esses silos prejudica muito o ciclo de desenvolvimento e o quão saudável é uma aplicação.

[01:09] Outra coisa que é legal falar é o tempo de reação à falhas, qual é o problema que isso gera. Porque quando você demora muito pra reagir às falhas você impacta demais o seu negócio.

[01:21] Então a gente tá falando de times que trabalham em silos, que pegam seus projetos e pacotes e passam pra outro time e entregam o bastão. A gente tá falando de processos que são muito burocráticos, mudanças que precisam ser aprovadas por pessoas, que geram gargalos. A gente tá falando de reagir lentamente às falhas.

[01:41] E tudo isso impacta o famoso Time to Market, quanto tempo demora pra eu reagir ao negócio e botar minha aplicação pra rodar. Então, trabalhando com Integração Contínua, trabalhando com Jenkins a gente reduz esse gap e facilita muito a vida tanto de quem trabalha, quanto de quem consume as aplicações.

[02:03] Então qual que é o meu objetivo com vocês? É ensinar a ferramenta mais utilizada pra Integração Contínua no mundo, que é o Jenkins. O Jenkins é muito versátil, tem muitos plugins, e você consegue fazer praticamente todo o processo manual, de hoje, e consegue converter ele pra um processo mais automatizado.

[02:25] Nós vamos construir um Pipeline desde o commit do código até a entrega do software, passando pelas etapas de teste, validação e homologação.

@@03
Agilidade com pipelines

Quais das afirmações abaixo representam uma vantagem da adoção de integração contínua no desenvolvimento de software?
Alternativa correta
Agregar todo o processo de construção de artefatos, controle de qualidade e entrega de software em um fluxo contínuo e transparente para todos os envolvidos
 
Alternativa correta! Esta afirmação reflete o resultado de uma boa integração contínua.
Alternativa correta
Melhora na cobertura de qualidade do código do projeto, visto que é possível analisar o produto durante o processo de construção
 
Alternativa correta! Esta afirmação reflete o resultado de uma boa integração contínua.
Alternativa correta
Os times vão trabalhar em silos, focando exclusivamente na qualidade de entrega das suas tasks diárias, reduzindo assim o re-trabalho
 
Alternativa correta
Diminuição do time to market na entrega do produto para o mercado
 
Alternativa correta! Esta afirmação reflete o resultado de uma boa integração contínua.

@@04
Projeto inicial do treinamento

Baixe aqui o ZIP do projeto inicial deste treinamento, necessário para a continuidade do mesmo.
Além disso, o código-fonte de aplicação de exemplo pode ser baixado aqui.

https://caelum-online-public.s3.amazonaws.com/1110-jenkins/01/1110-aula-inicial.zip

https://caelum-online-public.s3.amazonaws.com/1110-jenkins/01/jenkins-todo-list.zip

@@05
Ambiente inicial

O script que o instrutor segue durante a aula é o seguinte:
# Configurando o ambiente e conectando máquina virtual:
    # Máquina virtual com o vagrant
    # Requisitos: http://vagrantup.com e https://www.virtualbox.org/
        # Baixar e descompactar o arquivo 1110-aula-inicial.zip
        # Entendendo o Vagranfile
        # Subindo o ambiente virtualizado
            vagrant plugin install vagrant-disksize
            vagrant up
            vagrant ssh
                ps -ef | grep -i mysql # Verificando se o MySQL esta rodando
                mysql -u devops -p # Senha mestre; show databases
                mysql -u devops_dev -p # Senha mestre; show databases
                # Instalando o Jenkins
                    cd /vagrant/scripts
                # Visualizar o conteudo do arquivo de instalacao do jenkins
                    sudo ./jenkins.sh

                # Acessar:  192.168.33.10:8080
                    sudo cat /var/lib/jenkins/secrets/initialAdminPassword

                # Credenciais
                    Nome de usuário: alura
                    Senha: mestre123
                    Nome completo: Jenkins Alura
                    Email: aluno@alura.com.br

                # Reload nas permissoes do docker
                    sudo usermod -aG docker $USER
                    sudo usermod -aG docker jenkins
                    exit
            vagrant reloadCOPIAR CÓDIGO
[00:00] Olá aluno, tudo bem? Bem-vindo de volta, vamos continuar nosso curso de Jenkins. Na aula agora a gente vai entender como vai funcionar o nosso ambiente pra esse treinamento.

[0:12] Como cada um de vocês possui uma característica diferente de sistema operacional, distribuição pra quem utiliza Linux, a gente vai padronizar a nossa configuração da seguinte maneira: nós vamos utilizar o VirtualBox pras máquinas virtuais, acessando virtualbox.org a gente consegue fazer o download, cada um faz pra sua própria distribuição e o Vagrant pra fazer a orquestração dessas máquinas virtuais.

[0:40] Vocês tem como pré-requisito pra esse treinamento o curso de Vagrant, se vocês seguirem lá vocês vão entender exatamente como que funciona cada um dos blocos que a gente vai acabar utilizando.

[0:50] Tem mais uma ferramenta que a gente vai utilizar também que é o Cmder que é uma ferramenta pra quem usa Windows, é um terminal um pouquinho mais rico em recursos, diferente do prompt padrão. Já quem usa Linux ou Mac pode usar o próprio terminal mesmo, os comandos funcionam pros dois.

[01:08] Então o primeiro arquivo que vocês fizeram download pra aula foi o aula-inicial.zip, a gente vai extrair esse arquivo aqui pra entender como vai funcionar. Então a gente vai ver agora o primeiro arquivo do nosso treinamento que é o Vagrantfile, vamos editar aqui esse arquivo pra entender mais ou menos o que ele faz.

[01:31] Bem rapidamente, ele basicamente determina qual vai ser o sistema operacional que a gente vai usar, que vai ser o Ubuntu; o quanto de disco que a gente vai utilizar, por padrão o Vagrant usa 10GB, a gente vai precisar de um pouquinho mais pra fazer algumas coisas; o mapeamento das portas que a gente vai utilizar tá dentro dessa seção aqui; ele vai criar um IP, esse IP é muito importante, a gente vai usar ele pra quase todos os serviços que a gente vai acessar; e, aqui pra baixo, o quanto de memória e algumas configurações que são meio que necessárias pra gente poder trabalhar nesse início de treinamento.

[02:10] Então aqui, feito o download, os requisitos básicos a gente já cobriu, a gente descompactou esse arquivo e entendeu como o Vagrantfile funciona. Feito isso, depois da sua instalação, você acabou reiniciando a máquina, vamos só validar pra ver que realmente o Vagrant tá instalado e tá apto a funcionar.

[02:31] Então pra fazer essa validação a gente vai digitar vagrant -h, lembrando que eu tô já com o Cmder aberto e eu naveguei até o diretório do meu projeto, se eu digitar aqui um dir ele me mostra que, basicamente, ele tem o zip e os arquivos que a gente extraiu.

[02:50] Só o último overview em relação a esses arquivos, eu tenho alguns scripts aqui nesse diretório, esses scipts vão pré-instalar algumas coisas pra gente. Basicamente a pré-instalação vai ser a instalação do Docker que é a instalação padrão dele. Se a gente editar o arquivo aqui do Docker a gente vai ver como é que ele é composto, é do próprio site do Docker mesmo o arquivo de instalação dele.

[03:17] E tem um arquivinho, esse aqui é um complemento da nossa instalação só pra mostrar pra vocês, a gente chama o arquivo de instalação, puxa uma imagem aqui que a gente vai entender mais pra frente pra que que ela serve, e dá algumas permissões pros usuários.

[03:34] E tem um arquivinho aqui, que é o arquivo principal do nosso curso, entre aspas, que é como que o Jenkins é instalado. Então se a gente abrir aqui o arquivo de instalação a gente vai entender que é um arquivo bem simples, basicamente ele baixa a chave dos repositórios, a gente tá usando Ubuntu novamente, e faz um apt-get update e instala o Jenkins.

[03:56] Da onde veio esse script? Só por curiosidade, se a gente vir aqui no Google e digitar Jenkins Ubuntu, na própria wiki do Jenkins é o mesmo script que a gente tá trabalhando pra fazer a instalação. Então instalação feita aqui, agora a gente vai começar a colocar essa máquina pra rodar.

[04:22] Então pra fazer o Vagrant funcionar, a gente precisa primeiro fazer a instalação de um plugin pra que permita um tamanho de disco maior dentro do Vagrant. Esse plugin a gente instala com o seguinte comando: vagrant plugin install vagrant-disksize.

[04:41] Então a gente vem aqui no terminal e faz a instalação desse plugin. Esse plugin, enquanto ele instala, ele basicamente vai permitir que a gente coloque um parâmetro que aumente o tamanho do disco dentro do Vagrantfile, por default são 10GB, a gente vai colocar 30GB pra que caiba todos os nossos sistemas. Assim que terminar a instalação, a gente volta.

[05:02] Feita a instalação do plugin, a gente vai começar a provisionar a máquina com o seguinte comando: vagrant up. A partir de agora ele vai ler o Vagrantfile e vai fazer o provisionamento da máquina. Assim que a instalação terminar, a gente volta.

[05:24] Bom, nossa instalação acabou, então agora a gente vai configurar a nossa máquina pra fazer a instalação do Jenkins. Então a primeira coisa que a gente vai fazer é logar na máquina utilizando o comando vagrant ssh, com esse comando ele vai fazer a autenticação no Linux utilizando uma chave que o próprio sistema criou pra gente.

[05:50] Então conectamos, a gente tá vendo aí que tem o IP da máquina, tem o quanto que ela tá utilizando de recurso. Vamos fazer uma validação rápida que é se o banco de dados foi instalado, então cabe um parênteses aí.

[06:04] O banco de dados a gente deixou pré-configurado no Vagrant pra que vocês não se preocupem com isso e que é uma especificidade da nossa aplicação. Nesse caso nós criamos a seguinte configuração: nós criamos dois usuários que é usuário devops e o usuário devops dev.

[06:25] Qual que é a diferença entre eles? Um vai acessar o banco de produção e o outro o banco de desenvolvimento. Os dois têm a mesma senha, que é a senha mestre, é uma senha que a gente vai utilizar no decorrer do curso pro banco de dados, e, pra esses usuários, eu também criei os bancos de dados respectivos, tanto de dev quanto de prod. Então vamos validar isso aqui. Primeira coisa, vamos validar se o MySQL foi instalado, ps -ef | grep -i mysql. Então nosso MySQL tá rodando, tá legalzinho.

[07:02] Agora vamos conectar no banco e testar se as permissões estão ok. Primeira coisa que a gente vai fazer: mysql -u devops -p, a senha mestre, conectamos o banco. Vamos, esse bios, as databases que a gente tem, show databases. O banco de dados todo tá criado. Então esse vai ser o nosso de produção.

[07:32] Vamos fazer o teste agora com o usuário devops_dev, com a mesma senha mestre. Vamos dar uma olhadinha nos bancos de dados. Eu tenho o todo dev e que é o banco de dados que vai receber os dados em desenvolvimento, e eu tenho um outro que é o test_todo_dev, por quê? Porque quando a gente for criar o nosso Pipeline o teste vai ser executado somente com o usuário de desenvolvimento, o nosso Pipeline, quando ele passar pelo teste em produção, não faz sentido eu testar novamente.

[08:09] Então agora que a gente criou e já fez a validação dos nossos dados, vamos instalar o Jenkins que é o ator principal do nosso treinamento, pra isso a gente vai no diretório vagrant/scripts$. Esse /vagrant é automático, é uma configuração que vocês vão ter aí também. E dentro de scripts eu tenho o script jenkins.sh, então o que que eu vou fazer agora? Com o sudo eu vou rodar esse cara.

[08:44] Ele vai perguntar pra mim se eu quero fazer a instalação, ele atualizou os meus repositórios e aí a gente vai deixar ele instalar. A gente dá um ok e deixa ele instalar, assim que a instalação acabar a gente volta.

[09:00] Bom, a instalação acabou, agora tá na hora da gente acessar o Jenkins pela primeira vez. Pra isso a gente precisa primeiro descobrir qual que é o IP que a nossa máquina tem, com o seguinte comando: ip adress. Esse aqui é o IP que a gente vai usar.

[09:16] O Jenkins sobe na porta 8080 como padrão, então pra acessar o nosso Jenkins a gente abre o nosso navegador e digita o IP, dois pontos e a porta de conexão. Legal, apareceu a tela do Jenkins, tá na hora da gente fazer a instalação dele, e é muito simples fazer. Ele pede uma chave de instalação e ele ainda fala onde essa chave tá pra gente, /var/lib/jenkins. A gente vai dar um cat nessa chave aqui. Essa aqui é a chave de instalação.

[09:58] Porque que eu usei o sudo? Por que esse arquivo tá protegido pra edição e leitura. Copio a chave, colo a chave aqui na senha de administrador e dou um continuar. Feito isso ele vai perguntar pra gente o seguinte: você quer instalar algum plugin ou quer instalar os plugins que são sugeridos?

[10:18] Vamos instalar os sugeridos porque alguns básicos já vem e a gente já consegue criar o nosso primeiro job. Então eu clico pra instalar os plugins sugeridos e agora a gente vai esperar a instalação terminar, demora uns minutinhos, e aí a gente volta.

[10:33] Legal, instalação concluída dos plugins, ele pede pra criar um usuário administrador. Então o usuário vai ser o seguinte: o nome do usuário vai ser alura, a senha vai ser mestre123, eu tô criando mestre123, e não mestre, porque ele pede números e dígitos. A gente confirma a senha aqui também, mestre123, nome completo Aluno Alura.

[11:07] O e-mail é o e-mail pra notificações, a gente vai usar um e-mail de exemplo, você vai utilizar o seu e-mail ou o e-mail da sua companhia pra receber as notificações de acordo com job que você configurar. Aqui vai ser aluno@alura.com.br. Só uma ressalva, os plugins que nós vamos utilizar não enviaram e-mails, então fiquem sossegados se vocês não tiverem um e-mail pra colocar aqui.

[11:33] E agora a gente salva e continua, ele confirma pra gente que ele vai subir nesse IP e nessa porta, a gente salva e termina. Pronto, nosso Jenkins tá instalado e tá pronto pra utilizar.

[11:48] Nas próximas aulas a gente vai começar a configurar o nosso Jenkins e configurar os primeiros jobs.

@@06
O que é Jenkins?

Durante o curso usaremos o Jenkins como ferramenta principal, mas o que é Jenkins?


Alternativa correta
Um servidor de integração continua.
 
Alternativa correta! O Jenkins é um servidor de Integração Contínua open-source escrito em Java. Ele é o mais popular mas não a única opção. Outros servidores de *Integração Contínua * são TeamCity, Bamboo, Travis CI ou Gitlab CI entre vários outros.
Alternativa correta
Uma ferramenta que permite o monitoramento de tarefas de maneira ágil.
 
Alternativa errada! Monitorar tarefas e projetos é o papel de ferramentas como Trello, Jira ou PivotalTracker entre vários outros.
Alternativa correta
Uma ferramenta de build (construção de pacote)
 
Alternativa errada! Ferramentas de build clássicos são Maven, Gradle, npm, Grunt ou make entre outros.

@@07
Versionamento do código

Atenção! Atualmente não utilizamos mais o termo master como repositorio principal, e sim main, sendo esse é o novo padrão do GitHub desde o fim de 2020. Essa nomenclatura é mais inclusiva e facilita o entendimento entre pessoas desenvolvedoras.
O script que o instrutor segue durante a aula é o seguinte:
# Passos para a configuracao do git e versionamento do codigo
# Criar uma conta em github.com
    ssh-keygen -t rsa -b 4096 -C "<seu-usuario>@gmail.com"
    cat ~/.ssh/id_rsa.pub
# Configurar a chave no github
    git config --global user.name "<seu-usuario>"
    git config --global user.email <seu-usuario>@<seu-providor>
    ssh -T git@github.com
# Fazer download do código nos anexos e copiar para o diretório compartilhado app
    cd /vagrant/jenkins-todo-list
    git init
    git add .
    git commit -m "Meu primeiro commit"
    git log
# Criar um repositório no github: jenkins-todo-list
    git remote add origin git@github.com:<seu-usuario>/jenkins-todo-list.git
    git push -u origin masterCOPIAR CÓDIGO
[00:00] Ok pessoal, então agora é a hora da gente começar a montar o nosso ambiente. A gente já tem o Vagrant funcional, já tem o MySQL e o Jenkins também. Pra exemplo nós fornecemos pra vocês uma aplicação escrita em Django que é um todo list, é muito simples, grava no MySQL e a gente vai utilizar ele daqui pra frente.

[00:20] Pra começar, a primeira coisa que a gente tem que fazer é extrair o arquivo dentro do nosso diretório de trabalho. Então é o arquivo jenkins-todo-list.zip, a gente vai extrair ele aqui.

[00:35] Bom, se a gente olhar agora no nosso diretório do Vagrant eu tenho o código fonte da minha aplicação. Porque que eu preciso do código fonte? Por que agora eu versionar o código fonte, eu vou colocar esse código dentro do GitHub. Pra que isso aconteça eu preciso ter, primeira coisa, um par de chaves SSH pra fazer a conexão.

[00:57] Então pra criar esse par de chaves que que a gente faz? A gente passa o tipo da chave com o e-mail. Ele vai perguntar pra gente se vai ser a chave padrão, vai ser a chave padrão, a gente vai dar um enter. Não vai ter passphrase a gente vai conectar direto utilizando um par de chaves. E aí foi criado.

[01:20] Agora que que a gente precisa fazer? A gente precisa, localmente, configurar tanto o nosso git com e-mail, quanto com o nosso usuário. Então a primeira coisa vai ser configurar com usuário e depois a gente vai configurar com o e-mail. Desse jeito, a hora que a gente fizer o commit do nosso código push pro repositório, ele vai saber quem é que fez e vai ter os registros direitinho lá.

[01:51] Agora é a etapa que a gente vai acessar o GitHub, porque? Eu tenho já o par de chaves, só que agora eu preciso colocar a minha chave pública no GitHub pra permitir acesso aos meus repositórios. Então o que a gente vai fazer? A gente vai abrir aqui o nosso github.com, aqui no canto alto a gente vai clicar em Settings aí aqui embaixo eu tenho as chaves de SSH que eu posso instalar, remover e manusear elas de qualquer maneira.

[02:27] Então eu vou criar aqui chamado alura-jenkins e agora eu preciso saber qual é a minha chave pública, é só eu pegar o conteúdo desse arquivo aqui, /home/vagrant/.ssh/id_rsa.pub que é a minha chave pública. Copio e colo aqui no campo da chave.

[02:58] Ele vai pedir pra eu confirmar minha senha. Senha confirmada, instalado, agora eu vou fazer o teste pra saber se a minha chave pública foi corretamente instalada e se tá tudo certinho. Dou um ssh -T git@github.com. Ele vai perguntar se eu aceito fingerprint, eu dou um yes e pronto, ele me reconheceu e me autenticou.

[03:29] Agora que que a gente vai fazer? A gente vai versionar o nosso código pela primeira vez. Então vamos acessar o diretório dele aqui, um ls aqui, é esse carinha aqui. Primeira coisa que eu tenho que fazer é iniciar o meu repositório local, o git é distribuído, ele é diferente de outros versionadores, então eu preciso iniciar o repositório localmente. Como é que eu faço isso? git init, ele inicializou um repositório aqui pra mim, tá local ainda, então a gente vai chegar lá.

[04:04] Agora que que a gente vai fazer? Vai adicionar todos os arquivos dentro desse versionamento, da seguinte maneira: "git add .". Isso a gente fez agora, mas normalmente no dia a dia a gente não versiona todos os arquivos todas as vezes que a gente altera alguma coisa, a gente versiona o arquivo que é necessário naquele momento. Nesse caso a gente tá versionando de uma vez só.

[04:29] E agora a gente vai criar o primeiro commit, só vamos trocar a mensagem aqui e colocar assim: "Meu primeiro commit". E ele adicionou todos os arquivos que estavam dentro desse diretório, com exceção dos arquivos que eu listei, ou diretórios, dentro desse arquivo aqui que é o gitignore. Então aqui a gente só exclui exatamente o que nós não queremos que entre dentro do nosso versionamento.

[05:05] Vocês, também como pré-requisito, têm o curso de git da Alura, vocês já devem ter assistido. É muito simples de criar esse arquivo, vamos dar um head nele só pra ver o conteúdo dele, são só a cada linha uma entrada, na hora que eu versionei tudo que ele colocou aqui em cima não estava, ele até avisa que tem um arquivo gitignore aqui.

[05:29] Bom, será que ele realmente versionou? Isso aqui tá tudo local. A gente vai digitar git log e aí a gente tem nosso primeiro versionamento local. Então é o meu primeiro commit. Qual foi o autor, qual foi o e-mail, ele trouxe daquelas configurações que a gente fez. Qual que é a data que foi versionado e qual que é o head, qual que é o código do commit, a gente vai ver mais para frente como que a gente lida com isso aí.

[05:56] Bom, agora que a gente tá dentro do GitHub a gente precisa criar um repositório. Como que eu crio um repositório no GitHub? Eu já criei um aqui mas eu vou mostrar pra vocês. A gente vem aqui nesse sinal de mais, Novo Repositório, coloquem o nome do repositório, por exemplo jenkins-todo-list, nesse nosso caso, defina se ele é público ou privado.

[06:24] Vale ressaltar que, se a gente colocar ele como privado, a única maneira de fazer clone ou qualquer interação com ele é usando chave SSH. E aí vocês vão criar o repositório. Como eu já tenho esse repositório ele tá falando que já existe na minha conta, que é esse repositório aqui.

[06:42] Agora tá quase no final, a gente já versionou localmente, a gente já fez o nosso commit, eu só preciso avisar agora onde fica o repositório central dele, que é o caso do GitHub. A gente faz usando esse comando aqui: git remote add, vamos colocar ele aqui, git remote add origin e o usuário git@github.com: o repositório.

[07:10] Acabei adicionando, e agora eu tenho que fazer o que? Enviar meu código pro repositório, ele tá versionado só localmente agora. E é muito simples fazer isso, git push origin master. Pronto, meu código tá versionado, se eu chegar aqui no repositório e atualizar a página tá aqui. Tá versionado, todos os meus arquivos estão aqui. E agora, a próxima aula a gente vai aprender como criar o nosso primeiro job no Jenkins pra olhar esse repositório.

[07:58] Bom, a gente já tá com o ambiente configurado, com nosso código versionado, com o Jenkins instalado e o GitHub já tem o último código nosso. Tá faltando o quê agora? Criar o nosso primeiro job pra construir a aplicação. Mas isso aí eu vou te mostrar no próximo vídeo.

@@08
Branch principal

Atenção! Atualmente não utilizamos mais o termo master como repositorio principal, e sim main, sendo esse é o novo padrão do GitHub desde o fim de 2020. Essa nomenclatura é mais inclusiva e facilita o entendimento entre pessoas desenvolvedoras.

@@09
Fluxo de um versionamento de código

O ponto inicial da nossa Pipeline de integração continua é o repositório GIT que hospedamos no Github.
Das alternativas abaixo, assinale aquela que aponta o fluxo correto de um versionamento inicial de um código-fonte com o Git:

git init --> git push origin master --> git add . --> git remote add origin git@github.com:<seu-usuario>/jenkins-todo-list.git --> git pull
 
Alternativa correta
git init --> git clone -m "Meu primeiro commit" --> git push origin master
 
Alternativa correta
git init --> git add . --> git commit -m "Meu primeiro commit" --> git remote add origin git@github.com:<seu-usuario>/jenkins-todo-list.git --> git push origin master
 
Alternativa correta! Esse é o fluxo básico é:
init
add e commit
remote add
push to masterCOPIAR CÓDIGO
Parabéns, você acertou!

@@10
O primeiro job

O script que o instrutor segue durante a aula é o seguinte:
# Configurando a chave privada criada no ambiente da VM no Jenkins
    cat ~/.ssh/id_rsa
    Credentials -> Jenkins -> Global Credentials -> Add Crendentials -> SSH Username with private key [ github-ssh ]
# Criando o primeiro  job que vai monitorar o repositorio
    Novo job -> jenkins-todo-list-principal -> Freestyle project:
    Esse job vai fazer o build do projeto e registrar a imagem no repositório.
    # Gerenciamento de código fonte:
        Git: git@github.com:rafaelvzago/treinamento-devops-alura.git [SSH]
        Credentials: git (github-ssh)
        Branch: master
    # Trigger de builds
        Pool SCM: * * * * *
    # Ambiente de build
        Delete workspace before build starts
    # Salvar
    # Validar o log em: Git Log de consulta periódicaCOPIAR CÓDIGO
[00:00] Bom pessoal, agora chegou uma parte legal, a gente vai criar nosso primeiro job no Jenkins e é muito simples de fazer. Antes de criar o job eu preciso fazer algumas configurações dentro do meu ambiente.

[00:11] Então a gente vai lá no nosso Jenkins e eu vou aqui em credenciais, vai pedir pra eu autenticar aqui e vamos autenticar de novo, dentro de credenciais eu vou clicar aqui em Jenkins, as credenciais globais, e eu vou adicionar uma credencial nova. Que tipo de credencial que eu vou adicionar? SSH com username e chave privada. Por que que eu preciso fazer isso? Pra que eu consiga interagir com meu repositório no GitHub.

[00:46] Então o que eu vou fazer? Eu vou criar o meu id aqui, eu vou chamar de github-ssh. Isso aqui é só um id dentro do Jenkins pra você referenciar em jobs futuros, e é legal a gente manter o padrão porque os outros jobs utilizam esse mesmo id. A descrição eu vou colocar a mesma. O usuário do GitHub é sempre git, não precisa ser seu usuário porque você vai conectar com chave.

[01:14] E aí eu vou ter que adicionar minha chave, como é que eu faço isso? Eu clico aqui pra entrar diretamente, clico pra adicionar a minha chave, e ele quer saber qual que é a minha chave. Então vamos voltar aqui no nosso terminal.

[01:26] Aqui no nosso terminal a gente tem, dentro do nosso diretório de SSH, o seguinte arquivo: id_rsa, ele não é um arquivo id_rsa.pub, aquela lá é a chave pública. Agora eu preciso da chave privada, por quê? É o Jenkins que vai interagir com o meu GitHub, assim como o nosso terminal interagiu com o GitHub agora é a vez do Jenkins.

[01:55] Então, pra isso, eu preciso copiar a minha chave privada pro Jenkins. Então eu seleciono aqui, lembrando que vocês nunca devem passar essa chave pra ninguém, e a gente adiciona aqui como texto.

[02:14] Agora que a gente adicionou não tem passphrases, vocês lembram que a gente não configurou o passphrase. A gente dá um ok, nossa credencial tá configurada, agora é a hora de criar o nosso primeiro job.

[02:26] Então a gente vem aqui em Jenkins, Novo job, e a gente vai dar um nome pra esse job. O nome que a gente vai usar pra esse job vai ser jenkins-todo-list-principal. Por que que a gente deu esse nome de principal? Porque a maioria das interações com o banco vão ser feitas através desse job, e esse job é do tipo Freestyle. A gente vai entender a diferença de cada um deles, do Freestyle pro Pipeline, por exemplo, mas dessa vez a gente vai escolher o Freestyle.

[03:05] Clicamos aqui no Freestyle, vamos dar um ok. Ele vai criar o primeiro job. O job vai ser executado de acordo com as configurações que a gente definir. Então, primeira coisa que a gente vai fazer, a gente vai avisar que é um projeto git e vai copiar a URL do git. Atentem-se pra colocar o git@github pra pegar o clone da configuração via SSH e não com Https, isso faz diferença na hora de commitar o seu código.

[03:43] Aí a gente colocou aqui o nosso GitHub, se eu dar um tab ele vai falhar, ele vai falar "olha, eu não consigo conectar", por que? Porque esse repositório, do jeito que eu coloquei pra acessar, ele precisa ter as credenciais de SSH. Então eu venho aqui e seleciono github.ssh, automaticamente ele já fez a conexão e já entendeu que tá funcionando. Ou seja, agora nosso job já é capaz de ler e interagir com o GitHub, com nosso repositório.

[04:14] Algumas coisinhas que a gente vai ter que fazer: dentro do ambiente de build a gente vai marcar pra deletar o workspace antes do projeto iniciar, a gente faz isso pra evitar que fique alguma sujeira de código porque, na verdade, o workspace nada mais é do que um diretório dentro do Jenkins, então eu preciso limpar esse cara antes de fazer a configuração.

[04:38] Outra coisa que a gente vai fazer pra ele é o seguinte: ele vai consultar o git periodicamente. Primeiro vamos configurar, depois eu vou explicar pra vocês a diferença. Pra quem já mexeu com o crontab é a mesma sintaxe. Então eu vou colocar aqui primeiro, pra que ele verifique a cada minuto, a cada hora, a cada dia da semana, a cada mês e a cada dia do mês.

[05:03] Vai uma ressalva aqui, não significa que seu eu colocar mais passos pra baixo pra fazer um build, alguma coisa que impacte mais o código, que ele vá fazer isso a todo minuto. Nessa agenda que eu coloquei ele vai verificar a cada minuto se o meu repositório tem alguma alteração. Vamos ver isso aí como vai ficar.

[05:27] A gente vai dar um salvar, a gente vai esperar um pouquinho, ele vai disparar a primeira vez, ele vai consultar o repositório automaticamente, ele faz isso todo minuto. Vamos esperar um pouquinho que aí ele já vai aparecer.

[05:46] Olha lá, apareceu aqui pra gente, primeiro processo pendente, e ele começou a executar. Ele tem a hora, isso aqui é a hora do nosso Vagrant, da nossa máquina virtual, tá diferente da minha, mas não tem problema. Aqui ele executou com sucesso, eu vou clicar aqui nesse cara.

[06:05] Olha a saída do console, esse aqui é o nosso workspace, ele conectou no nosso repositório e viu que tem um código lá. Como não tinha nada, ele já sabe exatamente a partir de onde ele vai trabalhar, ou seja, a partir de agora todas as vezes que nós mudarmos o código dentro do GitHub, ele vai ficar checando e vai startar daqui para frente os builds que a gente pedir.

[06:34] Pessoal, antes da próxima aula tem mais uma informação que eu queria passar pra vocês: depois que a gente fez toda a instalação, tanto do ambiente quanto do Jenkins, a gente tem que fazer dois passos a mais agora, que seria o que? Adicionar tanto o meu usuário, no caso o usuário Vagrant ou o usuário de vocês, na máquina de vocês, e o usuário do Jenkins no grupo do Docker.

[07:01] O Docker foi instalado automaticamente, a gente vai falar deles mais para frente, mas é um passo adicional agora que vai fazer toda a diferença nos nossos jobs. E os comandos são muito simples. O primeiro comando a gente vai adicionar no grupo do Docker o usuário, o user, essa variável $USER é o próprio usuário que eu tô logado, e o usuário do Jenkins. Só pra gente evitar problemas de permissão na hora de rodar os containers.

[07:32] Pra efetivar todas as nossas alterações, a gente vai sair do nosso Vagrant e vai dar um vagrant reload. Assim as seções vão ser recarregadas com as permissões corretas e eu não vou mais precisar usar sudo pra rodar nenhum container.

[07:50] Então, agora, qual que é a próxima aula? Nós vamos entender passo a passo como que o build dessa aplicação é feito, pra que no futuro a gente consiga automatizar utilizando Jenkins e as nossas ferramentas que estão instaladas aqui.

@@11
Criação de jobs no Jenkins

Qual a diferença entre o Git Log de consulta periódica e o log da Saída do console, em um job no Jenkins?

O primeiro mostra o log do servidor do Jenkins, já o segundo mostra o log da execução do build do job
 
Alternativa errada!
Alternativa correta
O primeiro mostra a consulta ao repositório do código-fonte, já o segundo mostra o log da execução do build do job
 
Alternativa correta! O primeiro log mostra as consultas periódicas, configuradas no job, ao repositório, e somente após alguma alteração ele vai prosseguir com o build, onde seus logs podem ser acessados pela Saída do console.
Alternativa correta
O primeiro mostra o log da execução do build do job, já o segundo mostra a consulta ao repositório do código-fonte
 
Alternativa errada!
Alternativa correta
Eles são iguais, a única diferença é o local onde os acessamos

12
Qual repositório?

Aprendemos que o Jenkins verifica periodicamente o repositório para encontrar mudanças no código e iniciar o build do Job.
Qual o repositório o Jenkins está verificando?

O repositório local no computador.
 
Alternativa correta
O repositório remoto no Github.
 
Alternativa correta, pois configuramos o Github como repositório remoto que o Jenkins verifica periodicamente se há alterações no código.
Alternativa correta
O repositório local do container docker
 
Alternativa correta
O repositório remoto no Gitlab.

13
Consolidando o seu conhecimento

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com a próxima aula.
1) Configure o seu ambiente inicial. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Configurando o ambiente e conectando máquina virtual:
    # Máquina virtual com o vagrant
    # Requisitos: http://vagrantup.com e https://www.virtualbox.org/
        # Baixar e descompactar o arquivo 1110-aula-inicial.zip
        # Entendendo o Vagranfile
        # Subindo o ambiente virtualizado
            vagrant plugin install vagrant-disksize
            vagrant up
            vagrant ssh
                ps -ef | grep -i mysql # Verificando se o MySQL esta rodando
                mysql -u devops -p # Senha mestre; show databases
                mysql -u devops_dev -p # Senha mestre; show databases
                # Instalando o Jenkins
                    cd /vagrant/scripts
                # Visualizar o conteudo do arquivo de instalacao do jenkins
                    sudo ./jenkins.sh

                # Acessar:  192.168.33.10:8080
                    sudo cat /var/lib/jenkins/secrets/initialAdminPassword

                # Credenciais
                    Nome de usuário: alura
                    Senha: mestre123
                    Nome completo: Jenkins Alura
                    Email: aluno@alura.com.br

                # Reload nas permissoes do docker
                    sudo usermod -aG docker $USER
                    sudo usermod -aG docker jenkins
                    exit
            vagrant reloadCOPIAR CÓDIGO
2) Versione o seu código, que você baixou no início da aula. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Passos para a configuracao do git e versionamento do codigo
# Criar uma conta em github.com
    ssh-keygen -t rsa -b 4096 -C "<seu-usuario>@gmail.com"
    cat ~/.ssh/id_rsa.pub
# Configurar a chave no github
    git config --global user.name "<seu-usuario>"
    git config --global user.email <seu-usuario>@<seu-providor>
    ssh -T git@github.com
# Fazer download do código nos anexos e copiar para o diretório compartilhado app
    cd /vagrant/jenkins-todo-list
    git init
    git add .
    git commit -m "Meu primeiro commit"
    git log
# Criar um repositório no github: jenkins-todo-list
    git remote add origin git@github.com:<seu-usuario>/jenkins-todo-list.git
    git push -u origin masterCOPIAR CÓDIGO
3) Crie o seu primeiro job, que vai monitorar o seu repositório. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Configurando a chave privada criada no ambiente da VM no Jenkins
    cat ~/.ssh/id_rsa
    Credentials -> Jenkins -> Global Credentials -> Add Crendentials -> SSH Username with private key [ github-ssh ]
# Criando o primeiro  job que vai monitorar o repositorio
    Novo job -> jenkins-todo-list-principal -> Freestyle project:
    Esse job vai fazer o build do projeto e registrar a imagem no repositório.
    # Gerenciamento de código fonte:
        Git: git@github.com:rafaelvzago/treinamento-devops-alura.git [SSH]
        Credentials: git (github-ssh)
        Branch: master
    # Trigger de builds
        Pool SCM: * * * * *
    # Ambiente de build
        Delete workspace before build starts
    # Salvar
    # Validar o log em: Git Log de consulta periódica

Continue com os seus estudos, e se houver dúvidas, não hesite em recorrer ao nosso fórum!

@@14
O que aprendemos?

Nesta aula, aprendemos:
O que é integração contínua
A fazer a configuração do ambiente
VirtualBox
Vagrant
Cmder (para quem utiliza Windows)
Executar os scripts do Jenkins
Configuração do GitHub
Como fazer um versionamento básico do código do projeto
Como configurar um job no Jenkins para acessar o GitHub

#### 29/08/2023

@02-Build com Docker e Jenkins

@@01
Introdução ao pipeline

[00:00] Tudo bem pessoal? A gente vai começar agora a segunda aula.
[00:04] E essa série de vídeos da segunda aula vai fazer a cobertura da seguinte etapa no pipeline: se nós olharmos aqui, essa parte de versionamento já está pronta. Então a aula 2 cobre a parte onde nós vamos fazer o build da nossa imagem automaticamente.

[00:24] Primeiro eu vou mostrar para vocês manualmente como isso é feito e depois a gente vai fazer isso utilizando o Jenkins em conjunto com o Docker, criando a nossa imagem e fazendo o registro da imagem no Dockerhub.

[00:41] Então, vamos pra próxima aula.

@@02
Entendendo o build manual

O script que o instrutor segue durante a aula é o seguinte:
# Passos para configurar a app e subir manualmente
    # Criando o arquivo .env (temporário)
        cd /vagrant/jenkins-todo-list/to_do/
        vi .env
            [config]
            # Secret configuration
            SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'

            # conf
            DEBUG=True

            # Database
            DB_NAME = "todo_dev"
            DB_USER = "devops_dev"
            DB_PASSWORD = "mestre"
            DB_HOST = "localhost"
            DB_PORT = "3306"

    # Instalando o venv
        sudo pip3 install virtualenv nose coverage nosexcover pylint
    # Criando e ativando o venv (dev)
        cd ../    
        virtualenv  --always-copy  venv-django-todolist
        source venv-django-todolist/bin/activate
        pip install -r requirements.txt
    # Fazendo a migracao inicial dos dados
        python manage.py makemigrations
        python manage.py migrate
    # Criando o superuser para acessar a app
        python manage.py createsuperuser
    # Repetir o processo de migracaoção para o ambiente de produção:
        vi to_do/.env
            [config]
            # Secret configuration
            SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'

            # conf
            DEBUG=True

            # Database
            DB_NAME = "todo"
            DB_USER = "devops"
            DB_PASSWORD = "mestre"
            DB_HOST = "localhost"
            DB_PORT = "3306"

    # Fazendo a migracao inicial dos dados
        python manage.py makemigrations
        python manage.py migrate
    # Criando o superuser para acessar a app
        python manage.py createsuperuser

    # Verificar o ip do servidor
        ip addr
    # Rodando a app
        python manage.py runserver 0:8000
        http://192.168.33.10:8000COPIAR CÓDIGO
[00:00] Bom pessoal, agora tá na hora da gente fazer o build manual da nossa aplicação. Porque que a gente vai fazer isso? Pra que quando a gente automatize toda construção, a gente consiga enxergar a diferença de velocidade e de agilidade de um processo pro outro.

[00:15] Pra fazer o build, a aplicação é muito simples. Primeiro, dentro do meu diretório da aplicação, eu preciso criar um arquivo ".env". Que que esse arquivo ".env" contém? Vou pegar aqui um modelo dele. Basicamente, ele tem algumas configurações pra aplicação rodar, como por exemplo, o secret key que é utilizado pra fazer os hash de senha, por exemplo, nome de usuários de banco de dados, nome do banco de dados, a porta e onde o banco tá localizado.

[00:51] Atentem-se pro seguinte: eu estou conectando do banco de dados de dev. E porque que eu tô criando esse arquivo? Vamos primeiro salvar o arquivo aqui. A gente usou o VI pra quem não tá muito acostumado. Entrou no arquivo, aperto o I, colo o conteúdo, Esc, :x. A gente salvou o arquivo aqui, se eu der um "ls -la" tá aqui o meu arquivo env.

[01:14] Porque que a gente não coloca o arquivo env dentro da aplicação versionada? Porque isso vai pro GitHub, ou até, às vezes, pra um repositório privado. O problema é que o arquivo env contém as informações de conexão de banco, por exemplo, isso não pode ficar público.

[01:30] Com o arquivo copiado, que que a gente faz agora? A gente vai instalar algumas dependências do Python com o PIP, que é o gerenciador de pacotes do Python. Mandei instalar, ele tá instalando, é rápida a instalação, ele busca nos repositórios e faz a instalação pra gente.

[01:55] Legal, a instalação dos pacotes feita, o que eu vou fazer? Eu vou criar um Virtualenv pro Python, pra isolar a minha aplicação. Vocês tão percebendo o trabalho que dá fazer manual o build disso daqui. Então eu vou criar um Virtualenv, ele vai isolar os pacotes da minha aplicação. Legal, ele instalou os pacotes da minha aplicação e agora eu vou ativar o meu Virtualenv.

[02:25] Legal, Virtualenv ativado, eu consigo ver pelo começo do bash agora. Então o que eu tenho que pedir pra ele fazer agora? Instalar todos os requerimentos da minha aplicação. Se eu der um cat nesse arquivo vocês vão ver que tem várias dependências. Então, pra isso, eu vou executar o pip install - r requirements.

[02:44] Isso é legal ver, enquanto tá rodando, que tomar conta de uma aplicação, ou deployar uma aplicação, você não precisa, exatamente, configurar todo o ambiente dela manualmente porque a gente tá tendo muito trabalho pra fazer isso. O legal é a gente automatizar. Então as próximas aulas vão mostrar como que isso é aqui é feito de uma maneira invisível pra quem tá aplicando a Integração Contínua.

[03:09] Normalmente quando a gente roda o pip install eventualmente ele tem algum problema de cache, é só rodar de novo, só pra validar a instalação, mas foi tudo instalado. Isso não é um erro de aplicação e sim um erro do próprio PIP mesmo, que quando a gente automatiza não aparece.

[03:28] Legal, então a gente tá com as dependências instaladas, o que que a gente vai fazer agora? Tem um passo que a gente vai executar agora, uma sequência de passos na verdade, que é específico do Django. Como o nosso curso é de Jenkins a gente vai criar esse ambiente na mão na primeira vez, é uma vez só que a gente vai criar, que são as migrações de banco, que é onde ele vai criar as tabelas do banco de dados.

[03:55] Isso não vai entrar na nossa Integração Contínua porque é específico do Python. Se você trabalha com Java você vai ter que lidar com isso na hora de criar a sua aplicação. Você pode automatizar esses passos, isso é possível, mas como o foco do curso não é Django a gente só vai criar a primeira vez. É uma vez só que a gente executa e não executa mais.

[04:18] Então a gente vai rodar as migrações do banco, a gente vai criar as migrações do banco, baseado nos modelos e a gente vai migrar o banco. Então ele tá fazendo toda a configuração de tabelas e permissões, e agora o que eu tenho que fazer? Eu tenho que criar um super usuário, isso é específico do Django. É o usuário admin, o root do Django pra acessar as aplicações.

[04:51] Então eu vou criar um usuário aqui com o nome alura, o endereço de email vai ser aluno@alura.com.br, a senha vai ser mestre123, vamos manter aquele padrãozinho mestre123 pra sempre lembrar. E o super usuário foi criado.

[05:18] Bom, então agora tá na hora da gente ver a aplicação rodar, até agora a gente não viu como ela funciona, qual que é a carinha dela. A gente vai rodar um comando aqui que só é utilizado no ambiente de desenvolvimento, no pipeline nosso a gente vai usar outro que é para produção. Startei a aplicação.

[05:39] Então agora que a gente já colocou a aplicação pra rodar, a gente vai usar o mesmo IP que a gente utilizou pra abrir o Jenkins, pra abrir a aplicação, porque eles estão rodando no mesmo servidor. Só que o que muda agora é a porta que, ao invés de ser a porta 8080, que é onde o Jenkins tá rodando, a gente vai usar a porta 8000. Aí a aplicação apareceu. O usuário e a senha a gente acabou de criar, alura, mestre123. Abrimos a aplicação aqui.

[06:11] Pessoal, tem uma outra coisa que a gente vai fazer, primeiro a gente vai parar o nosso servidor aqui, que é o seguinte: tudo tá apontado pro banco de dev, a gente vai editar o nosso arquivo aqui e vai enxergar. Dentro do nosso arquivo de ambiente tá tudo apontado pra dev, só que a gente vai ter um ambiente de produção que é um outro schema e um outro usuário.

[06:36] Então vamos rodar rapidinho as migrations pro usuário de produção. Então a gente apaga a parte dev, porque a gente já tem essa tabela criada, a senha é a mesma, a gente só vai rodar os migrations de novo, makemigrations, na verdade nem precisava porque já estavam criadas as migrações.

[07:04] Agora o migrate vai fazer o que? Ele vai conectar no banco de produção, a gente trocou as tabelinhas lá e ele criou de novo. Isso é bem específico, só pra que na próxima etapa do build a gente não tenha problema. E a gente vai criar o super usuário da mesma maneira. Vamos criar os mesmos dados pra não ficar confuso, alura, aluno@alura.com.br, mestre123.

[07:37] Legal, criou o usuário, a gente sobe a aplicação pra validar que tá tudo funcionando, atualiza aqui a aplicação. Logou, conectamos. Pra utilizar a aplicação é muito simples, você coloca o título da sua task, o texto, pode marcar como completa ou não, e dar um salvar, sua task tá aqui.

[08:04] Bom pessoal, agora a gente acabou de ver o build manual, como ele é feito. Na próxima aula a gente vai acertar o último detalhe antes de colocar todo o nosso processo automatizado dentro do Jenkins. A gente se vê lá.

@@03
Comandos do build manual

Sejam os seguintes comandos no build manual:
python createsuperuser
python migrate COPIAR CÓDIGO
Qual a função deles, respectivamente?

Criar o virtualenv para instalar as dependências
 
Alternativa correta
Migrar as alterações dos seus modelos; criar o principal usuário para acessar a aplicação
 
Alternativa correta
Criar o principal usuário para acessar a aplicação; migrar as alterações dos seus modelos
 
Alternativa correta!

@@04
Configurando o daemon do Docker

O script que o instrutor segue durante a aula é o seguinte:
# Expor o deamon do docker
    sudo mkdir -p /etc/systemd/system/docker.service.d/
    sudo vi /etc/systemd/system/docker.service.d/override.conf
        [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2376
    sudo systemctl daemon-reload
    sudo systemctl restart docker.serviceCOPIAR CÓDIGO
[00:00] Bom pessoal, agora é o seguinte, a gente vai começar a automatizar todo esse processo de build, pra isso a gente vai colocar a nossa aplicação pra rodar dentro de containers.

[00:11] Lembrando que é extremamente importante vocês assistirem o curso de Docker que tem na Alura porque muitos dos conceitos que a gente vai usar aqui são explicados lá. Um conceito, especificamente, eu vou explicar pra vocês agora.

[00:26] O Jenkins tá rodando na mesma máquina que o meu Docker tá rodando, o Docker foi instalado quando vocês subiram a máquina virtual. Vamos dar uma checada aqui primeiro, sudo docker ps. Ele exibiu aqui pra gente que não tem container nenhum mas ele tá rodando.

[00:45] O Docker, apesar de estar rodando na mesma máquina, eventualmente eles poderiam estar rodando em máquinas distintas, pra isso a gente habilita o daemon do Docker pra que ele seja controlado remotamente. Isso não vem, por padrão, habilitado.

[01:01] Então como é que a gente faz pra habilitar isso daí? É muito simples, a gente cria um diretório, esse diretório aqui /etc/systemd/system/docker.service.d/, e dentro desse cara a gente cria esse arquivo aqui, que é o override.conf, nesse arquivo eu vou falar pra ele o seguinte: você vai startar expondo o seu daemon na porta 2376.

[01:43] O que que isso vai possibilitar? Que o meu Jenkins, mesmo não tendo o Docker instalado, eventualmente consiga controlar um Docker remoto. Isso serve pra várias aplicações, você não precisa controlar um parque de máquinas de 'n' Jenkins rodando slaves sendo que você consegue controlar tudo através do Master.

[02:07] Então a gente vai salvar esse arquivo aqui, e agora o que que a gente vai fazer? Depois de ter configurado esse arquivo, nós vamos dar um reload no daemon dele e vamos restartar o serviço do Docker. Dessa maneira a gente habilitou o Docker a ser acessado de outras máquinas, nesse caso é a mesma, mas vocês entenderam.

[02:34] Agora, porque que a gente fez tudo isso? Porque quando a gente criar agora os nossos jobs a gente tem um plugin do Docker dentro do Jenkins que vai poder acessar essa máquina e poder executar os comandos pra pegar toda a nossa aplicação e colocar dentro de container.

[02:52] Essa aula a gente entendeu como expor o daemon do Docker, na próxima aula a gente vai alterar aquele nosso job, que a gente criou no começo, pra que ele faça o build dessa imagem automaticamente pra gente.

[03:06] Eu espero vocês lá.

@@05
Docker daemon

No servidor onde o Docker está instalado, qual o arquivo que criamos para possibilitar o controle remoto do Docker?

/var/systemd/system/docker.service.d/override.conf
 
Alternativa correta
/etc/systemd/system/docker.service.d/override.conf
 
Alternativa correta!
Alternativa correta
/etc/systemd/system/docker.service.d/docker-override.conf
 
Alternativa correta
/etc/init.d/system/docker.service.d/override.conf

@@06
Build da imagem

O script que o instrutor segue durante a aula é o seguinte:
# Sugested Plugins
# Instalando os plugins
    Gerenciar Jenkins -> Gerenciar Plugins -> Disponíveis
        # docker
    Install without restart -> Depois reiniciar o jenkins
Gerenciar Jenkins -> Configurar o sistema -> Nuvem
    # Name: docker
    # URI: tcp://127.0.0.1:2376
    # Enabled
# This project is parameterized: 
    DOCKER_HOST
    tcp://127.0.0.1:2376
# Voltar no job criado na aula anterior
    # Manter a mesma configuracao do GIT para desenvolvimento
    # Build step 1: Executar Shell
# Validando a sintaxe do Dockerfile
docker run --rm -i hadolint/hadolint < Dockerfile
    # Build step 2: Build/Publish Docker Image
        Directory for Dockerfile: ./
        Cloud: docker
        Image: rafaelvzago/django_todolist_image_buildCOPIAR CÓDIGO
[00:00] E aí pessoal, beleza? Agora chegou a hora tão esperada de configurar o nosso job pra fazer o build automático da aplicação, dentro de um container. Apesar da explicação ser grande, a aplicação é muito simples.

[00:16] Então vamos lá. Primeira coisa, a gente vai logar no nosso Jenkins e a gente vai instalar alguns plugins, na verdade a gente vai instalar um plugin agora que é necessário pra que isso funcione.

[00:32] Então a gente vem em gerenciar Jenkins, gerenciar plugins, aqui a gente vem em disponíveis, que é o marketplace de plugins que o Jenkins tem, eu escrevo docker. Tá vendo aqui, esse aqui é o plugin do Docker. A gente vai clicar nesse carinha aqui pra instalar, vai mandar instalar e depois reiniciar. Ele vai instalar algumas dependências e, assim que terminar a instalação das dependências, a gente volta.

[01:06] Legal, terminou a instalação, agora a gente vai mandar reiniciar o Jenkins pra que ele carregue esse plugin novo. Enquanto ele reinicia, uma explicação básica sobre esse plugin: nós vamos habilitar o daemon, que nós configuramos no servidor, dentro dessa configuração de plugin, pra que o meu Jenkins seja capaz de executar comandos Docker remotamente, nesse caso vai ser localmente mas poderia ser em qualquer servidor. Então assim que terminar de reiniciar a gente volta.

[01:41] Então pessoal, enquanto instala e reinicia, só pra explicar o que a gente acabou de fazer: a gente instalou um client, um plugin do Docker, pra que ele possa acessar um servidor Docker e executar os comandos. Nesse caso vai estar na mesma máquina, mas a gente tá habilitando isso pra instalações e intervenções futuras. Então a gente vai esperar reiniciar, quando reiniciar a gente volta.

[02:04] Pronto, reiniciou, então agora a gente loga novamente, vou marcar aqui pra ele me manter logado e eu tenho que fazer algumas configurações agora. Primeira coisa que eu vou fazer: eu vou vir aqui no Jenkins, Gerenciar Jenkins, Configurar o sistema, é a configuração básica do Jenkins, e aí lá no final da página eu tenho Nuvem, se você tiver usando em inglês vai estar escrito Cloud, e eu vou adicionar uma nova Nuvem.

[02:44] O que que seria essa nova nuvem? São daemons que serão chamados. Basicamente assim, a gente tá instalando um plugin que vai chamar um daemon, então nessa parte ele tá aparecendo Docker aqui por que o plugin tá habilitado. É como o Jenkins interage com a Nuvem, nesse caso vai ser localmente mas poderia ser em qualquer lugar.

[03:05] Então quando eu cliquei aqui ele falou o seguinte "Olha, qual que é o nome que você vai dar?", a gente vai dar de docker mesmo aí ele pede os detalhes, a gente vai fazer uma configuração muito simples que é, onde ele pede qual que é a URI de conexão, a gente vai colocar tcp, o IP do servidor que tem o daemon exposto e a porta. Nesse caso eu coloquei 127.0.0.1 porque tá rodando nessa máquina. E aí eu vou a testar conexão.

[03:36] Olha lá, ele encontrou a versão 1809 do meu Docker rodando e o daemon exposto ou seja, aquela configuração nossa funcionou e agora o nosso Jenkins tá habilitado pra executar comandos em Docker nesse servidor.

[03:51] A gente vai salvar e chegou a parte de terminar o nosso build, fazer que o nosso job construa a nossa imagem automaticamente. Como é que a gente vai fazer isso? A gente vai clicar aqui dentro do job, que nós já tínhamos criado, configurar, e nós vamos adicionar dois passos de build.

[04:16] Como é que o Jenkins funciona? Aqui embaixo eu tenho os meus build steps, que são os passos do meu build, eu posso ter quantos passos eu quiser. Primeiro passo que eu vou configurar é um passo muito simples que vai fazer uma checagem muito rápida de como é que o meu Dockerfile está, se ele está ou não está de acordo com as últimas convenções, ele vai fazer um linter do meu Docker.

[04:45] Pra isso, que que eu faço? Eu vou adicionar um Build step pra executar um shell e, nessa execução do shell, que que eu vou fazer? Eu vou colar só isso aqui. Vou tirar o espaço adicional aqui. O que ele tá fazendo? Ele tá usando uma imagem chamada hadolint que ele vai fazer o linter do meu Dockerfile.

[05:11] De novo, se você tá em dúvida do que o Dockerfile faz e exatamente como a gente constrói, tem o curso de Docker da Alura pra vocês assistirem.

[05:21] Agora, se esse step faz passar, ou seja se não falhar, eu vou executar um segundo Buildstep que é o build da minha imagem do Docker, que agora sim eu vou automatizar tudo.

[05:36] Primeiro ele me pergunta onde que o meu Dockerfile está. Só para relembrar, dentro do root aqui da minha aplicação eu tenho meu Dockerfile que, se eu olhar aqui o conteúdo dele rapidinho, basicamente ele tá copiando os meus arquivos pra dentro de um diretório, copiando o arquivo de requerimentos, rodando o pip install, que nós fizemos tudo isso na mão, expondo a porta 8000 e executando a aplicação na porta 8000.

[06:12] Lembrando que quando eu construo a imagem, não necessariamente eu estou rodando meu container. Se eu não pedir pra rodar eu só vou até a imagem lá.

[06:20] Então vamos lá. A primeira coisa, vamos ver quantas imagens eu tenho aqui. sudo docker images, eu tenho só o hadolint, então tá zerada a minha instalação. Então o meu Dockerfile tá dentro do próprio diretório, porque quando ele dá o clone pra este diretório ele vai jogar dentro todos os arquivos. E aí ele vai perguntar qual que é o Cloud que vai construir, vai ser o Cloud do Docker que a gente já montou.

[06:49] E agora eu preciso dar um nome pra essa imagem, a gente vai usar esse nome aqui: django_todolist_image_build, é um nome de uma imagem que a nossa aplicação vai ter. E agora eu vou dar um salvar.

[07:12] Tem um detalhe que a gente precisa voltar a fazer que é: dentro do meu Jenkins eu preciso habilitar o meu plugin do Docker porque eu só configurei o daemon. Então como é que eu faço isso? Eu vou lá embaixo em Cloud, Cloud details e marco o enable. Se eu não fizer isso, esse Cloud provider, que ele vai usar, não vai funcionar, eu preciso habilitar, ele vai exibir naquela lista mas ele não vai estar habilitado. Feito isso é simples agora, eu venho aqui no meu job e mando construir.

[07:52] Então, resumindo, ele vai clonar o meu repositório, vai ler o meu Dockerfile, vai construir a minha imagem e vai registrar ela localmente. Então, assim que terminar a gente volta.

[08:07] Bom, o build acabou, foi um build com sucesso. Se a gente chegar na máquina agora e digitar sudo docker images, nossa imagem foi criada. Então quais são os próximos passos agora? É aprimorar esse nosso build, fazer alguns tratamentos de erro pra ver se não tem nada que vai quebrar o nosso código, e colocar nossa aplicação pra rodar da mesma maneira, automatizada sem que a gente tenha que ficar configurando os jobs na mão.

[08:43] Eu vejo vocês na próxima.

@@07
Construindo imagens para o Docker

Na construção de imagens para o Docker, recomenda-se o uso de um linter. Por quê?

Para garantir a utilização das melhores práticas na criação de um Dockerfile
 
Alternativa correta!
Alternativa correta
Com isso, garantimos que a imagem será registrada com sucesso no Docker Hub
 
Alternativa correta
Pois com o linter, garantimos que todos os comandos serão executados corretamente

08
Consolidando o seu conhecimento

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com a próxima aula.
1) Configure a sua aplicação e suba-a manualmente. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Passos para configurar a app e subir manualmente
    # Criando o arquivo .env (temporário)
        cd /vagrant/jenkins-todo-list/to_do/
        vi .env
            [config]
            # Secret configuration
            SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'

            # conf
            DEBUG=True

            # Database
            DB_NAME = "todo_dev"
            DB_USER = "devops_dev"
            DB_PASSWORD = "mestre"
            DB_HOST = "localhost"
            DB_PORT = "3306"

    # Instalando o venv
        sudo pip3 install virtualenv nose coverage nosexcover pylint
    # Criando e ativando o venv (dev)
        cd ../    
        virtualenv  --always-copy  venv-django-todolist
        source venv-django-todolist/bin/activate
        pip install -r requirements.txt
    # Fazendo a migracao inicial dos dados
        python manage.py makemigrations
        python manage.py migrate
    # Criando o superuser para acessar a app
        python manage.py createsuperuser
    # Repetir o processo de migracaoção para o ambiente de produção:
        vi to_do/.env
            [config]
            # Secret configuration
            SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'

            # conf
            DEBUG=True

            # Database
            DB_NAME = "todo"
            DB_USER = "devops"
            DB_PASSWORD = "mestre"
            DB_HOST = "localhost"
            DB_PORT = "3306"

    # Fazendo a migracao inicial dos dados
        python manage.py makemigrations
        python manage.py migrate
    # Criando o superuser para acessar a app
        python manage.py createsuperuser

    # Verificar o ip do servidor
        ip addr
    # Rodando a app
        python manage.py runserver 0:8000
        http://192.168.33.10:8000COPIAR CÓDIGO
2) Configure o daemon do Docker. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Expor o deamon do docker
    sudo mkdir -p /etc/systemd/system/docker.service.d/
    sudo vi /etc/systemd/system/docker.service.d/override.conf
        [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2376
    sudo systemctl daemon-reload
    sudo systemctl restart docker.serviceCOPIAR CÓDIGO
3) Faça o build da sua imagem. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Sugested Plugins
# Instalando os plugins
    Gerenciar Jenkins -> Gerenciar Plugins -> Disponíveis
        # docker
    Install without restart -> Depois reiniciar o jenkins
Gerenciar Jenkins -> Configurar o sistema -> Nuvem
    # Name: docker
    # URI: tcp://127.0.0.1:2376
    # Enabled
# This project is parameterized: 
    DOCKER_HOST
    tcp://127.0.0.1:2376
# Voltar no job criado na aula anterior
    # Manter a mesma configuracao do GIT para desenvolvimento
    # Build step 1: Executar Shell
# Validando a sintaxe do Dockerfile
docker run --rm -i hadolint/hadolint < Dockerfile
    # Build step 2: Build/Publish Docker Image
        Directory for Dockerfile: ./
        Cloud: docker
        Image: rafaelvzago/django_todolist_image_build

Continue com os seus estudos, e se houver dúvidas, não hesite em recorrer ao nosso fórum!

@@09
O que aprendemos?

Nesta aula, aprendemos:
Como fazer o build e deploy manual da aplicação
Como configurar o daemon do Docker
Como configurar o Jenkins e instalar os módulos necessários
Como configurar um job e gerar uma imagem