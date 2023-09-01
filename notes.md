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

####  31/08/2023 

@03-Ambiente de Produção e Desenvolvimento

@@01
Preparação dos ambientes

O script que o instrutor segue durante a aula é o seguinte:
# Instalar o plugin Config File Provider

# Configurar o Managed Files para Dev
    # Name : .env-dev
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

#Configurar o Managed Files para Prod
    # Name: .env.-prod
        [config]
        # Secret configuration
        SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'
        # conf
        DEBUG=False
        # Database
        DB_NAME = "todo"
        DB_USER = "devops"
        DB_PASSWORD = "mestre"
        DB_HOST = "localhost"
        DB_PORT = "3306"

# No job: jenkins-todo-list-principal importar o env de dev para teste:

    Adicionar passo no build: Provide configuration Files
    File: .env-dev
    Target: ./to_do/.env

    Adicionar passo no build: Executar Shell

# Criando o Script para Subir o container com o arquivo de env e testar a app:
    #!/bin/sh

    # Subindo o container de teste
    docker run -d -p 82:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/jenkins-todo-list-principal/to_do/.env:/usr/src/app/to_do/.env --name=todo-list-teste django_todolist_image_build

    # Testando a imagem
    docker exec -i todo-list-teste python manage.py test --keep
    exit_code=$?

    # Derrubando o container velho
    docker rm -f todo-list-teste

    if [ $exit_code -ne 0 ]; then
        exit 1
    fiCOPIAR CÓDIGO
[00:00] Oi pessoal, tudo bem? Nessa aula a gente vai entender como separar os ambientes da nossa aplicação. Nesse caso nós vamos utilizar dois ambientes que é o ambiente de desenvolvimento e o ambiente de produção.

[00:13] O que diferencia um ambiente do outro? É somente o arquivo .env. Se vocês se lembram, na aula que nós criamos o build manual, esse arquivo .env contém o endereço do banco, usuário, senha, algumas chaves de configuração de criptografia. Então a gente tem que separar esses arquivos porque ele não está versionado no nosso GitHub. Isso é muito importante porque é o que garante a segurança da nossa aplicação.

[00:41] E ele vai ser utilizado em tempo de execução. Como é que a gente faz isso dentro do Jenkins? Nós vamos precisar de um plugin chamado Config File Provider. Então a gente vai fazer a instalação dele agora. Dentro de gerenciar plugins, disponíveis, a gente vai digitar config file provider e vamos clicar pra instalar esse plugin. Ele vai instalar, a instalação dele é muito rápida.

[01:11] E agora a gente vai fazer a configuração desse plugin. Como que nós configuramos esse plugin? Dentro de "Gerenciar o Jenkins" nós vamos procurar uma entrada chamada "Managed files" dentro dessa configuração nós vamos criar dois arquivos.

[01:27] O primeiro deles vai ser um arquivo custom, se vocês olharem tem outros padrões aqui pré-determinados, aí vocês podem utilizar de acordo com a aplicação de vocês, nesse caso a gente vai criar um arquivo customizado. Ele vai me dar um ID, esse ID vai ser importante lá na frente quando a gente for configurar os nossos jobs. Mas, por enquanto, a gente vai clicar em submit, ele vai pedir um nome, vai ser .env-dev, e o conteúdo.

[01:58] No caso de desenvolvimento o conteúdo é esse aqui que a gente tá apontando pro banco de desenvolvimento. Vamos dar um submit.

[02:09] E agora a gente vai criar o arquivo de prod que é um arquivo customizado, um outro ID, isso aí é automático. O nome dele vai ser .env-prod e o conteúdo vai ser quase o mesmo, a diferença é que agora nós vamos apontar pro banco de produção.

[02:32] Então configurado, clicamos em submit. Vamos voltar lá no nosso job pra puxar esses arquivos quando necessário. Então, dentro desse job principal, qual vai ser a próxima etapa nossa dentro desse pipeline? Vai ser fazer o teste da aplicação. Nesse caso, pra testar, nós vamos utilizar o ambiente de Dev, o arquivo .env de dev. Nunca se deve apontar testes pra produção.

[03:05] Pra isso, dentro do job, a gente vai em Ambiente de build e vai ter uma opção chamada Provide Configuration files, clicando aqui ele vai mostrar um drop down com os dois arquivos criados, nesse caso a gente vai usar o arquivo de dev.

[03:24] Qual que é o target? Aonde ele vai salvar esse arquivo? Lembra que eu comentei que cada job tem um diretório dentro do Jenkins? Nós vamos salvar na raiz desse job com o nome .env, que é aquele arquivo que nós criamos lá quando fizemos o build manual. Só que nesse caso eu não preciso me preocupar em colocar ele dentro de "todo" na nossa aplicação, por quê? Nós, quando executarmos o container, ele vai receber como parâmetro de volume esse arquivo e aí ele vai subir a aplicação apontando.

[04:02] Então agora, que nós já temos o nosso arquivo de ambiente, tá na hora da gente rodar o teste da nossa aplicação. Pra isso, depois do linter ter rodado, ou seja, esse primeiro teste ter passado que o nosso Dockerfile é saudável e o build ter acontecido com esse nome de imagem, nós vamos adicionar um próximo step que vai ser um shell script, e esse shell vai ter esse conteúdo aqui. Vamos copiar lá e eu vou explicar pra vocês o que ele faz.

[04:43] Então basicamente, vamos expandir um pouquinho aqui pra ficar mais fácil de enxergar, eu vou rodar um container na porta 82. Porque que eu tô rodando na porta 82? Por que produção vai rodar na porta 80, que é a porta padrão; desenvolvimento vai rodar na porta 81, a gente vai chegar lá ainda, vai ser um job que vai criar só nossa aplicação rodando pra devs acessarem; e, pra teste, eu vou subir um container muito rápido na porta 82, passando dois parâmetros de volume.

[05:18] O primeiro é o volume do sock do MySQL, como o contêiner é imutável, ele não tem a capacidade de acessar o sock dele mesmo e, como eu tô utilizando um banco externo, eu tô mapeando pra minha aplicação que o sock, dentro de /var/run/mysqld/mysqld.sock, vai estar localmente localizado no mesmo lugar e não dentro do container. Sem essa opção nossa aplicação não vai funcionar.

[05:49] E tô passando um outro parâmetro pra ele, de volume, que é dentro de /var/lib/jenkins/workspace. Olha o nosso diretório aqui do job, lembra que eu falei que o Jenkins tem um diretório pra cada job? E eu tenho um arquivo de ambiente que a gente vai colocar aqui só o .env porque nós salvamos ele localmente e na raiz do nosso projeto.

[06:14] Agora passamos mais um último parâmetro que é o nome do nosso container, que seria todo-list-teste e a imagem. Reparem que eu tenho, como parâmetro último aqui, a imagem que eu vou fazer o build, o problema é que ainda nós não temos essa configuração definida. A gente vai entender daqui a pouquinho como funciona. De momento, que que nós vamos fazer? Copiar o nome da imagem e colocar a imagem dentro desse valor.

[07:03] Então agora temos todo o comando de run definido, ele vai subir esse container. Com esse container em execução, o que que a gente vai fazer agora? Nós vamos rodar o teste unitário da nossa aplicação, nesse caso é um teste bem simples que é para demonstrar pra vocês como funciona esse step. No caso de vocês, nas aplicações do dia-a-dia, os testes podem mudar, mas nesse caso é assim que funciona.

[07:33] Como é que ele executa o teste? Ele vai dar um exec nesse container todo-list-teste, que nós acabamos de subir, e passar um comando "python manage.py test". Eu tenho, dentro do meu código fonte, um arquivo de teste que vai testar se a minha aplicação tá saudável.

[07:54] Então aqui olha, se nós pegarmos o exit_code e ele não for igual a zero, qualquer coisa diferente de zero, então eu vou falhar esse step. Esse step falhando, o meu pipeline vai falhar. Feito isso, nós vamos salvar e agora nós vamos construir o nosso projeto.

[08:20] Vamos dar uma olhadinha aqui como é que tá os logs de execução. Olha, então enquanto ele tá construindo, só pra vocês entenderem o que tá acontecendo, ele puxou os arquivos lá do meu repositório e aqui ele tá copiando o arquivo de env, o env-dev, pra .env, por isso que a gente mapeou daquela maneira. Ele tá fazendo o build da minha imagem e, agora, assim que ele terminar o build a gente volta.

