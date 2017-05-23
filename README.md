# Statement Work 2
# TRABALHO DA ÁREA 2: JOGO DOWN UNDER AUTOMATIZADO EM JAVA WEB SERVICES

## Data de Entrega
21h15min do dia 27 de junho de 2017

## Formato de Entrega
Entregar os arquivos referentes ao código­fonte. Apresentar e descrever verbalmente o funcionamento do programa ao professor.

## Objetivos

* exercitar a programação distribuída usando Web Services na linguagem Java;
* desenvolver uma aplicação formada por dois programas, um servidor (executado por um processo) eum cliente (eventualmente executado por vários processos) que interagem entre si para a solução dedeterminado problema;
* desenvolver uma aplicação servidora capaz de receber acessos concorrentes, gerenciando váriosjogos simultaneamente, sem apresentar falhas de consistência

## Definição
1. Deve ser implementado em **Java Web Services** (SOAP – Simple Object Access Protocol)
2. Aplicação cliente servidor
3. 50 partidas simultânias
4. Receberá um arquivo _".in"_ e retornar os resultados em um arquivo _".out"_
5. Cada jogador receberá um número identificador único e deverá escolher um nome ainda não exitente no servidor, ou seja, único

## Especificação de Entradas e Saídas de um Cliente
1. A aplicação cliente deverá receber um arquivo _".in"_ e retornar um arquivo _".out"_
2. A especificação da entrada começa com o número total de operações remotas que o cliente deve enviar para o servidor. Na sequência aparece uma linha para cada operação remota. Cada uma destas linhas contém o código da operação seguido dos parâmetros separados por “:”.
3. Códigos para operação:
    1. Operação 0¹ – _preRegistro_ (usada para viabilizar o teste):
        1. informa ao servidor o nome de um jogador
        2. e o respectivo identificador que o servidor deverá utilizar para este segundo jogador.
        3. Esta operação retorna sempre 0
        4. não haverá nenhuma inconsistência nas entradas referente às operações de pré­registro (ou seja, elas serão sempre consistentes).
    2. Operação 1 – _registraJogador_ (relacionada ao jogo propriamente dito):
        1. recebe um _string_ com o nome do usuário/jogador
        2. Retorna o identificador (valor inteiro) do usuário (que corresponde a um número de identificação único para este usuário durante uma partida)
        3. Retorna -1 se este usuário já está cadastrado
        4. Retorna -2 se o número máximo de jogadores tiver sido atingido
    3. Operação 2 – _encerraPartida_ (relacionada ao jogo propriamente dito):
        1. Recebe o identificador do usuário (obtido através da chamada _registraJogador_).
        2. Retorna: -1 (erro)
        3. Retorna: 0 (ok).
    4. Operação 3 – _temPartida_ (relacionada ao jogo propriamente dito):
        1. Recebe o identificador do usuário (obtido através da chamada _registraJogador_).
        2. Retorna: ­-2 (tempo de espera esgotado), ­
        3. Retorna: -1 (erro)
        4. Retorna: 0 (ainda não há partida)
        5. Retorna: 1 (sim, há partida e o jogador inicia jogando com as esferas claras identificadas, por exemplo, com letras de “C”)
        6. Retorna: 2 (sim, há partida e o jogador é o segundo a jogar com os as esferas escuras, identificadas, por exemplo, a letra “E”)
    5. Operação 4 – _ehMinhaVez_ (relacionada ao jogo propriamente dito):
        1. recebe o identificador  do usuário   (obtido   através   da   chamada  _registraJogador_).
        2. Retorna: ­-2   (erro:   ainda   não   há   2 jogadores registrados na partida)
        3. Retorna: ­-1 (erro), 
        4. Retorna: 0 (não)
        5. Retorna: 1 (sim)
        6. Retorna: 2 (é o vencedor)
        7. Retorna: 3 (é o perdedor)
        8. Retorna: 4 (houve empate)
        9. Retorna: 5 (vencedor por WO)
        10. Retorna: 6 (perdedor por WO).
    6. Operação 5 – _obtemTabuleiro_ (relacionada ao jogo propriamente dito):
        1. recebe o identificador do usuário (obtido através da chamada  _registraJogador_). 
        2. Retorna: _string_ vazio em caso de erro 
        3. Retorna: _string_ representando o tabuleiro de jogo.
        4. O tabuleiro deve ser representado por 10 caracteres da seguinte forma: **“­CE­­^.^..”**
            * Os 5 primeiros caracteres indicam o estado dos orifícios da torre: 
                1. o primeiro orifício ainda pode receber esferas, 
                2. o segundo e o terceiro estão completo
                (respectivamente com uma esfera clara e uma esfera escura no topo), 
                3. e o quarto e o quinto orifício ainda podem receber esferas.
                4. Os últimos 5 caracteres indicam onde as duas últimas esferas foram soltas (o caractere '^' sinaliza o orifício onde as esferas foram soltas) primeiro e terceiro orifício. 
                5. Depois que todas as esferas de ambos os jogadores tiverem sido jogadas, a torre é deslizada sobre as casas   do   espaço   horizontal   e   elas   são   preenchidas   revelando   todas   as   esferas   jogadas.   Neste momento, um jogo em que a torre contém, por exemplo:
                    * EECEE
                    * CEECE
                    * CEECC
                    * CCCEE
                    * CCEEE
                    * EECEC
                    * CECCE
                    * CCECC
                6. deve apresentar o resultado com todas as esferas dispostas em uma única linha, com o número de pontos do jogador das esferas claras e com o número de esferas do jogador das esferas escuras separadas por vírgula. Para o exemplo acima, o seguinte string deve ser gerado: “EECEECEECECEECCCCCEECCEEEEECECCECCECCECC,3,2”
    7. Operação 6 – _soltaEsfera_ (relacionada ao jogo propriamente dito):
        1. recebe o identificador do usuário (obtido através da chamada _registraJogador_) e número de identificação do orifício da torre onde se deseja soltar a esfera (de 0 até 4, inclusive). 
        2. Retorna: 2 (partida encerrada, o que ocorrerá caso o jogador demore muito para enviar a sua jogada e ocorra o time-out de 60 segundos para envio de jogadas)
        3. Retorna: 1 (tudo certo)
        4. Retorna: 0 (movimento inválido, por exemplo, em um orifício que já tem 8 esferas)
        5. Retorna: -1 (erro: número inválido de orifício)
        6. Retorna: -2 (partida não iniciada: ainda não há dois jogadores registrados na partida)
        7. Retorna: -3 (não é a vez do jogador).
    8. Operação 7 – _obtemOponente_ (relacionada ao jogo propriamente dito):
        1. recebe o identificador do usuário (obtido através da chamada  _registraJogador_).
        2. Retorna um  _string_ vazio para erro
        3. Retorna um _string_ com o nome do oponente.

## Avaliação

O programa cliente será usado para submeter vários conjuntos de entradas e suas respectivas operaçõesao processo servidor. O requisito mínimo para entrega e apresentação corresponde a apresentar corretamente os resultados esperados para o exemplo de entrada apresentado nesta definição. Nos demais casos será avaliado tanto o número de entradas acertadas quanto o nível de erro apresentado.

