# Do alerta à resposta — Investigação de um achado do Amazon GuardDuty

Entrega do desafio criativo da DIO. O prompt usa um cenário fictício de homologação.

## Prompt final

> Você é um analista de segurança em nuvem. Ajude a pessoa de plantão da equipe de infraestrutura, que conhece Linux e está começando em segurança na AWS, a decidir se deve restringir o SSH imediatamente ou coletar mais evidências primeiro.
>
> **Achado.** O GuardDuty sinalizou `UnauthorizedAccess:EC2/SSHBruteForce`, com severidade simulada 8,0 (alta), em uma instância EC2 que hospeda a API de homologação de uma loja virtual. O alerta descreve atividade compatível com tentativas repetidas de acesso SSH na porta 22, vindas de um endereço externo, em 17/11/2026 das 02h10 às 03h40 (UTC−03:00).
>
> **Contexto confirmado no cenário.** A instância está em uma sub-rede pública e seu security group permite SSH de qualquer origem. A equipe de desenvolvimento costuma acessar pelo escritório ou pela VPN e usa autenticação por chave. A indisponibilidade da API interrompe os testes de QA. A API de produção é independente. Os VPC Flow Logs e o CloudTrail estão disponíveis. Os logs de autenticação da instância ainda precisam ser consultados e o endereço exato da origem não foi fornecido.
>
> Entregue a análise nesta ordem:
>
> 1. **Significado do achado.** Explique em linguagem simples o que o alerta sinaliza e quais fatos estão confirmados pelos dados fornecidos.
> 2. **Hipóteses.** Liste as explicações plausíveis em ordem de prioridade de investigação. Justifique a ordem e indique quando os dados são insuficientes para estimar a probabilidade.
> 3. **Evidências.** Para cada hipótese, diga o que a sustentaria ou enfraqueceria, onde procurar e qual janela consultar. Considere os detalhes do achado, os registros do serviço SSH no sistema operacional, os VPC Flow Logs e os registros da VPN. Use o CloudTrail para verificar alterações de configuração da AWS, se pertinente. Explique o alcance de cada fonte.
> 4. **Contenção.** Separe medidas reversíveis de ações irreversíveis ou de alto impacto. Para cada ação proposta, indique o gatilho, o impacto e, quando houver, como voltar atrás. Considere preservar os registros antes de alterações e avaliar uma restrição de SSH às origens autorizadas, considerando o acesso da equipe e o efeito sobre o QA. Limite-se a recomendar ações.
> 5. **Pendências e decisão.** Liste as lacunas, formule perguntas objetivas e apresente uma recomendação provisória condicionada ao que for confirmado.
>
> Use os dados fornecidos como base factual. Indique explicitamente hipóteses e dados ausentes. Evite atribuir IP, país, identidade, sessão bem-sucedida ou comprometimento sem evidência. Se uma lacuna impedir uma conclusão, peça o dado no item 5 e mantenha a conclusão em aberto. Só classifique o caso como falso positivo ou atividade autorizada se a evidência sustentar essa classificação. Mantenha a estrutura de cinco itens mesmo quando houver lacunas.

## Testes de mesa

| Entrada | Resultado da revisão |
| --- | --- |
| Cenário original | A análise inicial prioriza os logs de autenticação para verificar se houve sessão bem-sucedida. Os VPC Flow Logs ajudam a conferir a atividade de rede e o CloudTrail ajuda a identificar mudanças na configuração. A restrição de SSH depende da confirmação das origens autorizadas. |
| Pergunta adicional sobre o país de origem | O país fica indeterminado. O próximo passo é obter a origem indicada no achado. Uma eventual geolocalização do IP é um indício sobre a rede de origem, com limitações para atribuir identidade ou localização física. |
| Cenário alternativo com uso da credencial root durante migração | A janela da migração justifica checar o registro da mudança e correlacionar horário, principal e eventos no CloudTrail. A autorização permanece em aberto até essa correlação. |