[08:53] Bom, terminou e, se a gente olhar os logs aqui, ele executou um teste e esse teste foi com sucesso. Então agora o nosso job já tá fazendo os testes puxando a configuração de ambiente.

[09:10] Na próxima aula a gente vai aprender como passar parâmetros para outros jobs e com isso a gente vai conseguir construir tanto nosso ambiente de desenvolvimento quanto nosso ambiente de produção. Beleza, a gente se vê lá.

@@02
Ambientes de deploy

Considerando a abordagem utilizada para definir a configuração da aplicação nos ambientes de desenvolvimento e produção, escolha a alternativa que justifica tal escolha:

Nada justifica tal escolha
 
Alternativa correta
Apesar da escolha de separar os ambientes por arquivo de configuração, recomenda-se rodar os testes sistêmicos no banco de dados de produção
 
Alternativa correta
A abordagem escolhida permite que a mesma imagem construída no processo seja utilizada para testes, validações e também para rodar em produção, visto que o arquivo .env de cada ambiente é passado na execução do container
 
Alternativa correta!
Alternativa correta
Seria mais interessante construir a imagem com o arquivo de configuração de ambiente

@@03
Push da imagem para Docker Hub

O script que o instrutor segue durante a aula é o seguinte:
# Instalar o plugin: Parameterized Trigger 

# Modificar o Job para startar com 2 parametros:
    # Geral:
    Este build é parametrizado com 2 parametros de string
        Nome: image
        Valor padrão: <seu-usuario-no-dockerhub>/django_todolist_image_build

        Nome: DOCKER_HOST
        Valor padrão: tcp://127.0.0.1:2376

# No build step: Build / Publish Docker Image
    # Mudar o nome da imagem para: <seu-usuario-no-dockerhub>/django_todolist_image_build
    # Marcar: Push Image e configurar **suas credenciais** no dockerhub

# Mudar no job de teste a imagem para: ${image}
    docker run -d -p 82:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/jenkins-todo-list-principal/to_do/.env:/usr/src/app/to_do/.env --name=todo-list-teste ${image}COPIAR CÓDIGO
[00:00] Olá pessoal, tudo bem? Nessa aula a gente vai aprender a trabalhar com parâmetros, como que nós passamos parâmetros de um job, pra outro job. Pra isso nós vamos ter que instalar um novo plugin.

[00:14] Então a gente vem em "Gerenciar o Jenkins", "Gerenciar plugins", "Disponíveis", e nós vamos procurar esse plugin aqui. Esse plugin vai ser o plugin que a gente vai utilizar pra passar parâmetros. Então nós vamos clicar aqui em instalar e assim que a instalação terminar a gente volta.

[00:37] Legal, a instalação acabou. Então vamos voltar lá no nosso job, configurar isso aqui. Então a gente vem em Jenkins, no nosso job, e configurar. Pra esse job, ele em si não precisa de parâmetros pra execução porque ele vai construir a imagem, mas ele vai passar parâmetros pra um próximo de job que é o que vai subir a aplicação em dev. Pra isso eu preciso pré-definir esses parâmetros agora.

[01:06] Como é que eu faço? Nós clicamos aqui, "Este build é parametrizado", e nós vamos criar dois parâmetros pra ele. O primeiro parâmetro vai ser o seguinte: parâmetro string, o nome dele vai ser image, que vai ser a imagem que nós vamos construir. Se nós olharmos aqui embaixo, nós temos o nome de uma imagem na hora que nós construímos o container. Então a gente vai copiar essa imagem aqui e vai passar pra cá.

[01:44] Colamos a imagem aqui e nós temos que fazer uma alteração nesse valor padrão, porque? Agora a gente vai aprender a registrar essa imagem lá no Docker Hub. Pra isso, primeiro vamos configurar o valor aqui do nome da imagem, no nosso caso vai ser aluracursos/ o nome da imagem. Vamos colocar ele aqui em minúsculo.

[02:15] Nós vamos, na próxima aula, aprender a configurar esse repositório. Feito isso, nós vamos adicionar um outro parâmetro que é um parâmetro de string pra que eu passe pro próximo job. Esse parâmetro vai ser DOCKER_HOST, isso é só para garantir que o próximo job execute no mesmo servidor que o primeiro job executou os builds. Então, pra isso, nós passamos o valor tcp e o IP do servidor. E agora nós clicamos em aplicar.

[02:55] Bom, salvamos agora os parâmetros. Nós temos que fazer duas configurações pra enviar essa imagem pro registro agora. A primeira é, aqui no processo de build da imagem, nós vamos marcar uma opção chamada Push image e ele vai jogar essa imagem pra um registro. Pra qual o registro? Depende das credenciais que a gente utilizar.

[03:19] Então nós vamos vir aqui, vamos clicar aqui em Add dentro de Jenkins. E aqui é muito simples, ele vai pedir um usuário e uma senha, então nós vamos colocar o usuário aluracursos e, nesse nosso caso, a senha é inserida aqui de acordo com a configuração da sua conta lá no Docker Hub. Desse jeito, vamos colocar assim o ID dele como dockerhub.

[03:55] Por padrão, o job vai fazer o push da imagem direto pro Docker Hub. Como que ele sabe que é direto pro Docker Hub? Porque o daemon que tá rodando lá no meu servidor, onde o Docker foi exposto, tá configurado pro registro padrão que é o Docker Hub. Se você na sua empresa configurou o seu docker pra fazer o push pra um outro registro, um registro privado por exemplo, automaticamente ele vai pro registro privado já que eu tô acessando o Cloud do Docker instalado.

[04:24] Então, feito isso, eu vou dar uma description aqui chamada dockerhub, só pra gente colocar um valor e adicionar. E agora, se eu clicar aqui no dropdown, eu tenho o aluracursos pro Docker Hub.

[04:41] Então a última configuração que a gente tem que fazer, depois de colocar as credenciais, é mudar a imagem que vai ser aluracursos/ o nome dessa imagem. Por que a gente faz isso? Porque, basicamente, eu tô indo ao Docker Hub registrar uma imagem, se eu não colocar qual é o usuário e debaixo de qual usuário ele não funciona.

[05:04] Então agora a gente dá um salvar e manda construir com parâmetros. Ele traz os parâmetros default pra gente, que o próximo job vai utilizar, e eu dou um construir. Ele tá agendando a construção do job e, assim que a gente terminar o build, a gente volta.

[05:25] Legal, ele acabou o build da imagem. Vamos dar uma olhadinha lá no Docker Hub pra ver imagem registrada? Então a gente vai em "hub.docker.com/u/aluracursos". Então ele abriu aqui o Docker Hub e eu tenho a imagem que a gente acabou de fazer o push dela.

[05:47] Qual é a vantagem agora? Qualquer um dentro da minha companhia, se o registro for privado, ou nesse caso qualquer pessoa que tenha acesso ao Docker Hub consegue fazer o pull dessa imagem.

[05:58] Legal, agora na próxima aula nós vamos configurar um novo job que vai fazer o run dessa imagem pro desenvolvedor poder acessar a aplicação. A gente se vê lá.

@@04
Registrando a nossa imagem

Qual a vantagem de registrar a imagem no Docker Hub no início do pipeline?

A principal vantagem é que a imagem com a aplicação 100% funcional estará sempre disponível
 
Alternativa correta
Que, após registrar a imagem, qualquer um com acesso ao domínio pode baixar e rodar a aplicação
 
Alternativa correta! Devemos construir somente uma vez a imagem no pipeline.
Alternativa correta
Para garantir que o time de desenvolv

@@05
Consolidando o seu conhecimento

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com a próxima aula.
1) Prepare os seus ambientes. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Instalar o plugin Config File Provider

# Configurar o Managed Files para Dev
    # Name : .env-dev
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

# Configurar o Managed Files para Prod
    # Name: .env.-prod
        [config]
        # Secret configuration
        SECRET_KEY = 'r*5ltfzw-61ksdm41fuul8+hxs$86yo9%k1%k=(!@=-wv4qtyv'
        # conf
        DEBUG=False
        # Database
        DB_NAME = "todo"
        DB_USER = "devops"
        DB_PASSWORD = "mestre"
        DB_HOST = "localhost"
        DB_PORT = "3306"

