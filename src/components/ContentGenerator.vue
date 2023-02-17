<script setup>
//
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
              <v-btn color="indigo" active variant="outlined" class="mr-2 mb-2">
                專業
              </v-btn>
              <v-btn color="indigo" variant="outlined" class="mr-2 mb-2">
                有趣
              </v-btn>
              <v-btn color="indigo" variant="outlined" class="mr-2 mb-2">
                嚴肅
              </v-btn>
            </div>
          </div>
          <div class="my-4">
            <div class="title mb-2">
              格式
            </div>
            <div>
              <v-btn color="indigo" active variant="outlined" class="mr-2 mb-2">
                段落
              </v-btn>
              <v-btn color="indigo" variant="outlined" class="mr-2 mb-2">
                郵件
              </v-btn>
              <v-btn color="indigo" variant="outlined" class="mr-2 mb-2">
                文章
              </v-btn>
              <v-btn color="indigo" variant="outlined" class="mr-2 mb-2">
                詩詞
              </v-btn>
            </div>
          </div>
          <div class="my-4">
            <div class="title mb-2">
              長度
            </div>
            <div>
              <v-btn color="indigo" active variant="outlined" class="mr-2 mb-2">
                短
              </v-btn>
              <v-btn color="indigo" variant="outlined" class="mr-2 mb-2">
                中
              </v-btn>
              <v-btn color="indigo" variant="outlined" class="mr-2 mb-2">
                長
              </v-btn>
            </div>
          </div>
        </v-card-item>
        <v-card-actions class="justify-end pa-8 pt-0">
          <v-btn color="indigo" variant="outlined" block>
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
