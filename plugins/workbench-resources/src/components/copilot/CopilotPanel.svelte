<!--
// Copyright © 2026 Optale
//
// Main copilot side panel widget.
// Registers as a Widget in Huly's sidebar system.
// UI shell only — backend connection (WebSocket to copilot bridge) will be wired separately.
-->
<script lang="ts">
  import type { Widget } from '@hcengineering/workbench'
  import { Label, ScrollBox } from '@hcengineering/ui'
  import CopilotMessage from './CopilotMessage.svelte'
  import CopilotInput from './CopilotInput.svelte'
  import workbench from '../../plugin'

  export let widget: Widget | undefined = undefined
  export let height: string = '100%'
  export let width: string = '100%'

  interface Message {
    id: string
    role: 'user' | 'assistant'
    content: string
  }

  let messages: Message[] = []
  let sending: boolean = false

  function handleSend (event: CustomEvent<string>): void {
    const text = event.detail
    const userMsg: Message = {
      id: crypto.randomUUID(),
      role: 'user',
      content: text
    }
    messages = [...messages, userMsg]

    // Placeholder: echo back a stub response
    // Real implementation will connect to copilot bridge WebSocket
    sending = true
    setTimeout(() => {
      const assistantMsg: Message = {
        id: crypto.randomUUID(),
        role: 'assistant',
        content: 'Copilot bridge not connected yet. Backend wiring is pending architecture review.'
      }
      messages = [...messages, assistantMsg]
      sending = false
    }, 300)
  }
</script>

<div class="copilot-panel" style:height style:width>
  <div class="copilot-panel__header">
    <Label label={workbench.string.Copilot} />
  </div>

  <div class="copilot-panel__messages">
    {#if messages.length === 0}
      <div class="copilot-panel__empty">
        <div class="copilot-panel__empty-icon">
          <svg width="32" height="32" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="16" cy="16" r="14" stroke="currentColor" stroke-width="1.5" stroke-opacity="0.2"/>
            <circle cx="16" cy="16" r="6" fill="rgba(250, 158, 0, 0.3)"/>
            <circle cx="16" cy="16" r="2" fill="rgba(250, 158, 0, 0.8)"/>
          </svg>
        </div>
        <span class="copilot-panel__empty-text">
          Ask anything about your workspace
        </span>
      </div>
    {:else}
      <ScrollBox vertical>
        <div class="copilot-panel__message-list">
          {#each messages as msg (msg.id)}
            <CopilotMessage role={msg.role} content={msg.content} />
          {/each}
          {#if sending}
            <div class="copilot-panel__typing">
              <span class="copilot-panel__typing-dot" />
              <span class="copilot-panel__typing-dot" />
              <span class="copilot-panel__typing-dot" />
            </div>
          {/if}
        </div>
      </ScrollBox>
    {/if}
  </div>

  <CopilotInput disabled={sending} on:send={handleSend} />
</div>

<style lang="scss">
  .copilot-panel {
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  .copilot-panel__header {
    display: flex;
    align-items: center;
    padding: 0.75rem;
    font-size: 0.8125rem;
    font-weight: 600;
    color: var(--theme-content-color, rgba(255, 255, 255, 0.8));
    border-bottom: 1px solid var(--theme-divider-color, rgba(255, 255, 255, 0.06));
    flex-shrink: 0;
  }

  .copilot-panel__messages {
    flex: 1;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }

  .copilot-panel__message-list {
    padding: 0.5rem;
  }

  .copilot-panel__empty {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100%;
    gap: 0.75rem;
    color: var(--theme-halfcontent-color, rgba(255, 255, 255, 0.3));
  }

  .copilot-panel__empty-icon {
    opacity: 0.6;
  }

  .copilot-panel__empty-text {
    font-size: 0.8125rem;
    text-align: center;
    max-width: 12rem;
    line-height: 1.4;
  }

  .copilot-panel__typing {
    display: flex;
    align-items: center;
    gap: 0.25rem;
    padding: 0.75rem;
  }

  .copilot-panel__typing-dot {
    width: 4px;
    height: 4px;
    border-radius: 50%;
    background-color: rgba(250, 158, 0, 0.5);
    animation: copilot-typing 1.2s ease-in-out infinite;

    &:nth-child(2) {
      animation-delay: 0.2s;
    }
    &:nth-child(3) {
      animation-delay: 0.4s;
    }
  }

  @keyframes copilot-typing {
    0%, 60%, 100% {
      opacity: 0.3;
      transform: scale(1);
    }
    30% {
      opacity: 1;
      transform: scale(1.2);
    }
  }
</style>
