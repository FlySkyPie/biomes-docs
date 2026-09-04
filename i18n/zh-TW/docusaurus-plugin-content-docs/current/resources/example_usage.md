---
sidebar_position: 2
---

# 使用範例

### 定義資源路徑

```typescript
interface Health {
  health: number;
  maxHealth: number;
}

interface PlayerResource {
  health: Health;
  position: Vec3;
}

interface ExampleResourcePaths {
  // Players are looked up by their BiomesId.
  "/player": PathDef<[BiomesId], PlayerResource>;
  "/player/health": PathDef<[BiomesId], Health>;
  "/player/position": PathDef<[BiomesId], { position: Vec3 }>;
  // The clock has no parameters.
  "/clock": PathDef<[], { time: number }>;
}
```

### 定義型別組件

```ts
type ExampleResourcesBuilder = BiomesResourcesBuilder<ExampleResourcePaths>;
type ExampleResourceDeps = TypedResourceDeps<ExampleResourcePaths>;
type ExampleResources = TypedResources<ExampleResourcePaths>;
type ExampleReactResources = ReactResources<ExampleResourcePaths>;
```

### 建立資源

```ts
function genPlayerResource(deps: ExampleResourceDeps, id: BiomesId) {
  // Calling deps.get() here creates a dependency between
  // "/player" and "/player/health" + "/player/position".
  // Whenever the dependencies update, this generator function will rerun.
  const health = deps.get("/player/health", id);
  const { position } = deps.get("/player/position", id);

  return {
    health,
    position,
  };
}

function addExampleResources(builder: ExampleResourcesBuilder) {
  // Define a global resource.
  builder.addGlobal("/clock", { time: secondsSinceEpoch() });
  builder.add("/player", genPlayerResource);
  builder.add("/player/health", { health: 100, maxHealth: 100 });
  builder.add("/player/position", { position: [0, 0, 0] });
}
```

### 存取資源

> _注意：也可以使用 `ExampleReactResources` 完成相同操作。_

資源透過 `get()` 方法存取。

```ts
function healthBarColor(resources: ExampleResources, id: BiomesId): string {
  const { health, maxHealth } = resources.get("/player/health", id);
  const healthPercentage = Math.round((health / maxHealth) * 100);
  if (healthPercentage >= 80) {
    return "GREEN";
  } else if (healthPercentage >= 50) {
    return "YELLOW";
  } else if (healthPercentage > 0) {
    return "RED";
  } else {
    return "BLACK";
  }
}
```

### 更新資源

> _注意：也可以使用 `ExampleReactResources` 完成相同操作。_

資源透過 `set()` 方法更新。

```ts
const JUMP_POWER = 10;

function jump(resources: ExampleResources, id: BiomesId) {
  const { position } = resources.get("/player/position", id);
  // Perform a realistic jump.
  resources.set("/player/position", id, {
    position: [position[0], position[1] + JUMP_POWER, position[2]],
  });
}
```

### 在 React 中使用資源

如果您希望資源更新觸發 React 元件重新渲染，請使用 `ReactResources` 上的 `use()` 方法。`ReactResources` 可以透過 `ClientContext` 從所有遊戲元件中存取。

```tsx
const PlayerHealth: React.FC<{ playerId: BiomesId }> = ({ playerId }) => {
  const { reactResources, userId } = useClientContext();
  // Updates to this player's "/player/health" will cause a re-render.
  const { health, maxHealth } = reactResources.use("/player/health", playerId);

  return (
    <div>
      <h1>{`${health}/${maxHealth}`}</h1>
    </div>
  );
};
```