<!--
// Copyright © 2026 Optale
//
// Copilot message bubble component.
// Renders user and assistant messages in the copilot panel.
-->
<script lang="ts">
  export let role: 'user' | 'assistant' = 'assistant'
  export let content: string = ''
</script>

<div class="copilot-message" class:user={role === 'user'} class:assistant={role === 'assistant'}>
  <div class="copilot-message__role">
    {role === 'user' ? 'You' : 'Copilot'}
  </div>
  <div class="copilot-message__content">
    {content}
  </div>
</div>

<style lang="scss">
  .copilot-message {
    padding: 0.5rem 0.75rem;
    margin: 0.25rem 0;
    border-radius: 0.5rem;
    font-size: 0.8125rem;
    line-height: 1.4;

    &.user {
      background-color: rgba(250, 158, 0, 0.08);
    }

    &.assistant {
      background-color: var(--theme-popup-color, rgba(255, 255, 255, 0.04));
    }
  }

  .copilot-message__role {
    font-size: 0.6875rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.04em;
    color: var(--theme-halfcontent-color, rgba(255, 255, 255, 0.4));
    margin-bottom: 0.25rem;
  }

  .copilot-message__content {
    color: var(--theme-content-color, rgba(255, 255, 255, 0.8));
    white-space: pre-wrap;
    word-break: break-word;
  }
</style>
