<script lang="ts">
  interface Props {
    title: string;
    value: string | number;
    description?: string;
    icon?: string;
  }

  let { title, value, description = '', icon = '⚡' }: Props = $props();
</script>

<div class="metric-card">
  <div class="metric-header">
    <span class="icon">{icon}</span>
    <span class="title">{title}</span>
  </div>
  <div class="metric-value">{value}</div>
  {#if description}
    <div class="metric-desc">{description}</div>
  {/if}
</div>

<style>
  .metric-card {
    background: rgba(30, 41, 59, 0.5);
    border: 1px solid rgba(255, 255, 255, 0.08);
    border-radius: 10px;
    padding: 1rem 1.25rem;
    display: flex;
    flex-direction: column;
    gap: 0.35rem;
  }
  .metric-header {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    color: #94a3b8;
    font-size: 0.85rem;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    font-weight: 600;
  }
  .metric-value {
    font-size: 1.75rem;
    font-weight: 700;
    color: #f8fafc;
  }
  .metric-desc {
    font-size: 0.8rem;
    color: #64748b;
  }
</style>
