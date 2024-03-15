<template>
  <div class="app-container">
    <textarea v-model="tankaInput" placeholder="ここに短歌を入力してください"></textarea>
    <div class="tanka-container" ref="tankaContainer">
      <div class="tanka" v-html="formattedTanka"></div>
    </div>
    <button @click="exportToImage">画像保存</button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import html2canvas from 'html2canvas';

const tankaInput = ref('');

// 長音記号とアルファベットを縦書き表示に適した形に変換する
const formattedTanka = computed(() => {
  // TODO: アルファベットも縦書き表示とする
  return tankaInput.value.replace(/ー/g, '<span class="vertical-long-sound">ー</span>');
});

const exportToImage = async () => {
  const tankaContainer = document.querySelector('.tanka-container');
  if (!tankaContainer) return;

  const canvas = await html2canvas(tankaContainer);
  const image = canvas.toDataURL('image/png');
  const link = document.createElement('a');
  link.href = image;
  link.download = 'tanka.png';
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  justify-content: center;
}
.tanka-container {
  width: 1024px;
  height: 1280px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f9f9f9; /* 背景色の例 */
  margin-bottom: 20px;
    /* レスポンシブデザインに適したスタイル設定 */
  max-width: 100%;
  margin: 0 auto;
  background-color: #f9f9f9; /* 背景色の例 */
}
.tanka {
  writing-mode: vertical-rl;
  text-orientation: upright;
  font-size: 24px;
  line-height: 1.6;
  text-align: center; /* 中央揃えでテキストを表示 */
}
textarea {
  height: 120px; /* テキストエリアの高さを調整 */
  margin-bottom: 20px;
  width: 100%;
  max-width: 500px; /* 最大幅を指定 */
  height: 120px;
  margin: 20px 0;
}

.vertical-long-sound {
  writing-mode: vertical-rl;
}

.vertical-orientation {
  writing-mode: vertical-rl;
  text-orientation: upright;
}

/* レスポンシブデザイン用のメディアクエリ */
@media (max-width: 600px) {
  .tanka-container {
    margin: 0 10px;
  }
  textarea {
    margin: 10px 0;
  }
}
</style>
