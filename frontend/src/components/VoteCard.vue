<script setup>
import { ref } from 'vue';

const props = defineProps({
  battleInfo: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['vote']);

// どのプレイヤーが選択されたかを管理するローカルなstate
const selectedPlayerId = ref(null);

// 投票ボタンがクリックされたときに呼ばれる関数
const voteForPlayer = (player) => {
  // 確認ダイアログ
  const isConfirmed = window.confirm(`${player.playerName}に投票しますか？`);
  
  // OKが押されたときの処理、キャンセル時には何もしない
  if (isConfirmed) {
    selectedPlayerId.value = player.id; // 見た目のために選択状態を更新
    emit('vote', player.id); // 親コンポーネントに'vote'イベントを通知
  }
};
</script>

<template>
  <div class="container">
    <header class="title">
      <h1>BeatBox Battle</h1>
      <h2><span class="round">Round: </span><br>{{ battleInfo.round }}</h2>
    </header>

    <div class="image-placeholder">
      <img :src="battleInfo.battleImg" alt="Battle Image" class="battle-image-style">
    </div>

    <div class="vote-section">
      <button
        @click="voteForPlayer(battleInfo.REDCorner)"
        class="vote-btn player1"
        :class="{ selected: selectedPlayerId === battleInfo.REDCorner.id }"
      >
        {{ battleInfo.REDCorner.playerName }}
      </button>

      <p class="vote-text">↑ Vote to WINNER ↓</p>

      <button
        @click="voteForPlayer(battleInfo.BLUECorner)"
        class="vote-btn player2"
        :class="{ selected: selectedPlayerId === battleInfo.BLUECorner.id }"
      >
        {{ battleInfo.BLUECorner.playerName }}
      </button>
    </div>
  </div>
</template>

<style scoped>
/* 基本設定 */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
    width: 90%;
    max-width: 400px;
}

/* タイトル部分 */
.title h1,
.title h2{
    font-family: 'Inria Serif';
    font-style: italic;
    text-align: center;
    line-height: 1.2;
}

.title h1 {
    font-size: 2.5rem;
}

.title h2 {
    font-size: 2rem;
    margin-bottom: 10px;
}

.span.round {
  text-align: left;
}

.vote-text {
  font-size: 1rem;
  font-family: 'Inria Serif';
  font-style: italic;
  text-align: center;
  color: #fff;
}

/* 画像のプレースホルダー */
.image-placeholder {
    width: 100%;
    height: 200px;
    background-color: #000;
    
    /* 子要素を絶対位置で配置するための基準点 */
    position: relative; 
}

/* プレースホルダーの中の画像自体のスタイル */
.battle-image-style {
    /* 親要素を基準に絶対位置で配置 */
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%); /* 上下左右中央揃えのテクニック */
    
    /* 幅も高さも、親の箱のサイズを超えないようにする */
    max-width: 100%;
    max-height: 100%;
}

/* 投票セクション */
.vote-section {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 15px;
}

/* 投票ボタンの共通スタイル */
.vote-btn {
    width: 100%;
    padding: 20px;
    border-radius: 30px;
    border: none;
    font-size: 1.5rem;
    font-weight: bold;
    cursor: pointer;
    transition: transform 0.2s, box-shadow 0.2s;
}

.vote-btn:hover {
    transform: scale(1.02);
}

.player1 {
    background-color: #ff7f7f;
    color: #000;
}

.player2 {
    background-color: #6495ed;
    color: #fff;
}

.vote-text {
    font-size: 1rem;
    color: #aaa;
}

/* 選択されたボタンのスタイル */
.selected {
    box-shadow: 0 0 20px 5px #fff;
    transform: scale(1.05);
}
</style>