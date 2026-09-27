
# Exercício - Sistema de acompanhamento de chamados

Desejamos implementar um sistema para dar suporte ao registro e tratamento de chamados de clientes em uma dada empresa.

Para que possamos entender como dar suporte a este processo, precisamos entender como funciona a criação, o registro e o tratamento de chamados da empresa. A este primeiro passo chamamos de análise de negócio, e nesta fase construímos um modelo de análise de negócio.

Conhecendo o negócio, suas entidades e seus processos, somos capazes de identificar o que pode ser implementado em um sistema para dar suporte aos responsáveis pela execução do processo de registro e tratamento de chamados. Nesta fase definimos os requisitos do sistema que será implementado (requisitos de software), construindo o modelo de casos de uso.

A fase seguinte, sabendo o que o software irá realizar, é a fase de Análise e Projeto de Software. Nesta fase definimos o mapa de navegação, o modelo de análise, o modelo de projeto, o modelo de dados e o modelo de implantação.

## Análise de Negócios (Espaço do Problema)

### Modelo de Domínio de Negócios

Para a construção do modelo de negócios começamos pela construção do modelo de domínio, onde modelamos a estrutura da organização e da informação.

Empregados, times e sistemas de informação são objetos ativos (ou Business Workers), e objetos passivos, como documentos, artefatos e produtos, são chamados de entidades de negócios.

Representamos estes objetos ativos e entidades de negócios em diagramas de classe de domínio.

### Diagrama de Classes de Domínio

```plantuml
@startuml
scale 0.9

class Cliente {
  +nome
  +email
}

class Chamado {
  +codigo
  +descricao
  +dataAbertura
}

class Funcionario {
  +nome
  +matricula
}

class MembroHelpdesk
class MembroEquipeSuporte
class EquipeSuporte {
  +nome
}

MembroHelpdesk -up-|> Funcionario
MembroEquipeSuporte -up-|> Funcionario

Cliente "1" -- "*" Chamado : abre >
Chamado "*" -- "1" MembroHelpdesk : registrado por >
Chamado "*" -- "0..1" EquipeSuporte : encaminhado para >
EquipeSuporte "1" o-- "*" MembroEquipeSuporte : agrega >

@enduml
```

### Processos de Negócios

Em seguida, precisamos entender como um chamado é tratado pelos funcionários da empresa. Iremos modelar os processos de negócios. Para modelar processos, utilizaremos diagramas de atividade.

Em alto nível de abstração, podemos modelar o processo iniciando com um evento de recebimento de uma chamada do cliente, o tratamento do chamado reportado e, por fim, encerrando com a chamada de retorno ao cliente.

O processo, com os respectivos responsáveis pela execução das atividades, foi modelado no diagrama de atividades utilizando raias.

### Diagrama de Atividades

```plantuml
@startuml
scale 0.8
title Atendimento de um chamado

|Atendente Helpdesk|
start
:Recebe chamada do cliente;
:Tenta resolver o problema imediatamente;
if (Problema resolvido?) then ([sim])
  stop
else ([não])
  :Registra e analisa o chamado;
  if (Consegue solucionar sozinho?) then ([sim, responde direto])
  else ([precisa de suporte especializado])
    |Lider da Equipe|
    :Agenda atendimento do chamado;
    |Membro da Equipe|
    :Executa o tratamento do chamado;
    |Lider da Equipe|
    :Libera o chamado como resolvido;
  endif
endif

|Atendente Helpdesk|
:Responde ao cliente com a solução;
:Registra o retorno ao cliente;
stop
@enduml
```

### Ciclo de Vida de Entidades de Interesse

Podemos verificar que o ciclo de vida da entidade de negócio Chamado é bastante complexo. Vamos modelar seu ciclo de vida utilizando um diagrama de estados.

### Ciclo de Vida de um Chamado

```plantuml
@startuml
scale 0.8
title Ciclo de vida do Chamado

[*] --> EmAnalise : recebe
EmAnalise --> Encaminhado : encaminha
EmAnalise --> Respondido : responde
Encaminhado --> Agendado : agenda
Agendado --> Resolvido : trata
Resolvido --> Liberado : libera
Liberado --> Respondido : responde
Respondido --> [*] : encerra

@enduml
```

## Especificação da Solução

### Requisitos

Com todo o conhecimento do domínio de negócio, é possível definirmos o que o nosso sistema pode fazer pelos usuários. Para tanto, iremos construir um modelo de caso de uso.

Para construir o modelo de caso de uso, basta seguir os seguintes passos:

1. Identifique os atores. Quem irá utilizar o nosso sistema, pensando em papéis de usuários.
2. Identifique os casos de uso: o que os atores querem alcançar utilizando o sistema? Quais são suas metas ao utilizar o sistema.
3. Para cada caso de uso identificado, especifique a interação entre o ator e o sistema.

### Representação de Atores e Casos de Uso

Para identificar e representar os atores e os casos de uso, utilizamos o diagrama de casos de uso, como apresentado a seguir.

```plantuml
@startuml
scale 0.9
left to right direction
title Casos de uso - Sistema de Acompanhamento de Chamados

actor "Atendente Helpdesk" as hd
actor "Membro da Equipe de Suporte" as sup
actor "Lider da Equipe de Suporte" as ls

rectangle "Sistema de Acompanhamento de Chamados" {
  usecase "Buscar e visualizar chamado" as buscar

  hd --> (Registrar chamado)
  hd --> (Analisar chamado)
  hd --> (Tratar chamado)
  hd --> (Responder chamado)
  hd --> buscar

  sup --> (Tratar chamado agendado)
  sup --> buscar

  ls --> (Agendar tratamento de chamado)
  ls --> (Liberar chamado solucionado)
  ls --> buscar
}
@enduml
```

Realizado por:
Gabrielly Nogueira Rodrigues
RA: 10762766
