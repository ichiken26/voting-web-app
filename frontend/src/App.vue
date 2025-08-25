<script setup>
import { ref, onMounted } from 'vue';
import VoteCard from './components/VoteCard.vue';

// refを使ってリアクティブなデータを作成
const battleData = ref(null);
const isLoading = ref(true);

// コンポーネントがマウントされた時に実行
onMounted(() => {
  // 擬似的なAPIコール (1.5秒後にデータを取得)
  setTimeout(() => {
    battleData.value = {
      battleId: "2025-08-21_Semi-final_2100",
      battleDate: "2025-08-21",
      REDCourner: {
        id: 1,
        playerName: "NaPoM",
        voted: 0,
      },
      BLUECourner: {
        id: 2,
        playerName: "WING",
        voted: 0,
      },
      round: "Semi-final",
      startTime: new Date("2025-08-21T21:00:00.000Z"),
      finishTime: new Date("2025-08-21T22:00:00.000Z"),
      battleImg: "../img/NaPoMvsWING.jpg"
    };
    isLoading.value = false;
  }, 1500);
});

// 子コンポーネントから'vote'イベントを受け取った時の処理
const handleVote = (votedPlayerId) => {
  console.log(`投票を受け取りました！ Player ID: ${votedPlayerId}`);
  // ここでバックエンドに投票結果を送信するAPIを叩く処理を書く
  alert(`${votedPlayerId === 1 ? battleData.value.REDCourner.playerName : battleData.value.BLUECourner.playerName} に投票しました！`);
};
</script>

<template>
  <div class="app-container">
    <div v-if="isLoading">
      <p>データを読み込み中...</p>
    </div>
    <VoteCard
      v-else-if="battleData"
      :battle-info="battleData"
      @vote="handleVote"
    />
  </div>
</template>

<style scoped>
.app-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #000;
  color: #fff;
}
</style>