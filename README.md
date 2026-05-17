# inteli-px4-cicd-demo

Demo de pipeline CI/CD para firmware PX4 — Módulo 13 (DevOps), Inteli ES10 T13, 2026.1B.

Material didático que acompanha a Sprint 3 do módulo:

- **Aula 07** (19/05) — Hello World, Continuous Deployment
- **Aula 08** (22/05) — Extraindo métricas do pipeline de CI/CD
- **Aula 09** (26/05) — Você escreve (bons) testes de integração?
- **Aula 10** (28/05) — Esteira de CI como guardiã da qualidade

Spec completa: repo de aulas, `docs/superpowers/specs/2026-05-17-px4-cicd-demo-design.md`.

## Estrutura

- `PX4-Autopilot/` — submódulo do firmware (tag estável pinada)
- `libs/drone_modeling/` — funções puras de modelagem
- `tools/` — CLIs do pipeline (missão, métricas, gates, promoção)
- `tests/` — `unit/` (rápido) e `sitl/` (com simulador)
- `docker/` — `Dockerfile.sitl` (PX4+Gazebo headless), `Dockerfile.tools`
- `.github/workflows/` — pipelines CI/CD
- `docs/adrs/` — decisões arquiteturais

## Setup local

Requer: Linux/macOS, Docker, Python 3.11+, git.

```bash
git clone --recurse-submodules https://github.com/josercf/inteli-px4-cicd-demo.git
cd inteli-px4-cicd-demo
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements-dev.txt
pytest tests/unit -v
```

## Smoke test (após PR #1)

```bash
docker compose up --build --abort-on-container-exit --exit-code-from tester
```

Roda o simulador headless + executa testes de conexão MAVSDK. Deve fechar em <5 min.

## Dependências externas

- **GHCR** (`ghcr.io/josercf/*`) — registry das imagens.
- **SonarQube** — instância do instrutor; URL/token via secrets do GitHub (`SONAR_HOST_URL`, `SONAR_TOKEN`).

## Estado atual

Fase 0 (esqueleto): bootstrap Python, primeiro módulo testado (`libs/drone_modeling/dynamics.py`), ADR-001.

Próximo: PR #1 com pipeline GitHub Actions completo (lint, sonar, build SITL, deploy dev).

## Licença

MIT.
