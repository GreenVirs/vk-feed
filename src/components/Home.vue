<script setup lang="ts">
import {computed, ref} from "vue";
import {useRoute} from "vue-router";
const route = useRoute();
const token = ref('vk1.a.w_ijoQS9GjkVvUta_IdClcOdpCW86Yt2C6X5uVqcnmuEvk9r7NPnetjFxGntdo7AR6nzTH5sbe30qf8P2VF9Xn5HexJeK0LMtuzRdNVhDmiwPUlqCwxkuTS1CBrh9Z1uUZ2Q5CDn3COb3i58d5B2lAwKnucC_AiB98-IliaB-EPqmgNRcTXMLsGmuhByjkeyI68DkGnIb0x8Jo8vJuKbdQ');
const clientID = '51516758';
const secret = 'z1mbZHCgbEjW0DJd1SRr';
const link = computed(() => {
  return `https://oauth.vk.com/authorize?client_id=${clientID}&display=page&redirect_uri=${encodeURI('http://localhost:5173')}&scope=groups,market&response_type=token&revoke=1`
});

const groupToken = 'vk1.a.IH9qZZU3mIk2ir5xOcQhnrkJh0s34fdEQCG0dSNu5X-hEZUlTjdXO9MQjCLvJJlHMZCPHgN2zxSu4OS7hZU9UxqZzpPmW0-WYFXRvIXKWlYRUtpCRVgtp3A8NGD9x4lDXeJu3EvYn5j5hPHgTzRGErYNTLCwjSrnM3YvUT07IEKfeyOZpvpnvfRzD9K9CG7RfpNCQU2Mf4QFUqCSnDJr-w'
const createLink = (id) => {
  return `https://oauth.vk.com/authorize?client_id=${clientID}&display=page&redirect_uri=${encodeURI('http://localhost:5173')}&group_ids=${id}&scope=groups,market&response_type=token&revoke=1`
}

const groups = ref([]);
const head = document.head;
const load = () => new Promise((resolve, reject) => {
  const scr = document.createElement('script');
  scr.onload = resolve;
  scr.onerror = reject;
  scr.src = `https://api.vk.com/method/groups.get?filter=admin&access_token=${token.value}&extended=1&v=5.3`
  head.appendChild(scr);
})
const loadGroups = async () => {
  const data = await fetch(`/api/groups.get?filter=admin,editor&access_token=${token.value}&extended=1&v=5.131`);
  console.log('data.ok', data.ok);
  if (!data.ok) {
    return
  }
  const body = await data.json();
  console.log('body', body);
  groups.value = body.response.items;
}

const loadSettings = async (id) => {
  const data = await fetch(`/api/groups.setSettings?access_token=${groupToken}&group_id=${id}&v=5.131`);
  console.log('data.ok', data.ok);
  if (!data.ok) {
    return
  }
  const body = await data.json();
  console.log('body', body);
}

</script>

<template>
  <div>
    <a :href="link">Авторизация</a>
    <button v-if="token !== null" @click="loadGroups"></button>
    <ul>
      <li v-for="item in groups" :key="item.id">
        <button @click="loadSettings(item.id)">{{ item.name }}</button>
      </li>
    </ul>
  </div>
</template>
