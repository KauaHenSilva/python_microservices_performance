
# Performance Tests - Spring PetClinic Microservices

Este projeto realiza testes automatizados de performance no Spring PetClinic (microservices) usando Locust.

# Link do video do youtube

[![Acessar o site](https://img.shields.io/badge/Assistir_no_YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://youtu.be/ga5R7AfWHhc)

# Link do video do artigo

[![Acessar o Artigo](https://img.shields.io/badge/Acessar_o_Artigo-PDF-FF0000?style=for-the-badge&logo=adobeacrobatreader&logoColor=white)](https://drive.google.com/file/d/1ZIe9kGB1xOamI5jgjm1-grpg1F5-IZq7/view?usp=drive_link)

## Objetivo

Avaliar o desempenho do sistema sob diferentes cargas, identificando o ponto de saturação e o comportamento de falhas.

### Cenários Testados
- **Cenário A:** Carga baixa (~25 req/s)
- **Cenário B:** Carga média (~48 req/s)
- **Cenário C:** Carga alta (~75 req/s)

## Estrutura do Projeto

```
performance-tests/
├── locustfile.py           # Script principal do Locust
├── run-tests.ps1           # Script PowerShell para execução automatizada
├── analyze_results.py      # Script Python para análise dos resultados
├── main.ipynb              # Notebook para análise exploratória
├── requirements.txt        # Dependências Python
├── main.tex                # Relatório em LaTeX
├── results/
│   ├── resumo_cenarios.csv         # Resumo estatístico por cenário
│   ├── resumo_resultados.csv       # Resumo detalhado por repetição
│   ├── CenarioA/rep*/              # Resultados de cada repetição
│   ├── CenarioB/rep*/
│   ├── CenarioC/rep*/
│   ├── flags/                      # Flags de controle de execução
│   └── plots/                      # Gráficos gerados automaticamente
└── README.md
```

## Como Executar

### 1. Instale as dependências
```bash
pip install -r requirements.txt
```

### 2. Execute os testes
Recomendado usar o script automatizado:
```powershell
./run-tests.ps1
```
Ou execute manualmente via Locust:
```bash
locust -f locustfile.py --host=http://localhost:8080 --users=50 --run-time=10m --headless --csv=results/CenarioA/rep1/results
```

### 3. Analise os resultados
```bash
python analyze_results.py
```
Ou utilize o notebook `main.ipynb` para análise visual e estatística.

## Interpretação dos Resultados

Os arquivos CSV gerados trazem:
- `results_stats.csv`: Estatísticas agregadas por endpoint
- `results_failures.csv`: Detalhes dos erros
- `results_exceptions.csv`: Exceções capturadas

Os gráficos em `results/plots/` mostram:
- Throughput (Requests/s) por cenário
- Latência média e percentis
- Taxa de falhas
- Relação entre throughput e falhas

## Explicação Técnica: Falhas Zeradas

**É normal e esperado que as falhas sejam zero nos cenários A e B!**

- O sistema opera abaixo da capacidade máxima nesses cenários, por isso não há falhas.
- Apenas no Cenário C (maior carga) o sistema atinge o ponto de saturação e começam a aparecer falhas.
- A relação não é linear: existe um "ponto de quebra" onde os recursos se esgotam e as falhas surgem.
- Falhas zeradas em cargas baixas/médias indicam que o sistema está saudável e bem dimensionado.

**Resumo dos resultados:**
- Cenário A (~25 req/s): 0 falhas em todas as repetições
- Cenário B (~48 req/s): 0 falhas em todas as repetições
- Cenário C (~75 req/s): falhas variáveis (0 a 586 por repetição)

## Dicas e Troubleshooting

- Se a aplicação não responde, verifique os containers com `docker-compose ps`.
- Se houver muitos erros, reduza o número de usuários ou aumente o tempo entre requisições.
- Monitore recursos do sistema (CPU, memória, rede) durante os testes.

## Observações

- O primeiro minuto de cada teste é descartado (aquecimento).
- Execute múltiplas repetições para obter médias confiáveis.
- Os scripts e notebooks geram gráficos e relatórios automáticos em `results/plots/`.

---
**Dúvidas ou sugestões? Abra uma issue ou entre em contato!**
