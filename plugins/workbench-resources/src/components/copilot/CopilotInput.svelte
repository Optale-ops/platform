<!--
// Copyright © 2026 Optale
//
// Copilot input component.
// Text input with send button for the copilot panel.
-->
<script lang="ts">
  import { createEventDispatcher } from 'svelte'
  import { Button, IconArrowRight } from '@hcengineering/ui'

  export let disabled: boolean = false

  const dispatch = createEventDispatcher<{ send: string }>()

  let message: string = ''

  function handleSend (): void {
    const text = message.trim()
    if (text.length === 0 || disabled) return
    dispatch('send', text)
    message = ''
  }

  function handleKeydown (event: KeyboardEvent): void {
    if (event.key === 'Enter' && !event.shiftKey) {
      event.preventDefault()
      handleSend()
    }
  }
</script>

<div class="copilot-input">
  <textarea
    class="copilot-input__field"
    bind:value={message}
    on:keydown={handleKeydown}
    placeholder="Ask the copilot..."
    rows="1"
    {disabled}
  />
  <div class="copilot-input__actions">
    <Button
      icon={IconArrowRight}
      kind="ghost"
      size="small"
      disabled={disabled || message.trim().length === 0}
      on:click={handleSend}
    />
  </div>
</div>

<style lang="scss">
  .copilot-input {
    display: flex;
    align-items: flex-end;
    gap: 0.25rem;
    padding: 0.5rem 0.75rem;
    border-top: 1px solid var(--theme-divider-color, rgba(255, 255, 255, 0.06));
  }

  .copilot-input__field {
    flex: 1;
    background: var(--theme-popup-color, rgba(255, 255, 255, 0.04));
    border: 1px solid var(--theme-divider-color, rgba(255, 255, 255, 0.08));
    border-radius: 0.375rem;
    padding: 0.5rem 0.625rem;
    color: var(--theme-content-color, rgba(255, 255, 255, 0.8));
    font-size: 0.8125rem;
    line-height: 1.4;
    resize: none;
    outline: none;
    font-family: inherit;
    min-height: 2.25rem;
    max-height: 6rem;
    overflow-y: auto;

    &::placeholder {
      color: var(--theme-halfcontent-color, rgba(255, 255, 255, 0.3));
    }

    &:focus {
      border-color: rgba(250, 158, 0, 0.4);
    }
  }

  .copilot-input__actions {
    flex-shrink: 0;
    display: flex;
    align-items: center;
  }
</style>
