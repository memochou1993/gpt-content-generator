<script setup>
import { computed, reactive } from 'vue';

const MOOD_TYPES = [
  { text: '專業', value: '專業' },
  { text: '有趣', value: '有趣' },
  { text: '嚴肅', value: '嚴肅' },
];

const FORMAT_TYPES = [
  { text: '段落', value: '段落' },
  { text: '郵件', value: '郵件' },
  { text: '文章', value: '文章' },
  { text: '詩詞', value: '詩詞' },
];

const LENGTH_TYPES = [
  { text: '短', value: '100' },
  { text: '中', value: '250' },
  { text: '長', value: '500' },
];

const data = reactive({
  key: atob(localStorage.getItem('key') || ''),
  topic: '',
  moodTypes: [MOOD_TYPES[0]],
  formatType: FORMAT_TYPES[0],
  lengthType: LENGTH_TYPES[0],
});

const updateKey = () => {
  localStorage.setItem('key', btoa(data.key));
};

const selectMoodType = (moodType) => {
  data.moodTypes = data.moodTypes.map(({ text }) => text).includes(moodType.text)
    ? data.moodTypes.filter(({ text }) => text !== moodType.text)
    : [...data.moodTypes, moodType];
};

const selectFormatType = (formatType) => {
  data.formatType = formatType;
};

const selectLengthType = (lengthType) => {
  data.lengthType = lengthType;
};

const promptMoods = computed(() => data.moodTypes.map(({ value }) => value).join('、'));
const prompt = computed(() => `使用「${data.topic}」為題材，以「${promptMoods.value}」的語氣，生成約 ${data.lengthType.value} 字的${data.formatType.text}。`);

const generate = () => {
  console.log(prompt.value);
};
</script>

<template>
  <v-row class="justify-center column">
    <v-col cols="12" sm="10" md="6">
      <v-card
        color="blue-grey-lighten-5"
        height="100%"
      >
        <v-card-item class="pa-8">
          <div class="text-h5 mb-8 font-weight-bold text-indigo">
            文案生成器
          </div>
          <div class="my-4">
            <div class="title mb-2">
              題材
            </div>
            <div>
              <v-textarea
                v-model="data.topic"
                autofocus
                color="indigo"
                hide-details
                no-resize
                rows="3"
                variant="outlined"
              />
            </div>
          </div>
          <div class="my-4">
            <div class="title mb-2">
              語氣
            </div>
            <div>
              <v-btn
                v-for="(moodType) in MOOD_TYPES"
                :key="moodType.text"
                :active="data.moodTypes.map(({ text }) => text).includes(moodType.text)"
                color="indigo"
                variant="outlined"
                class="mr-2 mb-2"
                @click="selectMoodType(moodType)"
              >
                {{moodType.text}}
              </v-btn>
            </div>
          </div>
          <div class="my-4">
            <div class="title mb-2">
              格式
            </div>
            <div>
              <v-btn
                v-for="(formatType) in FORMAT_TYPES"
                :key="formatType.text"
                :active="data.formatType.text === formatType.text"
                color="indigo"
                variant="outlined"
                class="mr-2 mb-2"
                @click="selectFormatType(formatType)"
              >
                {{formatType.text}}
              </v-btn>
            </div>
          </div>
          <div class="my-4">
            <div class="title mb-2">
              長度
            </div>
            <div>
              <v-btn
                v-for="(lengthType) in LENGTH_TYPES"
                :key="lengthType.text"
                :active="data.lengthType.text === lengthType.text"
                color="indigo"
                variant="outlined"
                class="mr-2 mb-2"
                @click="selectLengthType(lengthType)"
              >
                {{lengthType.text}}
              </v-btn>
            </div>
          </div>
          <div class="my-4">
            <div class="title mb-2">
              金鑰
            </div>
            <div>
              <v-text-field
                v-model="data.key"
                color="indigo"
                density="compact"
                hide-details
                type="password"
                variant="outlined"
                @input="updateKey"
              />
            </div>
          </div>
        </v-card-item>
        <v-card-actions class="justify-end pa-8 pt-0">
          <v-btn
            :disabled="!data.topic || !data.key"
            block
            color="indigo"
            variant="outlined"
            @click="generate"
          >
            產生草稿
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
    <v-col cols="12" sm="10" md="6">
      <v-card
        color="blue-grey-lighten-5"
        height="100%"
      >
        <v-card-item class="pa-8">
          <div class="text-h5 mb-8 font-weight-bold text-indigo">
            生成結果
          </div>
          <div class="my-4">
            <div class="title mb-2">
              預覽
            </div>
            <div>
              <v-textarea
                color="indigo"
                hide-details
                no-resize
                rows="9"
                variant="outlined"
              />
            </div>
          </div>
        </v-card-item>
      </v-card>
    </v-col>
  </v-row>
</template>
