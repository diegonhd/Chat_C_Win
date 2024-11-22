# Chat em C no Windows
Este projeto inclui códigos de um servidor e um cliente em C, que juntos possibilitam a comunicação entre 10 usuários em um servidor local. 

## Descrição
Neste projeto, cada usuário é capaz de enviar mensagens e receber mensagens através da retransmissão de informações feita pelo servidor.
A comunicação vai para além de apenas caracteres, já que os usuários são capazes de enviar um para o outro 6 tipos de emoticons diferentes disponibilizadas em uma interface Unicode via Powershell simples e intuitiva.

## Compilação
Para compilar cada código é necessário digitar os seguintes comandos no terminal:
``gcc -o server .\socket_server.c -lws2_32``
``gcc -o client .\socket.client.c -lws2_32``

## Funcionalidade
### Bibliotecas importadas
1. Servidor 
  - Inclui bibliotecas para sockets (``winsock2.h``, ``ws2tcpip.h``), manipulação de arquivos (stdio.h), threads (process.h) e manipulação de console (conio.h).
2. Cliente
  - Usa a biblioteca ``Winsock2`` (Windows Sockets) para comunicação TCP/IP. A função ``WSAStartup`` inicializa o uso de sockets, enquanto ``socket``, ``connect``, ``send``, e ``recv`` gerenciam a conexão.

### Funções auxiliares
1. Servidor
  - ``BroadcastMessage``: Envia uma mensagem para todos os clientes conectados, exceto um cliente especificado. Também registra as mensagens no arquivo de histórico.
  - ``EnviarHistorico``: Lê o histórico de mensagens do arquivo e envia para um cliente recém-conectado.
  - ``ApagarHistorico``: Exclui o arquivo de histórico do disco.
  - ``configuracaoServidor``: Configura o socket do servidor, associando-o a uma porta e colocando-o em estado de escuta.
  - ``Servidor``: Gerencia a comunicação de um cliente conectado. É executada como uma thread.
2. Cliente
  - ``zerar_string``: Preenche uma string com espaços, útil para "limpar" mensagens antigas.
  - ``menu``: Exibe o menu inicial e retorna a opção escolhida pelo usuário.
  - ``limpar_tela``: Limpa o console (usando system("cls") no Windows).
  - ``imprimeicone``: Usa o PowerShell para imprimir emojis no console baseados em códigos Unicode.
  - ``exibir_menu_emoticons`` e ``obter_emoticon``: Exibem um menu de seleção de emojis e retornam o emoji correspondente em formato de texto.
  - ``obter_emoticon``: Retorna uma string correspondente a um emoji básico (emoticon) com base na escolha do usuário.
  - ``menu_comandos``: Exibe uma lista de comandos que o usuário pode usar no chat.
  - ``clientconfig``: Configura e conecta o cliente ao servidor.

#### Obs: Essas funções auxiliam na modularização do código, deixando o fluxo principal mais limpo e fácil de entender.

### Gerenciamento de Mensagens
- Broadcast de Mensagens:
  - A função BroadcastMessage envia mensagens para todos os clientes conectados, exceto um especificado (geralmente o remetente).
  - Também escreve as mensagens no arquivo de histórico.
- Enviar Histórico:
  - A função EnviarHistorico lê mensagens do arquivo de histórico e as envia para um cliente recém-conectado.
- Apagar Histórico:
  - A função ApagarHistorico exclui o arquivo de histórico do disco.
## Funções essenciais do Servidor
- A função ``configuracaoServidor`` inicializa o socket do servidor:
  1. ``WSAStartup``: Configura o uso de sockets no Windows.
  2. ``socket``: Cria um socket para comunicação TCP/IP.
  3. ``bind``: Associa o socket à porta especificada (PORT).
  4. ``listen``: Coloca o socket em estado de escuta.
- Na função main:
  1. Exibe o nome do host e os endereços IP.
  2. O servidor aceita conexões de novos clientes usando accept num loop, sendo que para cada cliente, uma nova thread é criada (``_beginthreadex``) para gerenciar a comunicação.
## Funções essenciais do Cliente
- A função ``clientconfig``:
  1. Inicializa o Winsock.
  2. Cria um socket para comunicação TCP.
  3. Configura o endereço do servidor (local neste caso, IP 127.0.0.1, porta 8888).
  4. Conecta o cliente ao servidor. Em caso de erro, o programa termina exibindo mensagens apropriadas.
- A função ``Cliente``:
  1. Exibe o menu inicial.
  2. Se o usuário escolhe entrar no chat, ele fornece um nome que é enviado ao servidor.
  3. Em um loop, o cliente:
    - Recebe mensagens do servidor e as exibe.
    - Lê a mensagem do usuário e a processa para envio.
- A função ``menu_comandos``:
  1. Exibe uma lista de comandos disponíveis para o usuário no chat.
- Função ``main``:
  1. Configura o cliente com ``clientconfig``.
  2. Passa o socket configurado para a função ``Cliente`` para iniciar a interação.