# No job: jenkins-todo-list-principal importar o env de dev para teste:

    Adicionar passo no build: Provide configuration Files
    File: .env-dev
    Target: ./to_do/.env

    Adicionar passo no build: Executar Shell

# Criando o Script para Subir o container com o arquivo de env e testar a app:
    #!/bin/sh

    # Subindo o container de teste
    docker run -d -p 82:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/jenkins-todo-list-principal/to_do/.env:/usr/src/app/to_do/.env --name=todo-list-teste django_todolist_image_build

    # Testando a imagem
    docker exec -i todo-list-teste python manage.py test --keep
    exit_code=$?

    # Derrubando o container velho
    docker rm -f todo-list-teste

    if [ $exit_code -ne 0 ]; then
        exit 1
    fiCOPIAR CÓDIGO
2) Faça o push da sua imagem para o Docker Hub. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Instalar o plugin: Parameterized Trigger 

# Modificar o Job para startar com 2 parametros:
    # Geral:
    Este build é parametrizado com 2 parametros de string
        Nome: image
        Valor padrão: <seu-usuario-no-dockerhub>/django_todolist_image_build

        Nome: DOCKER_HOST
        Valor padrão: tcp://127.0.0.1:2376

# No build step: Build / Publish Docker Image
    # Mudar o nome da imagem para: <seu-usuario-no-dockerhub>/django_todolist_image_build
    # Marcar: Push Image e configurar **suas credenciais** no dockerhub

# Mudar no job de teste a imagem para: ${image}
    docker run -d -p 82:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/jenkins-todo-list-principal/to_do/.env:/usr/src/app/to_do/.env --name=todo-list-teste ${image}

Continue com os seus estudos, e se houver dúvidas, não hesite em recorrer ao nosso fórum!

@@06
O que aprendemos?

Nesta aula, aprendemos:
Como separar os ambientes da aplicação
Como fazer a mudança entre os ambientes de desenvolvimento e produção
Como parametrizar um job
Como criar um job para registrar uma imagem no Docker Hub

@04-Definindo o pipeline como código

@@01
Lembrando do pipeline

[00:00] E aí pessoal, tudo bem? Antes da gente continuar vamos recapitular tudo que a gente fez até agora.
[00:05] A primeira coisa que a gente fez foi versionar o nosso código no Git Hub. Pra fazer o versionamento do código a gente criou um par de chaves, registrou no Git Hub a nossa chave pública e fez o versionamento dele.

[00:18] Depois nós construímos o primeiro job nosso, que é o job principal, que faz os seguintes passos: ele faz o pull periodicamente da imagem pra ver se teve alguma alteração no código, caso ele encontre uma alteração ele faz o build da imagem. Então ele vai lá e constrói a nossa imagem Docker, depois disso ele registra a imagem no Docker Hub e notifica pra fazer a próxima etapa de teste.

[00:47] O teste vai ser executado, se o teste for com sucesso vem o nosso próximo exercício agora. Na nossa próxima aula a gente vai começar a trabalhar com o job de deploy no ambiente de desenvolvimento. Ou seja, nós vamos subir a aplicação numa porta diferente da de produção.

[01:06] Pra mudar um pouco a abordagem e explicar pra vocês outras maneiras de fazer isso, nós não vamos mais fazer o job somente com as opções de freestyle, nós vamos começar a utilizar um pouco da linguagem Groovy. Ela é um pouco mais verbosa mas é mais simples criar o job. Então a gente se vê na próxima aula.

@@02
Integração com o Slack

O script que o instrutor segue durante a aula é o seguinte:
# Criar app no slack: alura-jenkins.slack.com
    URL básico: <Url do Jenkins app no seu canal do Slack>
    Token de integração: <Token do Jenkins app no seu canal do Slack>

# Instalar o plugin do slack: Gerenciar Jenkins > Gerenciar Plugins > Disponíveis: Slack Notification
        # Configurar no jenkins: Gerenciar Jenkins > Configuraçao o sistema > Global Slack Notifier Settings
            # Slack compatible app URL (optional): <Url do Jenkins app no seu canal do Slack>
            # Integration Token Credential ID : ADD > Jenkins > Secret Text
                # Secret: <Token do Jenkins app no seu canal do Slack>
                # ID: slack-token
            # Channel or Slack ID: pipeline-todolist

# As notificações vão funcionar da seguinte maneira:
Job: todo-list-desenvolvimento será feito pelo Jenkinsfile (Próximas aulas)
Job: todo-list-producao: Ações de pós-build > Slack Notifications: Notify Success e Notify Every FailureCOPIAR CÓDIGO
[00:00] Oi pessoal, tudo bem? Agora nessa aula a gente vai começar a montar o nosso job que vai fazer o deploy pra desenvolvimento, vai publicar nossa aplicação no ambiente de desenvolvimento. Pra isso a gente vai começar a configurar algumas partes do job que vão notificar o desenvolvedor que tá pronta a aplicação pra ele rodar.

[00:22] Pra isso a gente vai usar o Slack, o Slack é uma das ferramentas mais comuns de comunicação que a gente tem no mercado, e ela é simples de trabalhar e tem uma integração boa com o Jenkins.

[00:35] Como é que a gente vai fazer isso aí? A gente vai lá no nosso canal, pra quem criou um canal ou o time tem um canal, vai adicionar uma aplicação. A aplicação vai ser o Jenkins CI. Então a gente faz a instalação dele, informações da aplicação, e a gente vai instalar. Legal, aí ele vai pedir pra escolher um canal onde as notificações vão ser publicadas. A gente criou um canal chamado pipeline-todolist, então é nesse canal que vão ser publicadas as notificações pros desenvolvedores e pro time. E a gente vai adicionar a integração com o Jenkins.

[01:27] Ótimo, adicionada a integração, ele passa pra gente algumas configurações que é o que a gente vai fazer agora. Então, a primeira coisa que ele pede pra gente fazer é instalar o plugin no Jenkins que vai fazer a integração fácil. Então a gente vem aqui em Gerenciar plugins, Disponíveis, e vai digitar slack. Tem uma opção chamada Slack Notification, é esse plugin que a gente vai instalar. Então a gente vai esperar a instalação terminar e aí a gente volta.

[02:01] Legal, a instalação terminou. Então agora a gente vai configurar o nosso canal aqui dentro do Jenkins. Então a gente vem em configurar Jenkins, Configurar o sistema, e aí nós vamos lá embaixo e vamos encontrar a opção do Slack que está aqui "Global Slack Notifier Settings" e ele vai pedir alguns parâmetros como: a URL, caso a gente tenha, o subdomínio e o token de integração. Então vamos voltar na página de configuração do Slack.

[02:42] Então a URL vai ser essa daqui, ele já deu pra gente pronta, então eu adiciono aqui essa integração. E aí ele também deu um token pra gente, então a gente vai copiar esse token. Cada aplicação tem seu próprio token. A gente vem aqui, não temos o token instalado, a credencial instalada, a gente vai adicionar da mesma maneira que a gente fez com o Docker Hub e, nas nossas opções de credenciais, a gente vai colocar Secret text, e o Secret text é o nosso token.

[03:19] Aqui nós vamos nomear como slack-token, e a description a gente pode colocar a mesma. A gente vai dar um Add dentro dessa opção e agora a gente seleciona qual que é o canal de notificação. Se a gente voltar aqui, a gente agora só tem a parte do job pra configurar.

[03:39] Agora a gente, antes de terminar e fazer a parte do job, nós temos que colocar em qual canal que ele vai postar essa mensagem. No nosso Slack aqui nós criamos aquele canal que é o pipeline-todolist, então vamos colocar aqui pipeline-todolist e aí a gente vai testar a conexão.

[04:09] Foi com sucesso, vamos dar uma olhadinha lá no nosso Jenkins. Olha aqui, a integração foi feita. Então agora a gente consegue adicionar esses passos dentro do nosso job que a gente vai começar a construir agora.

[04:23] Eu vejo vocês na próxima aula.

@@03
Plugins no Jenkins

Qual o caminho no Jenkins para instalação de plugins?

Gerenciar Jenkins --> Gerenciar Plugins --> Instalados
 
Alternativa correta
Gerenciar Jenkins --> Gerenciar Plugins --> Disponíveis
 
Alternativa correta!
Alternativa correta
Gerenciar Jenkins --> Configurar o sistema --> Nuvem

@@04
Job para o ambiente de desenvolvimento

