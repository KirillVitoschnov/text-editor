<template>
  <div class="text-container">
    <b-form-group
        class="m-auto pb-2"
        v-slot="{ ariaDescribedby }"
    >
      <b-form-radio-group
          id="btn-radios-2"
          v-model="selected"
          :options="options"
          :aria-describedby="ariaDescribedby"
          button-variant="outline-primary"
          size="lg"
          name="radio-btn-outline"
          buttons
      ></b-form-radio-group>
    </b-form-group>
    <section v-if="selected==2" class="page-element">
      <h1 class="titleSection">Замена и удаление слов</h1>
      <div class="text-wrapper">
        <div class="textarea-wrap">
          <b-btn style="width: 100%;" onclick="document.getElementById('file-input').click();" class="m-3"
                 variant="outline-primary">Импортировать
          </b-btn>
          <input @change="uploadString" id="file-input" type="file" name="name" style="display: none;"/>
          <textarea
              class="textarea"
              id="textarea"
              v-model="textToChange"
              placeholder="Введите текст который нужно изменить"
              rows="3"
          ></textarea>
        </div>
        <div class="textarea-wrap" style="margin: auto 0;">
          <b-form-input v-model="textToFind"  class="m-3" placeholder="Cлово которое нужно найти"></b-form-input>
          <b-form-input v-model="textToReplace"  class="m-3" placeholder="Слово на которое нужно заменить"></b-form-input>
          <b-btn variant="outline-primary"  @click="replaceMethod"  class="m-3">Заменить</b-btn>
        </div>

      </div>
      <div class="btn-group-center">
        <b-btn @click="textToChange='';textToFind='';textToReplace=''" class="m-3" variant="outline-primary">Сбросить</b-btn>
      </div>
    </section>
    <section v-if="selected==1" class="page-element">
      <h1 class="titleSection">Конвертер регистров</h1>
      <div class="text-wrapper">
        <div class="textarea-wrap">
          <b-btn style="width: 100%;" onclick="document.getElementById('file-input').click();" class="m-3"
                 variant="outline-primary">Импортировать
          </b-btn>
          <input @change="uploadString" id="file-input" type="file" name="name" style="display: none;"/>
          <textarea
              class="textarea"
              id="textarea"
              v-model="textToChange"
              placeholder="Введите текст который нужно изменить"
              rows="3"
          ></textarea>
        </div>
        <div class="textarea-wrap">
          <b-btn style="width: 100%;" @click="downloadString()" class="m-3" variant="outline-primary">Экспортировать
          </b-btn>
          <textarea
              disabled
              class="textarea"
              id="textarea"
              v-model="texChanged"
              placeholder="Измененный текст..."
          ></textarea>
        </div>

      </div>
      <div class="btn-group-center">
        <b-btn @click="textToChange='';texChanged=''" class="m-3" variant="outline-primary">Сбросить</b-btn>
      </div>
      <b-form-group
          class="m-auto"
          v-slot="{ ariaDescribedby }"
      >
        <b-form-radio-group
            id="btn-radios-2"
            v-model="selectedRegister"
            :options="optionsRegister"
            :aria-describedby="ariaDescribedby"
            button-variant="outline-primary"
            size="lg"
            name="radio-btn-outline"
            buttons
        ></b-form-radio-group>
      </b-form-group>
    </section>
    <section v-if="selected==3" class="page-element">
      <h1 class="titleSection">Посчитать количество символов в тексте</h1>
      <div class="text-wrapper">
        <div class="textarea-wrap">
          <b-btn style="width: 100%;" onclick="document.getElementById('file-input').click();" class="m-3"
                 variant="outline-primary">Импортировать
          </b-btn>
          <input @change="uploadString" id="file-input" type="file" name="name" style="display: none;"/>
          <textarea
              class="textarea"
              id="textarea"
              v-model="textToChange"
              placeholder="Введите текст который нужно изменить"
              rows="3"
          ></textarea>
        </div>
        <div class="textarea-wrap">
          <table class="textCountTable table_blur">
            <tr>
              <th>Полная длинна текста (без перевода строки)</th>
              <th>Всего символов</th>
              <th>Без пробелов</th>
              <th>Количество слов</th>
              <th>Количество уникальных слов</th>

            </tr>
            <tr>
              <td>{{ symbolsCount.total_characters }}</td>
              <td>{{ symbolsCount.total_characters_length }}</td>
              <td>{{ symbolsCount.without_spaces }}</td>
              <td>{{ symbolsCount.word_count }}</td>
              <td>{{ symbolsCount.word_count_uniq }}</td>

            </tr>
            <tr v-if="symbolsCount.words.length>0">
              <th colspan="3">Ключевое слово</th>
              <th>Количество</th>
              <th>Процентное количество</th>
            </tr>
            <tr v-for="item in symbolsCount.words_filtered" :key="item">
              <td colspan="3">{{ item.word }}</td>
              <td>{{ item.count }}</td>
              <td>{{item.percent}}%</td>
            </tr>

          </table>
        </div>
      </div>
      <div class="btn-group-center">
        <b-btn @click="textToChange='';texChanged=''" class="m-3" variant="outline-primary">Сбросить</b-btn>
      </div>
    </section>


  </div>
