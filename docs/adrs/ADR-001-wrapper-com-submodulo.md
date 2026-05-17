# ADR-001: Repo wrapper com PX4 como submódulo

- **Data:** 2026-05-17
- **Status:** Aceita
- **Decisores:** José Romualdo, Hermano Peixoto

## Contexto

A construção da demo CI/CD para firmware PX4 (Sprint 3, ES10 T13) demanda
versionar tanto o firmware quanto material didático (libs Python, testes,
pipelines, manifestos). Duas alternativas naturais:

1. Fork direto do `PX4/PX4-Autopilot` (~2GB, ~50k commits) e adicionar
   material didático em diretórios novos.
2. Repo novo enxuto que referencia PX4 como submódulo git pinado em tag.

## Decisão

Optamos pela alternativa 2 — repo `josercf/inteli-px4-cicd-demo` com PX4
como submódulo pinado.

## Motivações

- **Pedagogia:** material didático separado do firmware ajuda alunos a
  identificar o que é deles vs. o que é do PX4 upstream.
- **Performance:** fork carrega histórico completo do PX4 em toda
  operação git; submódulo permite clone raso (`--depth 1`).
- **Reprodutibilidade:** tag pinada garante que todos rodam mesma versão;
  atualizações requerem decisão explícita (este ADR).
- **Custo de CI:** cache do submódulo é trivial (1 SHA); fork inteiro
  invalida cache a cada merge upstream.

## Riscos conhecidos

- **Aluno esquece `--recurse-submodules`:** mitigado por README e por
  workflow CI que faz `submodules: recursive`.
- **Atualização do PX4 requer ADR:** decisão deliberada; aceitamos
  fricção em troca de governança.

## Consequências

**Positivas:**
- Clone inicial <50MB (vs. ~2GB do fork).
- Material didático versionado independentemente do firmware.

**Negativas:**
- Aluno precisa entender submódulos (oportunidade de ensino).
- Patches no PX4 (se necessários) requerem fork separado.

## ADRs relacionadas

- ADR-002 (OpenChoreo simulado) — a criar
- ADR-003 (Gazebo headless) — a criar
