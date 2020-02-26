# TCP AND UDP CONNECTION

## Jean Morelli





### UDP - User Datagram Protocol

* Protocolo simples da camada de transporte

* Não é confiável
* Não faz o "handshake"
* Não há garantia que pacote chegue corretamente



#### Exemplo

```
# UDP CLIENT
from socket import *

from pip._vendor.distlib.compat import raw_input

serverName = '10.20.69.134'  # Server em qual quer conectar
serverPort = 12000  # Port em qual quer conectar
clientSocket = socket(AF_INET, SOCK_DGRAM)  #
message = bytes(raw_input('Input lowercase sentence:'), 'utf-8')
clientSocket.sendto(message, (serverName, serverPort))
modifiedMessage, serverAddress = clientSocket.recvfrom(2048)
print(modifiedMessage)
clientSocket.close()

# UDP SERVER
from socket import *
serverPort = 12000
serverSocket = socket(AF_INET, SOCK_DGRAM)
serverSocket.bind(('', serverPort))
print("The server is ready to receive")
while 1:
    message, clientAddress = serverSocket.recvfrom(2048)
    modifiedMessage = bytes("no.", 'utf-8')
    print(message)
    serverSocket.sendto(modifiedMessage, clientAddress)
```



Neste exemplo, fazemos uma conexão entre um servidor e o cliente. Isto é, ao rodar o servidor ele fica pronto para a escuta, após o cliente ser executado ele faz um pedido para o servidor. Neste caso, é para converter uma string para a mesma em "uppercase". Na parte do cliente é passado o server name no qual quer se conectar e a port number. Podemos ver no método **socket** onde é criado um novo objeto socket, é passado como argumento o **SOCK_DGRAM**, que se refere ao transporte UDP. Após o objeto ter sido criado pego o input do usuário passo para lowercase e por fim transformo a string in bytes no formatura utf-8, assim ficando pronto para ser transportado de acordo com o seu protocolo. Envio para o servidor que está a escuta e por fim fecho o client socket.





### TCP - Transmission Control Protocol

* Confiável
* Controle de _flow_
* Controle de congestionamento 
* Requer uma pré configuração entre cliente e servidor



#### Exemplo

```
# TCP CLIENT

from socket import *

from pip._vendor.distlib.compat import raw_input

serverName = '127.0.0.1'
serverPort = 12000
clientSocket = socket(AF_INET, SOCK_STREAM)
clientSocket.connect((serverName, serverPort))
sentence = bytes(raw_input('Input lowercase sentence:'), 'utf-8')
clientSocket.send(sentence)
modifiedSentence = clientSocket.recv(1024)
print('From Server:', modifiedSentence)
clientSocket.close()


# TCP SERVER

from socket import *
serverPort = 12000
serverSocket = socket(AF_INET,SOCK_STREAM)
serverSocket.bind(('', serverPort))
serverSocket.listen(1)
print('The server is ready to receive')
while 1:
    connectionSocket, addr = serverSocket.accept()
    sentence = connectionSocket.recv(1024)
    capitalizedSentence = sentence.upper()
    connectionSocket.send(capitalizedSentence)
    connectionSocket.close()
```



Como podemos ver, utilizamos o mesmo exemplo das strings. O processo é o mesmo, a maior diferença é quando o objeto socket é criado, passamos agora como argumento o **SOCK_STREAM** , isto significa que o transporte vai ser via TCP.

