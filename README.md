# StackSpot ErrorHandler

Este componente foi projetado para padronizar os retornos de erro das aplicações.

### Versões suportadas

- net5.0
- net6.0

### Uso

#### 1. Adicione o pacote NuGet `StackSpot.ErrorHandler` ao seu projeto.

```
dotnet add package StackSpot.ErrorHandler
```

#### 2. Adicione ao seu `IApplicationBuilder` no `Startup` da aplicação ou `Program`. 

```csharp
app.UseErrorHandler();
```

#### Implementação

#### Exceções tratadas

- Utilizando `HttpResponseException` - existe a possibilidade de personalizar a mensagem, bem como definir um `HttpStatusCode`. Além do retorno padronizado da api, existe a opção de gravar o log dos erros que estão ocorrendo, através da propriedade `logActive`, caso o valor seja igual a `false`, o log não é gravado.
- O `HttpResponseException` possui diversas sobrecargas, sendo possível atribuir valor as propriedades abaixo:

| **Campo** | **Descrição** |
| :--- | :--- |
| logActive | Campo que permite ativar a gravação de log |
| statusCode | `HttpStatusCode` que será retornado na requisição |
| logLevel | Level do log |
| message | Mensagem que será retornada na requisição |
| exception | Exceção a ser gravada |

Exemplos de uso: 

500 - Internal Server Error

```csharp
throw new HttpResponseException("Ocorreu um erro ao tentar salvar o registro", true);
```

Resultado:
```json
{
  "error_id": "62ec9afe-64ad-46cb-8df0-abfd306cb7dc",
  "message": "Ocorreu um erro ao tentar salvar o registro"
}
```

400 - Bad Request - neste caso abaixo, o log não foi ativado.

```csharp
throw new HttpResponseException(HttpStatusCode.BadRequest, "Campo Nome é obrigatório", false);
```

Resultado:
```json
{
  "error_id": "62ec9afe-64ad-46cb-8df0-abfd306cb7dc",
  "message": "Campo Nome é obrigatório"
}
```

400 - Bad Request

```csharp
throw new HttpResponseException(HttpStatusCode.BadRequest, "Campo Nome é obrigatório", true);
```

Resultado:
```json
{
  "error_id": "62ec9afe-64ad-46cb-8df0-abfd306cb7dc",
  "message": "Campo Nome é obrigatório"
}
```

- Utilizando `HttpResponseObjectException` - este tem o corpotamento similar do `HttpResponseException`, possuindo as mesmas possibilidades citadas anteriomente, só que neste caso também podemos receber um `HttpRequestMessage` e `HttpResponseMessage` como parametros.
- O `HttpResponseException` possui diversas sobrecargas, sendo possível atribuir valor as propriedades abaixo:

#### Exceções não tratadas

Sempre que ocorrer uma exceção não tratada, a exceção é suprimida e o log sempre será registrado. O `HttpStatusCode` será `500 - Internal Server Error` e a Api retornará algo como o exemplo abaixo:

Resultado:
```json
{
  "error_id": "62ec9afe-64ad-46cb-8df0-abfd306cb7dc",
  "message": "An unexpected system error has occurred"
}
```