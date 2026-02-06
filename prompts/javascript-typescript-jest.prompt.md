---
description: 'Melhores práticas para escrever testes JavaScript/TypeScript usando Jest, incluindo estratégias de mocking, estrutura de testes e padrões comuns.'
agent: 'agent'
---

### Estrutura dos Testes
- Nomeie arquivos de teste com o sufixo `.test.ts` ou `.test.js`
- Coloque arquivos de teste próximos ao código testado ou em um diretório dedicado `__tests__`
- Use nomes de teste descritivos que expliquem o comportamento esperado
- Use blocos describe aninhados para organizar testes relacionados
- Siga o padrão: `describe('Component/Function/Class', () => { it('should do something', () => {}) })`

### Mocking Eficaz
- Faça mock de dependências externas (APIs, bancos de dados, etc.) para isolar seus testes
- Use `jest.mock()` para mocks de módulo
- Use `jest.spyOn()` para mocks de funções específicas
- Use `mockImplementation()` ou `mockReturnValue()` para definir o comportamento do mock
- Resete mocks entre testes com `jest.resetAllMocks()` em `afterEach`

### Testando Código Assíncrono
- Sempre retorne promises ou use sintaxe async/await nos testes
- Use matchers `resolves`/`rejects` para promises
- Defina timeouts apropriados para testes lentos com `jest.setTimeout()`

### Testes de Snapshot
- Use testes de snapshot para componentes UI ou objetos complexos que mudam pouco
- Mantenha snapshots pequenos e focados
- Revise mudanças de snapshot cuidadosamente antes de commitar

### Testando Componentes React
- Use React Testing Library over Enzyme for testing components
- Test user behavior and component accessibility
- Query elements by accessibility roles, labels, or text content
- Use `userEvent` over `fireEvent` for more realistic user interactions

## Common Jest Matchers
- Basic: `expect(value).toBe(expected)`, `expect(value).toEqual(expected)`
- Truthiness: `expect(value).toBeTruthy()`, `expect(value).toBeFalsy()`
- Numbers: `expect(value).toBeGreaterThan(3)`, `expect(value).toBeLessThanOrEqual(3)`
- Strings: `expect(value).toMatch(/pattern/)`, `expect(value).toContain('substring')`
- Arrays: `expect(array).toContain(item)`, `expect(array).toHaveLength(3)`
- Objects: `expect(object).toHaveProperty('key', value)`
- Exceptions: `expect(fn).toThrow()`, `expect(fn).toThrow(Error)`
- Mock functions: `expect(mockFn).toHaveBeenCalled()`, `expect(mockFn).toHaveBeenCalledWith(arg1, arg2)`
