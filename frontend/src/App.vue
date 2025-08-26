<script setup>
import { ref, onMounted } from 'vue';
import VoteCard from './components/VoteCard.vue';

// refを使ってリアクティブなデータを作成
const battleData = ref(null);
const isLoading = ref(true);
const votedPlayer = ref(null);

// コンポーネントがマウントされた時に実行
onMounted(() => {
  // モックデータ (1.5秒後にデータを取得)
  setTimeout(() => {
    battleData.value = {
      battleId: "2025-08-21_Semi-final_2100",
      battleDate: "2025-08-21",
      REDCorner: {
        id: 1,
        playerName: "NaPoM",
        voted: 0,
      },
      BLUECorner: {
        id: 2,
        playerName: "WING",
        voted: 0,
      },
      round: "Semi-final",
      startTime: new Date("2025-08-21T21:00:00.000Z"),
      finishTime: new Date("2025-08-21T22:00:00.000Z"),
      battleImg: "./src/img/NaPoMvsWING.jpg",
      isViewScreen: true,
    };
    isLoading.value = false;
  }, 1500);
});

// 子コンポーネントから'vote'イベントを受け取った時の処理
const handleVote = (votedPlayerId) => {
  const player = votedPlayerId === 1
    ?battleData.value.REDCorner
    :battleData.value.BLUECorner;

  votedPlayer.value = player;
  console.log(`投票を受け取りました！ \nPlayer ID: ${votedPlayerId}\nPlayer Name: ${votedPlayer.value.playerName}`);
  // ここでバックエンドに投票結果を送信するAPIを叩く処理を書く
  // useFetch
};
</script>

<template>
  <div class="app-container">
    <div v-if="isLoading">
      <p>データを読み込み中...</p>
    </div>
    <div v-else>
      <div v-if="votedPlayer" class="voted-screen">
        <div 
          class="votedMessageBox"
          :class="{
            'red-bg': votedPlayer.id ===1,
            'blue-bg': votedPlayer.id === 2
          }"
        >
          <div 
            class="votedMessage"
            :class="{
              'red-bg': votedPlayer.id ===1,
              'blue-bg': votedPlayer.id === 2
            }"
          > 
            You voted to<br><span class="votedPlayerName">{{ votedPlayer.playerName }}</span>
          </div>
        </div>
      </div>
      <template v-else-if="battleData">
        <VoteCard
          v-if="battleData.isViewScreen"
          :battle-info="battleData"
          @vote="handleVote"
        />
        <div v-else class="waiting-screen">
          <p>Wait until game started...</p>
        </div>
      </template>
    </div>
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

  border: 8px solid white;
  box-sizing: border-box;
}

.waiting-screen {
  font-family: 'Inria Serif', serif;
  font-size: 1.5rem;
  text-align: center;
}
.votedMessageBox {
  width:280px;
  height:110px;
  background: #bbb;
  border-radius:55px;

  display: flex;
  justify-content: center;
  align-items: center;
}
.red-bg {
  background-color: #ff7f7f;
  color: #000;
}
.blue-bg {
  background-color: #6495ed;
  color: #fff;
}
.votedMessage {
  font-family: 'Inria Serif', serif;
  font-size: 2rem;
  text-align: center;
}
.votedPlayerName {
  font-family: 'Inria Serif', serif;
  font-style: italic;
  font-size: 3rem;
  text-align: center;
}
</style>