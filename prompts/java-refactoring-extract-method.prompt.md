---
name: 'Refatoracao de Metodos Java com Extract Method'
agent: 'agent'
description: 'Refatoracao usando Extract Method em Java'
---

# Refatoracao de Metodos Java com Extract Method

## Papel

Voce e especialista em refatoracao de metodos Java.

Abaixo ha **2 exemplos** (com titulo, codigo antes e codigo depois da refatoracao) que representam **Extract Method**.

## Codigo Antes da Refatoracao 1:
```java
public FactLineBuilder setC_BPartner_ID_IfValid(final int bpartnerId) {
	assertNotBuild();
	if (bpartnerId > 0) {
		setC_BPartner_ID(bpartnerId);
	}
	return this;
}
```

## Codigo Depois da Refatoracao 1:
```java
public FactLineBuilder bpartnerIdIfNotNull(final BPartnerId bpartnerId) {
	if (bpartnerId != null) {
		return bpartnerId(bpartnerId);
	} else {
		return this;
	}
}
public FactLineBuilder setC_BPartner_ID_IfValid(final int bpartnerRepoId) {
	return bpartnerIdIfNotNull(BPartnerId.ofRepoIdOrNull(bpartnerRepoId));
}
```

## Codigo Antes da Refatoracao 2:
```java
public DefaultExpander add(RelationshipType type, Direction direction) {
     Direction existingDirection = directions.get(type.name());
     final RelationshipType[] newTypes;
     if (existingDirection != null) {
          if (existingDirection == direction) {
               return this;
          }
          newTypes = types;
     } else {
          newTypes = new RelationshipType[types.length + 1];
          System.arraycopy(types, 0, newTypes, 0, types.length);
          newTypes[types.length] = type;
     }
     Map<String, Direction> newDirections = new HashMap<String, Direction>(directions);
     newDirections.put(type.name(), direction);
     return new DefaultExpander(newTypes, newDirections);
}
```

## Codigo Depois da Refatoracao 2:
```java
public DefaultExpander add(RelationshipType type, Direction direction) {
     Direction existingDirection = directions.get(type.name());
     final RelationshipType[] newTypes;
     if (existingDirection != null) {
          if (existingDirection == direction) {
               return this;
          }
          newTypes = types;
     } else {
          newTypes = new RelationshipType[types.length + 1];
          System.arraycopy(types, 0, newTypes, 0, types.length);
          newTypes[types.length] = type;
     }
     Map<String, Direction> newDirections = new HashMap<String, Direction>(directions);
     newDirections.put(type.name(), direction);
     return (DefaultExpander) newExpander(newTypes, newDirections);
}
protected RelationshipExpander newExpander(RelationshipType[] types,
          Map<String, Direction> directions) {
     return new DefaultExpander(types, directions);
}
```

## Tarefa

Aplique **Extract Method** para melhorar legibilidade, testabilidade, manutenibilidade, reusabilidade, modularidade, coesao, baixo acoplamento e consistencia.

Sempre retorne um metodo completo e compilavel (Java 17).

Execute etapas intermediarias internamente:
- Primeiro, analise cada metodo e identifique aqueles que excedem os limites:
  * LOC (Lines of Code) > 15
  * NOM (Number of Statements) > 10
  * CC (Cyclomatic Complexity) > 10
- Para cada metodo qualificado, identifique blocos de codigo que possam ser extraidos em metodos separados.
- Extraia pelo menos um novo metodo com um nome descritivo.
- Exiba apenas o codigo refatorado dentro de um unico bloco ```java```.
- Nao remova nenhuma funcionalidade do metodo original.
- Inclua um comentario de uma linha acima de cada novo metodo descrevendo seu proposito.

## Codigo a Ser Refatorado:

Agora, avalie todos os metodos com alta complexidade e refatore-os usando **Extract Method**
