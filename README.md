# TCFiapConsultContactsFunction

Este projeto é uma Azure Function desenvolvida em .NET 8.0. A aplicação expõe um endpoint HTTP para consulta de contatos, realizando filtragens baseadas em diversos parâmetros (como Id, Email, PhoneNumber, PhoneDdd, FirstName e LastName). A lógica de persistência e domínio é delegada ao **TechChallenge.SDK** para facilitar a manutenção e escalabilidade do código.

## Funcionalidades

- **Consulta de Contatos**: Permite buscar contatos no banco de dados aplicando filtros via query string.
- **Filtragem Dinâmica**: Suporta filtragem por:
  - Id (Guid)
  - Email (string)
  - PhoneNumber (int)
  - PhoneDdd (int)
  - FirstName e LastName (string)
- **Logs de Execução**: Registra informações e avisos durante o processamento da consulta.

## Tecnologias Utilizadas

- **.NET 8.0** – SDK para desenvolvimento do serviço.
- **Azure Functions (Http Trigger)** – Execução da função em ambiente serverless.
- **Docker** – Conteinerização da aplicação para facilitar o deploy e a escalabilidade.
- **TechChallenge.SDK** – Biblioteca que encapsula a lógica de domínio e persistência.

## Pré-requisitos

- .NET 8.0 SDK
- Azure Functions Core Tools
- (Opcional) Docker – Para executar a aplicação em contêiner.

## Instalação e Execução Local

### Clone o repositório

```
git clone https://seurepositorio.com/TCFiapConsultContactsFunction.git
cd TCFiapConsultContactsFunction
```

## Configurar a variável de ambiente

Defina a variável de ambiente `CONNECTION_DATABASE` com a string de conexão do seu banco de dados. Caso não seja definida, a aplicação utilizará uma string de conexão padrão:

```
export CONNECTION_DATABASE="Server=localhost;Database=ContactsDb;User Id=sa;Password=YourStrong!Password;TrustServerCertificate=True"
```


## Restaurar as dependências e compilar
```
dotnet restore
dotnet build
```

## Executar localmente
### A função estará disponível em:
http://localhost:7071/api/GetContactsFunction

## Deploy com Docker
O projeto inclui um Dockerfile para criação de uma imagem multi-stage.

## Build da imagem
No diretório raiz do projeto, execute:

```
docker build -t tcfiapconsultcontactsfunction .
docker run -d -p 80:80 --name contatosapp tcfiapconsultcontactsfunction
```

## Endpoint e Parâmetros
Endpoint
GET /api/GetContactsFunction

## Parâmetros Possíveis (via query string)
- Id: Guid (ex: ?Id=4f8a2c2e-9b2c-4d1a-9f1e-2e5a7e3e2a1b)
- Email: string (ex: ?Email=contato@exemplo.com)
- PhoneNumber: int (ex: ?PhoneNumber=12345678)
- PhoneDdd: int (ex: ?PhoneDdd=11)
- FirstName e LastName: string (ex: ?FirstName=Joao&LastName=Silva)

## Exemplo de Uso
Para consultar um contato filtrando por Id e Email:

```
GET https://fiapfunctions.azurewebsites.net/api/GetContactsFunction?Id=4f8a2c2e-9b2c-4d1a-9f1e-2e5a7e3e2a1b&Email=contato@exemplo.com
```

## Observações
SDK Module: O método RegisterSdkModule é responsável por registrar o módulo do TechChallenge.SDK, que gerencia as operações de persistência e domínio.
