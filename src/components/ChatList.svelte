<script lang="ts">
import { onMount } from "svelte";
import axios from "axios";
import { API_HOST } from "../constants";
import { goto } from '@mateothegreat/svelte5-router';
export let chatId: string | null;

let chats: { id: string; name: string }[] = [];
let errorMessage: string | null = null;

async function getData() {
  try {
    const response = await axios.get(`${API_HOST}/api/v1/chat/`);
    chats = response.data.data;
  } catch (error) {
    errorMessage = "チャットの取得に失敗しました。";
  }
}

onMount(async () => {
  await getData();
});

function selectChat(chatId: string) {
  goto(`/${chatId}`);
}
</script>

{#if errorMessage}
  <div class="error">{errorMessage}</div>
{/if}

<div class="chat-list">
  {#each chats as chat}
    <button class:selected={chat.id === chatId} onclick={() => selectChat(chat.id)}>
        {chat.name}</button>
  {/each}
</div>

<style>
button {
  margin-bottom: 10px;
  width: 100%;
}

.selected {
  background-color: #48c;
  color: white;
}

button:hover {
  background-color: #48c; /* 修正: 不正なカラーコードを補完 */
  color: white;
}
</style>