Errata: No momento [04:27] é mencionado GitHub porém na verdade é Docker Hub.
O script que o instrutor segue durante a aula é o seguinte:
# Novo Job: todo-list-desenvolvimento:
    # Tipo: Pipeline
    # Este build é parametrizado com 2 Builds de Strings:
        Nome: image
        Valor padrão: - Vazio, pois o valor sera recebido do job anterior.

        Nome: DOCKER_HOST
        Valor padrão: tcp://127.0.0.1:2376

    pipeline {

        agent any    

        stages {
            stage('Oi Mundo Pipeline como Codigo') {
                steps {
                    sh 'echo "Oi Mundo"'
                }
            }
        }
    }

    pipeline {
        environment {
            dockerImage = "${image}"
        }
        agent any

        stages {
            stage('Carregando o ENV de desenvolvimento') {
                steps {
                    configFileProvider([configFile(fileId: '<id do seu arquivo de desenvolvimento>', variable: 'env')]) {
                        sh 'cat $env > .env'
                    }
                }
            }
            stage('Derrubando o container antigo') {
                steps {
                    script {
                        try {
                            sh 'docker rm -f django-todolist-dev'
                        } catch (Exception e) {
                            sh "echo $e"
                        }
                    }
                }
            }        
            stage('Subindo o container novo') {
                steps {
                    script {
                        try {
                            sh 'docker run -d -p 81:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/jenkins-todo-list-desenvolvimento/.env:/usr/src/app/to_do/.env --name=django-todolist-dev ' + dockerImage + ':latest'
                        } catch (Exception e) {
                            slackSend (color: 'error', message: "[ FALHA ] Não foi possivel subir o container - ${BUILD_URL} em ${currentBuild.duration}s", tokenCredentialId: 'slack-token')
                            sh "echo $e"
                            currentBuild.result = 'ABORTED'
                            error('Erro')
                        }
                    }
                }
            }
            stage('Notificando o usuario') {
                steps {
                    slackSend (color: 'good', message: '[ Sucesso ] O novo build esta disponivel em: http://192.168.33.10:81/ ', tokenCredentialId: 'slack-token')
                }
            }
        }
    }

# todo-list-principal
    # Definir post build: image=$imageCOPIAR CÓDIGO
[00:00] Oi pessoal, tudo bem? Nessa aula a gente vai começar o nosso job pra fazer o deploy da aplicação no ambiente de desenvolvimento. A gente já configurou as notificações, agora tá na hora de montar o job.

[00:14] Então a gente vem aqui em Novo job e a gente vai dar um nome pra esse job. Vamos padronizar o nome da seguinte maneira: todo-list-desenvolvimento. A diferença é que agora a gente vai usar um pipeline ao invés de construir um projeto free-style. A gente clica aqui em Pipeline e clica no ok. Pronto o job tá aqui.

[00:38] Pra que ele funcione de uma maneira legal, sem que eu tenha que configurar toda vez a imagem e o docker host, a gente vai falar que esse job, esse build no caso, vai ser parametrizado com seguintes parâmetros de string: o DOCKER_HOST que vai receber o valor de "tcp://127.0.0.1:2376".

[01:18] A gente vai adicionar uma segunda string chamada image e, nesse caso, a gente vai deixar o valor padrão vazio, por quê? Ele vai receber do job anterior esse dado, é por isso que a gente instalou parametrized plugin.

[01:36] Bom, nesse job ele é um pouquinho diferente do anterior porque a gente vai usar uma linguagem interpretativa chamada Groovy. O Groovy é uma linguagem muito simples de se entender, vamos criar um exemplo aqui primeiro. Eu vou copiar esse pipeline aqui e eu vou explicar pra vocês aqui na interface mesmo.

[01:56] Então aqui, como ele funciona? Eu tô avisando pra ele que é um pipeline, ele vai rodar em qualquer agente do Jenkins. Nesse caso a gente tá trabalhando com o Jenkins na mesma máquina com uma instalação só, mas é possível trabalhar com o Jenkins com máquinas slaves e aí aqui a gente definiria em qual máquina esse meu job rodaria, se é um slave, qual grupo que eu posso rodar.

[02:23] Depois de eu colocar qual que é o agente, eu vou definir os stages que ele vai executar. Eu só tenho uma declaração de stages porém, dentro dela, eu tenho vários stages que ele vai passar. Nesse exemplo, nós criamos um stage chamado "Oi Mundo Pipeline com Código", a gente só vai printar uma mensagem no log.

[02:43] E dentro desse stage eu tenho steps, eu também posso ter múltiplos steps dentro dos stages, então eu posso ter 10 stages, dentro de cada um deles 10 steps. E aí a gente vai salvar, é bem simples.

[03:02] Quando a gente mandar construir ele vai pedir alguns parâmetros, a gente não vai passar agora o image porque a gente não tá utilizando por enquanto, e deu um ok. Ele começou a executar, vocês vão perceber uma mudança na interface nossa aqui.

[03:16] Vocês estão reparando que agora, além dos nossos logs, ele mostra os stages pra gente aqui, e ele falou que o stage "Oi Mundo Pipeline com Código" foi executado em 337 milissegundos e com sucesso, ele tá verde aqui. Se passar só o mouse em cima e depois clicar em logs, olha que legal, ele mostrou o "Oi Mundo" pra gente.

[03:41] Bom, agora a gente já executou nosso "Oi Mundo", vamos deixar isso aqui um pouquinho melhor. Vamos voltar aqui na configuração do nosso job, no nosso pipeline a gente vai colar um código novo agora. Esse código, primeiro vamos executar pra ver o que que ele vai fazer. Colamos aqui, salvamos.

[04:08] Então agora vamos ver o comportamento do job. Construir com parâmetros, muda um pouquinho. Por enquanto, a gente vai passar a nossa imagem manualmente, depois a gente vai entender como passar do job anterior pra esse, só pra gente visualizar o nosso build agora.

[04:27] Reparem que eu coloquei o repositório do Git Hub (na verdade é no Docker Hub), então ele sempre vai buscar a imagem mais atualizada no repositório. E eu vou dar um construir, eu vou esperar ele terminar de construir e a gente volta.

[04:41] Olha lá, terminou. E olha agora como mudou a nossa interface, eu tenho cada stage descrito, quanto tempo cada stage levou pra executar. Vamos dar uma olhadinha no nosso canal do Slack? Olha aqui, sucesso, seu novo build está disponível nesse IP. Vamos clicar aqui, olha lá a nossa aplicação subiu. Então o dev recebeu a notificação e ela tá funcionando. Pra gente ter certeza mesmo: alura, mestre123. Legal, tá funcional no ambiente de desenvolvimento que tá na porta distinta.

[05:20] Então vamos entender e o que que tem nesse pipeline agora. Primeira coisa, o agent continua igual, ele vai executar em todos os que tiver dentro desse cluster e agora é nesse aqui. O stages tem algumas definições diferentes.

[05:37] A primeira definição é: "Carregando o ENV de desenvolvimento", como que isso funciona? Ele vai ter um step aqui dentro que vai fazer o seguinte: ele vai usar o configFileProvider, aquele plugin que a gente instalou, e vai buscar o ID deste file. Esse ID aqui, se nós olharmos aqui no nosso Jenkins, Gerenciar Jenkins, Managed files, é esse ID aqui olha, se eu clicar aqui é esse ID. Aliás, esse é o de prod, vamos abrir o de dev. E é esse ID aqui, é o f00.

[06:13] Então eu tô falando pra ele "pega esse arquivo aqui, dá o nome de env", e agora eu tô executando uma variável env, eu tô dando um cat nessa variável e jogando pra dentro de .env que é o arquivo local meu, que vai subir o meu ambiente de desenvolvimento.

[06:31] Eu tento derrubar o container antigo, mesmo que ele não exista. Vocês lembram, a gente não criou container nenhum, não tinha container nenhum rodando. E se eu olhar o output do meu job aqui olha, container rodando. Se eu der um Logs aqui, ele até deu exception, ele falou "Olha, eu não consegui derrubar, até porque não tem container", mas ele continuou porque a ideia é que ele continue mesmo, ele só válida que a gente não vai subir um container com mesmo nome.

