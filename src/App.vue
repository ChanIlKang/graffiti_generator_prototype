<script setup>
import { ref } from 'vue'

// assets 폴더 안에 모든 라이터 스타일 폴더를 glob으로 가져오기
const images = import.meta.glob('@/assets/*/*.png', { eager: true, import: 'default' })

const textInput = ref('')
const graffitiImages = ref([])

const styles = ['style1', 'style2', 'style3', 'style4'] // 스타일 이름 = 라이터명
const selectedStyle = ref('style1') // 기본 선택값

const generateGraffiti = () => {
  graffitiImages.value = []

  const letters = textInput.value.toUpperCase().split('')
  letters.forEach(letter => {
    if (/[A-Z]/.test(letter)) {
      const imagePath = `/src/assets/${selectedStyle.value}/${letter}.png`
      const matchedImage = images[imagePath]
      if (matchedImage) {
        graffitiImages.value.push(matchedImage)
      }
    }
  })
}
</script>

<template>
  <div class="app">
    <header class="header">
      <img src="./assets/logo_white.png" alt="logo">
    </header>

    <main class="main">
      <section class="controls">
        <h2>Create Your Graffiti</h2>

        <div class="style_select_wrapper">
          <button v-for="styles in styles" :key="styles" :class="{'selected' : selectedStyle === styles}" @click="selectedStyle = styles">{{styles}}</button>
        </div>

        <input
          v-model="textInput"
          type="text"
          placeholder="Enter your text (only A-Z)"
          class="input"
          @keyup.enter="generateGraffiti"
        />

        <button @click="generateGraffiti" class="generate-button">
          Create Graffiti
        </button>
      </section>

      <section class="preview">
        <h2>Preview</h2>

        <div v-if="selectedStyle === 'style1'" class="graffiti-output">
          <img src="./assets/accesories/angel_ring.png" alt="angel_ring" class="angel-ring" />

          <div class="graffiti-output-text" :class="selectedStyle">
            <img src="./assets/accesories/quote_0.png" alt="quote" class="graffiti-image quote" :class="selectedStyle" :style="{ width: `${65 / graffitiImages.length + 2}%` }" />
            <img v-for="(gi, idx) in graffitiImages" :key="gi" alt="graffitiImages" class="graffiti-image" :class="selectedStyle" :src="gi" :style="{ width: `${65 / graffitiImages.length + 2}%`, zIndex: graffitiImages.length - idx }"/>
            <img src="./assets/accesories/quote_1.png" alt="quote" class="graffiti-image quote" :class="selectedStyle" :style="{ width: `${65 / graffitiImages.length + 2}%` }" />
          </div>
        </div>

        <div v-else>
          <div class="graffiti-output" :class="selectedStyle">
            <img v-for="(gi, idx) in graffitiImages" alt="graffitiImages" :key="gi" class="graffiti-image" :class="selectedStyle" :src="gi" :style="{zIndex: graffitiImages.length - idx}" />
          </div>
        </div>
      </section>
    </main>

    <footer class="footer">
      Made by DOMA, graffiti writer and developer
    </footer>
  </div>
</template>

<style scoped lang="scss">
.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: #1e1e1e;
  color: #fff;
  font-family: 'Arial', sans-serif;
}

.header {
  padding: 1rem;
  text-align: center;
  background-color: #111;
  font-size: 2rem;
  font-weight: bold;
  color: #ff4081;

  img {
    max-width: 1080px;
  }
}

.main {
  flex: 1;
  display: flex;
  padding: 2rem;
  gap: 2rem;
}

