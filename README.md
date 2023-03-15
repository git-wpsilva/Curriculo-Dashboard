# Currículo-Dashboard

Uma versão do tradicional currículo reformulado no **DataStudio**. Este modelo é customizável, podendo ser ajustado a profissionais de diferentes áreas de atuação, contudo, voltado mais para áreas de tecnologia, trazendo como diferencial um import automático de projetos do **GitHub**.

![Currículo](https://user-images.githubusercontent.com/126538842/225218532-2d2eeb2e-86f7-4a93-bc0c-a69cf3a98c5b.gif)

***Tanto a planilha quanto o dashboard estão em modo de leitura, apenas, mas vocês podem fazer uma cópia e editar.***

## Contexto

De início pensei em extrair automaticamente todas as informações, tanto as do currículo (**LinkedIn**), quanto dos projetos. Comecei por pesquisar as possibilidades de fazer isso do LinkedIn, estudei sobre os [Termos de Uso](https://www.linkedin.com/legal/l/api-terms-of-use) da plataforma e verifiquei que isso não seria possível, conforme este trecho:

`25 - “Access, store, display, or facilitate the transfer of any LinkedIn content obtained through the following methods: scraping, crawling, spidering or using any other technology or software to access LinkedIn content outside the APIs (such content, collectively, “Non-Official Content”). This restriction applies (1) whether the Non-Official Content was obtained directly or indirectly through a third party, such as a customer or third party developer, and (2) whether or not the Non-Official Content is stored or displayed in the Application or some other resource, product or service;
”`

Mesmo obtendo uma API no [LinkedIn Developer](https://developer.linkedin.com/) com uma *LinkedIn Page*, fugiria dos propósitos de suas Políticas. Seguindo para a seguinte abordagem:

## Painel de Cadastro

<img src="https://user-images.githubusercontent.com/126538842/225194311-8a91317a-4675-4b8b-8132-1935cf4d22c1.png" width="400" height="400">

O que eu pensei como coleta de dados então foi um painel no **Google Planilhas**, ele tem aquela cara de sites de cadastro com web design pré-histórico, mas completamente funcional rsrs, outra possibilidade válida de interface amigável seria um **Google Formulários**, sendo a melhor interface um **Google App Sheet**, eu cito as ferramentas do Google Workstation porque são as que eu utilizo, porém, as ferramentas da **Microsoft** funcionam tão bem quanto, sendo elas o **Excel**, **Forms** e **Power Apps**, respectivamente.

Esse painel é a aba `Currículo`, da planilha `CV_Dash_Base`. Ele possui uma série de Validações de Dados, para telefones, e-mails, sites, datas e campos com opções de resposta, em outra situação teria páginas/intervalos protegidos, mas neste caso estão sem isso com a opção de download para poder ser usado, editado e debugado.

As partes de *"Educação"* e *"Experiencia Profissional"* possuem uma quantidade de novos dados pré-estabelecidos, **8** e **5**, respectivamente. Você pode criar mais, mas para isso terá que adicioná-los nas referências também da aba `Base do Dashboard`. Essa base serve para transportar as informações do painel em um formato de tabela, que faz mais sentido para enviar ao **DataStudio**, além disso, nesta aba existe tratamento de dados com fórmulas como essa:

```
=SEERRO(SE(PROCURAR("https://";M2)>0;SEERRO(PROCV(EXT.TEXTO(SUBSTITUIR(
SUBSTITUIR(M2;"https://";""); "www.";"");1;5);'Validação de Dados'!AE:AF;2;0);
'Validação de Dados'!AF6);"");"")
```
Ela tem um tratamento de *string* que deixa ela mais "verbosa", porém, é apenas um PROCV dos principais sites que alguém colocaria no currículo, se o valor for encontrado na base `Validação de Dados`, ele retorna a imagem de ícone do site, se não, retorna uma versão genérica de ícone web. É nesta aba que as coisas acontecem, uma observação interessante é que se alguém marcar no painel o estado de Sergipe, por exemplo, na hora de selecionar a cidade só aparecerá as cidades do estado de Sergipe, sendo impossível registrar a cidade de outro estado por engano, <sub>([fonte](http://blog.mds.gov.br/redesuas/lista-de-municipios-brasileiros/) das cidades e estados)</sub>. A aba `Validação de Dados` é uma aba de apoio para essas manipulações, se quiser de fato entender como funciona o núcleo desta base, olhe pela `Validação de Dados` e `Base do Dashboard`.

## Integração com o GitHub

O **GitHub**, por outro lado, já permite de maneira muito simples uma série de integrações diferentes, para extrair automaticamente os dados de cada novo repositório publicado no GitHub eu utilizei o [Zapier](https://zapier.com/app/zaps)

Se você trabalha com dados, é inevitável um dia você conhecer o **Zapier**, você consegue extrair dados e integrar diversas plataformas diferentes, recomendo fortemente que pesquise e estude isso!

A conta gratuita permite programar até 5 Zaps, ou seja, 5 integrações, independente da sua quantidade, existe outra limitação de quantas resultados os zaps forneceram, que é 100 tarefas por mês. Para este projeto, isso não será nenhum problema ~~(a menos que você faça mais de 100 projetos no GitHub por mês rsrs)~~

Você configurará a aba `GitHub` para receber os dados do seu portfólio, se você possui portfólio em outros sites, possui o seu trabalho divulgado em outra plataforma, pode seguir esse mesmo princípio, o **Zapier** tem dezenas de integrações possíveis.

Este é o tipo de integração para o caso deste projeto:

<img src="https://user-images.githubusercontent.com/126538842/225201007-dd9ec16a-6bd8-4877-aba1-74c07af3dcef.png" width="400" height="400">

## Dashboard

Bem, agora que você já tem os dados do **GitHub** automáticos e as informações que inseriu no painel, já pode montar o seu dashboard!

Você clicará nas opções do Dashboard do template e ir em **"Criar uma cópia"**, substitua as planilhas de template pelas suas planilhas:

<img src="https://user-images.githubusercontent.com/126538842/225205379-6aa19407-2c94-473a-b69a-65a923be6450.png" width="400" height="250">

Após fazer isso, a próxima coisa a se fazer é regular os tipos de campos, para isso vá em `Recursos` e clique em `Gerenciar fontes de dados adicionadas`, clique em editar a fonte de dados da `Base do Dashboard`, a seguir uma tabela de como você precisa ajustar:

|    CAMPO    |    TIPO    |
|    ---    |    ---    |
|    Ano    |    Data    |
|    Atribuições    |    Texto    |
|    Cargo    |    Texto    |
|    Cidade    |    Cidade    |
|    Cidade[1]    |    Texto    |
|    Conclusão    |    Data (AAAAMMDD)    |
|    Curso    |    Texto    |
|    Data de Início    |    Data (AAAAMMDD)    |
|    Data de Término    |    Data (AAAAMMDD)    |
|    E-mail    |    Texto    |
|    E-mail foto    |    Imagem    |
|    Empresa    |    Texto    |
|    Estado    |    Subdivisão do país (1º nível)    |
|    Estado[1]    |    Texto    |
|    Experiência    |    Texto    |
|    Filtro    |    Número    |
|    Foto Link    |    Imagem    |
|    Foto Site[1]    |    Imagem    |
|    Foto Telefone[1]    |    Imagem    |
|    Foto Telefone[2]    |    Imagem    |
|    FotoSite[1]    |    Imagem    |
|    Imagens    |    Imagem    |
|    Início    |    Data (AAAAMMDD)    |
|    Instituição    |    Texto    |
|    LinkedIn Foto    |    Imagem    |
|    LinkedIn Link    |    URL    |
|    Nível    |    Número    |
|    Nível-Cargo    |    Texto    |
|    Nível-Cargo-Valor    |    Número    |
|    Nível-Curso    |    Texto    |
|    Nível-Curso-Valor    |    Número    |
|    Nome Completo    |    Texto    |
|    Objetivo    |    Texto    |
|    Outro Site[1]    |    URL    |
|    Outro Site[2]    |    URL    |
|    Outros    |    Texto    |
|    Realizações    |    Texto    |
|    Sites    |    URL    |
|    Sobre    |    Texto    |
|    Telefone[1]    |    Texto    |
|    Telefone[2]    |    Texto    |
|    UF    |    Subdivisão do país (1º nível)    |
|    UF[1]    |    Subdivisão do país (1º nível)    |
|    Último Cargo    |    Texto    |

O resultado terá essa cara aqui:

<img src="https://user-images.githubusercontent.com/126538842/225213741-7dbf535e-da21-452c-a4b6-f497e22a6a16.png" width="400" height="400">

Para regular a fonte de dados do `GitHub` é o mesmo processo, ela ficará deste jeito:

|	CAMPO	|	TIPO	|
|	---	|	---	|
|	Data	|	Data	|
|	Descrição	|	Texto	|
|	Link	|	URL	|
|	Repositório	|	Texto	|

Após fazer esses ajustes o dashboard está praticamente pronto, algo que pode ocorrer é algumas imagens importadas da minha máquina corromperem no momento da cópia, as imagens são:

- Máscara
- Ícone do GitHub
- Ícone de clique
- Palavra "Nível"
- Wallpaper

**Máscara:** A máscara é uma imagem de apoio que faz esse efeito de foto circular na foto de perfil. Essa foto de perfil possui 3 itens: A foto propriamente dita, na primeira camada (fundo), a máscara por cima e por fim, o círculo azul.
**Ícone do GitHub:** O ícone do GitHub é o usado na parte inferior do dashboard, inclusive, se você não tem um, pode apagar toda essa parte.
**Ícone de clique:** Toda região onde o visualizador pode clicar é marcada com um ícone de clique.
**Palavra "Nível":** No gráfico de *"Progressão Profissional/Acadêmica"* o eixo tem um nome diferente de nível e se alterá-lo, alteraria a legenda do gráfico tamb;em, o que eu não gostaria, como não achei uma forma melhor para fazer isso, colei uma imagem com a palavra que eu gostaria por cima.
**Wallpaper:** No fundo do dashboard existe um papel de parede.

Lembrando, essas imagens podem ou não corromper, caso ocorra isso, é só fazer um novo upload dessas imagens, você pode usar a pasta `Pack` para baixar essas imagens e fazer e inseri-las no dashboard. 

## Gráficos

#### Tela 1

Na primeira tela do dashboard aparece um cabeçário com nome, cargo, foto, informações de contato, sites cadastrados e o objetivo profissional.

Embaixa há 2 gráficos, o `Experiência Profissional` e `Experiência Acadêmica`, trazendo o seu histórico, a direita deles tem o gráfico `Progressão Profissional/Acadêmica`, o objetivo desse gráfico é em um único gráfico mostrar todos os inícios de novos cargos com os inícios de novos cursos, sendo que os cursos são vistos em um gráfico de linhas acumuladas, já que os cursos você acumula consigo, mas os cargos você sempre possui um por vez. Esses níveis eu defini de maneira muito simplório, os valores deles em si não importam, o objetivo deste gráfico é pegar as informações do `Experiência Profissional` e `Experiência Acadêmica` e ilustrar isso em uma espécie de linha do tempo, ainda assim, você pode alterar esses parâmetos em `Validação de Dados`, lá na planilha.

O gráfico de mapas mostra os locais onde você já atuou, ele sempre foco a visualização nos dados, então se você tem dados em estados diferentes, ele terá uma visibilidade entre estados, assim como outros distâncias.

A tabela do **GitHub** é sua lista de projetos.

Na `Experiência Profissional` existe um botão, esse botão é o `VER DETALHES`, ele serve para ver detalhes de cada experiência profissional e outras informações, em outra tela.

#### Tela 2

A tela 2 tem um breve cabeçário, de diferente ele traz as informações que você escreveu no seu *"Sobre"*, além disso, existe uma caixa de seleção chamada `Selecione um cargo`, essa tela mostrará as informações detalhadas dos cargos, mas um cargo por vez, conforme selecionado nesta caixa.

O campo de `Experiência Profissional` aqui trará todas as informações da tela 1 + as `Atribuições` e `Realizações`.

Para voltar para tela 1 é só clicar no botão `VOLTAR`, em cima da caixa de seleção.


## FIM!

Em caso de dúvidas, deixe um comentário aqui.

Até a próxima, abraços!

