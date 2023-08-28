#### 28/08/2023

Curso de Jenkins e Docker: Pipeline de entrega continua

https://stackoverflow.com/questions/70541720/jenkins-has-no-installation-candidate-error-while-trying-to-install-jenkins-on


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