</template>

<script>
export default {
  name: 'texEditor',
  data() {
    return {
      fileName: 'тестовый файл',
      textToFind:'',
      textToReplace:'',
      textToChange: '',
      texChanged: '',
      selected: '2',
      selectedRegister: '1',
      options: [
        {text: 'Конвертер регистров', value: '1'},
        {text: 'Замена и удаление слов', value: '2'},
        {text: 'Посчитать количество слов и символов в тексте', value: '3'},
        {text: 'Оформление текста', value: '4'},
      ],
      optionsRegister: [
        {text: 'ВЕРХНИЙ РЕГИСТР', value: '1'},
        {text: 'нижний регистр', value: '2'},
        {text: 'Заглавные Буквы', value: '3'},
        {text: 'иНВЕРСИЯ рЕГИСТРА', value: '4'},
        {text: 'атскет яисревнИ', value: '5'}
      ],
      symbolsCount: {
        total_characters_length: 0,
        total_characters: 0,
        without_spaces: 0,
        word_count: 0,
        word_count_uniq: 0,
        words: [],
        wordsFiltered: [],
      }
    }
  },
  methods: {
    replaceMethod(){
      this.textToChange=this.textToChange.replaceAll(this.textToFind,this.textToReplace)
    },
    saveText(e) {
      this.textToChange = e.target.result
    },
    uploadString(e) {
      var file = e.target.files[0];
      var reader = new FileReader();
      reader.onload = this.saveText;
      reader.readAsText(file)

    },
    downloadString() {
      var blob = new Blob([this.texChanged], {type: 'txt'});
      var a = document.createElement('a');
      a.download = this.fileName;
      a.href = URL.createObjectURL(blob);
      a.dataset.downloadurl = ['txt', a.download, a.href].join(':');
      a.style.display = "none";
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      setTimeout(function () {
        URL.revokeObjectURL(a.href);
      }, 1500);
    },
    symbolsCounter() {
      this.symbolsCount.total_characters_length = this.textToChange.toLowerCase().length
      this.symbolsCount.total_characters = this.textToChange.toLowerCase().replace(/(\r\n|\n|\r)/gm, "").length
      this.symbolsCount.without_spaces = this.textToChange.toLowerCase().replace(/(\r\n|\n|\r)/gm, "").replace(/ /g, "").length;
      this.symbolsCount.word_count = this.textToChange.toLowerCase().replace(/\r\n?|\n/g, ' ').replace(/ {2,}/g, ' ').replace(/^ /, '').replace(/ $/, '').split(' ').length
      this.symbolsCount.word_count_uniq = [...new Set(this.textToChange.toLowerCase().replace(/\r\n?|\n/g, ' ').replace(/ {2,}/g, ' ').replace(/^ /, '').replace(/ $/, '').split(' '))].length
      this.symbolsCount.words = [...new Set(this.textToChange.replace(/[^a-zа-яё0-9\s]/gi, '').replace(/\r\n?|\n/g, ' ').replace(/ {2,}/g, ' ').replace(/^ /, '').replace(/ $/, '').split(' '))]
      this.symbolsCount.words_filtered=[]
      for (var word = 0; word < this.symbolsCount.words.length; word++) {
        this.symbolsCount.words_filtered.push({
          'word': this.symbolsCount.words[word],
          'count': (this.textToChange.split(this.symbolsCount.words[word]).length - 1),
          'percent':Math.round((((this.textToChange.split(this.symbolsCount.words[word]).length - 1)/this.symbolsCount.word_count)*100)*100)/100
        })
      }


      if (this.symbolsCount.total_characters == 0  || this.symbolsCount.without_spaces==0 ) {
        this.symbolsCount.total_characters=0
        this.symbolsCount.words_filtered=[]
        this.symbolsCount.words = []
        this.symbolsCount.word_count = 0
        this.symbolsCount.word_count_uniq = 0
      }
    },
    changeRegister() {
      switch (this.selectedRegister) {
        case '1':
          this.texChanged = this.textToChange.toUpperCase()
          break;
        case '2':
          this.texChanged = this.textToChange.toLowerCase()
          break;
        case '3':
          this.texChanged = [...this.textToChange].map(letter => letter[0].toUpperCase() + letter.substring(1)).join('')
          break;
        case '4':
          this.texChanged = [...this.textToChange].map(letter => letter === letter.toUpperCase() ? letter.toLowerCase() : letter.toUpperCase()).join('')
          break;
        case '5':
          this.texChanged = [...this.textToChange].reverse().join('')
          break;

      }

    }
  },
  watch: {
    textToChange() {
      switch (this.selected) {
        case '1':
          this.changeRegister()
          break
        case '3':
          this.symbolsCounter()
          break
      }
    },
    selectedRegister() {
      this.changeRegister()
    }
  },

  mounted() {

  }
}
</script>
<style>
input[type="radio"] {
  display: none !important;
}

