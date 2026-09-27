Gabrielly Nogueira Rodrigues
RA: 10762766

# Exercício - Sistema de acompanhamento de chamados

Desejamos implementar um sistema para dar suporte ao registro e tratamento de chamados de clientes em uma dada empresa. Para que possamos entender como dar suporte a este processo, precisamos entender primeiro como funciona a criação, o registro e o tratamento de chamados da empresa, ente passo chamados de análise de negócio e nesta fase construímos um modelo de análise de negócio. Conhecendo o negócio, suas entidades e seus processos, somos capazes de identificar o que pode ser implementado em um sistema para dar suporte aos responsáveis pela execução do processo de registro e tratamento de chamados. 

Nesta fase definimos os requisitos do sistema que será implementado (requisitos de software) construindo o modelo de casos de uso.
A fase seguinte, sabendo o que o software irá realizar é a fase de Análise e Projeto de Software, é nesta fase definimos o mapa de navegação, o modelo de análise, o modelo de projeto, o modelo de dados e o modelo de implantação.

## Análise de Negócios (Espaço do problema)

### Modelo de domínio de negócios

Para a construção do modelo de negócios começamos pela construção do modelo de domínio base, onde modelamos a estrutura da organização e informação.
Empregados, times e sistemas de informação são objetos ativos e objetos passivos, como documentos, artefatos, produtos são chamados de entidades de negócios.

Representamos estes objetos ativos e entidades de negócios em diagramas de classe de domínio.

### Diagrama de classes de domínio

```plantuml
@startuml
class Cliente
class Chamado
class EquipeSuporte
class MembroEquipeSuporte
class MembroHelpdesk
class Funcionario

MembroHelpdesk --> Funcionario
Cliente "1" -right- "*" Chamado: reporta >
Chamado "*" -right- "1" MembroHelpdesk: registrado por >
Chamado "*" -down- "0..1" EquipeSuporte: tratado por >
EquipeSuporte o-right- "*" MembroEquipeSuporte: tem membro
MembroEquipeSuporte -u- |> Funcionario
@enduml
