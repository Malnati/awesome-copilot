---
name: agentic-eval
description: |
  Padroes e tecnicas para avaliar e melhorar saidas de agentes de IA. Use esta skill quando:
  - Implementar loops de auto-critica e reflexao
  - Construir pipelines evaluator-optimizer para geracao critica de qualidade
  - Criar workflows de refinamento de codigo orientados a testes
  - Projetar avaliacao baseada em rubricas ou LLM-as-judge
  - Adicionar melhoria iterativa a saidas do agente (codigo, relatorios, analise)
  - Medir e melhorar a qualidade das respostas do agente
---

# Padroes de Avaliacao de Agentes

Padroes de auto-melhoria por meio de avaliacao e refinamento iterativos.

## Visao Geral

Padroes de avaliacao permitem que agentes avaliem e melhorem suas proprias saidas, indo alem de geracao single-shot para loops de refinamento iterativos.

```
Generate → Evaluate → Critique → Refine → Output
    ↑                              │
    └──────────────────────────────┘
```

## Quando Usar

- **Geracao critica de qualidade**: Codigo, relatorios, analises que exigem alta precisao
- **Tarefas com criterios claros de avaliacao**: Metricas de sucesso definidas
- **Conteudo que exige padroes especificos**: Guias de estilo, conformidade, formatacao

---

## Padrao 1: Reflexao Basica

O agente avalia e melhora sua propria saida por meio de auto-critica.

```python
def reflect_and_refine(task: str, criteria: list[str], max_iterations: int = 3) -> str:
    """Generate with reflection loop."""
    output = llm(f"Complete this task:\n{task}")
    
    for i in range(max_iterations):
        # Self-critique
        critique = llm(f"""
        Evaluate this output against criteria: {criteria}
        Output: {output}
        Rate each: PASS/FAIL with feedback as JSON.
        """)
        
        critique_data = json.loads(critique)
        all_pass = all(c["status"] == "PASS" for c in critique_data.values())
        if all_pass:
            return output
        
        # Refine based on critique
        failed = {k: v["feedback"] for k, v in critique_data.items() if v["status"] == "FAIL"}
        output = llm(f"Improve to address: {failed}\nOriginal: {output}")
    
    return output
```

**Insight chave**: Use saida JSON estruturada para parsing confiavel dos resultados de critica.

---

## Padrao 2: Evaluator-Optimizer

Separe geracao e avaliacao em componentes distintos para responsabilidades mais claras.

```python
class EvaluatorOptimizer:
    def __init__(self, score_threshold: float = 0.8):
        self.score_threshold = score_threshold
    
    def generate(self, task: str) -> str:
        return llm(f"Complete: {task}")
    
    def evaluate(self, output: str, task: str) -> dict:
        return json.loads(llm(f"""
        Evaluate output for task: {task}
        Output: {output}
        Return JSON: {{"overall_score": 0-1, "dimensions": {{"accuracy": ..., "clarity": ...}}}}
        """))
    
    def optimize(self, output: str, feedback: dict) -> str:
        return llm(f"Improve based on feedback: {feedback}\nOutput: {output}")
    
    def run(self, task: str, max_iterations: int = 3) -> str:
        output = self.generate(task)
        for _ in range(max_iterations):
            evaluation = self.evaluate(output, task)
            if evaluation["overall_score"] >= self.score_threshold:
                break
            output = self.optimize(output, evaluation)
        return output
```

---

## Padrao 3: Reflexao Especifica para Codigo

Loop de refinamento orientado a testes para geracao de codigo.

```python
class CodeReflector:
    def reflect_and_fix(self, spec: str, max_iterations: int = 3) -> str:
        code = llm(f"Write Python code for: {spec}")
        tests = llm(f"Generate pytest tests for: {spec}\nCode: {code}")
        
        for _ in range(max_iterations):
            result = run_tests(code, tests)
            if result["success"]:
                return code
            code = llm(f"Fix error: {result['error']}\nCode: {code}")
        return code
```

---

## Estrategias de Avaliacao

### Baseada em Resultado
Avalie se a saida atinge o resultado esperado.

```python
def evaluate_outcome(task: str, output: str, expected: str) -> str:
    return llm(f"Does output achieve expected outcome? Task: {task}, Expected: {expected}, Output: {output}")
```

### LLM-as-Judge
Use um LLM para comparar e ranquear saidas.

```python
def llm_judge(output_a: str, output_b: str, criteria: str) -> str:
    return llm(f"Compare outputs A and B for {criteria}. Which is better and why?")
```

### Baseada em Rubrica
Pontue saidas com base em dimensoes ponderadas.

```python
RUBRIC = {
    "accuracy": {"weight": 0.4},
    "clarity": {"weight": 0.3},
    "completeness": {"weight": 0.3}
}

def evaluate_with_rubric(output: str, rubric: dict) -> float:
    scores = json.loads(llm(f"Rate 1-5 for each dimension: {list(rubric.keys())}\nOutput: {output}"))
    return sum(scores[d] * rubric[d]["weight"] for d in rubric) / 5
```

---

## Boas Praticas

| Pratica | Racional |
|----------|-----------|
| **Criterios claros** | Defina criterios de avaliacao especificos e mensuraveis desde o inicio |
| **Limites de iteracao** | Defina max de iteracoes (3-5) para evitar loops infinitos |
| **Checagem de convergencia** | Pare se a pontuacao da saida nao melhorar entre iteracoes |
| **Historico de log** | Mantenha a trilha completa para debugging e analise |
| **Saida estruturada** | Use JSON para parsing confiavel dos resultados de avaliacao |

---

## Checklist Rapido de Inicio

```markdown
## Evaluation Implementation Checklist

### Setup
- [ ] Define evaluation criteria/rubric
- [ ] Set score threshold for "good enough"
- [ ] Configure max iterations (default: 3)

### Implementation
- [ ] Implement generate() function
- [ ] Implement evaluate() function with structured output
- [ ] Implement optimize() function
- [ ] Wire up the refinement loop

### Safety
- [ ] Add convergence detection
- [ ] Log all iterations for debugging
- [ ] Handle evaluation parse failures gracefully
```
