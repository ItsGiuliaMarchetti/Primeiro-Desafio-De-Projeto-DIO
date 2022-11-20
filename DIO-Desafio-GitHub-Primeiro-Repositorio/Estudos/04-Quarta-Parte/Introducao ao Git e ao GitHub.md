# Introdução ao GIT

## Navegação Via *Command Line Interface*

#### Comandos Básicos Para um Bom Desempenho no Terminal :keyboard:

Interface Gráfica (GUI): tudo responde ao usuário

*Command Line Interface* (CLI): é diferente de uma interface gráfica, para interagir é necessário usar uma linha de comando.

#### Primeiros Comandos :bookmark_tabs:

* **dir** (lista de diretórios, mostra as pastas contidas no user).
* **cd** (*change directory*, usado para navegar entre os diretórios).
  * **cd /** (leva para o diretório C:)
  * **cd nome da pasta** (leva para pasta específica: digitar nome da pasta)
  * **cd .. **(faz o caminho inverso, volta uma pasta)
* **mkdir** (*make directory*, cria uma pasta)
* **echo nome > nome.extensão** (cria um arquivo, ex: echo hello > hello.txt)
* **del** (deleta apenas arquivos)
* **rmdir** (*remove directory*, remove tudo)

**ATALHOS ÚTEIS**: ctrl + l (limpa a tela); TAB (autocompletar)

## Entendendo Como o Git Funciona Por Baixo dos Panos

#### Tópicos Fundamentais para Entender o Funcionamento do Git :abc:

**SHA1 (*Secure Hash Algorithm*)**: Algoritmo de *Hash* Seguro. É um conjunto de funções *Hash* criptográficas projetadas pela Agência de Segurança Nacional dos EUA. Gera um conjunto de caracteres identificador de 40 dígitos. É uma forma curta de representar um arquivo.

*  **echo "nome do arquivo" | openssl sha1**

#### Objetos Internos do GIT

***BLOBS***: os arquivos ficam gravados dentro desse objeto chamado *blob*. Esse objeto contém metadados dentro dele. O objeto *blob* terá o tipo do objeto, o tamanho, uma barra contrário e zero \0 e o conteúdo de fato desse arquivo (estrutura básica).

Para gerar os 40 caracteres:

* **echo 'conteudo' \ git hash-object --stdin**
* **echo -e 'conteudo' | openssl sha1**

Outra forma:

* **echo -e 'blob 9\0conteudo' | openssl sha1**

***TREES***: armazenam *Blobs*, é uma crescente onde a *blob* é um bloco básico de composição, a *tree* armazenando e apontando para tipo de *blobs* diferentes. Também contém metadados, também possui o \0, mas agora a *tree* guarda o nome desse arquivo, diferente do *blob*.

É responsável por criar toda a estrutura onde estão os arquivos. Podem apontar tanto para o *blob* quanto para outras árvores. Porém, as árvores também possuem um SHA1 desse metadado. Os *blobs* tem o SHA1 do arquivo, as árvores apontam para essas *blobs* e tem um SHA1, os metadados das árvores, então, se algo mudar em um arquivo na qual essa árvore está apontando, quando ela for gerar um SHA1 desses metadados, o SHA da bolha vai ter mudado e isso consequentemente vai refletir no SHA da árvore. Tudo está relacionado. 

***COMMIT***: objeto mais importante de todos. É o comando que junta tudo, que dá sentido para a alteração. O *commit* aponta para uma árvore, aponta para um parente, para um autor, para uma mensagem. Também possui um *timestamp* (horário, data de criação). Também possui um SHA1 dos seus metadados. Se alterar uma *blob*, ele altera os metadados da árvore que também altera o *commit*. 

É super seguro, não tem como alguém alterar algo sem que apareça no histórico. 

**Sistema Distribuído Seguro**.

#### Chave SSH e Token :key:

**Chave SSH**: é um meio de estabelecer uma forma segura e encriptada entre 2 máquinas. Existe sempre a chave pública e a chave privada.

* **Geração da chave SSH**: 

1. **ssh-keygen -t ed25519 -C email** (escrever seu email)

Criar a senha, será criado em uma pasta: ir até a pasta para verificar as chaves.

2. Comando para visualizar o conteúdo dessas chaves: **cat id_ed25519.pub** (copiar essa chave pública e colar no GitHub).
3. **eval $(ssh-agent -s)**
4. **ssh-add id-ed25519** 

**Token de Acesso Pessoal**

É uma outra forma de segurança, usa HTTPS. Você gera um *Token* no GitHub, guarda esse *Token* e toda vez que você for fazer um *commit* o Git irá pedir usuário e senha (a senha sendo o *token*). 

## Iniciando o Git e Criando um Commit :nerd_face:

Todo comando do Git leva git na frente e depois o comando específico.

1. Criando um repositório
2. Iniciar o Git dentro da pasta criada no item anterior
   * **git init** (cria a pasta necessária para iniciar o git, a pasta .git e para visualizar essa pasta é necessário utilizar o comando **ls -a**)
3. Antes de criar o arquivo, é necessário configurar o git.
   * **git config --global user.email " email"** (digitar o email)
   * **git config --global user.name name (digitar o nome)** . IMPORTANTE: usar mesmo nome que o usado no GitHub.
4. Criando um arquivo (criar um arquivo markdown .md e editar esse arquivo).
5. Depois de editar o arquivo, usar o comando
   * **git add * **
   * **git commit -m "comentario sobre o que foi feito"** (sempre escrever um comentario)

## Ciclo de Vida dos Arquivos no Git :cyclone:

O comando Git Initi, além de criar a pasta .git, ele também inicia um conceito do Git chamado repositório.

Para que os arquivos possam ser enviados ao GitHub, eles precisam ser "comitados" porém existe uma etapa a ser seguida.

**Tracked and Untracked**

1. **Tracked**

* Quando você adiciona um arquivo (git add), esse arquivo vai para a parte de *Staged*.
* Quando você modifica um arquivo, esse arquivo sai do estado de *Unmodified* e vai para o *Modified*. Esse arquivo precisa do comando git add para sair do *Modified* e ir para o *Staged* que é o que buscamos.
* Quando você usa o comando *Commit* você automaticamente salva todas as alterações e esse arquivo volta para o *Unmodified*.

2. **Untracked**: são aqueles arquivos que o Git não tem ciência
   * Quando você cria e deleta um arquivo, esse arquivo é *untracked*.

**O que repositório significa**: 

**Working Directory <-> Stagind Area <-> Local Repository <-> Remote Repository**

Observações: os arquivos sempre alteram entre WD e SA. Da SA você precisa usar o *Commit* para integrar os arquivos para o LR e do LR os arquivos podem ser empurrados para o RR. As alterações na máquina não repercutem imediatamente para o repositório remoto, a não ser que você execute códigos específicos.

## Trabalhando com o GitHub :cat:

No site do GitHub, criar um novo repositório.

1. Ir até seu perfil: *Your Repositories*
2. *New*: colocar nome, descrição, se é privado ou não
3. Selecionar *README* se você ainda não tiver criado um
4. Clicar em *Create New Repository* para criar.

Isso irá gerar um link e você precisará desse link para linkar com a sua máquina. 

5. Copiar o URL gerado, ir para o Git Bash e empurras a versão do repositório local para o remoto.
6. Adicionar a origem: **git remote add origin link** (colar o link)
7. **git remote -v** (irá listar o que está cadastrado)
8. Para levar o código do repositório local para o remoto (empurrar): **git push origin master**

## Resolvendo Conflitos :boom:

As pessoas podem clonar o seu repositório (se estiver público) para fazer uma contribuição. Quando você edita e outra pessoa edita a mesma linha, os códigos mudam, sendo que o código da sua máquina e da máquina dessa pessoa contém a edição. Quando isso acontece, ocorre um problema.

A pessoa, ao empurrar a atualização dela para o GitHub, faz com que o código na sua máquina esteja desatualizado e quando você submeter o seu código, o GitHub irá apontar onde está o erro. Você terá que juntas as versões para que a sua seja a mais recente. Esse conflito é chamado de *merge*.

Você precisa corrigir manualmente, usando o comando: **git pull origin master** e seguindo as instruções fornecidas pelo Git Bash.

**Você consegue clonar os códigos das pessoas para fazer uma contribuição. Um jeito de clonar é pelo HTTPS, encontrado em *Code*. Você copia o link e cola no Git Bash usando o comando: git clone URL (onde URL é o link copiado).**