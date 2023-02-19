<script setup>
import { computed, reactive } from 'vue';
import {
  createCompletion, newClient, PARTICIPANT_AI, PARTICIPANT_HUMAN, FINISH_REASON_LENGTH,
} from '../services/openai';
import {
  MOOD_TYPES,
  FORMAT_TYPES,
  LENGTH_TYPES,
} from '../constants';

const data = reactive({
  loading: false,
  key: atob(localStorage.getItem('key') || ''),
  topic: localStorage.getItem('topic') || '',
  moodTypes: [MOOD_TYPES[0]],
  formatType: FORMAT_TYPES[0],
  lengthType: LENGTH_TYPES[0],
  result: '',
  finishReason: '',
  error: '',
});

const rememberTopic = () => {
  localStorage.setItem('topic', data.topic);
};

const rememberKey = () => {
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

const prompt = computed(() => `使用「${data.topic}」為題材，以「${data.moodTypes.map(({ value }) => value).join('、')}」的語氣，生成一段不超過 ${data.lengthType.value} 字的${data.formatType.text}。`);

const resetContent = () => {
  data.result = '';
  data.finishReason = '';
};

const generateContent = async ({ reset = true }) => {
  data.loading = true;
  if (reset) resetContent();
  const client = newClient(data.key);
  try {
    const res = await createCompletion(client)({
      prompt: `${PARTICIPANT_AI}: 我會使用繁體中文。\n${PARTICIPANT_HUMAN}: ${prompt.value}\n${PARTICIPANT_AI}: ${data.result}`,
      maxTokens: data.lengthType.value * 4,
    });
    const { choices } = res.data;
    const [choice] = choices;
    const { text, finish_reason: finishReason } = choice;
    data.result += text.trim();
    data.finishReason = finishReason;
  } catch (err) {
    data.error = err?.response?.data?.error?.message || err.message;
  }
  data.loading = false;
};
</script>

<template>
  <v-snackbar
    v-if="data.error"
    color="indigo-lighten-1"
    model-value
    @update:modelValue="data.error = ''"
  >
    {{ data.error }}
  </v-snackbar>
  <v-row class="justify-center column">
    <v-col cols="12" sm="10" md="6">
      <v-card
        color="blue-grey-lighten-5"
        height="100%"
      >
        <v-card-item class="pa-8 pb-4">
          <div class="text-h5 mb-8 font-weight-bold text-indigo">
            生成參數
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
                @input="rememberTopic"
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
                @input="rememberKey"
              />
            </div>
          </div>
        </v-card-item>
        <v-card-actions class="justify-end pa-8 pt-0">
          <v-btn
            block
            color="indigo"
            variant="outlined"
            @click="generateContent"
          >
            生成
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
    <v-col cols="12" sm="10" md="6">
      <v-card
        color="blue-grey-lighten-5"
        height="100%"
      >
        <v-card-item class="pa-8 pb-4">
          <div class="text-h5 mb-8 font-weight-bold text-indigo">
            生成結果
          </div>
          <div class="my-4">
            <div class="title mb-2">
              預覽
            </div>
            <div>
              <v-sheet
                v-if="data.loading"
                color="blue-grey-lighten-5"
                height="400"
                class="d-flex justify-center align-center"
              >
                <v-progress-circular
                  color="indigo"
                  indeterminate
                  size="75"
                  class="my-12"
                />
              </v-sheet>
              <v-textarea
                v-else
                v-model="data.result"
                color="indigo"
                hide-details
                no-resize
                rows="15"
                variant="outlined"
                style="height: 400px"
              />
            </div>
          </div>
        </v-card-item>
        <v-card-actions class="justify-end pa-8 pt-0">
          <v-btn
            v-if="data.finishReason === FINISH_REASON_LENGTH"
            block
            color="indigo"
            variant="outlined"
            @click="generateContent({ reset: false })"
          >
            繼續
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>
