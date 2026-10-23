# 📡 Monitor de Sites em Go

Um pequeno monitor de disponibilidade de sites feito no terminal, desenvolvido como parte do **Curso de Introdução à Linguagem Go da [Alura](https://www.alura.com.br/)**.

A ideia do projeto é colocar em prática os fundamentos da linguagem construindo algo útil: o programa lê uma lista de URLs, faz requisições HTTP para cada uma, verifica se o site respondeu com status `200` e registra o resultado em um arquivo de log com data e hora.

---

## ✨ O que o programa faz

- Exibe um menu interativo no terminal.
- Lê a lista de sites do arquivo `sites.txt`.
- Testa cada site com uma requisição HTTP `GET`.
- Considera o site **online** quando o status HTTP é `200`.
- Registra cada teste no arquivo `log.txt`, com data, hora, site e status.
- Permite consultar o histórico de testes pelo próprio menu.
- Repete o monitoramento **3 vezes**, com intervalo de **5 segundos** entre as rodadas.

---

## 🚀 Como executar

É necessário ter o Go instalado. Depois, na pasta do projeto:

```bash
go run hello.go
```

Também é possível compilar e executar o binário:

```bash
go build hello.go
./hello
```

---

## 🖥️ Como usar

Ao iniciar, o programa exibe o menu:

```text
1- Iniciar Monitoramento
2- Exibir Logs
0- Sair do Programa
```

- Digite `1` para monitorar os sites.
- Digite `2` para visualizar os logs.
- Digite `0` para encerrar o programa.

---

## 📄 Arquivos do projeto

| Arquivo     | Função                                                      |
| ----------- | ----------------------------------------------------------- |
| `hello.go`  | Código-fonte do programa.                                   |
| `sites.txt` | Lista de sites que serão monitorados, um por linha.         |
| `log.txt`   | Histórico de testes, criado automaticamente pelo programa.  |

### Formato do `sites.txt`

Cada linha deve conter uma URL completa, incluindo `http://` ou `https://`:

```text
https://www.alura.com.br
https://httpbin.org/status/200
https://httpbin.org/status/404
```

> O arquivo `log.txt` é criado automaticamente na primeira execução e está listado no `.gitignore`.

---

## 🔍 Como o código funciona

O fluxo principal acontece na função `main`:

```text
main
 └── loop do menu
      ├── 1 → iniciarMonitoramento()
      │        ├── leSitesDoArquivo()
      │        ├── para cada rodada:
      │        │    ├── testaSite()
      │        │    │    └── registralog()
      │        │    └── time.Sleep(5s)
      │        └── repete 3 vezes
      ├── 2 → imprimeLogs()
      └── 0 → os.Exit(0)
```

### Principais funções

| Função                  | Responsabilidade                                                        |
| ----------------------- | ----------------------------------------------------------------------- |
| `main`                  | Exibe o menu e direciona a opção escolhida.                             |
| `exibeIntroducao`       | Mostra o nome e a versão do programa.                                   |
| `exibeMenu`             | Imprime as opções disponíveis.                                          |
| `leComando`             | Lê a opção digitada pelo usuário.                                       |
| `iniciarMonitoramento`  | Coordena as rodadas de teste dos sites.                                 |
| `testaSite`             | Faz a requisição HTTP e verifica o status da resposta.                  |
| `leSitesDoArquivo`      | Lê o `sites.txt` e monta a lista de URLs.                               |
| `registralog`           | Acrescenta uma linha ao `log.txt` com o resultado do teste.             |
| `imprimeLogs`           | Lê e exibe o conteúdo do `log.txt`.                                     |

---

## 🧠 Conceitos de Go praticados

- Organização de um programa em `package main`.
- Declaração de constantes com `const`.
- Funções com parâmetros e retorno.
- Estruturas de repetição com `for` e `range`.
- Condicionais com `switch`.
- Trabalho com slices (`[]string`).
- Leitura de arquivos com `os.Open` e `bufio`.
- Escrita e append em arquivos com `os.OpenFile`.
- Requisições HTTP com `net/http`.
- Controle de tempo com `time.Sleep`.
- Tratamento de erros com `if err != nil`.
- Conversão de tipos e formatação de valores.
- Datas formatadas com o layout de referência do Go.

---

## 💡 Detalhes interessantes da implementação

### Datas no estilo Go

O log usa o formato:

```go
time.Now().Format("02/01/2006 15:04:05")
```

Essa data aparentemente estranha é a **data de referência do Go**. Em vez de usar letras como `YYYY-MM-DD`, o Go usa valores fixos:

```text
02/01/2006 15:04:05
```

Ou seja: dia `02`, mês `01`, ano `2006`, hora `15`, minuto `04` e segundo `05`.

### Log sem perder o histórico

O arquivo de log é aberto com:

```go
os.OpenFile(
    "log.txt",
    os.O_CREATE|os.O_RDWR|os.O_APPEND,
    0666,
)
```

- `os.O_CREATE`: cria o arquivo se ele ainda não existir.
- `os.O_RDWR`: permite leitura e escrita.
- `os.O_APPEND`: escreve sempre no final, preservando os registros anteriores.
- `0666`: permissões iniciais do arquivo (`rw-rw-rw-`), que podem ser reduzidas pelo `umask` do sistema.

Cada linha do log fica parecida com:

```text
23/10/2026 18:45:12 - https://www.alura.com.br - online: true
```

### Uma rodada, vários sites

O monitoramento possui dois níveis de repetição:

```go
for i := 0; i < monitoramentos; i++ {
    for i, site := range sites {
        testaSite(site)
    }
    time.Sleep(delay * time.Second)
}
```

O laço externo controla as **3 rodadas**. O laço interno percorre **todos os sites** de cada rodada. Depois de testar a lista inteira, o programa espera **5 segundos** antes de começar a próxima rodada.

---

## 🛠️ Ideias de evolução

- Tratar falhas de rede sem interromper o programa.
- Ignorar linhas vazias e validar as URLs do `sites.txt`.
- Fechar o `resp.Body` após cada requisição HTTP.
- Recarregar a lista de sites a cada rodada.
- Migrar de `io/ioutil` para `os.ReadFile`.
- Separar o código em pacotes e adicionar testes automatizados.
- Gerar um resumo com quantidade de sites online e offline.

---

## 🎓 Créditos

Projeto desenvolvido durante o **Curso de Introdução à Linguagem Go da Alura**.

Este repositório tem fins de estudo e faz parte da minha jornada de aprendizado em Go. 🐹
