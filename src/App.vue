<template>
  <div>
    <section class="hero is-info">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            Emoji 2 Text
          </h1>
          <h2 class="subtitle">
            Transforma tu texto en emojis.
            <a
              target="blank"
              href="https://twitter.com/brennerabarrios/status/1024758601406140417?s=21">Inspirado en el twit de @brennerabarrios</a>
          </h2>
        </div>
      </div>
    </section>

    <div id="app" class="container">
      <div class="columns">
        <div class="column">
          <textarea
            class="textarea is-primary"
            placeholder="Hello world!"
            v-model="message"
            maxlength="360"
            cols="30"
            rows="4"></textarea>
        </div>
      </div>

      <div class="columns">
        <div class="column">
          <form>
            <div class="field">
              <textarea
                class="textarea is-warning"
                placeholder="Hello world!"
                :value="getEmojis"
                cols="30"
                disabled
                rows="8">
              </textarea>
            </div>

            <div class="columns is-gapless is-mobile">
              <div class="column has-text-left is-vertical-center">
                Caracteres: {{count}}
              </div>
              <div class="column">
                <div class="field is-grouped is-grouped-right">
                  <div class="control">
                    <button
                      class="button is-primary"
                      type="button"
                      v-clipboard:copy="getEmojis">
                      Copiar!
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>

    <footer class="footer">
      <div class="content has-text-centered">
        {{getText}}
        <p>
          <strong>Text2Emoji</strong> by <a href="https://github.com/iddar">Iddar Olivares</a>. The source code is licensed
          <a href="http://opensource.org/licenses/mit-license.php">MIT</a>.
        </p>
      </div>
    </footer>
    <a href="https://github.com/iddar/text2emoji"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_green_007200.png" alt="Fork me on GitHub"></a>
  </div>
</template>

<script>
import findkey from 'lodash.findkey'
import GraphemeSplitter from 'grapheme-splitter'


import dic from './lib/dic.json'
import HelloWorld from './components/HelloWorld.vue'

const splitter = new GraphemeSplitter()

export default {
  name: 'app',
  data: () => {
    return {
      message: 'VETE SI NO SIENTEES QUE MI BOCA TE PROVOCA SENSACIONES CUANDO RONDA POR TUS LABIOS'
    }
  },
  computed: {
    count () {
      return this.getEmojis.length
    },
    getEmojis () {
      return [].map.call(this.message, letter => {
        if (letter === ' ') return '\n'
        let UpperCase = letter.toLocaleUpperCase()
        return UpperCase in dic ? dic[UpperCase] : ''
      }).join('')
    },
    getText () {
      return splitter.splitGraphemes(this.getEmojis).map(emoji => {
        if (emoji === '\n') return ' '
        return findkey(dic, o => o === emoji)
      }).join('')
    }
  },
  components: {
    HelloWorld
  }
}
</script>

<style>
@import url('https://cdnjs.cloudflare.com/ajax/libs/bulma/0.7.1/css/bulma.min.css');

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.is-vertical-center {
  display: flex;
  align-items: center;
}

</style>
