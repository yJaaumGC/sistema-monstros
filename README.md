# Sistema Monstros — gestão para empresas de reboque

Plataforma criada por **João Victor Moraes** para a **Monstros Reboque**: central de operação na web, aplicativo
Android para os motoristas, mapa ao vivo e checklist dos caminhões. Está em produção desde outubro de 2026 e é
usada todos os dias pelos atendentes e motoristas da empresa.

> **Esta é a vitrine do projeto.** O código é privado, porque o sistema está em produção e com dados reais.
> As telas abaixo são de uma demonstração com **dados fictícios**.

![Central de operação](docs/telas/controle.png)

## O que o sistema faz

### Central de operação (web)
- Lançamento dos atendimentos: motorista, protocolo, placas, seguradora, canal, origem e destino.
- Detalhes de cada atendimento: uso de patins, atraso, valor negociado e observações.
- Acompanhamento por status (em rota, chegou na origem, finalizado) e previsão de término com alerta de atraso.
- Comissões por motorista, fechamento de quinzena e relatórios em Excel.
- Busca e atalhos (hoje, turno atual, última hora, quinzena, atrasados).
- Tudo em tempo real: o que um atendente lança aparece na hora para os outros, sem atualizar a página.

![Atendimento na tabela do dia](docs/telas/linha.png)

![Detalhes do atendimento](docs/telas/novo-2.png)

### Acionamento do motorista
Ao salvar um atendimento, o motorista que está com o app aberto recebe o acionamento na hora, com alarme,
e tem 1 minuto para aceitar. A central vê se ele aceitou, recusou ou não respondeu.

![Aviso de acionamento aceito](docs/telas/acionamento-aceito.png)

### Aplicativo Android do motorista
- Alarme de acionamento que abre por cima de outros apps e da tela bloqueada, com contagem de 1 minuto para aceitar.
- Rastreamento GPS em segundo plano, mesmo com o app fechado (serviço nativo).
- Chegada na origem e no destino, rota no Waze ou no Google Maps, mensagens do cliente, checklist do caminhão
  e registro de abastecimento pelo celular.
- Atualização do app pelo próprio app, sem passar pela loja.

<p align="center">
  <img src="docs/telas/app-acionamento.png" alt="Celular do motorista recebendo um novo acionamento" width="300">
  &nbsp;&nbsp;
  <img src="docs/telas/app-servico.png" alt="Serviço atual no app do motorista" width="300">
</p>

### Motoristas ao vivo, mapa e despacho
![Motoristas ao vivo](docs/telas/ao-vivo.png)

![Mapa da operação](docs/telas/mapa.png)

![Central de despacho](docs/telas/despacho.png)

### Checklist dos caminhões
Check-in e check-out de cada turno com fotos e vídeos, controle de quilometragem, abastecimento com foto da
bomba e consumo médio real de cada caminhão.

![Checklist dos caminhões](docs/telas/checklist.png)

![Relatório dos turnos](docs/telas/relatorio.png)

### Mensagens prontas para o WhatsApp
Posição do guincho, previsão de liberação e bairros próximos, formatadas para os grupos.

![Mensagem de posição do reboque](docs/telas/posicao.png)

### Preparado para várias empresas
Cada empresa vê só os próprios dados (Row Level Security no PostgreSQL), tem os próprios usuários
(administrador, atendente e motorista) e a própria identidade visual (logo, cores, fundo e letras).

### Meu Reboque: o cliente acompanha o guincho
A central manda um link pelo WhatsApp e o cliente, sem instalar nada, vê o guincho chegando no mapa, a previsão,
as etapas do serviço e conversa com a central e com o motorista, com fotos. O número do motorista nunca aparece.
Quando o atendimento é finalizado, a conversa é encerrada. *(Em fase final de implantação.)*

<p align="center">
  <img src="docs/telas/meu-reboque.png" alt="Página Meu Reboque com o guincho no mapa e a previsão de chegada" width="300">
  &nbsp;&nbsp;
  <img src="docs/telas/meu-reboque-chat.png" alt="Conversa do cliente com a central e o motorista" width="300">
</p>

Do lado da central, a mesma conversa aparece no atendimento, com o link pronto para mandar ao cliente:

![Conversa do Meu Reboque na central](docs/telas/central-meu-reboque.png)

### Acesso para associados
O administrador cria um login para o associado com as placas dos veículos dele. O associado entra e vê só os
reboques dessas placas: o que está em andamento (acompanha ao vivo pelo Meu Reboque) e o histórico.
*(Em fase final de implantação.)*

<p align="center">
  <img src="docs/telas/associado.png" alt="Tela do associado com o reboque em andamento e o histórico" width="300">
  &nbsp;&nbsp;
  <img src="docs/telas/associado-ao-vivo.png" alt="Associado acompanhando o guincho ao vivo" width="300">
</p>

## Tecnologias

| Parte | Tecnologia |
|---|---|
| Central web | JavaScript, HTML e CSS (página única, funciona também offline) |
| Banco e back-end | Supabase: PostgreSQL com Row Level Security, funções no banco, Realtime e Storage |
| Funções no servidor | Supabase Edge Functions (TypeScript / Deno) |
| App Android | Capacitor + plugins nativos em Java (GPS em segundo plano, alarme, atualização) |
| Mapas | Leaflet + OpenStreetMap |
| Relatórios | ExcelJS |
| Publicação | GitHub Actions: publica o site sozinho e gera o APK assinado a cada mudança |

## Como as partes se conectam

```mermaid
flowchart LR
  A["Central web<br/>(atendentes)"] <-->|tempo real| B[("Supabase<br/>PostgreSQL + RLS")]
  C["App Android<br/>(motoristas)"] <-->|acionamento, GPS, chegadas| B
  F["Cliente e associado<br/>(Meu Reboque)"] <-->|acompanha e conversa| B
  D["GitHub Actions"] -->|publica o site e o APK| E["Hospedagem"]
  E --> A
  E --> C
```

## Autor

**João Victor Moraes** — desenvolvedor de software com foco em sistemas de baixo nível, engenharia reversa
e segurança da informação.

Quer ver uma demonstração ao vivo? Entre em contato pelo meu perfil.

Projeto criado para a Monstros Reboque. Todos os direitos reservados.
