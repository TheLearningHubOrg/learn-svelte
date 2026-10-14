<script lang="ts">
  import MetricCard from './lib/MetricCard.svelte';

  // Svelte 5 Runes: $state for reactive state
  let counter = $state(0);
  let step = $state(1);

  interface RoadmapItem {
    id: number;
    title: string;
    stage: string;
    completed: boolean;
  }

  let items = $state<RoadmapItem[]>([
    { id: 1, title: 'Reactivity & Signals ($state, $derived, $effect)', stage: 'Stage 1: Core', completed: true },
    { id: 2, title: 'Component Composition & Snippets', stage: 'Stage 2: Components', completed: true },
    { id: 3, title: 'State Management & Context API', stage: 'Stage 3: Architecture', completed: false },
    { id: 4, title: 'SvelteKit Routing & Server Side Loaders', stage: 'Stage 4: Fullstack', completed: false },
    { id: 5, title: 'Production Docker Containerization', stage: 'Stage 7: Production', completed: true }
  ]);

  let newItemTitle = $state('');

  // Svelte 5 Runes: $derived for computed state
  let doubled = $derived(counter * 2);
  let completedCount = $derived(items.filter(i => i.completed).length);
  let totalCount = $derived(items.length);
  let completionPercent = $derived(totalCount === 0 ? 0 : Math.round((completedCount / totalCount) * 100));

  // Svelte 5 Runes: $effect for reactive side-effects
  $effect(() => {
    if (counter >= 10) {
      console.log(`[Svelte 5 Runes] Milestone reached: counter = ${counter}`);
    }
  });

  function increment() {
    counter += step;
  }

  function decrement() {
    counter -= step;
  }

  function reset() {
    counter = 0;
  }

  function addItem() {
    if (!newItemTitle.trim()) return;
    items = [
      ...items,
      {
        id: Date.now(),
        title: newItemTitle.trim(),
        stage: 'Stage 6: Custom Topic',
        completed: false
      }
    ];
    newItemTitle = '';
  }

  function toggleItem(id: number) {
    items = items.map(item => item.id === id ? { ...item, completed: !item.completed } : item);
  }

  function removeItem(id: number) {
    items = items.filter(item => item.id !== id);
  }
</script>

<div class="container">
  <header class="header">
    <div class="badge">Svelte 5 &bull; Runes Edition</div>
    <h1 class="title">Learn Svelte</h1>
    <p class="subtitle">
      Zero-runtime reactivity compiler built with modern Svelte 5 Runes (<code style="color: #ff7700;">$state</code>, <code style="color: #ff7700;">$derived</code>, <code style="color: #ff7700;">$effect</code>).
    </p>
  </header>

  <!-- Metric Overview Cards -->
  <div class="grid">
    <MetricCard title="Counter Base" value={counter} description="Current reactive $state value" icon="🔢" />
    <MetricCard title="Derived Double" value={doubled} description="Computed via $derived rune" icon="⚡" />
    <MetricCard title="Curriculum Mastery" value={`${completionPercent}%`} description={`${completedCount} of ${totalCount} stages mastered`} icon="🎯" />
  </div>

  <div class="grid">
    <!-- Interactive Counter Demo -->
    <div class="card">
      <h2 class="card-title"><span>⚡</span> Runes Reactivity Sandbox</h2>
      <p class="card-desc">Experience granular compiler reactivity without virtual DOM overhead.</p>

      <div class="counter-value">{counter}</div>

      <div class="btn-group">
        <button onclick={decrement}>- Decrement</button>
        <button onclick={reset}>Reset</button>
        <button class="primary" onclick={increment}>+ Increment</button>
      </div>

      <div style="margin-top: 1.5rem; display: flex; align-items: center; justify-content: space-between;">
        <label for="step-select" style="font-size: 0.9rem; color: #94a3b8;">Increment Step:</label>
        <select id="step-select" bind:value={step} style="background: #0b1120; color: white; border: 1px solid rgba(255,255,255,0.1); border-radius: 6px; padding: 0.4rem 0.8rem;">
          <option value={1}>1</option>
          <option value={5}>5</option>
          <option value={10}>10</option>
        </select>
      </div>
    </div>

    <!-- 7-Stage Curriculum Checklist -->
    <div class="card">
      <h2 class="card-title"><span>📚</span> 7-Stage Progression Checklist</h2>
      <p class="card-desc">Interactive curriculum progress powered by immutable array updates.</p>

      <form onsubmit={(e) => { e.preventDefault(); addItem(); }} class="input-group">
        <input 
          type="text" 
          placeholder="Add topic or milestone..." 
          bind:value={newItemTitle} 
        />
        <button type="submit" class="primary">Add</button>
      </form>

      <ul class="item-list">
        {#each items as item (item.id)}
          <li class="item" class:done={item.completed}>
            <label style="display: flex; align-items: center; gap: 0.65rem; cursor: pointer; flex: 1;">
              <input 
                type="checkbox" 
                checked={item.completed} 
                onchange={() => toggleItem(item.id)} 
              />
              <span>{item.title}</span>
            </label>
            <button class="delete-btn" onclick={() => removeItem(item.id)} title="Delete item">&times;</button>
          </li>
        {/each}
      </ul>
    </div>
  </div>

  <footer>
    <p>&copy; The Learning Hub Organization &bull; Built with Svelte 5 and Vite</p>
  </footer>
</div>