[06:56] E aí eu tenho outro step que é subindo container novo. Então, só pra sintaxe pra vocês entenderem, eu dei um try e um catch. Então, dentro de steps, eu criei script e criei uma entrada de try. Ou seja, eu posso ter vários scripts, vários passos dentro desse script, pra cada step, e dentro de um stage eu posso ter vários steps.

[07:22] Então é em cascata: eu tenho stages; dentro de stages, stages eu vou ter um só, eu tenho 'n' steps, dependendo do que eu quero fazer, nesse caso a gente já tem dois aqui; e dentro de steps eu tenho as entradas que eu preciso. E inclusive eu usei o try catch aqui pra ver se eu pego alguma exceção.

[07:41] Aí o meu próximo step é subindo container novo. O que que ele faz? Dentro desse stage eu tenho o step que executa um script, que vai tentar subir o container na porta 81 com o daemon passando dois volumes, um mysql que a gente já viu no job de teste e o env que a gente acabou de criar. De novo, como eu sempre falo pra vocês cada workspace, cada job tem o seu diretório e dentro desse diretório tem um .env, que veio de onde? Ele veio desse passo aqui que nós pegamos o configFileProvider.

[08:24] Passei o nome do container, porque que é importante passar o nome? Porque o job anterior tenta derrubar pelo nome, por isso que eu tenho que dar um nome para ele. E eu passo a imagem.

[08:35] Da onde é que veio o dockerImage? Ele veio daqui olha, tem um step novo no pipeline aqui que é o ambiente. Eu defino as variáveis de ambiente e eu tô falando pra ele que o dockerImage vem de image, que é uma variável que vai receber via parâmetro. Então a gente vai entrar aqui no Configurar de novo, via parâmetro. Eu passei manualmente, mas a gente vai configurar o próximo job pra passar automaticamente.

[09:03] E aí ele tenta subir e ele notifica se der algum erro. Então no exception aqui ele dá um slackSend, que é o nosso amiguinho aqui que tá na documentação, passando a cor de erro, que é vermelho no caso, tá como padrão, e a mensagem que não foi possível subir.

[09:24] Bom, ele notifica o usuário, caso ele falhe aqui eu tenho result = 'ABORTED', ou seja, ao invés de ficar verdinho ele vai ficar vermelho, "Opa, falhou nesse step aqui". Eu forcei o erro nesse step. Senão, eu mando uma mensagem.

[09:41] Da onde veio aquela URL? Eu coloquei ela hardcoded aqui, porque? Eu sei que o meu servidor de produção tá rodando nessa URL, passando o tokenCredentialId, esse é o slack-token quando a gente criou lá na configuração do Jenkins e aí ele exibiu a notificação pra gente aqui e a gente consegue acessar a nossa aplicação sem maiores problemas.

[10:03] Bom, antes de terminar essa aula, vamos sabotar o nosso projeto pra ver como é que o erro aparece. Tá vendo aqui que a gente derruba o container antigo? O que a gente vai fazer? Vamos mudar isso aqui, vamos tentar derrubar um container django-todolist-dev1. O que vai acontecer agora? Nosso Job já executou o container, ele tá em produção, eu consigo acessar. Se eu vier aqui e clicar, tá funcionando minha aplicação.

[10:32] Se eu tentar subir um container de novo, na mesma porta, o que será que vai acontecer? Vamos mudar isso aqui. Vamos abrir nosso pipeline aqui e aqui nesse step a gente vai derrubar o container 1, que não existe. Vamos salvar e vamos construir agora.

[10:53] Então qual que é o parâmetro da imagem? A gente já passou naquele anterior, vamos construir. Vamos ver o que que vai acontecer. Ele tá executando, tá carregando o ambiente.

[11:04] Opa, falhou, olha o que aconteceu. Esses aqui, se vocês olharem, eles estão meio laranjinha, significa o seguinte: esse step funcionou, mas porque que ele não tá verde? Porque na hora de eu subir o container novo, falhou.

[11:18] Primeiro vamos olhar se a gente realmente recebeu a mensagem de que ele falhou. Olha lá, "FALHA, Não foi possível subir o container". Ele até dá pra gente qual que é o job que ele falhou. Se eu clicar aqui, ele vai pedir pra eu autenticar no Jenkins, a gente vai autenticar. E aqui ele falou que falhou, a gente vai clicar aqui e vai ver o output do console.

[11:45] Aqui embaixo ele tá como aborted, olha o erro aqui: "Conflito, o container já existe com esse nome". Ele nem chegou a validar a porta porque o container tem o mesmo nome.

[11:56] Então aqui nós conseguimos ver exatamente o erro. Eu não precisaria ir no Slack pra ver o erro, aqui mesmo no meu painel, se eu clicar em logs, ele vai falar pra mim o seguinte: "Eu já tenho o mesmo nome de container", porque o nosso passo anterior falhou. Pra corrigir isso aqui a gente simplesmente remove aquele caractere especial que a gente adicionou na hora de remover o container, ele vai conseguir deployar o código novo.

[12:23] Bom, é isso. Na próxima aula nós vamos, agora, integrar os dois jobs, o primeiro que é o principal, que faz o build, com esse job de desenvolvimento. Ou seja, processo anterior vai passar o nome da imagem pra esse job agora. E também a gente vai trabalhar com o job de produção.

[12:42] A gente se vê na próxima aula.

@@05
Trabalhando com o Groovy

Escolha a opção que melhor reflete o que é a linguagem Groovy:


É a linguagem de programação recomendada para aplicações que serão construídas através de jobs no Jenkins.
 
Alternativa correta
É uma sintaxe que utilizamos para criar os nossos pipelines com código
 
Alternativa correta!
Alternativa correta
No nosso exemplo, foi a linguagem utilizada na aplicação de todo-list.

06
Consolidando o seu conhecimento

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com a próxima aula.
1) Faça a integração com o Slack. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Criar app no slack: alura-jenkins.slack.com
    URL básico: <Url do Jenkins app no seu canal do Slack>
    Token de integração: <Token do Jenkins app no seu canal do Slack>

# Instalar o plugin do slack: Gerenciar Jenkins > Gerenciar Plugins > Disponíveis: Slack Notification
        # Configurar no jenkins: Gerenciar Jenkins > Configuraçao o sistema > Global Slack Notifier Settings
            # Slack compatible app URL (optional): <Url do Jenkins app no seu canal do Slack>
            # Integration Token Credential ID : ADD > Jenkins > Secret Text
                # Secret: <Token do Jenkins app no seu canal do Slack>
                # ID: slack-token
            # Channel or Slack ID: pipeline-todolist

# As notificações vão funcionar da seguinte maneira:
Job: todo-list-desenvolvimento será feito pelo Jenkinsfile (Próximas aulas)
Job: todo-list-producao: Ações de pós-build > Slack Notifications: Notify Success e Notify Every FailureCOPIAR CÓDIGO
2) Crie o job para o ambiente de desenvolvimento. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Novo Job: todo-list-desenvolvimento:
    # Tipo: Pipeline
    # Este build é parametrizado com 2 Builds de Strings:
        Nome: image
        Valor padrão: - Vazio, pois o valor sera recebido do job anterior.

        Nome: DOCKER_HOST
        Valor padrão: tcp://127.0.0.1:2376

    pipeline {

        agent any    

        stages {
            stage('Oi Mundo Pipeline como Codigo') {
                steps {
                    sh 'echo "Oi Mundo"'
                }
            }
        }
    }

    pipeline {
        environment {
            dockerImage = "${image}"
        }
        agent any

        stages {
            stage('Carregando o ENV de desenvolvimento') {
                steps {
                    configFileProvider([configFile(fileId: '<id do seu arquivo de desenvolvimento>', variable: 'env')]) {
                        sh 'cat $env > .env'
                    }
                }
            }
            stage('Derrubando o container antigo') {
                steps {
                    script {
                        try {
                            sh 'docker rm -f django-todolist-dev'
                        } catch (Exception e) {
                            sh "echo $e"
                        }
                    }
                }
            }        
            stage('Subindo o container novo') {
                steps {
                    script {
                        try {
                            sh 'docker run -d -p 81:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/jenkins-todo-list-desenvolvimento/.env:/usr/src/app/to_do/.env --name=django-todolist-dev ' + dockerImage + ':latest'
                        } catch (Exception e) {
                            slackSend (color: 'error', message: "[ FALHA ] Não foi possivel subir o container - ${BUILD_URL} em ${currentBuild.duration}s", tokenCredentialId: 'slack-token')
                            sh "echo $e"
                            currentBuild.result = 'ABORTED'
                            error('Erro')
                        }
                    }
                }
            }
            stage('Notificando o usuario') {
                steps {
                    slackSend (color: 'good', message: '[ Sucesso ] O novo build esta disponivel em: http://192.168.33.10:81/ ', tokenCredentialId: 'slack-token')
                }
            }
        }
    }

