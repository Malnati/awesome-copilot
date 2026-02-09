---
description: 'Melhores praticas para escrever testes JavaScript/TypeScript usando Jest, incluindo estrategias de mocking, estrutura de testes e padroes comuns.'
agent: 'agent'
---

### Estrutura dos Testes
- Nomeie arquivos de teste com o sufixo `.test.ts` ou `.test.js`
- Coloque arquivos de teste proximos ao codigo testado ou em um diretorio dedicado `__tests__`
- Use nomes de teste descritivos que expliquem o comportamento esperado
- Use blocos describe aninhados para organizar testes relacionados
- Siga o padrao: `describe('Component/Function/Class', () => { it('should do something', () => {}) })`

### Mocking Eficaz
- Faça mock de dependencias externas (APIs, bancos de dados, etc.) para isolar seus testes
- Use `jest.mock()` para mocks de modulo
- Use `jest.spyOn()` para mocks de funcoes especificas
- Use `mockImplementation()` ou `mockReturnValue()` para definir o comportamento do mock
- Resete mocks entre testes com `jest.resetAllMocks()` em `afterEach`

### Testando Codigo Assincrono
- Sempre retorne promises ou use sintaxe async/await nos testes
- Use matchers `resolves`/`rejects` para promises
- Defina timeouts apropriados para testes lentos com `jest.setTimeout()`

### Testes de Snapshot
- Use testes de snapshot para componentes UI ou objetos complexos que mudam pouco
- Mantenha snapshots pequenos e focados
- Revise mudancas de snapshot cuidadosamente antes de commitar

### Testando Componentes React
- Use React Testing Library em vez de Enzyme para testar componentes
- Teste o comportamento do usuario e a acessibilidade do componente
- Consulte elementos por roles de acessibilidade, labels ou conteudo de texto
- Use `userEvent` em vez de `fireEvent` para interacoes mais realistas

## Common Jest Matchers
- Basico: `expect(value).toBe(expected)`, `expect(value).toEqual(expected)`
- Truthiness: `expect(value).toBeTruthy()`, `expect(value).toBeFalsy()`
- Numeros: `expect(value).toBeGreaterThan(3)`, `expect(value).toBeLessThanOrEqual(3)`
- Strings: `expect(value).toMatch(/pattern/)`, `expect(value).toContain('substring')`
- Arrays: `expect(array).toContain(item)`, `expect(array).toHaveLength(3)`
- Objetos: `expect(object).toHaveProperty('key', value)`
- Excecoes: `expect(fn).toThrow()`, `expect(fn).toThrow(Error)`
- Funcoes mock: `expect(mockFn).toHaveBeenCalled()`, `expect(mockFn).toHaveBeenCalledWith(arg1, arg2)`