.controls {
  flex: 1;
  background-color: #292929;
  padding: 2rem;
  border-radius: 10px;

  h2 {
    margin-bottom: 1rem;
  }

  .input, .select {
    width: 100%;
    padding: 0.75rem;
    margin-bottom: 1rem;
    border: none;
    border-radius: 5px;
  }

  .style_select_wrapper {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    gap: 12px;

    button {
      background-color: #ff4081;
      border: 1px solid #ff4081;
      border-radius: 4px;
      color: white;
      padding: 4px 8px;
      font-size: 20px;
      font-weight: 600;
      margin-bottom: 12px;
      cursor: pointer;
      transition: all 0.2s ease;

      &:hover {
        background-color: #d2346a;
        transition: all 0.2s ease;
      }

      &.selected {
        background-color: white;
        color: #ff4081;
        transition: all 0.2s ease;
      }
    }
  }

  .generate-button {
    width: 100%;
    padding: 1rem;
    background-color: #ff4081;
    border: none;
    border-radius: 5px;
    font-weight: bold;
    color: white;
    cursor: pointer;
    transition: background-color 0.3s;

    &:hover {
      background-color: #ff79a8;
    }
  }
}

.preview {
  flex: 2;
  background-color: #292929;
  padding: 2rem;
  border-radius: 10px;
  text-align: center;

  h2 {
    margin-bottom: 1rem;
  }

  .graffiti-output {
    border-radius: 12px;
    background-color: #f9f9f9;
    max-width: 1130px;
    padding: 20px;

    .angel-ring {
      height: 150px; /* 기본 이미지 높이 */
      flex-shrink: 0; /* 줄어들지 않게 */
      object-fit: contain;
    }
    .graffiti-output-text {
      display: flex;
      flex-wrap: nowrap;
      justify-content: center;
      align-items: center;
      padding: 10px;
    }
  }

  .graffiti-image {
    position: relative;
    width: 150px; /* 기본 이미지 높이 */
    flex-shrink: 1; /* 줄어들지 않게 */
    object-fit: contain;
    margin-left: -0.1%;
  }

  /* 선택한 스타일(doma, cent 등)에 따라 다르게 꾸미고 싶으면 추가 가능 */
  //.graffiti-output.doma { background-color: #ffffff; }
  //.graffiti-output.cent { background-color: #ffffff; }
  //.graffiti-output.wezt { background-color: #ffffff; }
  //.graffiti-output.seach { background-color: #ffffff; }
}

/* 스타일 별 효과 (아주 간단한 예시) */
.Wild\ Style {
  font-style: italic;
  color: lime;
}

.Bubble\ Style {
  color: cyan;
  letter-spacing: 5px;
}

.Throw\ Up {
  font-weight: bold;
  color: magenta;
}

.Blockbuster {
  color: orange;
  font-weight: bolder;
}

.footer {
  text-align: center;
  padding: 1rem;
  background-color: #111;
  font-size: 0.9rem;
  color: #aaa;
}

/* 모바일 (최대 767px) */
@media (max-width: 767px) {
  .main {
    flex-direction: column;
    padding: 1rem;
  }

  .controls, .preview {
    padding: 1rem;
  }

  .style_select_wrapper {
    grid-template-columns: 1fr 1fr; // 버튼 2개씩
  }

  .graffiti-output {
    padding: 10px;

    .angel-ring {
      height: 100px; // angel ring 작게
    }
  }

  .graffiti-image {
    width: 80px !important; // 이미지 작게
    margin-left: -3% !important; // 겹치는 정도 키움
  }

  .generate-button {
    padding: 0.75rem;
  }

  .header img {
    max-width: 90%; // 로고 크기 줄이기
  }
}

/* 태블릿 (768px ~ 1024px) */
@media (min-width: 768px) and (max-width: 1024px) {
  .main {
    flex-direction: column;
    padding: 1.5rem;
  }

  .controls, .preview {
    padding: 1.5rem;
  }

  .style_select_wrapper {
    grid-template-columns: 1fr 1fr 1fr; // 버튼 3개씩
  }

  .graffiti-output {
    padding: 15px;

    .angel-ring {
      height: 120px;
    }
  }

  .graffiti-image {
    width: 120px;
    margin-left: -3%;
  }

  .header img {
    max-width: 95%;
  }
}

</style>