# todo-list-principal
    # Definir post build: image=$image

Continue com os seus estudos, e se houver dúvidas, não hesite em recorrer ao nosso fórum!

@@07
O que aprendemos?

Nesta aula, aprendemos:
A fazer a integração com o Slack
Instalar e configurar a aplicação Jenkins CI no Slack
Instalar e configurar o plugin Slack Notification no Jenkins
Uma introdução a processos contínuos com Groovy
A definir um pipeline, criando um job que, entre os seus passos, vai rodar a aplicação e notificar o usuário no Slack


#### 01/09/2023

@05-Usando deploy com aprovação

@@01
Job de produção

O script que o instrutor segue durante a aula é o seguinte:
# Criar o job para colocar a app em producao:
    Nome: todo-list-producao
    Tipo: Freestyle
    # Este build é parametrizado com 2 Builds de Strings:
        Nome: image
        Valor padrão: - Vazio, pois o valor sera recebido do job anterior.

        Nome: DOCKER_HOST
        Valor padrão: tcp://127.0.0.1:2376

    # Ambiente de build > Provide configuration files
        File: .env-prod
        Target: .env

    # Build > Executar shell
        #Execute shell
        #!/bin/sh
        { 
            docker run -d -p 80:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/todo-list-producao/.env:/usr/src/app/to_do/.env --name=django-todolist-prod $image:latest

        } || { # catch
            docker rm -f django-todolist-prod
            docker run -d -p 80:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/todo-list-producao/.env:/usr/src/app/to_do/.env --name=django-todolist-prod $image:latest
        }    

Ações de pós-build > Slack Notifications: Notify Success e Notify Every FailureCOPIAR CÓDIGO
[00:00] Tudo bem pessoal? Vamos continuar agora com o nosso pipeline, só pra gente recapitular aonde a gente parou.

[00:07] Primeiro a gente fez a parte do build da imagem, nós já fizemos a parte de deployar a imagem pro ambiente de desenvolvimento. Agora é a etapa da gente deployar em produção.

[00:22] Qual que é a diferença de desenvolvimento pra produção? Desenvolvimento muda o arquivo .env pra você conectar no banco de desenvolvimento. Produção é a mesma coisa. Então, o que nós vamos fazer agora? Nós vamos criar um job pra fazer o deploy em produção, utilizando o arquivo de configuração pra produção.

[00:43] Vale ressaltar que quando a gente fala de ambiente de produção cada empresa tem a sua política, cada projeto. Normalmente esse deploy não é feito automaticamente, a não ser que você tenha uma política muito bem estruturada de responsabilidades, entre times de desenvolvimento e produção. Nesse nosso caso a gente vai criar o job primeiro, depois nós vamos colocar uma decisão pro usuário. "Depois de deployar em desenvolvimento você quer colocar em produção, sim ou não?"

[01:11] Então vamos lá. Vindo pro Jenkins aqui, a gente vai criar um novo job com o nome todo-list-producao e esse job vai ser free-style, a gente vai criar novamente um free-style. Então que que a gente precisa fazer aqui para que esse job funcione? Primeiro a gente tem que avisar que ele é parametrizado. E quais são os parâmetros que ele vai receber? O DOCKER_HOST, que a gente já tem o valor aqui, e também a image que vai estar vazia porque quem vai passar essa informação é o job anterior.

[01:59] Como eu falei para vocês, depois do deploy de desenvolvimento vai ter opção pro usuário "Você quer ou não quer pôr em produção?", se ele quiser o job vai passar o nome da imagem nova pra ele. Então o valor padrão vai estar vazio.

[02:12] Depois disso a gente vai ter que configurar como que as variáveis de ambiente vão ser trabalhadas nesse job. É muito simples, a gente vem aqui embaixo e seleciona "Provide Configuration files". Quando vocês selecionarem ele vai dar aquele dropdown, que a gente já viu uma vez, que mostra qual dos arquivos a gente vai ter que escolher. A gente vai escolher o de prod agora. E qual que é o target dele? Ele vai ficar aqui dentro desse diretório como .env.

[02:53] Lembrando que cada job tem o seu diretório específico dentro do Jenkins. Então, dessa maneira a gente tem o .env dentro de /var/jenkins e dentro desse diretório todo-list-producao.

[03:08] Feito isso, agora a gente vai colocar um shell script pra subir o container de produção. Lembrando que muda a porta de conexão então, nesse caso, a porta de conexão vai ser a 80. A de produção é 80, e a desenvolvimento é 81.

[03:24] Antes de subir que que a gente tenta fazer? Derrubar o container de produção pra subir o container novo. Existem outras maneiras de fazer isso, a gente vai ver mais pra frente em outros cursos da Alura como que a gente trabalha com Kubernetes pra fazer a orquestração. Essa é uma maneira mais simples pra gente terminar a pipeline nosso de uma maneira saudável.

[03:44] Então o que ele tenta fazer? Primeiro ele tenta derrubar e subir o container. O que ele tenta fazer? Primeiro ele tenta subir o container na porta 80 passando tanto o sock de conexão do MySQL, quanto as variáveis de ambiente dentro do arquivo .env.

[04:03] Eu também dou um nome pra ele que é o django-todolist-prod, que é o nome do container, e essa imagem vem lá do parâmetro que a gente vai receber do job anterior. Se ele não conseguir subir por algum motivo, ou porque o container já tá rodando ou porque a porta tá ocupada, ele vai derrubar o container com o mesmo nome, ele vai procurar um container com mesmo nome, e vai tentar subir novamente. Se mesmo assim ele não conseguir, aí o nosso job falha.

[04:31] Então agora o que que a gente faz? Copia esse shell script, adiciona o Executar shell, passa o script aqui pra ele, e, nas ações de pós-build a gente vai marcar, no Slack Notifications, duas opções: se for com sucesso ou se for com falha. Por quê? Nós aprendemos a usar o Slack Notifications dentro do Groovy, essa é uma outra maneira da gente utilizar via interface usando o free-style.

[05:05] Vou dar um salvar e agora eu vou mandar construir com parâmetros. Como ainda ele não tá recebendo do job anterior, ele vai receber manualmente esse valor, por enquanto. Eu vou dar um construir.

[05:24] Ele tá rodando, rodou com sucesso. E agora a gente acessa a URL da aplicação sem a porta 81, que é a porta normal que é a porta 80 de conexão. A gente já tem aquele usuário que é alura, senha mestre123. Conectamos aqui, ele vai carregar os nossos "todos" pra produção. Pronto, carregou. Essa é a nossa interface de produção sem usar a porta 81.

[05:58] Na próxima aula a gente vai, agora, configurar os três jobs pra interagirem com os parâmetros, pra que a gente consiga passar valores de um job pro outro. Até a próxima.

@@02
Integração dos jobs

O script que o instrutor segue durante a aula é o seguinte:
# Post build actions para os 3 jobs

Job: jenkins-todo-list-principal > Ações de pós-build > Trigger parameterized buld on other projects
    Projects to build: todo-list-desenvolvimento
    # Add parameters > Predefined parameters
        image=${image}