.table_blur {
  background: #f5ffff;
  border-collapse: collapse;
  text-align: center;
}

.table_blur th {
  border-top: 1px solid #007bff;
  border-bottom: 1px solid #007bff;
  box-shadow: #007bff;
  background: #007bff;
  color: white;
  padding: 10px 15px;
  position: relative;
}

.table_blur th:after {
  content: "";
  display: block;
  position: absolute;
  left: 0;
  top: 25%;
  height: 25%;
  width: 100%;
  background: linear-gradient(rgba(255, 255, 255, 0), rgba(255, 255, 255, .08));
}

.table_blur tr:nth-child(odd) {
  background: #ebf3f9;
}

.table_blur th:first-child {
  border-left: 1px solid #777777;
  border-bottom: 1px solid #777777;
  box-shadow: inset 1px 1px 0 #999999, inset 0 -1px 0 #999999;
}

.table_blur th:last-child {
  border-right: 1px solid #777777;
  border-bottom: 1px solid #777777;
  box-shadow: inset -1px 1px 0 #999999, inset 0 -1px 0 #999999;
}

.table_blur td {
  border: 1px solid #e3eef7;
  padding: 10px 15px;
  position: relative;
  transition: all 0.5s ease;
}

.table_blur tbody:hover td {
  color: transparent;
  text-shadow: 0 0 3px #a09f9d;
}

.table_blur tbody:hover tr:hover td {
  color: #444444;
  text-shadow: none;
}

.textCountTable {
  margin: auto
}

.textarea-wrap {
  width: 45% !important;
  display: flex;
  flex-direction: column;
}

.btn-group-center {
  margin: auto
}

.m-auto {
  margin: auto
}

.text-container {

  display: flex;
  flex-direction: column;
}

.text-wrapper {
  justify-content: space-between;
  display: flex;
  flex-wrap: wrap;
  width: 90%;
  margin: auto;
}

.page-element {
  display: flex;
  flex-direction: column;
}

.textarea {
  overflow-x: hidden;
  padding: 20px;
  min-height: 500px;
  height: max-content;
  width: 100% !important;
  margin: 1rem;
}

.titleSection {
  text-align: center;
  margin-bottom: 1rem;
}

</style>


