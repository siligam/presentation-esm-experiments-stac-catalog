<template>
  <div class="item-card" :class="size === 'sm' && 'item-card--sm'">
    <span v-if="title && !inlineTitle" class="item-card-title">{{ title }}</span>
    <div v-if="$slots.stat || (title && inlineTitle)" class="item-card-header">
      <span v-if="title && inlineTitle" class="item-card-header-label" :class="`item-card-stat--${variant}`">{{ title }}</span>
      <div class="item-card-stat" :class="[`item-card-stat--${variant}`, size === 'sm' && 'item-card-stat--sm']">
        <slot name="stat" />
      </div>
    </div>
    <div class="item-card-desc">
      <slot />
    </div>
  </div>
</template>

<script setup>
defineProps({
  title: { type: String, default: '' },
  variant: { type: String, default: 'default' }, // 'default' | 'success' | 'danger'
  inlineTitle: { type: Boolean, default: false },
  size: { type: String, default: 'default' } // 'default' | 'sm'
})
</script>

<style>
.item-card {
  border: 1px solid color-mix(in srgb, var(--slidev-theme-primary, currentColor) 30%, transparent);
  border-radius: 12px;
  padding: 1.25rem;
  background: color-mix(in srgb, var(--slidev-theme-primary, currentColor) 8%, transparent);
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  transition: box-shadow 0.2s ease, border-color 0.2s ease;
}

.item-card:hover {
  border-color: color-mix(in srgb, var(--slidev-theme-primary, currentColor) 60%, transparent);
  box-shadow: 0 4px 24px color-mix(in srgb, var(--slidev-theme-primary, currentColor) 15%, transparent);
}

.item-card-title {
  font-size: 0.7rem;
  font-style: italic;
  letter-spacing: 0.04em;
  opacity: 0.5;
  margin-bottom: 0.1rem;
}

.item-card-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.5rem;
  margin-bottom: 0.25rem;
}

.item-card-header-label {
  font-size: 1.1rem;
  font-weight: 600;
  letter-spacing: 0.02em;
}

.item-card-stat {
  font-size: 1.4rem;
  font-weight: 700;
  line-height: 1.2;
  color: var(--slidev-theme-primary, currentColor);
}

.item-card--sm { padding: 0.75rem 1rem; }
.item-card-stat--sm { font-size: 1.1rem; }
.item-card-stat--success { color: #16a34a; }
.item-card-stat--danger  { color: #dc2626; }
.item-card-stat--neutral { color: #6b7280; }

.item-card-desc {
  font-size: 0.9rem;
  line-height: 1.5;
  color: inherit;
  opacity: 0.75;
}

.item-card-desc ul {
  list-style: disc;
  padding-left: 1em;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}
</style>