Job: todo-list-desenvolvimento

    pipeline {
        environment {
            dockerImage = "${image}"
        }
        agent any

        stages {
            stage('Carregando o ENV de desenvolvimento') {
                steps {
                    configFileProvider([configFile(fileId: '2ed9697c-45fc-4713-a131-53bdbeea2ae6', variable: 'env')]) {
                        sh 'cat $env > .env'
                    }
                }
            }
            stage('Derrubando o container antigo') {
                steps {
                    script {
                        try {
                            sh 'docker rm -f django-todolist-dev'
                        } catch (Exception e) {
                            sh "echo $e"
                        }
                    }
                }
            }        
            stage('Subindo o container novo') {
                steps {
                    script {
                        try {
                            sh 'docker run -d -p 81:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/todo-list-desenvolvimento/.env:/usr/src/app/to_do/.env --name=django-todolist-dev ' + dockerImage + ':latest'
                        } catch (Exception e) {
                            slackSend (color: 'error', message: "[ FALHA ] Não foi possivel subir o container - ${BUILD_URL} em ${currentBuild.duration}s", tokenCredentialId: 'slack-token')
                            sh "echo $e"
                            currentBuild.result = 'ABORTED'
                            error('Erro')
                        }
                    }
                }
            }
            stage('Notificando o usuario') {
                steps {
                    slackSend (color: 'good', message: '[ Sucesso ] O novo build esta disponivel em: http://192.168.33.10:81/ ', tokenCredentialId: 'slack-token')
                }
            }
            stage ('Fazer o deploy em producao?') {
                steps {
                    script {
                        slackSend (color: 'warning', message: "Para aplicar a mudança em produção, acesse [Janela de 10 minutos]: ${JOB_URL}", tokenCredentialId: 'slack-token')
                        timeout(time: 10, unit: 'MINUTES') {
                            input(id: "DeployGate", message: "Deploy em produção?", ok: 'Deploy')
                        }
                    }
                }
            }
            stage (deploy) {
                steps {
                    script {
                        try {
                            build job: 'todo-list-producao', parameters: [[$class: 'StringParameterValue', name: 'image', value: dockerImage]]
                        } catch (Exception e) {
                            slackSend (color: 'error', message: "[ FALHA ] Não foi possivel subir o container em producao - ${BUILD_URL}", tokenCredentialId: 'slack-token')
                            sh "echo $e"
                            currentBuild.result = 'ABORTED'
                            error('Erro')
                        }
                    }
                }
            }
        }
    }COPIAR CÓDIGO
[00:00] Tudo bem pessoal? Vamos pra mais uma aula? Agora a gente vai aprender a integrar os jobs que a gente criou, passando parâmetros de um pro outro.

[00:09] Então, primeiro job que a gente vai fazer a configuração é no principal. A gente vem aqui em configurar, aqui embaixo nas opções de pós-build a gente vai escolher a opção de fazer o trigger num build parametrizado pra outros projetos. Clicamos aqui, aí ele vai perguntar o seguinte, então agora vamos escolher qual job que o principal vai disparar. Clicando nele aqui a gente vai colocar aqui todo-list-desenvolvimento, aí ele vai ser o job que vai passar o parâmetro.

[00:48] E a gente vai fazer o seguinte: vai adicionar um parâmetro que vai ser passado pro próximo job, e ele vai ser pré-definido. Qual vai ser esse parâmetro? Vai ser o image que a gente sabe que ele espera no próximo lado.

[01:07] E qual vai ser o valor desse parâmetro? Image também só que agora a gente coloca em dólar, por quê? Esse image vai referenciar desse parâmetro aqui. Esse vai ser o inicial, daqui pra frente todos os jobs que forem acionados na cadeia, que nós definimos, vai receber essa mesma imagem como parâmetro.

[1:32] A gente dá um salvar. Então agora a gente vai configurar o segundo job que é o job de desenvolvimento que vai subir os nossos containers de dev. Então a gente volta aqui, dentro do job de desenvolvimento a gente vai em Configurar e nós faremos duas alterações aqui.

[01:52] A primeira alteração, assim como no job passado, o próximo job que vai ser chamado pra fazer a configuração de produção. Só que pra que isso aconteça, como esse job ele é escrito em Groovy, eu preciso trocar o arquivo Groovy que a gente tá usando até agora.

[02:11] O que tem de diferente nesse arquivo novo? Deixa eu abrir aqui pra mostrar pra vocês. Bom, aqui no arquivo, no começo é igual ao que a gente já tinha usado, a gente só tem alguns estágios novos.

[02:23] O primeiro estágio é o seguinte: eu vou fazer uma pergunta pro usuário "Olha, você quer fazer o deploy em produção?" Isso vai aparecer na interface do Jenkins. Quais são os steps desse estágio aqui? Eu vou mandar uma mensagem de warning lá no slack e vou avisar o usuário: "Olha, você quer aplicar a mudança em produção? Você tem uma janela de 10 minutos pra fazer isso". Isso aqui é um exemplo somente. E aí ele dá a URL, a qual o usuário acessa pra aceitar ou não aceitar o deploy, passando lá nossas credenciais do slack token do mesmo jeito.

[02:57] Como é que eu defino esse tempo e como é que eu ponho essa mensagem na tela pro usuário? Eu uso uma função de time-out com tempo de 10 minutos, é o tempo e a unidade que eu escolhi, vocês podem mudar de vocês no projeto de vocês não tem problema nenhum, e aí eu coloco um input.

[03:13] O input, o que que ele vai fazer? Ele vai mostrar um pop-up dentro daquele job perguntando pro cara: "Você quer ou não quer fazer o deploy?". E aí ele vai fazer o seguinte: qual é a mensagem? "Você quer fazer o deploy em produção?". Se clicar em ok, o que acontece? Ele continua o pipeline dentro do Groovy.

[03:31] E qual que é o próximo passo? É o deploy. Se você não clicar pra fazer o deploy em produção, ele vai abortar o seu processo, ele já fez o deploy em desenvolvimento, seu job vai ficar marcado como aborted, não como erro.

[03:44] E aí dentro desse estágio de deploy, mais um stage, eu tenho uma função nova que vocês não viram ainda, que é como eu chamo o próximo job via Groovy.

[03:56] Lá no primeiro job a gente fez via interface gráfica, aqui a gente chama a função build job, passa o nome do job que é o todo-list-producao, passa o parâmetro que é o nome da imagem, então aqui a gente passa como parâmetros do tipo string, o nome do parâmetro é imagem, e o valor dele é o dockerImage que a gente já sabe, porque lá em cima em ambientes a gente já recebeu do primeiro job a mesma imagem.

[04:25] É aquela ideia que a gente conversou, você constrói uma vez só e vai até o final com o seu artefato.

[04:32] E agora que a gente fez o deploy em produção, a gente vai ter as duas aplicações rodando paralelamente. Vamos ver como isso funciona na prática?

[04:40] Bom, pra isso tudo funcionar pegamos todo o nosso script aqui, substituímos o script que a gente tinha que era um pouquinho menor e salvamos. Visualizando aqui nosso pipeline completo.

[04:59] Nós vamos agora fazer o commit no código, o código vai ser atualizado aqui no Jenkins, o Jenkins vai observar o meu GitHub, ele vai fazer o build da imagem, vai registrar a imagem no Docker Hub, vai escutar os testes da imagem, vai notificar o resultado no Slack, se for com sucesso ele vai dar o trigger no job de Dev, o job de Dev vai subir, vai notificar qual que é a URL e vai perguntar pro usuário "Você quer ou não colocar em produção?".

[05:30] Na interface do Jenkins a gente vai escolher sim ou não, e aí vai determinar se esse job vai ser executado ou não vai. Então vamos lá.

[05:41] Bom, voltamos aqui então no nosso ambiente que é dentro do nosso Vagrant da nossa máquina virtual, a gente acessou o diretório Vagrant e o diretório do código-fonte da aplicação, que foi aquele primeiro que a gente acessou lá nas primeiras aulas. E agora a gente vai fazer uma alteração nesse código. Lembrando que tá tudo configurado e conectado com GitHub.

[06:02] Então a gente vai editar dentro de templates/registration/login.html e, pra exemplificar, vamos trocar aqui de Login pra Entrar, é só um exemplo. Salvamos. Então agora a gente vai fazer o commit desse código no GitHub.

[06:25] Primeiro a gente adiciona o código. Limpar a tela aqui pra vocês. Agora a gente adicionou o código, a gente vai gerar a mensagem de commit, git commit -m "Alterando o valor do botao de entrar", é só um exemplo. E agora a gente vai fazer o push pro nosso repositório. Então a gente vai digitar "git push origin master". Feito isso, a gente vai esperar mensagem de confirmação, a gente volta lá pro Jenkins pra observar que todos os três jobs vão ser executados automaticamente.

[07:14] A gente teve um probleminha na rede aqui, então a gente vai reexecutar o comando "git push origin master", ele vai fazer o push do código pro GitHub, assim que terminar a gente volta.

[07:32] Legal, acabou o nosso commit aqui, então agora a gente vai aqui pros nossos pipelines. Na visão geral do Jenkins pra visualizar como é que ele tá fazendo esse fluxo inteiro. Automaticamente ele startou o build do job principal. A gente não tá alterando nada, isso aqui é tudo automático.

[07:58] Reparem o seguinte, ele terminou de executar o job principal e automaticamente ele chamou o job de desenvolvimento, só que ele parou no step aqui: "Fazer o deploy em produção?"

[08:11] Se a gente clicar aqui no nosso job, a gente vai perceber que nesse step de deploy em produção, se eu parar com o mouse em cima ele vai perguntar: "Você quer ou não colocar essa aplicação em produção?". A gente vai clicar em deploy e aí ele vai chamar o próximo job que é o job todo-list-producao que vai fazer o que a gente já tinha configurado pra ele fazer.

[08:33] Terminou, executou com sucesso. Se a gente olhar aqui no nosso Slack, ele começou o build disponível de dev pra gente, avisou que a mudança em produção tinha 10 minutos de janela e deu a URL pra fazer o acesso de aprovação ou não, que é a mesma URL que abre essa área pra gente. E aí ele avisou: "Olha, nosso job tá em produção".

[09:00] Vamos validar agora às duas URL que a gente produziu. A primeira é a porta 81 da URL de desenvolvimento. Então a gente acessa a aplicação aqui, a senha nossa é mestre123. Entrei, esse aqui é o meu ambiente de desenvolvimento, meu banco de dados inclusive de desenvolvimento.

[09:22] E o de produção, qual que é? O de produção é a mesma URL só que sem a porta 81, só a URL mesmo. A gente acessa com mestre123 e pronto, são duas aplicações rodando na mesma máquina que foram buildadas automaticamente, foram deployadas automaticamente, só que em portas diferentes pelo Jenkins.

[09:48] Vamos dar uma olhada no Docker Hub só pra gente ver a nossa imagem registrada lá, "hub.docker.com/u/aluracursos". A gente vai ver a nossa imagem. A nossa imagem foi deployada há 12 minutos atrás, que foi quando a gente executou o último job na gravação aqui. Eu já tenho cinco pulls na minha imagem, ou seja, cinco vezes ela foi executada pra rodar os meus containers.

[10:25] Bom, na próxima aula a gente vai trabalhar agora com qualidade de código. Nós vamos criar um job pra fazer a análise e a cobertura do nosso código, e gerar relatórios pra que a gente possa melhorar ou corrigir eventuais mau usos da linguagem, no caso do Django ou de qualquer linguagem que vocês trabalharem.

[10:45] Nós trabalharemos com aplicação SonarQube integrado com o Jenkins. A gente se vê na próxima aula.

@@03
Deploy em produção

Qual é a sintaxe correta para colocar uma decisão no pipeline, utilizando a linguagem Groovy?

timeout(time: 10, unit: 'MINUTES') {
    input(id: "Deploy Gate", message: "Deploy em produção?", ok: 'Deploy')
}
 
Alternativa correta!
Alternativa correta
tout(time: 10, unit: 'MINUTES') {
    input(id: "Deploy Gate", message: "Deploy em produção?", ok: 'Deploy')
}
 
Alternativa correta
timeout(time: 10 'MINUTES') {
    input(id: "Deploy Gate", message: "Deploy em produção?", ok: 'Deploy')
}
 
Alternativa correta
time-outs(time: 10, unit: 'MINUTES') {
    input(id: "Deploy Gate", message: "Deploy em produção?", ok: 'Deploy')
}

@@04
Passando parâmetros de um job para outro

Qual é uma das vantagens de passar parâmetros de um job para outro no Jenkins?

Essa é única maneira de habilitar o trigger de outros jobs automaticamente
 
Alternativa correta
Isso possibilita que o Jenkins combine todos os jobs encadeados em um novo job com todos os passos
 
Alternativa errada!
Alternativa correta
Com essa integração, alguns parâmetros podem ser passados para o primeiro job e os demais, quando configurados, podem receber automaticamente esses valores, como por exemplo o nome da imagem
 
Alternativa correta!

05
Consolidando o seu conhecimento

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito, excelente. Se ainda não, é importante que você execute o que foi visto nos vídeos para poder continuar com a próxima aula.
1) Crie o job para colocar a aplicação em produção. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Criar o job para colocar a app em producao:
    Nome: todo-list-producao
    Tipo: Freestyle
    # Este build é parametrizado com 2 Builds de Strings:
        Nome: image
        Valor padrão: - Vazio, pois o valor sera recebido do job anterior.

        Nome: DOCKER_HOST
        Valor padrão: tcp://127.0.0.1:2376

    # Ambiente de build > Provide configuration files
        File: .env-prod
        Target: .env

    # Build > Executar shell
        #Execute shell
        #!/bin/sh
        { 
            docker run -d -p 80:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/todo-list-producao/.env:/usr/src/app/to_do/.env --name=django-todolist-prod $image:latest

        } || { # catch
            docker rm -f django-todolist-prod
            docker run -d -p 80:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/todo-list-producao/.env:/usr/src/app/to_do/.env --name=django-todolist-prod $image:latest
        }    

Ações de pós-build > Slack Notifications: Notify Success e Notify Every FailureCOPIAR CÓDIGO
2) Faça a integração do jobs. Você pode se guiar pelo script que o instrutor seguiu durante a aula:

# Post build actions para os 3 jobs

Job: jenkins-todo-list-principal > Ações de pós-build > Trigger parameterized buld on other projects
    Projects to build: todo-list-desenvolvimento
    # Add parameters > Predefined parameters
        image=${image}

Job: todo-list-desenvolvimento

    pipeline {
        environment {
            dockerImage = "${image}"
        }
        agent any

        stages {
            stage('Carregando o ENV de desenvolvimento') {
                steps {
                    configFileProvider([configFile(fileId: '2ed9697c-45fc-4713-a131-53bdbeea2ae6', variable: 'env')]) {
                        sh 'cat $env > .env'
                    }
                }
            }
            stage('Derrubando o container antigo') {
                steps {
                    script {
                        try {
                            sh 'docker rm -f django-todolist-dev'
                        } catch (Exception e) {
                            sh "echo $e"
                        }
                    }
                }
            }        
            stage('Subindo o container novo') {
                steps {
                    script {
                        try {
                            sh 'docker run -d -p 81:8000 -v /var/run/mysqld/mysqld.sock:/var/run/mysqld/mysqld.sock -v /var/lib/jenkins/workspace/todo-list-desenvolvimento/.env:/usr/src/app/to_do/.env --name=django-todolist-dev ' + dockerImage + ':latest'
                        } catch (Exception e) {
                            slackSend (color: 'error', message: "[ FALHA ] Não foi possivel subir o container - ${BUILD_URL} em ${currentBuild.duration}s", tokenCredentialId: 'slack-token')
                            sh "echo $e"
                            currentBuild.result = 'ABORTED'
                            error('Erro')
                        }
                    }
                }
            }
            stage('Notificando o usuario') {
                steps {
                    slackSend (color: 'good', message: '[ Sucesso ] O novo build esta disponivel em: http://192.168.33.10:81/ ', tokenCredentialId: 'slack-token')
                }
            }
            stage ('Fazer o deploy em producao?') {
                steps {
                    script {
                        slackSend (color: 'warning', message: "Para aplicar a mudança em produção, acesse [Janela de 10 minutos]: ${JOB_URL}", tokenCredentialId: 'slack-token')
                        timeout(time: 10, unit: 'MINUTES') {
                            input(id: "Deploy Gate", message: "Deploy em produção?", ok: 'Deploy')
                        }
                    }
                }
            }
            stage (deploy) {
                steps {
                    script {
                        try {
                            build job: 'todo-list-producao', parameters: [[$class: 'StringParameterValue', name: 'image', value: dockerImage]]
                        } catch (Exception e) {
                            slackSend (color: 'error', message: "[ FALHA ] Não foi possivel subir o container em producao - ${BUILD_URL}", tokenCredentialId: 'slack-token')
                            sh "echo $e"
                            currentBuild.result = 'ABORTED'
                            error('Erro')
                        }
                    }
                }
            }
        }
    }

Continue com os seus estudos, e se houver dúvidas, não hesite em recorrer ao nosso fórum!

@@06
O que aprendemos?

Nesta aula, aprendemos:
A criar um job para fazer o deploy no ambiente de produção, utilizando o seu próprio arquivo de configuração
Como integrar os três jobs criados até aqui, passando parâmetros de um para o outro